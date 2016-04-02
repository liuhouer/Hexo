title: "hibernate更新部分字段"
date: 2015-4-17 10:59:44
tags: [hibernate,java]
categories: hibernate
---

1.XML中设置property 标签 update = “false” ，如下：我们设置 age 这个属性在更改中不做更改 
 
    <property name=”age” update=”false”></property><BR> 
 
    在Annotation中 在属性GET方法上加上@Column(updatable=false) 
 
<!--more-->

    @Column(updatable=false)
        public int getAge() {
        return age;
        }
 
    我们在执行 Update方法会发现，age 属性 不会被更改
 
    Hibernate: 
        UPDATE
        Teacher 
        SET
        birthday=?,
        name=?,
        title=? 
        WHERE
        id=?
 
    缺点：不灵活····这个很少有人选择这种处理方法吧
 
 
 
 
2.第2种方法:使用XML中的 dynamic-update="true"
 
    <class name="com.sccin.entity.Student" table="student" dynamic-update="true"/><BR> 
 
    OK,这样就不需要在字段上设置了。
    // ····这个很少有人选择这种处理方法吧，因为现在是注解的天下啊
    但这样的方法在Annotation中没有
 
 
 
 
3.第2种方式:使用HQL语句（灵活，方便）
使用HQL语句修改数据 
 
                                                                                                                                                                                    
 
     public void update(){ 
 
     Session session = HibernateUitl.getSessionFactory().getCurrentSession();
 
     session.beginTransaction();
 
     Query query = session.createQuery("update Teacher t set t.name = 'yangtianb' where id = 3"); 
 
     query.executeUpdate();  
 
     session.getTransaction().commit();  
 
     Hibernate 执行的SQL语句：
     Hibernate:
        update
        Teacher
        set
        name='yangtianb'
        where
        id=3
 
  	// ····这个很少有人选择这种处理方法吧，用了hibernate还写基础的sql，不靠谱。
 
4.第4种方法:
 
    将需要更新的对象加载上来后，利用BeanUtils类的copyProperties方法，将需要更新的值copy到对象中，最后再调用update方法。
 
    注意：这里推荐使用的方法并非org.apache.comm*****.beanutils包中的方法，而是org.springframework.beans.BeanUtils中的copyProperties方法。原因是Spring工具类提供了copyProperties(source, target, ignoreProperties)方法，它能在复制对象值的时候忽略指定属性值，保护某些值不被恶意修改，从而更安全的进行对象的更新。此外，根据一些测试结果spring中的copyProperties方法效率要高于apache的方法（这点未作进一步验证）。
 
    参考代码：
 
    Admin persistent = adminService.load(id);// 加载对象
    BeanUtils.copyProperties(admin, persistent, new String[]{"id", "username"});// 复制对象属性值时忽略id、username属性，可避免username被恶意修改
    adminService.update(persistent);// 更新对象

    // ····推荐这个，用起来还是很方便的 啊



5.第5种方法:这也是挺方便的一种方式吧
 
   	不用工具的话  只能先查询出来，再部分setPropertity
   	//参数 String nickname。。。。。
   	例如：User m1 = userservice.getModel(userid);
   	m1.setNickName(nickname)；
   	m1.setXXX(XXX);
   	m1.update()；

 