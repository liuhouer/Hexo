
title: "hibernate基于注解的主键生成策略"
date: 2016-03-15 15:18:21
tags: [hibernate]
categories: hibernate
---

# 楔子

> **最近想把UUID32位的主键策略改为6位的INT自增的主键，来改善个人网站的优雅和简洁性。于是找了些资料整理在这。由于我的网站用的注解，来减少xml配置文件带来的烦躁，所以主键策略一定要是基于注解的。**






# 一. 自定义主键生成策略，由*@GenericGenerator*实现。**

*hibernate*在*JPA*的基础上进行了扩展，可以用一下方式引入*hibernate*独有的主键生成策略，就是通过*@GenericGenerator*加入的。**

比如说，*JPA*标准用法**
```

@Id  
@GeneratedValue(GenerationType.AUTO)  

就可以用hibernate特有以下用法来实现

@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "assigned")  


@GenericGenerator的定义:

@Target({PACKAGE, TYPE, METHOD, FIELD})   
@Retention(RUNTIME)   
public @interface GenericGenerator {   
  
String name();   
  
String strategy();   
  
Parameter[] parameters() default {};   
}  
```

* name属性指定生成器名称。
* strategy属性指定具体生成器的类名。
* parameters得到strategy指定的具体生成器所用到的参数。


<!--more-->

对于这些*hibernate*主键生成策略和各自的具体生成器之间的关系*,*在*org.hibernate.id.IdentifierGeneratorFactory*中指定了*,*

```

static {   
   GENERATORS.put("uuid", UUIDHexGenerator.class);   
   GENERATORS.put("hilo", TableHiLoGenerator.class);   
   GENERATORS.put("assigned", Assigned.class);   
   GENERATORS.put("identity", IdentityGenerator.class);   
   GENERATORS.put("select", SelectGenerator.class);   
   GENERATORS.put("sequence", SequenceGenerator.class);   
   GENERATORS.put("seqhilo", SequenceHiLoGenerator.class);   
   GENERATORS.put("increment", IncrementGenerator.class);   
   GENERATORS.put("foreign", ForeignGenerator.class);   
   GENERATORS.put("guid", GUIDGenerator.class);   
   GENERATORS.put("uuid.hex", UUIDHexGenerator.class); //uuid.hex is deprecated   
   GENERATORS.put("sequence-identity", SequenceIdentityGenerator.class);   
}  
```
# 二. 上面十二种策略，加上*native*，*hibernate*一共默认支持十三种生成策略。

## 1、native

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "native")   

```

## 2、uuid

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "uuid")   
```

## 3、hilo

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "hilo")   
```

## 4、assigned

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "assigned")   
```

## 5、identity

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "identity")   
```

## 6、select

```
@GeneratedValue(generator = "paymentableGenerator")   
@GenericGenerator(name="select", strategy="select",   
      parameters = { @Parameter(name = "key", value = "idstoerung") })  
```

## 7、sequence

```
@GeneratedValue(generator = "paymentableGenerator")   
@GenericGenerator(name = "paymentableGenerator", strategy = "sequence",   
          parameters = { @Parameter(name = "sequence", value = "seq_payablemoney") })  
```

## 8、seqhilo

```
@GeneratedValue(generator = "paymentableGenerator")   
@GenericGenerator(name = "paymentableGenerator", strategy = "seqhilo",   
          parameters = { @Parameter(name = "max_lo", value = "5") })  
```

## 9、increment

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "increment")   
```

## 10、foreign

```
@GeneratedValue(generator = "idGenerator")   
@GenericGenerator(name = "idGenerator", strategy = "foreign",parameters = { @Parameter(name = "property", value = "employee") })  


注意：直接使用@PrimaryKeyJoinColumn 报错（?）

@OneToOne(cascade = CascadeType.ALL)   
@PrimaryKeyJoinColumn   
例如

@Entity  
public class Employee {   
  @Id Integer id;   
       
  @OneToOne @PrimaryKeyJoinColumn  
   EmployeeInfo info;   
      
}  
应该为

@Entity  
public class Employee {   
  @Id   
  @GeneratedValue(generator = "idGenerator")   
  @GenericGenerator(name = "idGenerator", strategy = "foreign",   
          parameters = { @Parameter(name = "property", value = "info") })   
   Integer id;   
       
  @OneToOne  
   EmployeeInfo info;   
      
} 
``` 
## 11、guid

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "guid")   
```
## 12、uuid.hex

```
@GeneratedValue(generator = "paymentableGenerator")     
@GenericGenerator(name = "paymentableGenerator", strategy = "uuid.hex")   
```

## 13、sequence-identity

```
@GeneratedValue(generator = "paymentableGenerator")   
@GenericGenerator(name = "paymentableGenerator", strategy = "sequence-identity",   
          parameters = { @Parameter(name = "sequence", value = "seq_payablemoney") })  

```
# 三、通过@GenericGenerator自定义主键生成策略
如果实际应用中，主键策略为程序指定了就用程序指定的主键（*assigned*），没有指定就从*sequence*中取。**
明显上面所讨论的策略都不满足，只好自己扩展了，集成*assigned*和*sequence*两种策略。


```
public class AssignedSequenceGenerator extends SequenceGenerator implements   
PersistentIdentifierGenerator, Configurable {   
private String entityName;   
     
public void configure(Type type, Properties params, Dialect dialect) throws MappingException {   
   entityName = params.getProperty(ENTITY_NAME);   
  if (entityName==null) {   
   throw new MappingException("no entity name");   
   }   
     
  super.configure(type, params, dialect);     
}   
  
public Serializable generate(SessionImplementor session, Object obj)   
  throws HibernateException {   
     
   Serializable id = session.getEntityPersister( entityName, obj )   
     .getIdentifier( obj, session.getEntityMode() );   
     
  if (id==null) {   
    id = super.generate(session, obj);   
   }   
     
  return id;   
}   
}  
```
实际应用中，定义同sequence。

```
@GeneratedValue(generator = "paymentableGenerator")   
@GenericGenerator(name = "paymentableGenerator", strategy = "AssignedSequenceGenerator",   
      parameters = { @Parameter(name = "sequence", value = "seq_payablemoney") })  

```
值得注意的是，定义的这种策略，就像打开了潘多拉魔盒，非常不可控。正常情况下，不建议这么做。
 
策略解释

``` 

* “assigned”  
  主键由外部程序负责生成，在   save()   之前指定一个。  
  
*  “hilo”  
  通过hi/lo   算法实现的主键生成机制，需要额外的数据库表或字段提供高位值来源 
   
*  “seqhilo”  
  与hilo   类似，通过hi/lo   算法实现的主键生成机制，需要数据库中的   Sequence，适用于支持   Sequence   的数据库，如Oracle。  
  
*  “increment”  
  主键按数值顺序递增。此方式的实现机制为在当前应用实例中维持一个变量，以保存着当前的最大值，之后每次需要生成主键的时候将此值加1作为主键。这种方式可能产生的问题是：不能在集群下使用。  
  
*  “identity”  
  采用数据库提供的主键生成机制。如DB2、SQL   Server、MySQL   中的主键生成机制。  
  
*  “sequence”  
  采用数据库提供的   sequence   机制生成主键。如   Oralce   中的Sequence。  
  
*  “native”  
  由   Hibernate  根据使用的数据库自行判断采用   identity、hilo、sequence   其中一种作为主键生成方式。    
  
*  “uuid.hex”  
  由   Hibernate   基于128   位   UUID   算法   生成16   进制数值（编码后以长度32   的字符串表示）作为主键。  
  
*  “uuid.string”  
  与uuid.hex   类似，只是生成的主键未进行编码（长度16），不能应用在   PostgreSQL   数据库中。  
  
*  “foreign”  
  使用另外一个相关联的对象的标识符作为主键。 

```

---

设置自增长主键的初始值

```
alter table test AUTO_INCREMENT = 500000;
```



