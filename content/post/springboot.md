---
title: springboot
date: 2026-03-09
slug: springboot
categories:
    - springboot
tags:
    - springboot
---

## 把接口参数中的空白值替换为null值
[参考文档](https://zhuanlan.zhihu.com/p/344190594)

## xxx-job分布式定时器
[详情](https://www.jianshu.com/p/fa7186bea84b)

## Velocity java代码生成
[基本用法](https://blog.csdn.net/nengyu/article/details/6671904)

## 使用undertow转发中文字符编码乱码问题
[文章](https://blog.csdn.net/hcd321/article/details/84904985)

## 后台接口自动生成
- 注解侵入性严重
  [Swagger详情](https://www.jianshu.com/p/12f4394462d5)
  [knife4j详情](https://blog.csdn.net/weixin_35681869/article/details/104809867) 比Swagger好

- 无注解侵入性严重
  [smart-doc](https://blog.csdn.net/weixin_42543046/article/details/114931230)

## 项目启动后热更新配置文件
[详情](https://blog.csdn.net/Climber4211/article/details/107669644)

## 动态增删Controller
[参考](https://www.cnblogs.com/colin-xun/p/10573504.html)

## 事件发布以及注册
[参考](https://www.cnblogs.com/juncaoit/p/13275339.html)

# 注意事项：
1、如果2个事件之间是继承关系，会先监听到子类事件，处理完再监听父类。

2、监听器方法一定要try-catchy异常，否则会造成发布事件（有事务的）的方法进行回滚

3、可以使用@Order注解控制多个监听器的执行顺序，@Order 传入的值越小，执行顺序越高

## 缓存注解@Cacheable、@CacheEvict、@CachePut使用
[详情](https://www.cnblogs.com/fashflying/p/6908028.html)


## spring整个Bean初始化中的执行顺序：
Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)
## @PostConstruct
该注解被用来修饰一个void（）方法。被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。PostConstruct在构造函数之后执行，init（）方法之前执行

## @Scope（@Scope("prototype")）
单例变原型
## spring bean作用域有以下5个：
- singleton：单例模式，当spring创建applicationContext容器的时候，spring会欲初始化所有的该作用域实例，加上lazy-init就可以避免预处理；

- prototype：原型模式，每次通过getBean获取该bean就会新产生一个实例，创建后spring将不再对其管理；

（下面是在web项目下才用到的）

- request：搞web的大家都应该明白request的域了吧，就是每次请求都新产生一个实例，和prototype不同就是创建后，接下来的管理，spring依然在监听；

- session：每次会话，同上；

- global session：全局的web域，类似于servlet中的application。

## 定时器
@Scheduled(fixedRate = 1000)
@Scheduled(cron = "30 * * * * ?")
@EnableScheduling

## jpa
[详情](https://www.jianshu.com/p/c23c82a8fcfc)
### 实体类
#### 关于枚举（不推荐，推荐使用数据字典）
如果属性使用枚举类型，可使用javax的转换类
注解：
```java
@Convert(converter = StatusConverter.class)
关于枚举数据库存值
@Enumerated(EnumType.STRING) // 为枚举值
```
转换类
```java
import javax.persistence.AttributeConverter;
import javax.persistence.Converter;

/**
 * 状态枚举转换
 */
@Converter
public class StatusConverter implements AttributeConverter<StatusEnum, Integer> {

    /**
     * 转换为数据库内容时的操作
     */
    @Override
    public Integer convertToDatabaseColumn(ClassifyStatusEnum value) {
        return value.getIndex();
    }

    /**
     * 转换为实体枚举对象的操作
     */
    @Override
    public ClassifyStatusEnum convertToEntityAttribute(Integer dbData) {
        return ClassifyStatusEnum.toEnum(dbData);
    }

}
```

## @Entity
指定该类是一个实体
##  @Table(name = "Ammeter")
指定注释实体的主表
name为数据库表名 默认为类名(有OneToOne ManyToOne注解时推荐填写)
## @Id
指定实体的主键
##  @GeneratedValue
提供主键值的生成策略规范
### GenerationType四中类型
TABLE：使用一个特定的数据库表格来保存主键。
SEQUENCE：根据底层数据库的序列来生成主键，条件是数据库支持序列。
IDENTITY：主键由数据库自动生成（主要是自动增长型） （推荐）
AUTO：主键由程序控制。

## @Column
指定持久属性或字段的映射列。 如果未指定Column注释，则应用默认值
- name 数据库字段名 默认为属性名，会自动转驼峰为下划线 （推荐使用默认的）
- nullable 字段是否可为空，默认true
- columnDefinition 为列生成 DDL 时使用的 SQL 片段

## @JoinColumn
指定用于加入实体关联或元素集合的列
- name 数据库字段名 默认为属性名，会自动转驼峰为下划线 (在这个推荐使用自定义 如 )
- nullable 字段是否可为空，默认true
- columnDefinition 为列生成 DDL 时使用的 SQL 片段
- foreignKey 用于在表生成生效时指定或控制外键约束的生成。 如果未指定此元素，则将应用持久性提供程序的默认外键策略
### columnDefinition 用法
第一个为数据库类型
UNSIGNED 为无符号
COMMENT 数据库注释 注意 后面的 ‘ ’
1. long
   ``
       @Column(columnDefinition = "BIGINT UNSIGNED COMMENT '主键'")
       ``
2. String
   ``
      @Column(columnDefinition = "VARCHAR(255) COMMENT '描述'")
      ``
3. Boolean
   ``
       @Column( columnDefinition = "TINYINT UNSIGNED COMMENT '状态'")
       ``
4. LocalDateTime
   ``
       @Column( columnDefinition = "DATETIME COMMENT '时间'")
   ``

## @OneToOne  @ManyToOne @OneToMany
- targetEntity 作为关联目标的实体类 默认为属性类
- fetch 加载策略 默认 为及时加载 可修改为 FetchType.LAZY 懒加载
  关于 OneToMany
  请使用 mappedBy 的内容为 ManyToOne 的属性名
  并且增加注解
  @JsonProperty(access = JsonProperty.Access.WRITE_ONLY) // 序列化时不进行序列化

## @DynamicUpdate
## @DynamicInsert
## @JsonIgnoreProperties({"hibernateLazyInitializer", "handler"})

# @Embeddable
实体使用后。属性会作为其他实体的属性。并生成表字段

### 关于子查询
jpa不支持进行子查询
[参考文档](https://blog.csdn.net/wang124454731/article/details/54139528)
### 方法的用法
```java
public interface DemoJpaRepositories extends JpaRepository<DemoJpa,Integer> {

    //根据firstName与LastName查找(两者必须在数据库有)
    DemoJpa findByFirstNameAndLastName(String firstName, String lastName);

    //根据firstName或LastName查找(两者其一有就行)
    DemoJpa findByLastNameOrFirstName(String lastName,String firstName);

    //根据firstName查找它是否存在数据库里<类似与以下关键字>
    //DemoJpa findByFirstName(String firstName);
    DemoJpa findByFirstNameIs(String firstName);

    //在Age数值age到age2之间的数据
    List<DemoJpa> findByAgeBetween(Integer age, Integer age2);

    //小于指定age数值之间的数据
    List<DemoJpa> findByAgeLessThan(Integer age);

    //小于等于指定age数值的数据
    List<DemoJpa> findByAgeLessThanEqual(Integer age);

    //大于指定age数值之间的数据
    List<DemoJpa> findByAgeGreaterThan(Integer age);

    //大于或等于指定age数值之间的数据
    List<DemoJpa> findByAgeGreaterThanEqual(Integer age);

    //在指定age数值之前的数据类似关键字<LessThan>
    List<DemoJpa> findByAgeAfter(Integer age);

    //在指定age数值之后的数据类似关键字<GreaterThan>
    List<DemoJpa>  findByAgeBefore(Integer age);

    //返回age字段为空的数据
    List<DemoJpa> findByAgeIsNull();

    //返回age字段不为空的数据
    List<DemoJpa> findByAgeNotNull();

    /**
     * 该关键字我一度以为是类似数据库的模糊查询,
     * 但是我去官方文档看到它里面并没有通配符。
     * 所以我觉得它类似
     * DemoJpa findByFirstName(String firstName);
     * @see https://docs.spring.io/spring-data/jpa/docs/2.1.5.RELEASE/reference/html/#jpa.repositories
     */
    DemoJpa findByFirstNameLike(String firstName);

    //同上
    List<DemoJpa> findByFirstNameNotLike(String firstName);

    //查找数据库中指定类似的名字(如：输入一个名字"M" Jpa会返回多个包含M开头的名字的数据源)<类似数据库模糊查询>
    List<DemoJpa> findByFirstNameStartingWith(String firstName);

    //查找数据库中指定不类似的名字(同上)
    List<DemoJpa> findByFirstNameEndingWith(String firstName);

    //查找包含的指定数据源(这个与以上两个字段不同的地方在与它必须输入完整的数据才可以查询)
    List<DemoJpa> findByFirstNameContaining(String firstName);

    //根据age选取所有的数据源并按照LastName进行升序排序
    List<DemoJpa> findByAgeOrderByLastName(Integer age);

    //返回不是指定age的所有数据
    List<DemoJpa> findByAgeNot(Integer age);

    //查找包含多个指定age返回的数据
    List<DemoJpa> findByAgeIn(List<Integer> age);

}
```

## jackson中@JsonInclude注解详解
# 用处
这个注解就是用来在实体类序列化成json的时候在某些策略下，加了该注解的字段不去序列化该字段

# 用法
## ALWAYS
这个是默认策略，任何情况下都序列化该字段，和不写这个注解是一样的效果
## NON_NULL
这个最常用，属性为NULL 不序列化
## NON_ABSENT
属性为默认值不序列化
## NON_EMPTY
这个属性 空（""） 或者为 NULL都不序列化 这个也比较常用
## NON_DEFAULT
这个也好理解，如果字段是默认值的话就不序列化。

## CUSTOM
值，该值指示将使用单独的筛选器对象（由valueFilter为值本身指定，和/或contentFilter为结构化类型的内容指定）来确定包含条件。使用要序列化的值调用Filter对象的equals（）方法；如果返回真值，则排除（即过滤掉）；如果包含假值
## USE_DEFAULTS
伪值用于指示更高级别的默认值有意义，以避免覆盖包含值。例如，如果为属性返回，则将使用包含属性的类的默认值（如果有定义）；如果没有为此定义，则全局序列化包含详细信息。

# 接收前端参数格式化
```shell
@DateTimeFormat(pattern="yyyy-MM-dd HH:mm:ss")

yml
spring:
  jackson:
    # 日期格式化
    date-format: yyyy-MM-dd HH:mm:ss
```
# 返回给前端时间格式化
## 单属性配置
```shell
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
```
## 全局配置
### Data
```yaml
spring:
  jackson:
    # 日期格式化
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```
### LocalDateTime
```java

import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateTimeSerializer;
import org.springframework.boot.autoconfigure.jackson.Jackson2ObjectMapperBuilderCustomizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

/**
 * 本地日期时间格式化类
 *
 * @author zhonglingping
 */
@Configuration
public class LocalDateTimeConfig {

    @Bean
    public LocalDateTimeSerializer localDateTimeDeserializer() {
        return new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
    }

    @Bean
    public Jackson2ObjectMapperBuilderCustomizer jackson2ObjectMapperBuilderCustomizer() {
        return builder -> builder.serializerByType(LocalDateTime.class, localDateTimeDeserializer());
    }

}
```

# 配置类
```java

@Slf4j
@Configuration
public class JacksonConfig {

    @Primary
    @Bean
    public ObjectMapper getObjectMapper(Jackson2ObjectMapperBuilder builder, JacksonProperties jacksonProperties) {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
        // 全局配置序列化返回 JSON 处理
        SimpleModule simpleModule = new SimpleModule();
        // long类型
        simpleModule.addSerializer(Long.class, BigNumberSerializer.INSTANCE);
        simpleModule.addSerializer(Long.TYPE, BigNumberSerializer.INSTANCE);
        // 大整数
        simpleModule.addSerializer(BigInteger.class, BigNumberSerializer.INSTANCE);
        simpleModule.addSerializer(BigDecimal.class, ToStringSerializer.instance);
        // LocalDateTime
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern(jacksonProperties.getDateFormat());
        simpleModule.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(dateTimeFormatter));
        simpleModule.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(dateTimeFormatter));
        // LocalDate
        DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern(DatePattern.NORM_DATE_PATTERN);
        simpleModule.addSerializer(LocalDate.class, new LocalDateSerializer(dateFormatter));
        simpleModule.addDeserializer(LocalDate.class, new LocalDateDeserializer(dateFormatter));
        // LocalTime
        DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern(DatePattern.NORM_TIME_PATTERN);
        simpleModule.addSerializer(LocalTime.class, new LocalTimeSerializer(timeFormatter));
        simpleModule.addDeserializer(LocalTime.class, new LocalTimeDeserializer(timeFormatter));

        objectMapper.registerModule(simpleModule);
        objectMapper.setTimeZone(TimeZone.getDefault());
        log.info("初始化 jackson 配置完毕");
        return objectMapper;
    }

}
```

# 快速失败
```java
@Bean
public Validator validator() {
    ValidatorFactory validatorFactory = Validation.byProvider(HibernateValidator.class)
            .configure()
            // 快速失败模式
            .failFast(true)
            .buildValidatorFactory();
    return validatorFactory.getValidator();
}
```