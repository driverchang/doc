###### JPA学习笔记

1. JPA （Java Persistence API）的好处：使用JPA可以直接开始使用对象操作数据库，由框架根据映射的关系生成SQL，不用开发人员编写SQL语句。这屏蔽了各种数据库SQL的差异，编写的代买就可以一套代码兼容多种数据库。

2. 一些常用的映射注解及其说明：

   - @Entity声明该实体类是一个JPA标准的实体类
   - @Table指定实体类关联的表，注意如果不写表名，默认使用类名对应表名
   - @Column指定实体类属性对应的表字段，如果属性和字段一致，可以不写
   - @Id声明属性是一个OID，对应的一定是数据库的主键字段
   - @GenerateValue声明属性（Object ID）的主键生成策略
   - @SequenceGenerate使用SEQUENCE策略时，用于设置策略的参数
   - @TableGenerate使用TABLE主键策略时，用于设置策略的参数
   - @JoinTable关联查询时，表与表是多对多的关系时，指定多对多关联表中间表的参数
   - @JoinColumn关联查询时，表与表是一对一、一对多、多对一以及多对多的关系时，声明表关联的外键字段作为连接表的条件。必须配合关联表的注解一起使用 <key>
   - @OneToMany关联表注解，表示对应的实体和本类是一对多的关系
   - @ManyToOne关联表注解，表示对应的实体和本类是多对一的关系
   - @ManyToMany关联表注解，表示对应的实体和本类是多对多的关系
   - @OneToOne关联表注解，表示对应的实体和本类是一对一的关系

3. JPA常用的API及其说明：

   - Persistence用于读取配置文件，获得实体管理工厂

   - EntityManagerFactory用于管理数据库的连接，获得操作对象实体管理类

   - EntityManager实体管理类，用于操作数据库表，操作对象

     ```
     //获得实体管理类对像
     EnyityManager entityManager = JPAUtils.getEntityManager();
     ```

     

   - EntityTransaction用于管理事务。开始，提交，回滚

     ```
     //打开事务
     EnityTransaction transaction = entityManager.getTransaction();
     //启动事务
     transaction.begin();
     //提交
     transaction.commit();
     ```

     

   - TypeQuery用于操作JPQL的查询的

   - Query用于操作JPQL的查询接口，执行没有返回数据的JPQL（增删改）

   - CriteriaBuilder用户使用标准查询接口 Criteria查询接口

4. EntityManager.find()与EntityManager.getReference()的异同：

   - 相同点：这两个方法都接受实体的class和代表实体主键的对象作为参数。由于它们使用了Java泛型方法，无需任何显示的类型转换即可获得特定类型的实体对象。在EntityManager关闭的时候抛出IllegalStateException,当传入的第一个参数不是实体或者第二个参数不是一个有效的主键的情况下抛出。
   - 不同点：find()返回指定OID的实体，如果这个实体存在于当前的persistencecontext中，那么返回值是被缓存的对象；否则会创建一个新的实体，并从数据库中加载相关的持久状态。如果数据库不存在指定的OID的记录，那么find()方法返回null。getReference()方法和find()相似。不同的是：如果缓存中没有指定的实体，EntityManager会创建一个新的实体，但是不会立即访问数据库来加载持久状态，而是在第一次访问某个属性的时候才加载。此外，getReference()方法不返回null，如果数据库找不到相应的实体，这个方法会抛出javax.persistence.EntityNotFoundException。