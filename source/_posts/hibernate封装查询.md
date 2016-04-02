title: "hibernate 封装dao查询 总结"
date: 2014/11/24 14:32:36
tags: [java,hibernate,dao]
categories: hibernate
---




返回map型的List集合，不用数组去取下标，高效很多
List<Map<String, Object>>
------->
getSlaveSession().createSQLQuery(sql.toString()).setResultTransformer(Transformers.ALIAS_TO_ENTITY_MAP).list();

<!--more-->



    public List query(String hql, Object... args) {
        Query query = getMasterSession().createQuery(hql);
        for (int i = 0; i < args.length; i++)    query.setParameter(i, args[i]);
        return query.list();
    }  


///////////////////////////////////////////////////////////dao
package com.yunlu.dao;

import java.io.Serializable;
import java.util.LinkedHashMap;
import java.util.List;

import com.yl.util.QueryResult;

@SuppressWarnings("hiding")
public interface HibernateDao<T, PK extends Serializable> {

	public <T extends Serializable> PK save(T entity);

	public <T extends Serializable> void update(T entity);

	public <T extends Serializable> void update(LinkedHashMap<String, T> update);

	public <T extends Serializable> void update(
			LinkedHashMap<String, T> update, String where, Object[] parameter);

	public <T extends Serializable> void delete(T entity);

	public void delete(PK... primaryKeyId);

	public void delete(String where, Object[] parameter);

	public T find(PK primarKeyId);

	public List<T> findAll();

	public <T extends Serializable> QueryResult<T> findByCondition(
			int firstResult, int maxResults, String where,
			LinkedHashMap<String, String> orderBy);

	public <T extends Serializable> QueryResult<T> findByCondition(String where);
}

//////////////////////////////////////////////////////////////////////////////////////////////impl
package com.yunlu.dao.impl;

import java.io.Serializable;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import javax.annotation.Resource;

import org.apache.commons.lang.StringUtils;
import org.hibernate.Query;
import org.hibernate.SessionFactory;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

import com.yl.util.QueryResult;
import com.yl.util.ReflectManager;
import com.yunlu.dao.HibernateDao;

@SuppressWarnings({ "unchecked", "hiding" })
public abstract class HibernateDaoImpl<T, PK extends Serializable> implements
		HibernateDao<T, PK> {

	private LinkedHashMap<String, Object> temp = new LinkedHashMap<String, Object>();

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> PK save(T entity) {
		PK pk = null;
		pk = (PK) sessionFactory.getCurrentSession().save(entity);
		sessionFactory.getCurrentSession().flush();
		return pk;
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> void update(T entity) {
		// sessionFactory.getCurrentSession().update(entity);
		sessionFactory.getCurrentSession().saveOrUpdate(entity);
		sessionFactory.getCurrentSession().flush();
	}

	private <T extends Serializable> String getUpdateStr(
			LinkedHashMap<String, T> update) {
		StringBuilder updateStr = new StringBuilder();
		int i = 1;
		for (Map.Entry<String, T> entry : update.entrySet()) {
			updateStr.append(entry.getKey()).append("=").append(":u").append(i)
					.append(",");
			this.temp.put("u" + i, entry.getValue());
			i++;
		}

		updateStr.deleteCharAt(updateStr.length() - 1);

		return updateStr.toString();
	}

	private void setUpdateParameter(Query query) {
		for (Map.Entry<String, Object> entry : this.temp.entrySet()) {
			query.setParameter(entry.getKey(), entry.getValue());
		}
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> void update(LinkedHashMap<String, T> update) {

		this.update(update, null, null);

	}

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> void update(
			LinkedHashMap<String, T> update, String where, Object[] parameter) {

		String entityName = t.getName();

		// this.sessionFactory.getCurrentSession().createQuery("update "+entityName+"  o  o.name=:name").setParameter("name","xiaoxiao").executeUpdate();

		StringBuilder hql = new StringBuilder();

		hql.append("update " + entityName + " as o set "
				+ this.getUpdateStr(update));

		if (where != null && parameter != null) {
			hql.append(" ").append(where);
		}

		System.out.println(hql.toString());
		Query query = sessionFactory.getCurrentSession().createQuery(
				hql.toString());

		if (parameter != null) {
			this.setQueryParameter(query, parameter);
		}
		if (update != null) {
			this.setUpdateParameter(query);
		}

		query.executeUpdate();
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> void delete(T entity) {
		sessionFactory.getCurrentSession().delete(entity);
		sessionFactory.getCurrentSession().flush();
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public void delete(PK... primaryKeyId) {
		for (PK pkId : primaryKeyId) {
			Object entity = sessionFactory.getCurrentSession().get(t, pkId);
			sessionFactory.getCurrentSession().delete(entity);
			sessionFactory.getCurrentSession().flush();
		}
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public void delete(String where, Object[] parameter) {
		String entityName = t.getName();
		StringBuilder hql = new StringBuilder();
		hql.append("delete " + entityName + " as o");
		if (where != null && parameter != null) {
			hql.append(" ").append(where);
		}

		Query query = sessionFactory.getCurrentSession().createQuery(
				hql.toString());
		if (parameter != null) {
			this.setQueryParameter(query, parameter);
		}
		query.executeUpdate();

	}

	@Transactional(propagation = Propagation.REQUIRED)
	public T find(PK primarKeyId) {
		T findObj = null;
		findObj = (T) sessionFactory.getCurrentSession().get(t, primarKeyId);
		return findObj;
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public List<T> findAll() {
		String entityName = t.getName();
		String hql = "from " + entityName;
		Query query = sessionFactory.getCurrentSession().createQuery(hql);
		return query.list();
	}

	private void setQueryParameter(Query query, Object[] parameter) {
		if (parameter != null) {
			for (int i = 0; i < parameter.length; i++) {
				query.setParameter("p" + (i + 1), parameter[i]);
			}
		}

	}

	protected String getOrderStr(LinkedHashMap<String, String> orderBy) {
		StringBuilder orderStr = new StringBuilder();

		for (String key : orderBy.keySet()) {
			orderStr.append(key).append(" ").append(orderBy.get(key))
					.append(",");
		}

		orderStr.deleteCharAt(orderStr.length() - 1);

		return orderStr.toString();
	}

	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> QueryResult<T> findByCondition(
			int firstResult, int maxResults, String where,
			LinkedHashMap<String, String> orderBy) {

		String entityName = t.getName();

		StringBuilder hql = new StringBuilder();

		hql.append("select o from " + entityName + " as o");

		if (StringUtils.isNotEmpty(where)) {
			hql.append(where);
		}

		if (orderBy != null) {
			hql.append(" order by ").append(this.getOrderStr(orderBy));
		}

		Query query = null;

		if (firstResult != -1 && maxResults != -1) {
			query = sessionFactory.getCurrentSession()
					.createQuery(hql.toString()).setFirstResult(firstResult)
					.setMaxResults(maxResults);

		} else {
			query = sessionFactory.getCurrentSession().createQuery(
					hql.toString());
		}

		QueryResult<T> queryResult = new QueryResult<T>();
		queryResult.setResultlist(query.list());
		queryResult.setTotalrecord((long) query.list().size());

		return queryResult;

	}
	
	@Transactional(propagation = Propagation.REQUIRED)
	public <T extends Serializable> QueryResult<T> findByCondition(
			 String where) {

		String entityName = t.getName();

		StringBuilder hql = new StringBuilder();

		hql.append("select o from " + entityName + " as o");

		if (StringUtils.isNotEmpty(where)) {
			hql.append(where);
		}


		Query query = null;

		
		query = sessionFactory.getCurrentSession().createQuery(
					hql.toString());

		QueryResult<T> queryResult = new QueryResult<T>();

		queryResult.setResultlist(query.list());
		queryResult.setTotalrecord((long) query.list().size());

		return queryResult;

	}

	protected SessionFactory sessionFactory;

	public SessionFactory getSessionFactory() {
		return sessionFactory;
	}

	@Resource(name = "HibernateSessionFactory")
	public void setSessionFactory(SessionFactory sessionFactory) {
		this.sessionFactory = sessionFactory;
	}

	private Class<?> t;

	public HibernateDaoImpl() {
		t = ReflectManager.getFirstGenericTypeSuperclass(this.getClass());
	}

}













