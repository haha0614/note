# SpringCloud

在线文档：https://b11et3un53m.feishu.cn/wiki/space/7229522334074372099?ccm_open_type=lark_wiki_spaceLink&open_tab_from=wiki_home

## 微服务介绍

**微服务**是一种软件架构风格, 它是以专注于单一职责的很多小型项目为基础, 组合出复杂的大型应用

![image-20240709073743186](./图片/image-20240709073743186.png)

- 服务拆分
- 远程调用
- 服务治理
- 请求路由
- 身份认证
- 配置管理
- 服务保护
- 分布式事务
- 异步通信
- 消息可靠性
- 延迟消息
- 分布式搜索
- 倒排索引
- 数据聚合

# MybatisPlus

[MyBatis-Plus 🚀 为简化开发而生 (baomidou.com)](https://baomidou.com/)

## 入门

### 基本步骤

1. 引入MybatisPlus依赖, 代替Mybatis依赖

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.5.7</version>
   </dependency>
   ```

2. 定义Mapper接口并继承BaseMapper(泛型指定为要操作的实体类)

   ```java
   public interface UserMapper extends BaseMapper<User> {
   }
   ```

### 常见注解

**MybatisPlus**通过扫描实体类, 并基于反射获取实体类信息作为数据库表信息

#### 约定:

- 类名驼峰转下划线作为表名
- 名为id的字段作为主键
- 变量名驼峰转下划线作为表的字段名

#### 常见注解

MybatisPlus中比较常用的几个注解如下:

- **`@TableName`**: 用来指定表名

  ```java
  @TableName("表名")
  ```

- **`@TableId`**:用来指定表中的主键字段信息

  ```java
  @TableId(value = "主键名", type = IdType.类型)
  ```

- **`@TableField`**:用来指定表中的普通字段信息

  ```java
  @TableField("字段名")
  private String name;
  ```

##### IdType常见枚举

- `AUTO`:数据库自增长
- `INPUT`:通过set方法自行输入
- `ASSIGN_ID`:分配ID·接口IdentifierGenerator的方法nextId来生成id·默认实现类为DefaultIdentifierGenerator雪花算法

<font color='red'>**注意事项**</font>:如果没有指定类型并且在插入数据时没有指定主键值并且没有在配置文件手动配置配置默认的类型, 默认使用雪花算法

##### 使用@TableField的常见场景

- 成员变量名与数据库字段名不一致

  ```java
  @TableField("字段名")
  private String name;
  ```

- 成员变量名以is开头·且是布尔值

  ```java
  @TableField("is_married")
  private Boolean isMarried;
  ```

- 成员变量名与数据库关键字冲突

  ```java
  @TableField("`order`")
  private Integer order;
  ```

- 成员变量不是数据库字段

  ```java
  @TableField(exist = false)
  private String address;
  ```

官方文档:[注解配置 | MyBatis-Plus (baomidou.com)](https://baomidou.com/reference/annotation/)

#### 总结

MybatisPlus是如何获取实现CRUD的数据库表信息的?

- 默认以类名驼峰转下划线作为表名
- 默认把名为id的字段作为主键
- 默认把变量名驼峰转下划线作为表的字段名

MybatisPlus的常见注解有哪些?

- **`@TableName`**: 指定表名称及全局配置
- **`@TableId`**: 指定id字段及相关配置
- **`@TableField`**: 指定普通字段及相关配置

IdType的常见类型有哪些?

- `AUTO`、`ASSIGN_ID`、`INPUT`

使用`@TableField`的常见场景是?

- 成员变量名与数据库字段名不一致
- 成员变量名以is开头·且是布尔值

- 成员变量名与数据库关键字冲突

- 成员变量不是数据库字段

### 常见配置

**MybatisPlus**的配置项继承了Mybatis原生配置和一些自己特有的配置.例如:

```yml
mybatis-plus: 
	type-aliases-package: com.qqzj.mp... #别名扫描包(实体类包位置)
	mapper-locations: "classpath*:/mapper/**/*.xml" #Mapper.xml文件地址,默认值
	configuration:
		map-underscore-to-camel-case: true #是否开启下划线和驼峰的映射
		cache-enabled: false #是否开启二级缓存
	global-config:
		db-config:
			id-type: assign_id #id为雪花算法生成
			update-strategy: not_null #更新策略: 只更新非空字段
```

官方文档:[使用配置 | MyBatis-Plus (baomidou.com)](https://baomidou.com/reference/)

#### 总结

MybatisPlus使用的基本流程是什么?

1. 引入起步依赖
2. 自定义Mapper继承BaseMapper
3. 在实体类上添加注解声明 表信息
4. 在application.yml中根据需要添加配置

## 核心功能

### 条件构造器

**MybatisPlus**支持各种复杂的where条件, 可以满足日常开发的所有需求

![image-20240709190756077](./图片/image-20240709190756077.png)

![image-20240709190810666](./图片/image-20240709190810666.png)

![image-20240709190908674](./图片/image-20240709190908674.png)

![image-20240709191005499](./图片/image-20240709191005499.png)

![image-20240709191033387](./图片/image-20240709191033387.png)

#### 练习

```java
/**
 * 查询出名字中带o的,存款大于等于1000的人的id,username,info,balance字段
 */
@Test
void testQueryWrapper() {
    //构建查询条件
    QueryWrapper<User> wrapper = new QueryWrapper<User>()
          .select("id", "username", "info", "balance")
          .like("username", "o")
          .ge("balance", 1000);
    //查询
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}

/**
 * 更新用户名为jack的账户余额为2000
 */
@Test
void testUpdateByQueryWrapper(){
    //要更新的数据
    User user = new User();
    user.setBalance(2000);
    //更新的条件
    QueryWrapper<User> wrapper = new QueryWrapper<User>()
                .eq("username","jack");
    //执行更新
    userMapper.update(user,wrapper);
}

/**
 * 更新id为1,2,4的用户的余额,扣200
 */
@Test
void testUpdateByUpdateWrapper(){
    UpdateWrapper<User> wrapper = new UpdateWrapper<User>()
          .setSql("balance = balance - 200")
          .in("id",1L,2L,4L);
    userMapper.update(wrapper);
}

/**
 * lambdaQueryWrapper
 */
@Test
void testLambdaQueryWrapper(){
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<User>()
          .select(User::getId,User::getUsername,User::getBalance,User::getBalance)
          .like(User::getUsername,"o")
          .ge(User::getBalance,1000);
    userMapper.selectList(wrapper).forEach(System.out::println);
}
```

#### 总结

条件构造器的用法:

- `QueryWrapper`和`LambdaQueryWrapper`通常用来构建`select`、`delete`、`update`的`where`条件部分
- `UpdateWrapper`和`LambdaUpdateWrapper`通常只有在`set`语句比较特殊才使用
- 尽量使用`LambdaQueryWrapper`和`LambdaUpdateWrapper`, 避免硬编码

### 自定义SQL

我们可以利用MybatisPlus的Wrapper来构建复杂的where条件, 然后自己定义SQL语句中剩下的部分.

1. 基于Wrapper构建where条件

   ```java
   List<Long> ids = List.of(1L, 2L, 4L);
   int amount = 200;
   // 1.构建条件
   LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<User>().in(User::getId, ids);
   // 2.自定义SQL方法调用
   userMapper.updateBalanceByIds(wrapper, amount);
   ```

2. 在Mapper方法参数中用`@Param`注解声明wrapper变量名称, 必须是ew

   ```java
   void updateBalanceByIds(@Param("ew") LambdaQueryWrapper<User> wrapper, @Param("amount") int amount);
   ```

3. 自定义SQL, 并使用Wrapper条件

   ```xml
   <update id="updateBalanceByIds">
       UPDATE tb_user SET balance = balance - #{amount} ${ew.customSqlSegment}
   </update>
   ```
   

> 案例

```java
/**
 * 更新id为1,2,4的用户的余额,扣200
*/
@Test
void testCustomSqlUpdate(){
    //更新条件
    List<Long> ids = List.of(1L,2L,4L);
    int amount = 200;
    //定义条件
    LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<User>()
        .in(User::getId,ids);
    userMapper.updateBalanceByIds(wrapper,amount);
}
```

```java
public interface UserMapper extends BaseMapper<User> {
    void updateBalanceByIds(@Param(Constants.WRAPPER) LambdaUpdateWrapper<User> wrapper, @Param("amount") int amount);
}
```

```xml
<update id="updateBalanceByIds">
    update user set balance = balance - #{amount} ${ew.customSqlSegment}
</update>
```

### Service接口

#### 入门

![image-20240710064521880](./图片/image-20240710064521880.png)

> Service接口

```java
public interface IUserService extends IService<User> {
}
```

> Service实现类

```java
@Service
public class IUserServiceImpl extends ServiceImpl<UserMapper,User> implements IUserService  {
}
```

> 调用

```java
@Test
void testSaveUser(){
    User user = new User();
    //user.setId(5L);
    user.setUsername("LiLei");
    user.setPassword("123");
    user.setPhone("18688990011");
    user.setBalance(200);
    user.setInfo("{\"age\": 24, \"intro\": \"英文老师\", \"gender\": \"female\"}");
    user.setCreateTime(LocalDateTime.now());
    user.setUpdateTime(LocalDateTime.now());
    iUserService.save(user);
}

@Test
void testQuery(){
    iUserService.listByIds(List.of(1L, 2L, 3L)).forEach(System.out::println);
}
```

##### 总结

MP的Service接口使用流程是怎样的?

- 自定义Service接口继承IService接口

  ```java
  public interface IUserService 
      	extends IService<User>{}
  ```

- 自定义Service实现类, 实现自定义接口并继承ServiceImpl类

  ```java
  public class UserServiceImpl
      	extends ServiceImpl<UserMapper, User>
      	implements IUserService{
  }
  ```

#### 案例1

基于Restful风格实现下列接口

![image-20240710071248669](./图片/image-20240710071248669.png)

> Controller层

```java
@RestController
@RequestMapping("/users")
@Api(value = "用户管理",tags = "用户管理")
@Slf4j//日志输出
@RequiredArgsConstructor//必备的构造函数
public class UserController {
	private final IUserService iUserService;
	/**
	 * 新增用户
	 * @param userFormDTO 用户信息
	 */
	@PostMapping
	@ApiOperation(value = "新增用户")
	public void addUser(@Param("用户信息") @RequestBody UserFormDTO userFormDTO){
		//将DTO对象拷贝到PO对象中
		//import org.springframework.beans.BeanUtils;
		//BeanUtils.copyProperties(userFormDTO,user);
		
		//import cn.hutool.core.bean.BeanUtil;
		User user = BeanUtil.copyProperties(userFormDTO, User.class);
		//新增
		iUserService.save(user);
	}
	
	/**
	 * 根据id删除用户
	 * @param id 用户id
	 */
	@DeleteMapping("/{id}")
	@ApiOperation(value = "删除用户")
	public void deleteUserById(@ApiParam("用户id") @PathVariable Long id){
		iUserService.removeById(id);
	}
	
	/**
	 * 根据id查询用户信息
	 * @param id 用户ID
	 * @return 用户信息
	 */
	@ApiOperation(value = "根据id查询用户")
	@GetMapping("/{id}")
	public UserVO getUserById(@Param("用户ID") @PathVariable Long id){
		User user = iUserService.getById(id);
		return BeanUtil.copyProperties(user, UserVO.class);
	}
	
	/**
	 * 批量查询用户信息
	 * @param ids 用户ID集合
	 * @return 用户信息集合
	 */
	@GetMapping
	@ApiOperation(value = "根据ID批量查询")
	public List<UserVO> listUsers(@Param("用户ID集合") @RequestParam("ids") List<Long> ids){
		List<User> users = iUserService.listByIds(ids);
		return BeanUtil.copyToList(users, UserVO.class);
	}
	
	/**
	 * 根据id扣减余额
	 * @param id 用户ID
	 * @param money 需要扣减的金额
	 */
	@ApiOperation(value = "根据id扣除指定金额")
	@PutMapping("/{id}/deduction/{money}")
	public void deductMoneyById(@Param("用户ID") @PathVariable Long id,@Param("金额") @PathVariable Integer money){
		iUserService.deductMoneyById(id,money);
	}
}
```

> Service层

```java
public interface IUserService extends IService<User> {
	/**
	 * 根据ID扣减用户余额
	 * @param id 用户ID
	 * @param money 指定金额
	 */
	void deductMoneyById(Long id, Integer money);
}
```

> ServiceImpl

```java
@Service
public class IUserServiceImpl extends ServiceImpl<UserMapper, User> implements IUserService {
	
	/**
	 * 根据ID扣减用户余额
	 *
	 * @param id    用户ID
	 * @param money 指定金额
	 */
	@Override
	public void deductMoneyById(Long id, Integer money) {
		//1. 查询用户
		User user = this.getById(id);
		//2. 校验用户状态
		if (user == null || user.getStatus() != 1) {
			throw new RuntimeException("用户不存在或状态异常");
		}
		//3. 检验余额是否充足
		if (user.getBalance() < money) {
			throw new RuntimeException("余额不足");
		}
		//4. 扣减余额
		baseMapper.deductBanlanceById(id,money);
	}
}
```

> Mapper层

```java
public interface UserMapper extends BaseMapper<User> {
	void updateBalanceByIds(@Param(Constants.WRAPPER) LambdaUpdateWrapper<User> wrapper, @Param("amount") int amount);
	
	/**
	 * 根据ID扣减指定用户余额
	 * @param id 用户ID
	 * @param money 指定金额
	 */
	@Update("update user set balance = balance - #{money} where id = #{id}")
	void deductBanlanceById(@Param("id") Long id,@Param("money") Integer money);
}
```

> mapper.xml

```xml
<mapper namespace="com.itheima.mp.mapper.UserMapper">
    <update id="updateBalanceByIds">
        update user set balance = balance - #{amount} ${ew.customSqlSegment}
    </update>
</mapper>
```

#### 案例2

IService的Lambda查询

需求: 实现一个根据复杂条件查询用户的接口, 查询条件如下:

- name: 用户名关键字, 可以为空
- status: 用户状态, 可以为空
- minBalance: 最小余额, 可以为空
- maxBanlance: 最大余额, 可以为空

> Controller

```java
	/**
	 *
	 * @return
	 */
	@GetMapping("/list")
	@ApiOperation("根据复杂条件查询用户接口")
	public List<UserVO> queryUsers(UserQuery userQuery){
		List<User> users =  iUserService.queryUsers(userQuery.getName(),userQuery.getStatus(),userQuery.getMinBalance(),userQuery.getMaxBalance());
		return BeanUtil.copyToList(users,UserVO.class);
	}
```

> Service

```java
/**
 * 根据复杂条件查询用户
 * @param name 用户名
 * @param status 用户状态
 * @param minBalance 最小余额
 * @param maxBalance 最大余额
 * @return 用户列表
 */
List<User> queryUsers(String name, Integer status, Integer minBalance, Integer maxBalance);
```

> ServiceImpl

```java
@Override
public List<User> queryUsers(String name, Integer status, Integer minBalance, Integer maxBalance) {
    return lambdaQuery()
          .like(name != null, User::getUsername, name)
          .eq(status != null, User::getStatus, status)
          .ge(minBalance != null, User::getBalance, minBalance)
          .le(maxBalance != null, User::getBalance, maxBalance)
          .list();
}
```

#### 案例3

需求: 改造根据id修改用户余额的接口, 要求如下

1. 完成对用户状态校验
2. 完成对用户余额校验
3. 如果扣减后余额为0, 则将用户status修改为冻结状态(2)

```java
/**
 * 当用户余额扣减后等于0, 状态转为冻结
 * @param id 用户id
 * @param money 指定金额
 */
@Override
public void deductMoneyById2(Long id, Integer money) {
    //1. 查询用户
    User user = this.getById(id);
    //2. 校验用户状态
    if (user == null || user.getStatus() != 1) {
       throw new RuntimeException("用户不存在或状态异常");
    }
    //3. 检验余额是否充足
    if (user.getBalance() < money) {
       throw new RuntimeException("余额不足");
    }
    //4. 扣减余额
    int remainBalance = user.getBalance() - money;
    lambdaUpdate()
          .set(User::getBalance, user.getBalance() - money)
          .set(remainBalance == 0, User::getStatus, 2)
          .eq(User::getId, id)
          .eq(User::getBalance, user.getBalance())//乐观锁
          .update();
}
```

#### 案例4

需求: 批量插入10万条用户数据, 并作出对比:

- 普通for循环插入

  ```java
  @Test
  void testSaveOneByOne() {
      long start = System.currentTimeMillis();
      for (int i = 0; i < 100000; i++) {
          userService.save(buildUser(i));
      }
      long end = System.currentTimeMillis();
      System.out.println("for循环耗时：" + (end - start));
  }
  耗时:210000ms
  ```

- IService的批量插入

  ```java
  @Test
  void testSaveBatch(){
      long start = System.currentTimeMillis();
      List<User> list = new ArrayList<>(1000);
      for (int i = 1; i <= 100000; i++) {
          list.add(buildUser(i));
          if (i % 1000 == 0) {
              userService.saveBatch(list);
              list.clear();
          }
      }
      long end = System.currentTimeMillis();
      System.out.println("批量插入耗时：" + (end - start));//20336
  }
  耗时:23006ms
  ```

- 开启`rewriteBatchedStatements = true`参数

  > yaml文件

  ```yaml
  spring:
    datasource:
      url: jdbc:mysql://127.0.0.1:3306/mp?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai&rewriteBatchedStatements=true
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: 1234
  ```

  ```java
  @Test
  void testSaveBatch(){
      long start = System.currentTimeMillis();
      List<User> list = new ArrayList<>(1000);
      for (int i = 1; i <= 100000; i++) {
          list.add(buildUser(i));
          if (i % 1000 == 0) {
              userService.saveBatch(list);
              list.clear();
          }
      }
      long end = System.currentTimeMillis();
      System.out.println("批量插入耗时：" + (end - start));
  }
  耗时:9473ms
  ```

##### 结论

- 普通for循环逐条插入速度极差, 不推荐
- MP的批量新增, 基于预编译的批处理, 性能不错
- 配置jdbc参数, 开启`rewriteBatchedStatements`性能最好

## 扩展功能

### 代码生成

[Mybatis X 插件 | MyBatis-Plus (baomidou.com)](https://baomidou.com/guides/mybatis-x/)

### 静态工具

#### 案例

需求:

1. 改造根据ID查询用户的接口, 查询用户的同时, 查询出用户对应的地址

   ```java
   /**
    * 查询用户及其地址信息
    * @param id 用户ID
    * @return 用户信息
    */
   @Override
   public UserVO queryUserWithAddresses(Long id) {
       //1. 查询用户
       User user = this.getById(id);
       if (user == null || user.getStatus() == 2){
           throw new RuntimeException("用户不存在或状态异常");
       }
       //封装VO
       UserVO userVO = new UserVO();
       BeanUtil.copyProperties(user, userVO);
       //2. 查询地址
       List<Address> addresses = Db.lambdaQuery(Address.class)
           .eq(Address::getUserId, id)
           .list();
       //3.封装VO
       if (CollUtil.isNotEmpty(addresses)){
           userVO.setAddresses(BeanUtil.copyToList(addresses, AddressVO.class));
       }
       return userVO;
   }
   ```

2. 改造根据ID批量查询用户的接口, 查询用户的同时, 查询出用户对 应的所有地址

   ```java
   /**
    * 根据用户ID集合查询所有用户以及地址列表
    * @param ids 用户ID列表
    * @return 用户及地址列表
    */
   @Override
   public List<UserVO> listUsersAndAddresses(List<Long> ids) {
       //1. 查询用户
       List<User> users = this.listByIds(ids);
       if (CollUtil.isEmpty(users)){
           return Collections.emptyList();
       }
       //2. 查询地址
       //2.1 获取用户ID集合
       List<Long> userIds = users.stream().map(User::getId).collect(Collectors.toList());
       //2.2 根据用户ID查询地址
       List<Address> addresses = Db.lambdaQuery(Address.class)
           .in(Address::getUserId, userIds)
           .list();
       //转换地址VO
       List<AddressVO> addressVOS = BeanUtil.copyToList(addresses, AddressVO.class);
       Map<Long, List<AddressVO>> addressMap = new HashMap<>(0);
       if (CollUtil.isNotEmpty(addressVOS)){
           //分组
           addressMap = addressVOS.stream().collect(Collectors.groupingBy(AddressVO::getUserId));
       }
       //3. 封装VO
       List<UserVO> list = new ArrayList<>(users.size());
       for (User user : users) {
           //转换userVo
           UserVO userVO = BeanUtil.copyProperties(user, UserVO.class);
           userVO.setAddresses(addressMap.get(user.getId()));
           list.add(userVO);
       }
       return list;
   }
   ```

3. 实现根据用户ID查询收货地址功能, 需要验证用户状态, 冻结用户抛出异常

   ```java
   /**
    * 根据用户ID查询地址集合
    * @param userId 用户ID
    * @return 地址集合
    */
   @Override
   public List<AddressVO> getAddressesByUserId(Long userId) {
       User user = Db.getById(userId, User.class);
       if (user == null || user.getStatus() == 2){
           throw new RuntimeException("用户不存在或已冻结");
       }
       List<Address> addresses = this.lambdaQuery()
           .eq(Address::getUserId, userId)
           .list();
       List<AddressVO> addressVOS = BeanUtil.copyToList(addresses, AddressVO.class);
       if (CollUtil.isEmpty(addressVOS)){
           return Collections.emptyList();
       }
       return addressVOS;
   }
   ```

### 逻辑删除

**逻辑删除**就是基于代码逻辑模拟删除效果, 但并不会真正删除数据. 思路如下:

- 在表中添加一个字段标记数据是否被删除
- 当删除数据时把标记设置为1
- 查询时只查询标记为0的数据

**MybatisPlus**提供了逻辑删除功能, <font color='red'>无需改变方法调用的方式</font>, 而是在底层帮我们自动修改CRUD语句. 我们要做的就是在application.yml文件中配置逻辑删除的字段名称和值即可

```yml
mybatis-plus: 
	global-config:
		db-config:
			logic-delete-field: flag # 全局逻辑删除的实体字段名, 字段类型可以是boolean、integer
			logic-delete-value: 1 # 逻辑已删除值(默认为1)
			logic-not-delete-value: 0 # 逻辑未删除值(默认为0)
```

> <font color='red'>**注意**</font>: 逻辑删除本身也有自己的问题, 比如: 
>
> - 会导致数据库表垃圾数据越来越多, 影响查询效率
> - SQL中全部需要对逻辑删除字段做判断, 影响查询效率
>
> 因此, 不太推荐采用逻辑删除功能, 如果数据不能删除, 可以采用把数据迁移到其他表的方法

### 枚举处理器

#### 通用枚举

在application.yml中配置全局枚举处理器

```yml
mybatis-plus:
	configuration:
		default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
```

#### 总结

如何实现PO类中的枚举类型变量与数据库字段的转换?

1. 给枚举中的与数据库对应的value值添加`@EnumValue`注解

   ```java
   @EnumValue
   private final int value;
   @JsonValue
   private final String desc;
   ```

2. 在配置文件中配置统一的枚举处理器, 实现类型转换

   ```yml
   mybatis-plus:
   	configuration:
   		default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
   ```

### JSON处理器

#### 使用步骤:

1. 创建JSON数据对应的实体类, 并替换对应字段类型
2. 在实体类对应字段属性上添加`@TableField(tapeHandler = JacksonTypeHandler.class)`
3. 在实体类类名上添加注解`@TableName(autoResultMap = true)`

## 插件功能

MybatisPlus提供的内置拦截器有下面这些:

![image-20240713073354514](./图片/image-20240713073354514.png)

### 分页插件

首先, 要在配置类中注册MybatisPlus的核心插件, 同时添加分页插件:

```java
@Configuration
public class MybatisConfig{
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        // 1.初始化核心插件
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 2.添加分页插件
        PaginationInnerInterceptor pageInterceptor = new PaginationInnerInterceptor(DbType.MySQL);
        pageInterceptor.setMaxLimit(1000L); //设置分页上限
        interceptor.addInnerInterceptor(pageInterceptor);
        return interceptor;
    }
}
```

使用:

```java
/**
 * 分页查询
 */
@Test
void pageQueryTest() {
    // 1.准备分页条件
    // 1.1 分页条件
    Page<User> page = Page.of(1, 3);
    // 1.2 排序条件
    page.addOrder(OrderItem.ascs("balance", "id"));
    // 2.分页查询
    page = iUserService.page(page);
    // 3.解析
    long total = page.getTotal();
    long pages = page.getPages();
    List<User> records = page.getRecords();
    long size = page.getSize();
    long current = page.getCurrent();

    System.out.println("total = " + total);
    System.out.println("pages = " + pages);
    records.forEach(System.out::println);
    System.out.println("size = " + size);
    System.out.println("current = " + current);
}
```

#### 通用分页实体

##### 案例1:简单分页查询案例

需求: 遵循下面的接口规范, 编写一个UserController接口, 实现User的分页查询

![image-20240713075817315](./图片/image-20240713075817315.png)

![image-20240713075915470](./图片/image-20240713075915470.png)

```java
@Override
public PageDTO<UserVO> pageQueryUsers(UserQuery userQuery) {
    String name = userQuery.getName();
    Integer status = userQuery.getStatus();
    Integer minBalance = userQuery.getMinBalance();
    Integer maxBalance = userQuery.getMaxBalance();
    // 1.构建查询条件
    // 1.1分页条件
    Page<User> page = Page.of(userQuery.getPageNo(), userQuery.getPageSize());
    // 1.2排序条件
    OrderItem orderItem = null;
    if (userQuery.getSortBy() != null) {
       orderItem = userQuery.getIsAsc() ? OrderItem.asc(userQuery.getSortBy()) : OrderItem.desc(userQuery.getSortBy());
    } else {
       orderItem = OrderItem.desc("update_time");
    }
    page.addOrder(orderItem);
    // 2.分页查询
    Page<User> userPage = lambdaQuery()
          .like(name != null, User::getUsername, name)
          .eq(status != null, User::getStatus, status)
          .ge(minBalance != null, User::getBalance, minBalance)
          .le(maxBalance != null, User::getBalance, maxBalance)
          .page(page);
    // 3.封装VO
    PageDTO<UserVO> userVO = new PageDTO<>();
    userVO.setTotal(userPage.getTotal());
    userVO.setPages(userPage.getPages());
    userVO.setList(userPage.getRecords() == null ? Collections.emptyList() : BeanUtil.copyToList(userPage.getRecords(), UserVO.class));
    // 4.返回
    return userVO;
}
```

##### 案例2:通用分页实体

需求: 

1. 在PageQuery中定义方法, 将PageQuery对象转为MybatisPlus中的Page对象

   ```java
   @Data
   @ApiModel("分页查询实体")
   public class PageQuery {
   	@ApiModelProperty("页码")
       private Integer pageNo = 1;
   	@ApiModelProperty("页码")
       private Integer pageSize = 5;
   	@ApiModelProperty("排序字段")
       private String sortBy;
   	@ApiModelProperty("是否升序")
       private Boolean isAsc = true;
   
       public <T>  Page<T> toMpPage(OrderItem ... orders){
           // 1.分页条件
           Page<T> p = Page.of(pageNo, pageSize);
           // 2.排序条件
           if (StrUtil.isNotBlank(sortBy)) {
   	        OrderItem orderItem = isAsc ? OrderItem.asc(sortBy) : OrderItem.desc(sortBy);
   	        p.addOrder(orderItem);
           }else if(orders != null){
               p.addOrder(orders);
           }
           return p;
       }
   
       public <T> Page<T> toMpPage(String defaultSortBy, boolean isAsc){
   	    OrderItem orderItem = isAsc ? OrderItem.asc(defaultSortBy) : OrderItem.desc(defaultSortBy);
           return this.toMpPage(orderItem);
       }
   
       public <T> Page<T> toMpPageDefaultSortByCreateTimeDesc() {
           return toMpPage("create_time", false);
       }
   
       public <T> Page<T> toMpPageDefaultSortByUpdateTimeDesc() {
           return toMpPage("update_time", false);
       }
   }
   ```

2. 在PageDTO中定义方法, 将MybatisPlus中的Page结果转为PageDTO结果

   ```java
   @Data
   @NoArgsConstructor
   @AllArgsConstructor
   @ApiModel("分页结果")
   public class PageDTO<V> {
       @ApiModelProperty("总条数")
       private Long total;
       @ApiModelProperty("总页数")
       private Long pages;
       @ApiModelProperty("数据列表")
       private List<V> list;
   
       /**
        * 返回空分页结果
        * @param p MybatisPlus的分页结果
        * @param <V> 目标VO类型
        * @param <P> 原始PO类型
        * @return VO的分页对象
        */
       public static <V, P> PageDTO<V> empty(Page<P> p){
           return new PageDTO<>(p.getTotal(), p.getPages(), Collections.emptyList());
       }
   
       /**
        * 将MybatisPlus分页结果转为 VO分页结果
        * @param p MybatisPlus的分页结果
        * @param voClass 目标VO类型的字节码
        * @param <VO> 目标VO类型
        * @param <PO> 原始PO类型
        * @return VO的分页对象
        */
       public static <VO, PO> PageDTO<VO> of(Page<PO> p, Class<VO> voClass) {
           // 1.非空校验
           List<PO> records = p.getRecords();
           if (records == null || records.size() <= 0) {
               // 无数据，返回空结果
               return empty(p);
           }
           // 2.数据转换
           List<VO> vos = BeanUtil.copyToList(records, voClass);
           // 3.封装返回
           return new PageDTO<>(p.getTotal(), p.getPages(), vos);
       }
   
       /**
        * 将MybatisPlus分页结果转为 VO分页结果，允许用户自定义PO到VO的转换方式
        * @param p MybatisPlus的分页结果
        * @param convertor PO到VO的转换函数
        * @param <VO> 目标VO类型
        * @param <PO> 原始PO类型
        * @return VO的分页对象
        */
       public static <VO, PO> PageDTO<VO> of(Page<PO> p, Function<PO, VO> convertor) {
           // 1.非空校验
           List<PO> records = p.getRecords();
           if (records == null || records.size() <= 0) {
               // 无数据，返回空结果
               return empty(p);
           }
           // 2.数据转换
           List<VO> vos = records.stream().map(convertor).collect(Collectors.toList());
           // 3.封装返回
           return new PageDTO<>(p.getTotal(), p.getPages(), vos);
       }
   }
   ```

# Docker

<center>快速构建、运行、管理应用的工具</center>

## [‌‌⁠﻿⁠﻿‌‌﻿⁠⁠‍‍‍⁠﻿﻿‍‬‬﻿﻿‬﻿‍‬‌‬‬安装Docker - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/Rfocw7ctXij2RBkShcucLZbrn2d)

```java
tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com",
        "https://mirrors.tuna.tsinghua.edu.cn",
        "http://mirrors.sohu.com",
        "https://hcvkosva.mirror.aliyuncs.com",
        "https://ccr.ccs.tencentyun.com",
        "https://docker.m.daocloud.io",
        "https://docker.awsl9527.cn"
    ]
}
EOF
    
# 重新加载配置
systemctl daemon-reload

# 重启Docker
systemctl restart docker
```



## 镜像和容器

当我们利用Docker安装应用时, Docker会自动搜索并下载应用**镜像(image)**.镜像不仅包含应用本身, 还包含应用运行所需要的环境、配置、系统函数库. Docker会在运行镜像时创建一个隔离环境, 称为**容器(container)**

**镜像仓库**:存储和管理镜像的平台, Docker官方维护了一个公共仓库: [Docker Hub](hub.docker.com).

[Docker中文网 (github.net.cn)](https://docker.github.net.cn/)

![image-20240713105914646](./图片/image-20240713105914646.png)

## [部署MySQL]([‬‬‌‌﻿‌‬﻿‍﻿⁠⁠‌⁠‌‬‍﻿﻿‬‍‍‬‍⁠‍day02-Docker - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec))

## 命令解读

```clike
docker run -d \
    --name mysql \
    -p 3306:3306 \
    -e TZ-Asia/Shanghai \
    -e MYSQL_ROOT_PASSWORD=123 \
    mysql
```

- **docker run**: 创建并运行一个容器, **-d**是让容器在后台运行

- **--name mysql**: 给容器起个名字, 必须唯一

- **-p 3306:3306**: 设置端口映射

  ![image-20240713110823423](./图片/image-20240713110823423.png)

  - **容器是隔离环境**，外界不可访问。但是可以**将****宿主机****端口****映射容器内到端口**，当访问宿主机指定端口时，就是在访问容器内的端口了。
  - 容器内端口往往是由容器内的进程决定，例如MySQL进程默认端口是3306，因此容器内端口一定是3306；而宿主机端口则可以任意指定，一般与容器内保持一致。
  - 格式: `-p 宿主机端口:容器内端口`，示例中就是将宿主机的3306映射到容器内的3306端口

- **-e KEY=VALUE**: 是设置环境变量

  - `-e TZ=Asia/Shanghai` : 配置容器内进程运行时的一些参数
    - 格式：`-e KEY=VALUE`，KEY和VALUE都由容器内进程决定
    - 案例中，`TZ=Asia/Shanghai`是设置时区；`MYSQL_ROOT_PASSWORD=123`是设置MySQL默认密码

- **mysql**: 指定运行的镜像的名字

  - 镜像名称一般分两部分组成: `[repository]:[tag]`

    - 其中`repository`就是镜像名

    - `tag`是镜像的版本

      ![image-20240713111240561](./图片/image-20240713111240561.png)

  - 在没有指定`tag`时, 默认是`latest`, 代表最新版本的镜像

## Docker基础

### 常见命令

Docker最常见的命令就是操作镜像、容器的命令, 详见官方文档: [Docker官方](https://docs.docker.com/)

[中文参考文档| Docker 文档 (github.net.cn)](https://docker.github.net.cn/reference/)

| **命令**       | **说明**                       | **文档地址**                                                 |
| :------------- | :----------------------------- | :----------------------------------------------------------- |
| docker pull    | 拉取镜像                       | [docker pull](https://docs.docker.com/engine/reference/commandline/pull/)中文:[Docker_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/) |
| docker push    | 推送镜像到DockerRegistry       | [docker push](https://docs.docker.com/engine/reference/commandline/push/)中文:[Docker_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/push/) |
| docker images  | 查看本地镜像                   | [docker images](https://docs.docker.com/engine/reference/commandline/images/)中文:[Docker图像_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/) |
| docker rmi     | 删除本地镜像                   | [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)中文:[docker 镜像 rm_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/rm/) |
| docker run     | 创建并运行容器（不能重复创建） | [docker run](https://docs.docker.com/engine/reference/commandline/run/)中文:[docker 运行_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/run/) |
| docker stop    | 停止指定容器                   | [docker stop](https://docs.docker.com/engine/reference/commandline/stop/)中文:[docker 容器停止_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/stop/#description) |
| docker start   | 启动指定容器                   | [docker start](https://docs.docker.com/engine/reference/commandline/start/)中文:[docker 容器启动_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/start/) |
| docker restart | 重新启动容器                   | [docker restart](https://docs.docker.com/engine/reference/commandline/restart/)中文:[docker 容器重启_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/restart/) |
| docker rm      | 删除指定容器                   | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/rm/)中文:[docker 容器 rm_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/rm/) |
| docker ps      | 查看容器                       | [docker ps](https://docs.docker.com/engine/reference/commandline/ps/)中文:[docker 撰写 ps_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/compose/ps/) |
| docker logs    | 查看容器运行日志               | [docker logs](https://docs.docker.com/engine/reference/commandline/logs/)中文:[docker 容器日志_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/logs/#description) |
| docker exec    | 进入容器                       | [docker exec](https://docs.docker.com/engine/reference/commandline/exec/)中文:[docker 执行_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/container/exec/) |
| docker save    | 保存镜像到本地压缩文件         | [docker save](https://docs.docker.com/engine/reference/commandline/save/)中文:[docker 镜像保存_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/save/) |
| docker load    | 加载本地压缩文件到镜像         | [docker load](https://docs.docker.com/engine/reference/commandline/load/)中文:[docker 镜像加载_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/load/) |
| docker inspect | 查看容器详细信息               | [docker inspect](https://docs.docker.com/engine/reference/commandline/inspect/)中文:[docker 镜像检查_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/image/inspect/) |

![image-20240713114806707](./图片/image-20240713114806707.png)

### 命令别名

在Linux的root目录下

```PowerShell
# 修改/root/.bashrc文件
vi /root/.bashrc
内容如下：
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias dps='docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"'
alias dis='docker images'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

### 数据卷

**数据卷(volume)**是一个虚拟目录, 是**容器内目录**与**宿主机目录**之间映射的桥梁

![image-20240713165628165](./图片/image-20240713165628165.png)

| **命令**              | **说明**             | **文档地址**                                                 |
| :-------------------- | :------------------- | :----------------------------------------------------------- |
| docker volume create  | 创建数据卷           | [docker volume create](https://docs.docker.com/engine/reference/commandline/volume_create/)中文:[docker 卷创建_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/volume/create/) |
| docker volume ls      | 查看所有数据卷       | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_ls/)中文:[docker 卷 ls_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/volume/ls/) |
| docker volume rm      | 删除指定数据卷       | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_prune/)中文:[docker 卷 rm_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/volume/rm/) |
| docker volume inspect | 查看某个数据卷的详情 | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_inspect/)中文:[docker 卷检查_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/volume/inspect/) |
| docker volume prune   | 清除数据卷           | [docker volume prune](https://docs.docker.com/engine/reference/commandline/volume_prune/)中文:[docker 卷修剪_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/cli/docker/volume/prune/) |

- 在执行`docker run`命令时, 使用**`-v 数据卷:容器内目录`**可以完成数据卷挂载
- 当创建容器时, 如果挂载了数据卷且数据卷不存在, 会自动创建数据卷

- 在执行`docker run`命令时, 使用**`-v 本地目录:容器内目录`**可以完成本地目录挂载
- 本地目录必须以“/”或者“./”开头, 如果直接以名称开头, 会被识别为数据卷而非本地目录
  - `-v mysql:/var/lib/mysql`会被识别为一个数据卷教mysql
  - `-v ./mysql:...`会被识别为当前目录下的mysql目录

MySQL数据目录: /var/lib/mysql

MySQL初始化脚本: /docker-entrypoint-initdb.d

MySQL配置文件: /etc/mysql/conf.d

```powershell
docker run -d \
    --name mysql \
    -p 3306:3306 \
    -e TZ-Asia/Shanghai \
    -e MYSQL_ROOT_PASSWORD=123 \
    -v /root/mysql/data:/var/lib/mysql \
    -v /root/mysql/init:/docker-entrypoint-initdb.d \
    -v /root/mysql/conf:/etc/mysql/conf.d \
    mysql
```

### 自定义镜像

镜像就是包含了应用程序、程序运行的系统函数库、运行配置等文件的文件包. 构建镜像的过程其实就是把上述文件打包的过程

构建一个Java镜像的步骤:

1. 准备一个Linux运行环境
2. 安装JRE并配置环境变量
3. 拷贝jar包
4. 编写运行脚本

部署一个Java应用的步骤:

1. 准备一个Linux服务器
2. 安装JRE并配置环境变量
3. 拷贝jar包
4. 运行jar包

![image-20240713182444122](./图片/image-20240713182444122.png)

#### Dockerfile

**Dockerfile**就是一个文本文件, 其中包含一个个的**指令(Instruction)**, 用指令来说明要执行什么操作来构建镜像. 将来Docker可以根据Dockerfile帮我们构建镜像.常见指令如下:

| **指令**       | **说明**                                     | **示例**                     |
| :------------- | :------------------------------------------- | :--------------------------- |
| **FROM**       | 指定基础镜像                                 | `FROM centos:6`              |
| **ENV**        | 设置环境变量，可在后面指令使用               | `ENV key value`              |
| **COPY**       | 拷贝本地文件到镜像的指定目录                 | `COPY ./xx.jar /tmp/app.jar` |
| **RUN**        | 执行Linux的shell命令，一般是安装过程的命令   | `RUN yum install gcc`        |
| **EXPOSE**     | 指定容器运行时监听的端口，是给镜像使用者看的 | EXPOSE 8080                  |
| **ENTRYPOINT** | 镜像中应用的启动命令，容器运行时调用         | ENTRYPOINT java -jar xx.jar  |

更新详细语法说明, 参考官方文档:[官方]( https://docs.docker.com/engine/reference/builder)

中文:[Dockerfile 参考_Docker中文网 (github.net.cn)](https://docker.github.net.cn/reference/dockerfile/)

我们可以基于Ubuntu基础镜像, 利用Dockerfile描述镜像结构

```dockerfile
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量, JDK的安装目录、容器内时区
ENV JAVA_DIR=/usr/local
# 拷贝JDK和Java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar
# 安装JDK
RUN cd $JAVA_DIR \ && tar -xf ./jdk8.tar.gz \ && mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/bin
ENV PATH=$PATH:$JAVA_HOME/bin
# 入口, Java项目的启动命令
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

也可以直接基于JDK为基础镜像, 省略前面的步骤:

```dockerfile
# 基础镜像
FROM openjdk:11.0-jre-buster
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

当编写好了Dockerfile, 可以利用下面命令来构建镜像:

```dockerfile
docker build -t myImage:1.0 .
```

- **`-t`**: 是给镜像起名, 格式依然是`repository:tag`的格式, 不指定`tag`时,默认为`latest`
- **`.`**: 是指定Dockerfile所在目录, 如果就在当前目录, 则指定为**`.`**

#### 总结

镜像的结构是怎样的?

- 镜像中包含了应用程序所需要的运行环境、函数库、配置、以及应用本身等各种文件, 这些文件分层打包而成.

Dockerfile是做什么的?

- Dockerfile就是利用固定的指令来描述惊喜那个的结构和构建过程, 这样Docker才可以以此来构建镜像

构建镜像的命令是什么?

- `docker build -t 镜像名 Dockerfile目录`

### 网络

默认情况下, 所有容器都是以bridge方式连接到Docker的一个虚拟网桥上:

![image-20240713190734755](./图片/image-20240713190734755.png)

```shell
[root@192 demo]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:4c:67:dd brd ff:ff:ff:ff:ff:ff
    inet 192.168.11.128/24 brd 192.168.11.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::f2f7:7fb5:52a5:bc04/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:65:de:72:e4 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:65ff:fede:72e4/64 scope link
       valid_lft forever preferred_lft forever
25: vethc2e6c8f@if24: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 9e:1e:83:b1:68:2f brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::9c1e:83ff:feb1:682f/64 scope link
       valid_lft forever preferred_lft forever
27: vethf7435ea@if26: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether de:54:58:8b:ea:12 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::dc54:58ff:fe8b:ea12/64 scope link
       valid_lft forever preferred_lft forever
31: veth54dd53e@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether ce:09:28:69:c8:aa brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::cc09:28ff:fe69:c8aa/64 scope link
       valid_lft forever preferred_lft forever

```

加入自定义网络的容器才可以通过容器名互相访问, Docker的网络操作命令如下:

| **命令**                    | **说明**                 | **文档地址**                                                 | 中文地址                                                     |
| :-------------------------- | :----------------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| `docker network create`     | 创建一个网络             | [docker network create](https://docs.docker.com/engine/reference/commandline/network_create/) | [docker 网络创建](https://docker.github.net.cn/reference/cli/docker/network/create/) |
| `docker network ls`         | 查看所有网络             | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_ls/) | [docker 网络 ls](https://docker.github.net.cn/reference/cli/docker/network/ls/) |
| `docker network rm`         | 删除指定网络             | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_rm/) | [docker 网络 rm](https://docker.github.net.cn/reference/cli/docker/network/rm/) |
| `docker network prune`      | 清除未使用的网络         | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_prune/) | [docker 网络修剪](https://docker.github.net.cn/reference/cli/docker/network/prune/) |
| `docker network connect`    | 使指定容器连接加入某网络 | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_connect/) | [docker 网络连接](https://docker.github.net.cn/reference/cli/docker/network/connect/) |
| `docker network disconnect` | 使指定容器连接离开某网络 | [docker network disconnect](https://docs.docker.com/engine/reference/commandline/network_disconnect/) | [docker 网络断开](https://docker.github.net.cn/reference/cli/docker/network/disconnect/) |
| `docker network inspect`    | 查看网络详细信息         | [docker network inspect](https://docs.docker.com/engine/reference/commandline/network_inspect/) | [docker 网络检查](https://docker.github.net.cn/reference/cli/docker/network/inspect/) |

## DockerCompose

Docker Compose通过一个单独的**docker-compose.yml**模板文件(YAML格式)来定义一组相关联的应用容器, 帮助我们实现**多个相互关联的Docker容器的快速部署**

![image-20240714094732185](./图片/image-20240714094732185.png)

> docker run对比docker compose

![image-20240714094945818](./图片/image-20240714094945818.png)

| **docker run 参数** | **docker compose 指令** | **说明**   |
| :------------------ | :---------------------- | :--------- |
| --name              | container_name          | 容器名称   |
| -p                  | ports                   | 端口映射   |
| -e                  | environment             | 环境变量   |
| -v                  | volumes                 | 数据卷配置 |
| --network           | networks                | 网络       |

**docker compose**的命令格式如下:

```clike
docker compose [OPTIONS] [COMMAND]
```

| **类型** | **参数或指令** | **说明**                                                     |
| :------- | :------------- | :----------------------------------------------------------- |
| Options  | -f             | 指定compose文件的路径和名称                                  |
|          | -p             | 指定project名称。project就是当前compose文件中设置的多个service的集合，是逻辑概念 |
|          |                |                                                              |
| Commands | up             | 创建并启动所有service容器                                    |
|          | down           | 停止并移除所有容器、网络                                     |
|          | ps             | 列出所有启动的容器                                           |
|          | logs           | 查看指定容器的日志                                           |
|          | stop           | 停止容器                                                     |
|          | start          | 启动容器                                                     |
|          | restart        | 重启容器                                                     |
|          | top            | 查看运行的进程                                               |
|          | exec           | 在指定的运行中容器中执行命令                                 |

# 微服务

## 认识微服务

### 单体架构

**单体架构**: 将业务的所有功能集中在一个项目中开发, 打成一个jar包部署

**优点**:

- 架构简单
- 部署成本低

**缺点**:

- 团队协作成本高
- 系统发布效率低
- 系统可用性差

总结:

单体架构适合开发功能相对简单, 规模较小的项目.

### 微服务

**微服务**架构, 是服务化思想指导下的一套最佳时间架构方案. 服务化, 就是把单体架构中的功能模块拆分为多个独立项目

- 粒度小
- 团队自治
- 服务自治

### SpringCloud

SpringCloud是目前国内使用最广泛的微服务框架, 官网地址: https://spring.io/projects/spring-cloud.

SpringCloud集成了各种微服务功能组件, 并基于SpringBoot实现了这些组件的自动装配, 从而提供了良好的开箱即用体验

![image-20240715195756317](./图片/image-20240715195756317.png)

## 微服务拆分

![image-20240715201811289](./图片/image-20240715201811289.png)

### 服务拆分原则

#### 什么时候拆分?

- **创业型项目**: 先采用单体架构, 快速开发, 快速试错. 随着规模扩大, 逐渐拆分
- **确定的大型项目**: 资金充足, 目标明确, 可以直接选择微服务架构, 避免后续拆分的麻烦

#### 怎么拆分?

从拆分目标来说, 要做到:

- **高内聚**: 每个微服务的职责要尽量单一, 包含的业务相互关联度高、完整度高
- **低耦合**: 每个微服务的功能要相对独立, 尽量减少对其他微服务的依赖

从拆分方式来说, 一般包含两种方式:

- **纵向拆分**: 按照业务模块来拆分
- **横向拆分**: 抽取公共服务, 提高复用性

### 拆分服务

工程结构有两种:

- 独立Project
  - 用一个空文件夹保存每一个微服务独立的项目文件
- Maven聚合
  - 用一个项目管理每一个微服务独立的模块

需求:

- 将hm-service中与商品管理相关功能拆分到一个微服务module中, 命名为item-service
- 将hm-service中的与购物车有关的功能拆分到一个微服务module中, 命名为cart-service

### 远程调用

Spring给我们提供了一个RestTemplate工具, 可以方便的实现Http请求的发送. 使用步骤如下:

1. 注入RestTemplate到Spring容器

   ```java
   @Bean
   public RestTemplate restTemplate(){
       return new RestTemplate();
   }
   ```

2. 发起远程调用

   ```java
   public <T> ResponseEntity<T> exchange(
   	String url, // 请求路径
       HttpMethod method, // 请求方式
       @Nullable HttpEntity<?> requestEntity, // 请求实体, 可以为空
       Class<T> responseType, // 返回值类型
       Map<String, ?> urlVariables // 请求参数
   )
   ```

## 服务治理

>  服务远程调用时存在的问题

在上一章我们实现了微服务拆分，并且通过Http请求实现了跨微服务的远程调用。不过这种手动发送Http请求的方式存在一些问题。

试想一下，假如商品微服务被调用较多，为了应对更高的并发，我们进行了多实例部署，如图：

![image-20240716095054412](./图片/image-20240716095054412.png)

此时，每个`item-service`的实例其IP或端口不同，问题来了：

- item-service这么多实例，cart-service如何知道每一个实例的地址？
- http请求要写url地址，`cart-service`服务到底该调用哪个实例呢？
- 如果在运行过程中，某一个`item-service`实例宕机，`cart-service`依然在调用该怎么办？
- 如果并发太高，`item-service`临时多部署了N台实例，`cart-service`如何知道新实例的地址？

### 注册中心原理

![image-20240716123228270](./图片/image-20240716123228270.png)

#### 总结

服务治理中的三个角色分别是什么?

- 服务提供者: 暴露服务接口, 供其他服务调用
- 服务消费者: 调用其他服务提供的接口
- 注册中心: 记录并监控微服务各实例状况, 推送服务变更信息

消费者如何知道提供者的地址?

- 服务提供者会在启动时注册自己信息到注册中心, 消费者可以从注册中心订阅和拉取服务信息

消费者如何得知服务状态变更?

- 服务提供者通过心跳机制向注册中心报告自己的健康状态, 当心跳异常时注册中心会将异常服务剔除, 并通知订阅了该服务的消费者

当提供者有多个实例时, 消费者该选择哪一个?

- 消费者可以通过负载均衡算法, 从多个实例中选择一个

### Nacos注册中心

目前开源的注册中心框架有很多，国内比较常见的有：

- Eureka：Netflix公司出品，目前被集成在SpringCloud当中，一般用于Java应用
- Nacos：Alibaba公司出品，目前被集成在SpringCloudAlibaba中，一般用于Java应用
- Consul：HashiCorp公司出品，目前集成在SpringCloud中，不限制微服务语言

以上几种注册中心都遵循SpringCloud中的API规范，因此在业务开发使用上没有太大差异。由于Nacos是国内产品，中文文档比较丰富，而且同时具备**配置管理**功能（后面会学习），因此在国内使用较多，课堂中我们会Nacos为例来学习。

Nacos是目前国内企业中占比最多的注册中心组件. 它是阿里巴巴的产品, 目前已经加入SpringCloudAlibaba中.

![image-20240716124229266](./图片/image-20240716124229266.png)

我们基于Docker来部署Nacos的注册中心，首先我们要准备MySQL数据库表，用来存储Nacos的数据。由于是Docker部署，所以大家需要将资料中的SQL文件导入到你**Docker中的MySQL容器**中：

![565ad907-42ea-4455-b02f-e5daf00c976d](./图片/565ad907-42ea-4455-b02f-e5daf00c976d.png)

最终表结构如下：

![2eab08c0-6061-4201-9b38-c7bec43a64cd](./图片/2eab08c0-6061-4201-9b38-c7bec43a64cd.png)

然后，找到课前资料下的nacos文件夹：

![3bd82150-5f7c-4a88-8116-95d5cb9909e0](./图片/3bd82150-5f7c-4a88-8116-95d5cb9909e0.png)

其中的`nacos/custom.env`文件中，有一个MYSQL_SERVICE_HOST也就是mysql地址，需要修改为你自己的虚拟机IP地址：

![da664c6f-03d3-4600-a830-4ac686d28a4c](./图片/da664c6f-03d3-4600-a830-4ac686d28a4c.png)

然后，将课前资料中的`nacos`目录上传至虚拟机的`/root`目录。

进入root目录，然后执行下面的docker命令：

```java
docker run -d \
    --name nacos \
    --env-file ./nacos/custom.env \
    -p 8848:8848 \
    -p 9848:9848 \
    -p 9849:9849 \
    --restart=always \
    --network=qqzj-news \
    nacos/nacos-server:v2.4.0
```

启动完成后，访问下面地址：http://192.168.11.128:8848/nacos/，注意将`192.168.150.101`替换为你自己的虚拟机IP地址。

首次访问会跳转到登录页，**账号密码都是nacos**

![8d666868-f442-4682-8da3-820d157d6794](./图片/8d666868-f442-4682-8da3-820d157d6794.png)

### 服务注册

服务注册步骤如下:

1. 引入nacos discovery依赖:

   ```xml
   <!--nacos 服务注册发现-->
   <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
   </dependency>
   ```

2. 配置Nacos地址

   ```yml
   spring:
   	application:
   		name: item-service # 服务名称
   	cloud: 
   		nacos:
   			server-addr: 192.168.11.128:8848 # nacos地址
   ```

### 服务发现

**消费者**需要连接Nacos以拉取和订阅服务, 因此服务发现的前两步与服务注册是一样的, 后面再加上服务调用即可:

1. 引入nacos discovery依赖

2. 配置nacos地址

3. 服务发现

   ```java
   private final   discoveryClient;
   
   private void handleCartItems(List<CartVO> vos) {
           // 1.获取商品id
           Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
           // 2.查询商品
           // 2.1 根据服务名称获取服务的实例列表
           List<ServiceInstance> instances = discoveryClient.getInstances("item-service");
           if (CollUtil.isEmpty(instances)){
               return;
           }
           // 2.2 手写负载均衡, 从实例列表中选择一个实例
           ServiceInstance instance = instances.get(RandomUtil.randomInt(instances.size()));
           // 2.3 利用RestTemplate发起Http请求, 得到Http的响应
           ResponseEntity<List<ItemDTO>> response = restTemplate.exchange(
                   instance.getUri() + "/items?ids={ids}",
                   HttpMethod.GET,
                   null,
                   new ParameterizedTypeReference<List<ItemDTO>>() {
                   },
                   Map.of("ids", CollUtils.join(itemIds, ","))
           );
           // 2.2 解析响应
           if (!response.getStatusCode().is2xxSuccessful()){
               //响应失败, 直接结束
               return;
           }
           List<ItemDTO> items = response.getBody();
           if (CollUtils.isEmpty(items)) {
               return;
           }
           // 3.转为 id 到 item的map
           Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
           // 4.写入vo
           for (CartVO v : vos) {
               ItemDTO item = itemMap.get(v.getItemId());
               if (item == null) {
                   continue;
               }
               v.setNewPrice(item.getPrice());
               v.setStatus(item.getStatus());
               v.setStock(item.getStock());
           }
       }
   ```

## OpenFeign

### 快速入门

OpenFeign是一个声明式的http客户端, 是SpringCloud在Eureka公司开源的Feign基础上改造而来. 官方地址: https://github.com/OpenFeign/feign

其作用就是基于SpringMVC的常见注解, 帮我们优雅的实现http请求的发送

![image-20240716163847602](./图片/image-20240716163847602.png)

![image-20240716164012847](./图片/image-20240716164012847.png)

OpenFeign已经被SpringCloud自动装配, 实现起来非常简单:

1. 引入依赖, 包括OpenFeign和负载均衡组件SpringCloudLoadBalancer

   ```xml
     <!--openFeign-->
     <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-openfeign</artifactId>
     </dependency>
     <!--负载均衡器-->
     <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-loadbalancer</artifactId>
     </dependency>
   ```

2. 通过`@EnableFeignClients`注解, 启用OpenFeign功能

   ```java
   @EnableFeignClients
   @SpringBootApplication
   public class ....
   ```

3. 编写FeignClient

   ```java
   package com.hmall.cart.client;
   
   import ...;
   
   @FeignClient("item-service")
   public interface ItemClient {
       @GetMapping("/items")
       List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);
   }
   ```

4. 使用FeignClient, 实现远程调用

   ```java
   List<ItemDTO> items = itemClient.queryItemByIds(List.of(1,2,3));
   ```

### 连接池

OpenFeign对Http请求做了优雅的伪装, 不过其底层发起Http请求, 依赖于其他的框架. 这些框架可以自己选择, 包括以下三种:

- **HttpURLConnection**: 默认实现, 不支持连接池
- **Apache HttpClient**: 支持连接池
- **OKHttp**: 支持连接池

具体源码可以参考**FeignBlockingLoadBalancerClient**类中的**delegate**成员变量

OpenFeign整合OKHttp的步骤如下:

1. 引入依赖

   ```xml
   <!--OK http 的依赖 -->
   <dependency>
     <groupId>io.github.openfeign</groupId>
     <artifactId>feign-okhttp</artifactId>
   </dependency>
   ```

2. 开启连接池功能

   ```yml
   feign:
     okhttp:
       enabled: true # 开启OKHttp功能
   ```

### 最佳实践

#### 实践一: 麻烦, 模块多

![image-20240716172045547](./图片/image-20240716172045547.png)

#### 实践二: 耦合度高

![image-20240716172318090](./图片/image-20240716172318090.png)

当定义的FeignClient不在SpringBootApplication的扫描包范围时, 这些FeignClient无法使用. 有两种方式解决:

方式一: 指定FeignClient所在包

```java
@EnableFeignClients(basePackages = "com.hmall.api.clients")
```

方式二: 指定FeignClient字节码

```java
@EnableFeignClients(clients = {UserClient.class})
```

### 日志

OpenFeign只会在FeignClient所在包的日志级别为**DEBUG**时, 才会输出日志. 而且其日志级别有4级:

- **NONE**: 不记录任何日志信息, 这是默认值
- **BASIC**: 仅记录请求的方法, URL以及响应状态码和执行时间
- **HEADERS**: 在BASIC的基础上, 额外记录了请求和响应的头信息
- **FULL**: 记录所有请求和响应的明细, 包括头信息、请求体、元数据.

由于Feign默认的日志级别就是NONE, 所以默认我们看不到请求日志

要自定义日志级别需要声明一个类型为Logger.Level的Bean, 在其中定义日志级别:

```java
public class DefaultFeignConfig{
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.FULL;
    }
}
```

但此时这个Bean并未生效, 要想配置某个FeignClient的日志, 可以在`@FeignClient`注解中声明:

```java
@FeignClient(value = "item-service", configuration = DefaultFeignConfig.class)
```

如果想要**全局配置**, 让所有FeignClient都按照这个日志配置, 则需要在`@EnableFeignClients`注解中声明:

```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfig.class)
```

### 总结

如何利用OpenFeign实现远程调用?

1. 引入依赖, 包括OpenFeign和负载均衡组件SpringCloudLoadBalancer

   ```xml
     <!--openFeign-->
     <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-openfeign</artifactId>
     </dependency>
     <!--负载均衡器-->
     <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-loadbalancer</artifactId>
     </dependency>
   ```

2. 通过`@EnableFeignClients`注解, 启用OpenFeign功能

   ```java
   @EnableFeignClients
   @SpringBootApplication
   public class ....
   ```

3. 编写FeignClient

   ```java
   package com.hmall.cart.client;
   
   import ...;
   
   @FeignClient("item-service")
   public interface ItemClient {
       @GetMapping("/items")
       List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);
   }
   ```

如何配置OpenFeign的连接池?

1. 引入Http客户端依赖, 例如OKHttp、HttpClient

   ```xml
   <!--OK http 的依赖 -->
   <dependency>
     <groupId>io.github.openfeign</groupId>
     <artifactId>feign-okhttp</artifactId>
   </dependency>
   ```

2. 配置yaml文件, 打开OpenFeign连接池开关

   ```yml
   feign:
     okhttp:
       enabled: true # 开启OKHttp功能
   ```

OpenFeign使用的最佳实践方式是什么?

![image-20240716172045547](./图片/image-20240716172045547.png)

- 由服务提供者编写独立module, 将FeignClient及DTO抽取

![image-20240716172318090](./图片/image-20240716172318090.png)

如何配置OpenFeign输出日志的级别?

- 声明类型为Logger.Level的Bean

```java
public class DefaultFeignConfig{
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.FULL;
    }
}
```

- 在`@FeignClient`或`@EnableFeignClients`注解上使用

```java
@FeignClient(value = "item-service", configuration = DefaultFeignConfig.class)
```

```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfig.class)
```

# 微服务——网关及配置管理

## 网关

**网关**: 就是网络的关口, 负责请求的路由、转发、身份校验

![image-20240718085006775](./图片/image-20240718085006775.png)

在SpringCloud中网关的实现包括两种: 

![image-20240718085051190](./图片/image-20240718085051190.png)

## 网关路由

### 入门

1. 创建新模块

2. 引入网关依赖

   ```xml
   <!--网关-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-gateway</artifactId>
   </dependency>
   <!--nacos discovery-->
   <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
   </dependency>
   <!--负载均衡-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-loadbalancer</artifactId>
   </dependency>
   ```

3. 编写启动类

4. **配置路由规则**

   ```yml
   spring:
   	cloud: 
   		gateway:
   			routes:
   				- id: item # 路由规则ID, 自定义, 唯一
   				  uri: lb://item-service #路由目标微服务, lb代表负载均衡
   				  predicates: # 路由断言, 判断请求是否符合规则, 符合则路由到目标
   				  	- Path=/items/** # 以请求路径做判断, 以/items开头则符合
   				 - id: xx
   				   uri: lb://xx-service
   				   predicates:
   				   	- Path=/xx/**
   ```

### 路由属性

**网关路由**对应的Java类型是RouteDefinition, 其中常见的属性有:

- id: 路由唯一标识
- uri: 路由目标地址
- <font color='red'>predicates</font>: 路由断言, 判断请求是否符合当前路由
- <font color='red'>filters</font>: 路由过滤器, 对请求或响应做特殊处理

#### 路由断言

| **名称**               | **说明**                       | **示例**                                                     |
| :--------------------- | :----------------------------- | :----------------------------------------------------------- |
| After                  | 是某个时间点后的请求           | - After=2037-01-20T17:42:47.789-07:00[America/Denver]        |
| Before                 | 是某个时间点之前的请求         | - Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]        |
| Between                | 是某两个时间点之前的请求       | - Between=2037-01-20T17:42:47.789-07:00[America/Denver], 2037-01-21T17:42:47.789-07:00[America/Denver] |
| Cookie                 | 请求必须包含某些cookie         | - Cookie=chocolate, ch.p                                     |
| Header                 | 请求必须包含某些header         | - Header=X-Request-Id, \d+                                   |
| Host                   | 请求必须是访问某个host（域名） | - Host=**.somehost.org,**.anotherhost.org                    |
| Method                 | 请求方式必须是指定方式         | - Method=GET,POST                                            |
| Path                   | 请求路径必须符合指定规则       | - Path=/red/{segment},/blue/**                               |
| Query                  | 请求参数必须包含指定参数       | - Query=name, Jack或者- Query=name                           |
| RemoteAddr             | 请求者的ip必须是指定范围       | - RemoteAddr=192.168.1.1/24                                  |
| weight                 | 权重处理                       | - Weight=group1,2                                            |
| XForwarded Remote Addr | 基于请求的来源IP做判断         | - XForwardedRemoteAddr=192.168.1.1/24                        |

官网: [Spring Cloud 网关](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/)

### 路由过滤器

网关中提供了33中路由过滤器, 每种过滤器都有独特的作用.

|         名称         |            说明            |                       示例                        |
| :------------------: | :------------------------: | :-----------------------------------------------: |
|   AddRequestHeader   |  给当前请求添加一个请求头  |      AddrequestHeader=headerName,headerValue      |
| RemoveRequestHeader  |   移除请求中的一个请求头   |          RemoveRequestHeadler=headerName          |
|  AddResponseHeader   | 给响应结果中添加一个响应头 |     AddResponseHeader=headerName,headerValue      |
| RemoveResponseHeader | 从响应结果中移除一个响应头 |          RemoveResponseHeader=headerName          |
|     RewritePath      |        请求路径重写        | RewritePath=/red/?(?\<segment>.\*), /$\\{segment} |
|     StripPrefix      |  去除请求路径中的N段前缀   |      StripPrefix=1,则路径/a/b转发时只保留/b       |
|          ……          |             …              |                        ………                        |

官网: [Spring Cloud 网关](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gatewayfilter-factories)

## 网关登录校验

### 网关请求处理流程

![image-20240719084424516](./图片/image-20240719084424516.png)

### 自定义过滤器

网关过滤器有两种, 分别是:

- GatewayFilter: 路由过滤器, 作用于任意指定的路由; 默认不生效, 要配置到路由后生效.
- GlobalFilter: 全局过滤器, 作用范围是所有路由; 声明后自动生效

两种过滤器的**过滤方法**签名完全一致:

![image-20240719085022965](./图片/image-20240719085022965.png)

![image-20240719085049047](./图片/image-20240719085049047.png)

![image-20240719085201630](./图片/image-20240719085201630.png)

自定义GlobalFilter比较简单, 直接实现GlobalFilter接口即可:

```java
@Slf4j
@Component
public class MyGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // TODO 模拟登录校验逻辑
        // 1.获取请求头
        HttpHeaders headers = exchange.getRequest().getHeaders();
        log.info("请求头: {}",headers.toString());
        // 2.放行
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        // 过滤器执行顺序, 值越小, 优先级越高
        return 0;
    }
}
```

自定义GatewayFilter不是直接实现GatewayFilter, 而是实现AbstractGatewayFilterFactory, 示例如下:

```java
@Slf4j
@Component
public class PrintAnyGatewayFilterFactory extends AbstractGatewayFilterFactory<Object> {
    @Override
    public GatewayFilter apply(Object config) {
        return new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                // 1.编写过滤器逻辑
                log.info("PrintAnyGatewayFilterFactory执行了~");
                // 2.放行
                return chain.filter(exchange);
            }
        };
    }
}
```

其中: 类名中的后缀必须是GatewayFilterFactory, 方便配置使用,前面的PrintAny为过滤器名字, 写在yml配置文件中

> yml文件配置

```yml
spring:
	cloud:
		gateway:
			default-filters:
				- AddRequestHeader=a,b
				# 过滤器名字=过滤器参数
```

如果需要配置过滤器执行顺序(ordered), 可以使用其装饰类OrderedGatewayFilter:

```java
@Slf4j
@Component
public class PrintAnyGatewayFilterFactory extends AbstractGatewayFilterFactory<Object> {
    @Override
    public GatewayFilter apply(Object config) {
        return new OrderedGatewayFilter(new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                // 1.编写过滤器逻辑
                log.info("PrintAnyGatewayFilterFactory执行了~");
                // 2.放行
                return chain.filter(exchange);
            }
        }, 1);
    }
}
```

> yml文件

```yml
gateway:
	routes: <5 items>
	default-filters:
		- PrintAny
```

> 执行结果

```java
2024-07-19 09:24:04.293  INFO 20124 --- [ctor-http-nio-5] c.hmall.gateway.filters.MyGlobalFilter   : 请求头: [Host:"localhost:8080", Connection:"close", sec-ch-ua:""Not/A)Brand";v="8", "Chromium";v="126", "Google Chrome";v="126"", Accept:"application/json, text/plain, */*", sec-ch-ua-mobile:"?0", authorization:"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VyIjoxLCJleHAiOjE3MjEzNTQwNDR9.Dw_Qlrv4dVKx1RRwUwcXXtohPhGD9RhhrrOtzNd-5yR0_tb6PoYJ7--PI-Qvc2WPOEfC9EyTbQ7Ry6gv6bVNPdwPYXBo7wzEF93mXbbFFsZ9byKEVGwT7IU15v3Cwgapr1N2VS7MurENEu8iZzTbSGLz3D89hjdDuXh1ElzH5fTuRKYrg55xOWC8pLItX0a-tO4qVibyWPuu1xsXIuhr4Yhj5HOa2EurPNvxvcpjhokwNGII8W5ttmzUc_PGKFp0_H0KgB73UI4Pz0hEHafY76DX5jEqHcZxEi-VcxxAPywTQt6Ct3YIhWll_o8JKaJPxIzA55z1igw3eEvk_CF_-A", User-Agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36", sec-ch-ua-platform:""Windows"", Sec-Fetch-Site:"same-origin", Sec-Fetch-Mode:"cors", Sec-Fetch-Dest:"empty", Referer:"http://localhost:18080/", Accept-Encoding:"gzip, deflate, br, zstd", Accept-Language:"zh-CN,zh;q=0.9", Cookie:"Idea-dbd1d880=64d396e8-bf0a-4c66-a533-867c08e2bfad; Webstorm-e9471347=b924acbd-b705-4789-ac40-b70228409759"]
2024-07-19 09:24:04.293  INFO 20124 --- [ctor-http-nio-5] c.h.g.f.PrintAnyGatewayFilterFactory     : PrintAnyGatewayFilterFactory执行了~
```

定义带参数的GatewayFilter:

> yml文件

```yml
gateway:
	routes: <5 items>
	default-filters:
		- PrintAny=1,2,3
```

> PrintAnyGatewayFilterFactory类

```java
@Slf4j
@Component
public class PrintAnyGatewayFilterFactory extends AbstractGatewayFilterFactory<PrintAnyGatewayFilterFactory.Config> {
    @Override
    public GatewayFilter apply(Config config) {
        return new OrderedGatewayFilter(new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                log.info("a: {}; b: {}; c: {}", config.getA(), config.getB(), config.getC());
                // 1.编写过滤器逻辑
                log.info("PrintAnyGatewayFilterFactory执行了~");
                // 2.放行
                return chain.filter(exchange);
            }
        }, 1);
    }

    // 自定义配置属性, 成员变量名称很重要, 下面会用到
    @Data
    public static class Config {
        private String a;
        private String b;
        private String c;
    }

    // 将变量名称依次返回, 顺序很重要, 将来读取参数时需要按顺序获取
    @Override
    public List<String> shortcutFieldOrder() {
        return List.of("a", "b", "c");
    }

    // 将Config字节码传递给父类, 父类负责帮我们读取yml配置
    public PrintAnyGatewayFilterFactory() {
        super(Config.class);
    }
}
```

> 日志输出

```java
2024-07-19 09:38:16.949  INFO 17544 --- [ctor-http-nio-2] c.h.g.f.PrintAnyGatewayFilterFactory     : a: 1; b: 2; c: 3
2024-07-19 09:38:16.949  INFO 17544 --- [ctor-http-nio-2] c.h.g.f.PrintAnyGatewayFilterFactory     : PrintAnyGatewayFilterFactory执行了~
```

### 实现登录校验

> com.hmall.gateway.config.AuthProperties

```java
@Data
@Component
@ConfigurationProperties(prefix = "hm.auth")
public class AuthProperties {
    private List<String> includePaths;
    private List<String> excludePaths;
}
```

> com.hmall.gateway.config.JwtProperties

```java
@Data
@ConfigurationProperties(prefix = "hm.jwt")
public class JwtProperties {
    private Resource location;
    private String password;
    private String alias;
    private Duration tokenTTL = Duration.ofMinutes(10);
}
```

> com.hmall.gateway.config.SecurityConfig

```java
@Configuration
@EnableConfigurationProperties(JwtProperties.class)
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Bean
    public KeyPair keyPair(JwtProperties properties){
        // 获取秘钥工厂
        KeyStoreKeyFactory keyStoreKeyFactory =
                new KeyStoreKeyFactory(
                        properties.getLocation(),
                        properties.getPassword().toCharArray());
        //读取钥匙对
        return keyStoreKeyFactory.getKeyPair(
                properties.getAlias(),
                properties.getPassword().toCharArray());
    }
}
```

> com.hmall.gateway.utils.JwtTool

```java
@Component
public class JwtTool {
    private final JWTSigner jwtSigner;

    public JwtTool(KeyPair keyPair) {
        this.jwtSigner = JWTSignerUtil.createSigner("rs256", keyPair);
    }

    /**
     * 创建 access-token
     *
     * @param userId 用户信息
     * @return access-token
     */
    public String createToken(Long userId, Duration ttl) {
        // 1.生成jws
        return JWT.create()
                .setPayload("user", userId)
                .setExpiresAt(new Date(System.currentTimeMillis() + ttl.toMillis()))
                .setSigner(jwtSigner)
                .sign();
    }

    /**
     * 解析token
     *
     * @param token token
     * @return 解析刷新token得到的用户信息
     */
    public Long parseToken(String token) {
        // 1.校验token是否为空
        if (token == null) {
            throw new UnauthorizedException("未登录");
        }
        // 2.校验并解析jwt
        JWT jwt;
        try {
            jwt = JWT.of(token).setSigner(jwtSigner);
        } catch (Exception e) {
            throw new UnauthorizedException("无效的token", e);
        }
        // 2.校验jwt是否有效
        if (!jwt.verify()) {
            // 验证失败
            throw new UnauthorizedException("无效的token");
        }
        // 3.校验是否过期
        try {
            JWTValidator.of(jwt).validateDate();
        } catch (ValidateException e) {
            throw new UnauthorizedException("token已经过期");
        }
        // 4.数据格式校验
        Object userPayload = jwt.getPayload("user");
        if (userPayload == null) {
            // 数据为空
            throw new UnauthorizedException("无效的token");
        }

        // 5.数据解析
        try {
           return Long.valueOf(userPayload.toString());
        } catch (RuntimeException e) {
            // 数据格式有误
            throw new UnauthorizedException("无效的token");
        }
    }
}
```

![image-20240719103017724](./图片/image-20240719103017724.png)

> application.yaml

```yml
server:
  port: 8080
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: 192.168.11.128:8848
    gateway:
      routes:
        - id: item-service
          uri: lb://item-service
          predicates:
            - Path=/items/**,/search/**

        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/users/**

        - id: trade-service
          uri: lb://trade-service
          predicates:
            - Path=/orders/**

        - id: pay-service
          uri: lb://pay-service
          predicates:
            - Path=/pay-orders/**

        - id: cart-service
          uri: lb://cart-service
          predicates:
            - Path=/carts/**
#      default-filters:
#        - PrintAny=1,2,3
hm:
  jwt:
    location: classpath:hmall.jks
    alias: hmall
    password: hmall123
    tokenTTL: 30m
  auth:
    excludePaths:
      - /search/**
      - /users/login
      - /items/**
      - /hi
```

> com.hmall.gateway.filters.AuthGlobalFilter

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class AuthGlobalFilter implements GlobalFilter, Ordered {

    private final AuthProperties authProperties;
    private final JwtTool jwtTool;
    private final AntPathMatcher antPathMatcher = new AntPathMatcher();

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取request
        ServerHttpRequest request = exchange.getRequest();
        // 2.判断是否需要做登录拦截
        if (isExclude(request.getPath().toString())) {
            // 放行
            return chain.filter(exchange);
        }
        // 3.获取token
        String token = null;
        List<String> authorization = request.getHeaders().get("authorization");
        if (authorization != null && !authorization.isEmpty()) {
            token = authorization.get(0);
            log.info("获取到token: {}", token);
        }
        // 4.校验并解析token
        Long userId = null;
        try {
            userId = jwtTool.parseToken(token);
        } catch (UnauthorizedException e) {
            // 拦截, 设置响应状态码为401
            log.info("未登录");
            ServerHttpResponse response = exchange.getResponse();
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            return response.setComplete();
        }
        // TODO:5.传递用户信息
        log.info("userId = {}", userId);
        // 6.放行
        return chain.filter(exchange);
    }

    private boolean isExclude(String path) {
        for (String excludePath : authProperties.getExcludePaths()) {
            if (antPathMatcher.match(excludePath, path)) {
                log.info("直接放行路径: {}", path);
                return true;
            }
        }
        log.info("{}路径不存在", path);
        return false;
    }

    @Override
    public int getOrder() {
        return Ordered.LOWEST_PRECEDENCE - 1;
    }
}
```

### 网关传递用户

步骤一: 在网关的登录校验过滤器中, 把获取到的用户写入请求头

要修改转发到微服务的请求, 需要用到**ServerWebExchange**类提供的API

```java
exchange.mutate()//mutate就是对下游请求做更改
    .request(builder -> builder.header("user-info", userInfo))
    .build();
```

```java
// 5.传递用户信息
String userInfo = userId.toString();
exchange.mutate()//mutate就是对下游请求做更改
    .request(builder -> builder.header("user-info", userInfo))
    .build();
log.info("userId = {}", userId);
```

步骤二: 在hm-common中编写SpringMVC拦截器, 获取登录用户

由于每个微服务都可能有获取登录用户的需求, 因此我们直接在hm-common模块定义拦截器, 这样微服务只需要引入依赖即可生效, 无需重复编写

> com.hmall.common.interceptor.UserInfoInterceptor 拦截器类

```java
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 1.获取用户登录信息
        String userInfo = request.getHeader("user-info");
        // 2.判断是否获取了用户
        if (StrUtil.isNotBlank(userInfo)){
            //如果有, 存入ThreadLocal
            UserContext.setUser(Long.valueOf(userInfo));
        }
        // 3.放行
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        UserContext.removeUser();
    }
}
```

> com.hmall.common.config.MVCConfig 配置类

```java
@Configuration
@ConditionalOnClass(DispatcherServlet.class)
//网关也引用了这个依赖, 但是网关没有SpringMVC相关组件, 所以需要排除掉, 不要让网关使用这个配置类
//所有的微服务都使用了SpringMVC, 只有网关没有使用, SpringMVC的核心类就是DispatcherServlet, 所以代表着只要有这个类就说明有SpringMVC, 就代表是微服务
public class MVCConfig implements WebMvcConfigurer{
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserInfoInterceptor()).addPathPatterns("/**");
    }
}
```

> META-INF/spring.factories 

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.hmall.common.config.MyBatisConfig, \
  com.hmall.common.config.JsonConfig, \
  com.hmall.common.config.MVCConfig
```

### OpenFeign传递用户

微服务项目中的很多业务要多个微服务共同合作完成, 而这个过程中也需要传递登录用户信息, 例如:

![image-20240719121743771](./图片/image-20240719121743771.png)

OpenFeign中提供了一个拦截器接口, 所有由OpenFeign发起的请求都会先调用拦截器处理请求:

```java
public interface RequestInterceptor {

  /**
   * Called for every request. Add data using methods on the supplied {@link RequestTemplate}.
   */
  void apply(RequestTemplate template);
}
```

其中的RequestTemplate类中提供了一些方法可以让我们修改请求头:

![image-20240719125800175](./图片/image-20240719125800175.png)

> com.hmall.api.config.DefaultFeignConfig

```java
public class DefaultFeignConfig {
    /*@Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.FULL;
    }*/

    @Bean
    public RequestInterceptor userInfoRequestInterceptor(){
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate template) {
                Long userId = UserContext.getUser();
                if (userId != null) {
                    template.header("user-info", userId.toString());
                }
            }
        };
    }
}
```

> 启动类

```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfig.class)
```

### 微服务登录解决方案

![image-20240719133023606](./图片/image-20240719133023606.png)

## 配置管理

### 配置管理服务

- 微服务重复配置过多, 维护成本高
- 业务配置经常变动, 每次修改都要重启服务
- 网关路由配置写死, 如果变更需要重启网关

![image-20240719133824095](./图片/image-20240719133824095.png)

### 配置共享

步骤一: 添加配置到Nacos

添加一些共享配置到Nacos中, 包括: jdbc、MP、日志、Swagger、OpenFeign等配置

步骤二: 拉取共享配置

基于NacosConfig拉取共享配置代替微服务的本地配置

![image-20240719135500547](./图片/image-20240719135500547.png)

1. 引入依赖

   ```xml
   <!--nacos配置管理-->
   <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
   </dependency>
   <!--读取bootstrap文件-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-bootstrap</artifactId>
   </dependency>
   ```

2. 新建bootstrap.yaml

   ```yaml
   spring:
     application:
       name: cart-service # 服务名称
     profiles:
       active: dev
     cloud:
       nacos:
         server-addr: 192.168.11.128:8848 # nacos地址
         config:
           file-extension: yaml # 文件后缀名
           shared-configs: # 共享配置
             - data-id: shared-jdbc.yml # 共享Mybatis配置
             - data-id: shared-log.yml # 共享日志配置
             - data-id: shared-swagger.yml # 共享swagger配置
   ```

### 配置热更新

#### 配置热更新

**配置热更新**: 当修改配置文件中的配置时, 微服务<font color='red'>**无需重启**</font>即可使配置生效

**前提条件**:

1. nacos中要有一个与微服务名有关的配置文件.

   ```tex
   [spring.application.name-[spring.active.profile].[file-extension]]
   ```

   ![image-20240720083124129](./图片/image-20240720083124129.png)

2. 微服务中要以特定方式读取需要热更新的配置属性

   ```java
   @Data
   @ConfigurationProperties(prefix = "hm.cart")
   public class CartProperties {
       private int maxItems;
   }
   ```

   或者:

   ```java
   @Data
   @RefreshScope
   public class CartProperties {
       @Value("${hm.cart.maxItems}")
       private int maxItems;
   }
   ```

案例: 实现购物车添加商品上限的配置热更新

需求: 购物车的限定数量目前是写死在业务中的, 将其改为读取配置文件属性, 并将配置交给Nacos管理, 实现热更新

```java
private void checkCartsFull(Long userId) {
    Long count = lambdaQuery().eq(Cart::getUserId, userId).count();
    if (count >= 10) {
        throw new BizIllegalException(StrUtil.format("用户购物车课程不能超过{}", 10));
    }
}
```

改造:

> nacos

![image-20240720084916055](./图片/image-20240720084916055.png)

> com.hmall.cart.config.CartProperties

```java
@Data
@Component
@ConfigurationProperties(prefix = "hm.cart")
public class CartProperties {
    private int maxItems;
}
```

### 动态路由

#### 动态路由

要实现**动态路由**首先要将路由配置保存在Nacos, 当Nacos中的路由配置变更时, 推送最新配置到网关, 实时更新网关中的路由信息.

我们需要完成两件事情:

1. 监听Nacos配置变更的消息
2. 当配置变更时, 将最新的路由信息更新到网关路由表

#### 监听Nacos配置

监听Nacos配置变更可以参考官方文档: https://nacos.io/zh-cn/docs/sdk.html

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class DyhnamicRouteLoader {

    private final NacosConfigManager nacosConfigManager;

    private final static String DATA_ID = "gateway-routes.json";
    private final static String GROUP = "DEFAULT_GROUP";

    @PostConstruct//当类(bean)被初始化之后执行该方法
    public void initRouteConfigListener() throws NacosException {
        // 1.项目启动先拉取一次配置, 并添加配置监听器
        String configInfo = nacosConfigManager.getConfigService()
                .getConfigAndSignListener(DATA_ID, GROUP, 5000, new Listener() {
                    @Override
                    public Executor getExecutor() {
                        return null;
                    }

                    @Override
                    public void receiveConfigInfo(String configInfo) {
                        // 2.监听到配置变更, 需要去更新路由表
                        updateConfigInfo(configInfo);
                    }
                });
        // 3.第一次读取到配置, 也需要更新到路由表
        updateConfigInfo(configInfo);
    }

    public void updateConfigInfo(String configInfo){
        // TODO 路由表更新
    }
}
```

#### 更新路由表

监听到路由信息后, 可以利用**RouteDefinitionWriter**来更新路由表

```java
/**
 * @author Spencer Gibb
 */
public interface RouteDefinitionWriter {
    /**
 	 * 更新路由到路由表, 如果路由id重复, 则会覆盖旧的路由
	 */
    Mono<Void> save(Mono<RouteDefinition> route);
    /**
 	 * @根据路由id删除某个路由
	 */
    Mono<Void> delete(Mono<String> routeId);
}
```

#### 路由配置语法

为了方便解析从Nacos读取到的路由配置, 推荐使用JSON格式的路由配置, 模板如下:

```json
{
    "id": "item",
    "predicates": [{
        "name": "Path",
        "args": {"_genkey_0":"/items/**", "_genkey_1":"/search/**"}
    }],
    "filters": [],
    "uri": "lb://item-service"
}
```

以上JSON配置就等同于：

```yaml
spring:
	cloud:
		gateway:
			routes:
				- id: item
				  uri: lb://item-service
				  predicates:
					  - Path=/items/**,/search/**
```

> 最终配置

![image-20240720111545426](./图片/image-20240720111545426.png)

```json
[
    {
        "id": "item",
        "predicates": [{
            "name": "Path",
            "args": {"_genkey_0":"/items/**", "_genkey_1":"/search/**"}
        }],
        "filters": [],
        "uri": "lb://item-service"
    },
    {
        "id": "cart",
        "predicates": [{
            "name": "Path",
            "args": {"_genkey_0":"/carts/**"}
        }],
        "filters": [],
        "uri": "lb://cart-service"
    },
    {
        "id": "user",
        "predicates": [{
            "name": "Path",
            "args": {"_genkey_0":"/users/**", "_genkey_1":"/addresses/**"}
        }],
        "filters": [],
        "uri": "lb://user-service"
    },
    {
        "id": "trade",
        "predicates": [{
            "name": "Path",
            "args": {"_genkey_0":"/orders/**"}
        }],
        "filters": [],
        "uri": "lb://trade-service"
    },
    {
        "id": "pay",
        "predicates": [{
            "name": "Path",
            "args": {"_genkey_0":"/pay-orders/**"}
        }],
        "filters": [],
        "uri": "lb://pay-service"
    }
]
```

# 服务保护和分布式事务

## 微服务

- 远程调用 -> OpenFeign
- 服务治理 -> Nacos
- 请求路由 & 身份认证 -> Gateway
- 配置管理 -> Nacos
- 服务保护
- 分布式事务

## 雪崩问题

### 雪崩问题

微服务调用链路中的某个服务故障, 引起整个链路中的所有微服务都不可用, 这就是雪崩

![image-20240720145429100](./图片/image-20240720145429100.png)

#### 总结

雪崩问题产生的原因是什么?

- **微服务相互调用, 服务提供者出现故障或阻塞**
- **服务调用者没有做好异常处理, 导致自身故障**
- **调用链中的所有服务级联失败, 导致整个集群故障**

解决问题的思路有哪些?

- **尽量避免服务出现故障或阻塞**
  - **保证代码的健壮性;**
  - **保证网络畅通;**
  - **能应对较高的并发请求;**
- **服务调用者做好远程调用异常的后备方案, 避免故障扩散**

### 解决方案

#### 服务保护方案 - 请求限流

请求限流: 限制访问微服务的请求的并发量, 避免服务因流量激增出现故障

![image-20240720151649891](./图片/image-20240720151649891.png)

#### 服务保护方案 - 线程隔离

线程隔离: 也叫做舱壁模式, 模拟船舱隔板的防水原理. 通过限定每个业务能使用的线程数量而将故障业务隔离, 避免故障扩散

![image-20240720151943995](./图片/image-20240720151943995.png)

![image-20240720152354524](./图片/image-20240720152354524.png)

#### 服务保护方案 - 服务熔断

服务熔断: 由**断路器**统计请求的异常比例或慢调用比例, 如果超出阈值则会**熔断**该业务, 则拦截该接口的请求.

熔断期间, 所有请求快速失败, 全都走fallback逻辑.

![image-20240720153000702](./图片/image-20240720153000702.png)

#### 总结

解决雪崩问题的常见方案有哪些?

- **请求限流**: 限制流量在服务可以处理的范围, 避免因突发流量而故障
- **线程隔离**: 控制业务可用的线程数量, 将故障隔离在一定范围
- **服务熔断**: 将异常比例过高的接口断开, 拒绝所有请求, 直接走fallback

- **失败处理**: 定义fallback逻辑, 让业务失败时不再抛出异常, 而是返回默认数据或友好提示

### 服务保护技术

![image-20240720153634988](./图片/image-20240720153634988.png)

## Sentinel

[‌‬‬‍‬⁠‬‌⁠‬⁠‍‍‍⁠﻿‬‍‍‌⁠﻿‍⁠‬‬‌‬‬‍day05-服务保护和分布式事务 - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/QfVrw3sZvihmnPkmALYcUHIDnff)

### 初识Sentinel

Sentinel是阿里巴巴开源的一款微服务流量控制组件. 官网地址: https://sentinelguard.io/zh-cn/index.html

#### 配置使用

控制台版本jar包: [sentinel-dashboard-1.8.6.jar](..\图片\Sentinel\sentinel-dashboard.jar) 

启动jar包:

```cmd
java -Dserver.port=8090 -Dcsp.sentinel.dashboard.server=localhost:8090 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```

浏览器进入: localhost:8090

账号密码都是sentinel

java项目引入依赖:

```xml
<!--sentinel-->
<dependency>
    <groupId>com.alibaba.cloud</groupId> 
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

配置控制台:

> application.yaml

```yaml
spring:
  cloud: 
    sentinel:
      transport:
        dashboard: localhost:8090
```

访问对应接口, 并查看控制台

![image-20240720160509879](./图片/image-20240720160509879.png)

#### 簇点链路

**簇点链路**: 就是单机(单个微服务)调用链路. 是一次请求进入服务后经过的每一个被Sentinel监控的资源链. 默认Sentinel会监控SpringMVC的每一个Endpoint(Http接口). 限流、熔断等都是针对簇点链路中的**资源**设置的. 而资源名默认就是接口的请求路径:

![image-20240720160739147](./图片/image-20240720160739147.png)

Restful风格的API请求路径一般都相同, 这会导致簇点资源名称重复. 因此我们要修改配置, 把**请求方式+请求路径**作为簇点资源名称:

```yaml
spring:
  cloud: 
    sentinel:
      transport:
        dashboard: localhost:8090
      http-method-specify: true # 开启请求方式前缀
```

### 请求限流

在簇点链路后面点击流控按钮, 即可对其做限流配置:

![image-20240720161910866](./图片/image-20240720161910866.png)

### 线程隔离

当商品服务出现阻塞或故障, 调用商品服务的购物车服务可能因此而被拖慢, 甚至资源耗尽. 所以必须限制购物车服务中查询商品这个业务的可用线程数, 实现线程隔离

![image-20240720170425893](./图片/image-20240720170425893.png)

在sentinel控制台中, 会出现Feign接口的簇点资源, 点击后面的流控按钮, 即可配置现成隔离:

![image-20240720180352845](./图片/image-20240720180352845.png)

**<font color='red'>如果设置单机阈值为5, 代表可以使用五个并发线程, 如果单线程QPS为2, 则5线程的QPS为10</font>**

QPS = 1000ms/单次请求持续时间(RT)

### Fallback

![image-20240720183054465](./图片/image-20240720183054465.png)

1. 将FeignClient作为Sentinel的簇点资源

   ```yaml
   feign:
   	sentinel:
   		enabled: true
   ```

2. FeignClient的Fallback有两种配置方式:
   - 方式一: FallbackClass, 无法对远程调用的异常做处理
   - 方式二: FallbackFactory, 可以对远程调用的异常做处理, 通常都会选择这种

#### 给一个FeignClient编写Fallback逻辑步骤

```java
@FeignClient(value = "userservice")
public interface UserClient {
    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

<font color='red'>步骤一</font>: 自定义类, 实现FallbackFactory, 编写对某个FeignClient的Fallback逻辑:

```java
@Slf4j
public class UserClientFallbackFactory implements FallbackFactory<UserClient> {
    @Override
    public UserClient create(Throwable throwable) {
        // 创建UserClient接口实现类, 实现其中的的方法, 编写失败降级的处理逻辑
        return new UserClient() {
            @Override
            public User findById(Long id) {
                // 记录异常信息, 可以返回空或抛出异常
                log.error("查询用户失败", throwable);
                return null;
            }
        };
    }
}
```

<font color='red'>步骤二</font>: 将刚刚定义的UserClientFallbackFactory注册为一个Bean:

```java
@Bean
public UserClientFallbackFactory userClientFallback(){
    return new UserClientFallbackFactory();
}
```

<font color='red'>步骤三</font>: 在UserClient接口中使用UserClientFallbackFactory:

```java
@FeignClient(value = "userservice", fallbackFactory = UserCLientFallFactory.class)
public interface UserClient {
    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

### 服务熔断

#### 服务熔断

熔断是解决雪崩问题的重要手段. 思路是由<font color='red'>**断路器**</font>统计服务调用的异常比例、慢请求比例, 如果超出阈值则会<font color='red'>**熔断**</font>该服务. 即拦截访问该服务的一切请求; 而当服务恢复时, 断路器会放行访问该服务的请求.

![image-20240720190138244](./图片/image-20240720190138244.png)

#### 断路器原理

![image-20240720190414316](./图片/image-20240720190414316.png)

#### 配置

点击控制台中簇点资源后的熔断按钮, 即可配置熔断策略

![image-20240720190509742](./图片/image-20240720190509742.png)

## 分布式事务

### 分布式事务

以下单业务为例, 前端请求首先进入订单服务, 创建订单并写入数据库. 然后订单服务调用购物车服务和库存服务:

1. 购物车服务负责清理购物车信息
2. 库存服务负责扣减商品库存

![image-20240720191531949](./图片/image-20240720191531949.png)

当库存服务出现异常时(如库存不足), 此时购物车服务是无法感知到异常发生的, 所以无法触发回滚而是直接删除购物车数据

在分布式系统中, 如果一个业务需要多个服务合作完成, 而且每一个服务都有事务, 多个事务必须同时成功或失败, 这样的事务就是**分布式事务**. 其中的每个服务的事务就是一个**分支事务**. 整个业务称为**全局事务**.

### 初识Seata

Seata是2019年1月份蚂蚁金服和阿里巴巴共同开源的分布式事务解决方案. 致力于提供高性能和简单易用的分布式事务服务, 为用户打造一站式的分布式解决方案.

官网地址: http://seata.io/, 其中的文档、博客中提供了大量的使用说明、源码分析.

### 分布式事务解决思路

解决分布式事务, 各个子事务之间必须能感知到彼此的事务状态, 才能保证状态一致.

![image-20240720193836497](./图片/image-20240720193836497.png)

### Seata架构

Seata事务管理中有三个重要的角色:

- **TC(Transaction Coordinator) - 事务协调者:** 维护全局和分支事务的状态, 协调全局事务提交或回滚. 
- **TM(Transaction Manager) - 事务管理器**: 定义全局事务的范围、开始全局事务、提交或回滚全局事务
- **RM(Resource Manager) - 资源管理器**: 管理分支事务, 与TC交谈以注册分支事务和报告分支事务的状态.

![image-20240720194754158](./图片/image-20240720194754158.png)

### 部署TC服务

#### 1.准备数据库表

Seata支持多种存储模式，但考虑到持久化的需要，我们一般选择基于数据库存储。执行课前资料提供的`《seata-tc.sql》`，导入数据库表：

![download_image](./图片/download_image.png)

#### 2.准备配置文件

课前资料准备了一个seata目录，其中包含了seata运行时所需要的配置文件：

 [application.yml](..\图片\seata\application.yml) 

![image-20240720195125959](./图片/image-20240720195125959.png)

其中包含中文注释，大家可以自行阅读。

我们将整个seata文件夹拷贝到虚拟机的`/root`目录：

![93bfb7af-7775-4195-a412-0576e5c2b28d](./图片/93bfb7af-7775-4195-a412-0576e5c2b28d.png)

#### 3.Docker部署

需要注意，要确保nacos、mysql都在hm-net网络中。如果某个容器不再hm-net网络，可以参考下面的命令将某容器加入指定网络：

```Shell
docker network connect [网络名] [容器名]
```

在虚拟机的`/root`目录执行下面的命令：

```Shell
docker run --name seata \
-p 8099:8099 \
-p 7099:7099 \
-e SEATA_IP=192.168.11.128 \
-v ./seata:/seata-server/resources \
--privileged=true \
--network hmall \
-d \
seataio/seata-server:1.5.2
```

```
docker run -d \
    --name seata \
    --restart=always \
    -p 8091:8091 \
    -p 7091:7091 \
    -e SEATA_IP=192.168.31.128 \
    -v /root/seata/config:/seata-server/resources \
    --privileged=true \
    --network qqzh-news \
    seataio/seata-server:2.0.0
```

### 微服务集成Seata

#### 微服务集成Seata

首先, 要在项目中引入Seata依赖:

```xml
<!--Seata-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
</dependency>
```

然后, 在application.yml中添加配置, 让微服务找到TC服务地址:

```yaml
seata:
  registry: # 注册中心的配置, 微服务根据这些信息去注册中心获取TC服务地址
    type: nacos # 注册中心类型 Nacos
    nacos:
      server-addr: 192.168.11.128:8848
      namespace: ""
      group: DEFAULT_GROUP
      application: seata-server # seata服务名称
      username: nacos
      password: nacos
  tx-service-group: hmall # 事务组名称
  service:
    vgroup-mapping: # 事务组与TC集群的映射关系
      hmall: "default"
```

![image-20240722071309084](./图片/image-20240722071309084.png)

启动报错时在运行配置中添加虚拟机选项: `--add-opens=java.base/java.lang=ALL-UNNAMED`

### XA模式

#### XA模式

XA规范是X/Open组织定义的分布式事务处理(DTP, Distributed Transaction Processing)标准, XA规范描述了全局的TM与局部的RM之间的接口, 几乎所有主流的关系型数据库都对XA规范提供了支持. Seata的XA模式如下:

![image-20240722083424658](./图片/image-20240722083424658.png)

一阶段的工作:

1. RM注册分支事务到TC
2. RM执行分支业务SQL但不提交
3. RM报告执行状态到TC

二阶段的工作:

- TC检测各分支事务执行状态
  - 如果都成功, 通知所有RM提交事务
  - 如果有失败, 通知所有RM回滚事务
- RM接收TC指令, 提交或回滚事务

#### 总结

XA模式的优点是什么?

- 事务的强一致性, 满足ACID原则
- 常用数据库都支持, 实现简单, 并且没有代码侵入

XA模式的缺点是什么?

- 因为一阶段需要锁定数据库资源, 等待二阶段结束才释放, 性能较差
- 依赖关系型数据库实现事务

#### 实现XA模式

Seata的starter已经完成了XA模式的自动装配, 实现非常简单, 步骤如下:

1. 修改application.yml文件(每个参与事务的微服务), 开启XA模式

   ```yaml
   seata: 
   	data-source-proxy-mode: XA # 开启数据源代理的XA模式
   ```

2. 给发起全局事务的入口方法添加`@GlobalTransactional`注解, 本例中是OrderServiceImpl中的create方法:

   ```java
   @Override
   @GlobalTransactional
   public Long createOrder(OrderFormDTO order) {
       //...
       return order.getId();
   }
   ```

3. 重启服务并调试

### AT模式

#### AT模式

Seata主推的是AT模式, AT模式同样是分阶段提交的事务模型, 不过却弥补了XA模型中资源锁定周期过长的缺陷.

阶段一RM的工作:

- 注册分支事务

- **<font color='red'>记录undo-log(数据快照)</font>**

- 执行业务SQL并<font color='red'>**提交**</font>

- 报告事务状态

  ![image-20240722090141411](./图片/image-20240722090141411.png)

阶段二提交时RM的工作:

- 删除undo-log即可

阶段二回滚时RM的工作:

- 根据undo-log恢复数据到更新前

![image-20240722090446485](./图片/image-20240722090446485.png)

#### 实现AT模式

首先, 添加资料中的seata-at.sql到微服务对应的数据库中:

```sql
-- for AT mode you must to init this sql for you business database. the seata server not need it.
CREATE TABLE IF NOT EXISTS `undo_log`
(
    `branch_id`     BIGINT       NOT NULL COMMENT 'branch transaction id',
    `xid`           VARCHAR(128) NOT NULL COMMENT 'global transaction id',
    `context`       VARCHAR(128) NOT NULL COMMENT 'undo_log context,such as serialization',
    `rollback_info` LONGBLOB     NOT NULL COMMENT 'rollback info',
    `log_status`    INT(11)      NOT NULL COMMENT '0:normal status,1:defense status',
    `log_created`   DATETIME(6)  NOT NULL COMMENT 'create datetime',
    `log_modified`  DATETIME(6)  NOT NULL COMMENT 'modify datetime',
    UNIQUE KEY `ux_undo_log` (`xid`, `branch_id`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8mb4 COMMENT ='AT transaction mode undo table';
```

然后, 修改application.yml文件, 将事务模式修改为AT模式:

```yaml
seata: 
	data-source-proxy-mode: XA # 开启数据源代理的AT模式
```

### 总结

简述AT模式与XA模式最大的区别是什么?

- **XA模式一阶段不提交事务, 锁定资源; AT模式一阶段直接提交, 不锁定资源**
- **XA模式依赖数据库机制实现回滚; AT模式利用数据快照实现数据回滚.**
- **XA模式强一致; AT模式最终一致**

# RabbitMQ

<center><b>高性能的异步通讯组件</b></center>

<center><a helf="https://b11et3un53m.feishu.cn/wiki/OQH4weMbcimUSLkIzD6cCpN0nvc">‌‍‍‍‍‬⁠‬⁠⁠﻿‬﻿‌‬‍﻿﻿﻿‬‬⁠⁠‬﻿‌day06-MQ基础 - 飞书云文档 (feishu.cn)</a></center>

![image-20240722094706236](./图片/image-20240722094706236.png)

![image-20240722094836812](./图片/image-20240722094836812.png)

## 初识MQ

### 同步调用

#### 同步调用

![image-20240722095244614](./图片/image-20240722095244614.png)

![image-20240722095932044](./图片/image-20240722095932044.png)



- 拓展性差
- 性能下降
- 级联失败

#### 总结

同步调用的优势是什么?

- 时效性强, 等到收到结果后才返回

同步调用的问题是什么?

- 拓展性差
- 性能下降
- 级联失败问题

### 异步调用

#### 异步调用

异步调用通常是基于消息通知的方式, 包含三个角色:

![image-20240722100602645](./图片/image-20240722100602645.png)

- 消息发送者: 投递消息的人, 就是原来的**调用者**
- 消息接收者: 接收和处理消息的人, 就是原来的**服务提供者**
- 消息代理: 管理、暂存、转发消息, 可以理解为微信服务器

应用: 支付服务不再同步调用业务关联度低的服务, 而是发送消息通知到Broker

![image-20240722101229324](./图片/image-20240722101229324.png)

![image-20240722101312073](./图片/image-20240722101312073.png)

优势:

- 解除耦合, 拓展性强
- 无需等待, 性能好
- 故障隔离
- 缓存消息, 流量削峰填谷

#### 总结

异步调用的优势是什么?

- 耦合度低, 拓展性强
- 异步调用, 无需等待, 性能高
- 故障隔离, 下游服务故障不影响上游业务
- 缓存消息, 流量削峰填谷

异步调用的问题是什么?

- 不能立即得到调用结果, 时效性差
- 不确定下游业务执行是否成功
- 业务安全依赖于Broker的可靠性

### MQ技术选型

MQ(MessageQueue), 中文是消息队列, 字面来看就是存放消息的队列. 也就是异步调用中的Broker.

|            | RabbitMQ                | ActiveMQ                       | RocketMQ   | Kafka      |
| ---------- | ----------------------- | ------------------------------ | ---------- | ---------- |
| 公司/社区  | Rabbit                  | Apache                         | 阿里       | Apache     |
| 开发语言   | Erlang                  | Java                           | Java       | Scala&Java |
| 协议支持   | AMQP，XMPP，SMTP，STOMP | OpenWire,STOMP，REST,XMPP,AMQP | 自定义协议 | 自定义协议 |
| 可用性     | 高                      | 一般                           | 高         | 高         |
| 单机吞吐量 | 一般                    | 差                             | 高         | 非常高     |
| 消息延迟   | 微秒级                  | 毫秒级                         | 毫秒级     | 毫秒以内   |
| 消息可靠性 | 高                      | 一般                           | 高         | 一般       |

## RabbitMQ

### 安装部署

[‌‍‍‍‍‬⁠‬⁠⁠﻿‬﻿‌‬‍﻿﻿﻿‬‬⁠⁠‬﻿‌‬‬⁠day06-MQ基础 - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/OQH4weMbcimUSLkIzD6cCpN0nvc)

```Shell
docker run \
 -e RABBITMQ_DEFAULT_USER=qqzj \ 用户名
 -e RABBITMQ_DEFAULT_PASS=123456 \ 密码
 -v mq-plugins:/plugins \ 插件的挂载
 --name mq \ 容器名
 --hostname mq \ 主机名
 -p 15672:15672 \ 控制台端口
 -p 5672:5672 \ 收发消息端口
 --network hmall\ 网络名
 --restart=always \
 -d \
 rabbitmq:3.8-management
```

### 基本介绍

RabbitMQ的整体架构及核心概念:

- virtual-host: 虚拟主机, 起到数据隔离的作用

- publisher: 消息发送者
- consumer: 消息的消费者
- queue: 队列, 存储消息
- exchange: 交换机, 负责路由消息

![image-20240722110147527](./图片/image-20240722110147527.png)

## 快速入门

### 案例

需求: 在RabbitMQ的控制台完成下列操作:

- 新建队列hello.queue1和hello.queue2
- 向默认的amp.fanout交换机发送一条消息
- 查看消息是否到达hello.queue1和hello.queue2
- 总结规律

### 总结

消息发送的注意事项有哪些?

- 交换机只能路由消息, 无法存储消息
- 交换机只会路由消息给与其绑定的队列, 因此队列必须与交换机绑定

## 数据隔离

### 案例

需求: 在RabbitMQ的控制台完成下列操作: 

- 新建一个用户hmall
- 为hmall用户创建一个virtual host
- 测试不同virtual host之间的数据隔离现象

## Java客户端

### 快速入门

#### AMQP

**AMQP**(**A**dvanced **M**essage **Q**ueuing **P**rotocol), 是用于应用程序之间传递业务消息的开放标准. 该协议与语言和平台无关, 更符合微服务中独立性的要求

![image-20240722140803518](./图片/image-20240722140803518.png)

#### Spring AMQP

Spring AMQP是基于AMQP协议定义的一套API规范, 提供了模板来发送和接收消息. 包含两部分, 其中spring-amqp是基础抽象, spring-rabbit是底层的默认实现

![image-20240722140744747](./图片/image-20240722140744747.png)

SpringAMQP的官方地址: https://spring.io/projects/spring-amqp

#### 案例

需求入下:

- 利用控制台创建队列simple.queue
- 在publisher服务中, 利用SpringAMQP直接向simple.queue发送消息
- 在consumer服务中, 利用SpringAMQP编写消费者, 监听simple.queue队列

![image-20240722141442985](./图片/image-20240722141442985.png)

#### 收发消息步骤

1. 引入spring-amqp依赖

   在父工程中引入spring-amqp依赖, 这样publisher和consumer服务都可以使用:

   ```xml
   <!--AMQP依赖，包含RabbitMQ-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-amqp</artifactId>
   </dependency>
   ```

2. 配置RabbitMQ服务端信息

   在每个微服务中引入MQ服务端信息, 这样微服务才能连接到RabbitMQ

   ```yaml
   spring: 
   	rabbitmq: 
   		host: 192.168.11.128 # 主机名
   		port: 5672 # 端口
   		virtual-host: /hmall # 虚拟主机
   		username: hmall # 用户名
   		password: 123456 # 密码
   ```

3. 发送消息

   SpringAMQP提供了RabbitTemplate工具类, 方便我们发送消息. 发送消息代码如下:

   ```java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   
   @Test
   public void testSimpleQueue() {
       // 队列名称
       String queueName = "simple.queue";
       // 消息
       String message = "hello, spring amqp!";
       // 发送消息
       rabbitTemplate.convertAndSend(queueName, message);
   }
   ```

4. 接收消息

   SpringAMQP提供声明式的消息监听, 我们只需要通过**注解**在方法上声明要监听的队列名称, 将来SpringAMQP就会把消息传递给当前方法

   ```java
   @Slf4j
   @Component
   public class SpringRabbitListener {
       @RabbitListener(queues = "simple.queue")
       public void listenSimpleQueueMessage(String msg) throws InterruptedException {
           log.info("Spring消费者接收到消息: [{}]", msg);
       }
   }
   ```

### Work Queues

#### Work Queues

Work Queues, 任务模型. 简单来说就是**让多个消费者绑定到一个队列, 共同消费队列中的消息**.

![image-20240722144354662](./图片/image-20240722144354662.png)

#### 案例

模拟WorkQueue, 实现一个队列绑定多个消费者

基本思路如下:

1. 在RabbitMQ的控制台创建一个队列, 名为work.queue
2. 在publisher服务中定义测试方法, 发送50条消息到work.queue
3. 在consumer服务中定义两个消息监听者, 都监听work.queue队列

> 代码

```java
@RabbitListener(queues = "work.queue")
public void listenWorkQueueMessage1(String message) {
    System.err.println("Spring1消费者接收到消息: [{}" + message + " + time: " + LocalTime.now() + "]");
}

@RabbitListener(queues = "work.queue")
public void listenWorkQueueMessage2(String message) {
    System.out.println("Spring2消费者接收到消息: [{}" + message + " + time: " + LocalTime.now() + "]");
}
```

> 结果

```java
Spring1消费者接收到消息: [{}hello, spring amqp!0 + time: 17:41:25.307232400]
Spring1消费者接收到消息: [{}hello, spring amqp!2 + time: 17:41:25.311198300]
Spring1消费者接收到消息: [{}hello, spring amqp!4 + time: 17:41:25.311198300]
Spring1消费者接收到消息: [{}hello, spring amqp!6 + time: 17:41:25.311695800]
Spring1消费者接收到消息: [{}hello, spring amqp!8 + time: 17:41:25.312191700]
Spring1消费者接收到消息: [{}hello, spring amqp!10 + time: 17:41:25.312191700]
Spring1消费者接收到消息: [{}hello, spring amqp!12 + time: 17:41:25.312687600]
Spring1消费者接收到消息: [{}hello, spring amqp!14 + time: 17:41:25.312687600]
Spring1消费者接收到消息: [{}hello, spring amqp!16 + time: 17:41:25.313183200]
Spring1消费者接收到消息: [{}hello, spring amqp!18 + time: 17:41:25.313679200]
Spring1消费者接收到消息: [{}hello, spring amqp!20 + time: 17:41:25.313679200]
Spring1消费者接收到消息: [{}hello, spring amqp!22 + time: 17:41:25.314175300]
Spring1消费者接收到消息: [{}hello, spring amqp!24 + time: 17:41:25.314175300]
Spring1消费者接收到消息: [{}hello, spring amqp!26 + time: 17:41:25.314678400]
Spring1消费者接收到消息: [{}hello, spring amqp!28 + time: 17:41:25.314678400]
Spring1消费者接收到消息: [{}hello, spring amqp!30 + time: 17:41:25.315167200]
Spring1消费者接收到消息: [{}hello, spring amqp!32 + time: 17:41:25.315663]
Spring1消费者接收到消息: [{}hello, spring amqp!34 + time: 17:41:25.316159100]
Spring1消费者接收到消息: [{}hello, spring amqp!36 + time: 17:41:25.316159100]
Spring1消费者接收到消息: [{}hello, spring amqp!38 + time: 17:41:25.316159100]
Spring1消费者接收到消息: [{}hello, spring amqp!40 + time: 17:41:25.316654900]
Spring1消费者接收到消息: [{}hello, spring amqp!42 + time: 17:41:25.316654900]
Spring1消费者接收到消息: [{}hello, spring amqp!44 + time: 17:41:25.317151100]
Spring1消费者接收到消息: [{}hello, spring amqp!46 + time: 17:41:25.317151100]
Spring1消费者接收到消息: [{}hello, spring amqp!48 + time: 17:41:25.317151100]
Spring2消费者接收到消息: [{}hello, spring amqp!1 + time: 17:41:25.307232400]
Spring2消费者接收到消息: [{}hello, spring amqp!3 + time: 17:41:25.308232700]
Spring2消费者接收到消息: [{}hello, spring amqp!5 + time: 17:41:25.308232700]
Spring2消费者接收到消息: [{}hello, spring amqp!7 + time: 17:41:25.308719300]
Spring2消费者接收到消息: [{}hello, spring amqp!9 + time: 17:41:25.308719300]
Spring2消费者接收到消息: [{}hello, spring amqp!11 + time: 17:41:25.311198300]
Spring2消费者接收到消息: [{}hello, spring amqp!13 + time: 17:41:25.311695800]
Spring2消费者接收到消息: [{}hello, spring amqp!15 + time: 17:41:25.311695800]
Spring2消费者接收到消息: [{}hello, spring amqp!17 + time: 17:41:25.312191700]
Spring2消费者接收到消息: [{}hello, spring amqp!19 + time: 17:41:25.312687600]
Spring2消费者接收到消息: [{}hello, spring amqp!21 + time: 17:41:25.313183200]
Spring2消费者接收到消息: [{}hello, spring amqp!23 + time: 17:41:25.313679200]
Spring2消费者接收到消息: [{}hello, spring amqp!25 + time: 17:41:25.313679200]
Spring2消费者接收到消息: [{}hello, spring amqp!27 + time: 17:41:25.314175300]
Spring2消费者接收到消息: [{}hello, spring amqp!29 + time: 17:41:25.314678400]
Spring2消费者接收到消息: [{}hello, spring amqp!31 + time: 17:41:25.315167200]
Spring2消费者接收到消息: [{}hello, spring amqp!33 + time: 17:41:25.315663]
Spring2消费者接收到消息: [{}hello, spring amqp!35 + time: 17:41:25.315663]
Spring2消费者接收到消息: [{}hello, spring amqp!37 + time: 17:41:25.316159100]
Spring2消费者接收到消息: [{}hello, spring amqp!39 + time: 17:41:25.316159100]
Spring2消费者接收到消息: [{}hello, spring amqp!41 + time: 17:41:25.316654900]
Spring2消费者接收到消息: [{}hello, spring amqp!43 + time: 17:41:25.316654900]
Spring2消费者接收到消息: [{}hello, spring amqp!45 + time: 17:41:25.316654900]
Spring2消费者接收到消息: [{}hello, spring amqp!47 + time: 17:41:25.317151100]
Spring2消费者接收到消息: [{}hello, spring amqp!49 + time: 17:41:25.317151100]
```

1. 消费者1每秒处理40条消息, 消费者2每秒处理5条消息

> 代码

```java
@RabbitListener(queues = "work.queue")
public void listenWorkQueueMessage1(String message) {
    System.err.println("Spring1消费者接收到消息: [{}" + message + " + time: " + LocalTime.now() + "]");
    try {
        Thread.sleep(25);
    } catch (InterruptedException e) {
        throw new RuntimeException(e);
    }
}

@RabbitListener(queues = "work.queue")
public void listenWorkQueueMessage2(String message) {
    System.out.println("Spring2消费者接收到消息: [{}" + message + " + time: " + LocalTime.now() + "]");
    try {
        Thread.sleep(200);
    } catch (InterruptedException e) {
        throw new RuntimeException(e);
    }
}
```

> 结果

```java
Spring2消费者接收到消息: [{}hello, spring amqp!1 + time: 17:38:13.487895500]
Spring1消费者接收到消息: [{}hello, spring amqp!0 + time: 17:38:13.487895500]
Spring1消费者接收到消息: [{}hello, spring amqp!2 + time: 17:38:13.513282800]
Spring1消费者接收到消息: [{}hello, spring amqp!4 + time: 17:38:13.539795700]
Spring1消费者接收到消息: [{}hello, spring amqp!6 + time: 17:38:13.566684800]
Spring1消费者接收到消息: [{}hello, spring amqp!8 + time: 17:38:13.593354800]
Spring1消费者接收到消息: [{}hello, spring amqp!10 + time: 17:38:13.619768600]
Spring1消费者接收到消息: [{}hello, spring amqp!12 + time: 17:38:13.646562700]
Spring1消费者接收到消息: [{}hello, spring amqp!14 + time: 17:38:13.672865400]
Spring2消费者接收到消息: [{}hello, spring amqp!3 + time: 17:38:13.688240200]
Spring1消费者接收到消息: [{}hello, spring amqp!16 + time: 17:38:13.699623300]
Spring1消费者接收到消息: [{}hello, spring amqp!18 + time: 17:38:13.724946400]
Spring1消费者接收到消息: [{}hello, spring amqp!20 + time: 17:38:13.750535900]
Spring1消费者接收到消息: [{}hello, spring amqp!22 + time: 17:38:13.777296100]
Spring1消费者接收到消息: [{}hello, spring amqp!24 + time: 17:38:13.803514700]
Spring1消费者接收到消息: [{}hello, spring amqp!26 + time: 17:38:13.829705400]
Spring1消费者接收到消息: [{}hello, spring amqp!28 + time: 17:38:13.854897200]
Spring1消费者接收到消息: [{}hello, spring amqp!30 + time: 17:38:13.881691800]
Spring2消费者接收到消息: [{}hello, spring amqp!5 + time: 17:38:13.888710600]
Spring1消费者接收到消息: [{}hello, spring amqp!32 + time: 17:38:13.907173200]
Spring1消费者接收到消息: [{}hello, spring amqp!34 + time: 17:38:13.933988700]
Spring1消费者接收到消息: [{}hello, spring amqp!36 + time: 17:38:13.960402]
Spring1消费者接收到消息: [{}hello, spring amqp!38 + time: 17:38:13.987239900]
Spring1消费者接收到消息: [{}hello, spring amqp!40 + time: 17:38:14.013928]
Spring1消费者接收到消息: [{}hello, spring amqp!42 + time: 17:38:14.039578300]
Spring1消费者接收到消息: [{}hello, spring amqp!44 + time: 17:38:14.065369700]
Spring2消费者接收到消息: [{}hello, spring amqp!7 + time: 17:38:14.089921100]
Spring1消费者接收到消息: [{}hello, spring amqp!46 + time: 17:38:14.091449800]
Spring1消费者接收到消息: [{}hello, spring amqp!48 + time: 17:38:14.117361600]
Spring2消费者接收到消息: [{}hello, spring amqp!9 + time: 17:38:14.291904800]
Spring2消费者接收到消息: [{}hello, spring amqp!11 + time: 17:38:14.503087]
Spring2消费者接收到消息: [{}hello, spring amqp!13 + time: 17:38:14.704260800]
Spring2消费者接收到消息: [{}hello, spring amqp!15 + time: 17:38:14.919922200]
Spring2消费者接收到消息: [{}hello, spring amqp!17 + time: 17:38:15.134583300]
Spring2消费者接收到消息: [{}hello, spring amqp!19 + time: 17:38:15.349525100]
Spring2消费者接收到消息: [{}hello, spring amqp!21 + time: 17:38:15.550295]
Spring2消费者接收到消息: [{}hello, spring amqp!23 + time: 17:38:15.766075100]
Spring2消费者接收到消息: [{}hello, spring amqp!25 + time: 17:38:15.980510200]
Spring2消费者接收到消息: [{}hello, spring amqp!27 + time: 17:38:16.180915300]
Spring2消费者接收到消息: [{}hello, spring amqp!29 + time: 17:38:16.381455700]
Spring2消费者接收到消息: [{}hello, spring amqp!31 + time: 17:38:16.583458100]
Spring2消费者接收到消息: [{}hello, spring amqp!33 + time: 17:38:16.797636]
Spring2消费者接收到消息: [{}hello, spring amqp!35 + time: 17:38:17.011513200]
Spring2消费者接收到消息: [{}hello, spring amqp!37 + time: 17:38:17.213016200]
Spring2消费者接收到消息: [{}hello, spring amqp!39 + time: 17:38:17.427489100]
Spring2消费者接收到消息: [{}hello, spring amqp!41 + time: 17:38:17.643347100]
Spring2消费者接收到消息: [{}hello, spring amqp!43 + time: 17:38:17.845072700]
Spring2消费者接收到消息: [{}hello, spring amqp!45 + time: 17:38:18.059585200]
Spring2消费者接收到消息: [{}hello, spring amqp!47 + time: 17:38:18.261164900]
Spring2消费者接收到消息: [{}hello, spring amqp!49 + time: 17:38:18.475372300]
```

#### 消费者消息推送限制

默认情况下, RabbitMQ会将消息依次轮询投递给绑定在队列上的每一个消费者. 但这并没有考虑到消费者是否已经处理完消息, 可能出现消息堆积.

因此我们需要修改application.yml, 设置preFetch值为1, 确保同一时刻最多投递给消费者1条消息;

```yaml
spring: 
	rabbitmq: 
		listener: 
			simple:
				prefetch: 1 # 每次只能获取一条消息, 处理完成才能获取下一个消息
```

> 结果

```java
Spring1消费者接收到消息: [{}hello, spring amqp!0 + time: 17:45:21.231678200]
Spring2消费者接收到消息: [{}hello, spring amqp!1 + time: 17:45:21.231678200]
Spring1消费者接收到消息: [{}hello, spring amqp!2 + time: 17:45:21.258922900]
Spring1消费者接收到消息: [{}hello, spring amqp!3 + time: 17:45:21.288484]
Spring1消费者接收到消息: [{}hello, spring amqp!4 + time: 17:45:21.318279800]
Spring1消费者接收到消息: [{}hello, spring amqp!5 + time: 17:45:21.347168900]
Spring1消费者接收到消息: [{}hello, spring amqp!6 + time: 17:45:21.373735200]
Spring1消费者接收到消息: [{}hello, spring amqp!7 + time: 17:45:21.403577700]
Spring1消费者接收到消息: [{}hello, spring amqp!8 + time: 17:45:21.432933600]
Spring2消费者接收到消息: [{}hello, spring amqp!9 + time: 17:45:21.436407100]
Spring1消费者接收到消息: [{}hello, spring amqp!10 + time: 17:45:21.461701400]
Spring1消费者接收到消息: [{}hello, spring amqp!11 + time: 17:45:21.489377]
Spring1消费者接收到消息: [{}hello, spring amqp!12 + time: 17:45:21.517649]
Spring1消费者接收到消息: [{}hello, spring amqp!13 + time: 17:45:21.546463]
Spring1消费者接收到消息: [{}hello, spring amqp!14 + time: 17:45:21.573339]
Spring1消费者接收到消息: [{}hello, spring amqp!15 + time: 17:45:21.601610800]
Spring1消费者接收到消息: [{}hello, spring amqp!16 + time: 17:45:21.628890800]
Spring2消费者接收到消息: [{}hello, spring amqp!17 + time: 17:45:21.639761300]
Spring1消费者接收到消息: [{}hello, spring amqp!18 + time: 17:45:21.655592900]
Spring1消费者接收到消息: [{}hello, spring amqp!19 + time: 17:45:21.684161600]
Spring1消费者接收到消息: [{}hello, spring amqp!20 + time: 17:45:21.711663]
Spring1消费者接收到消息: [{}hello, spring amqp!21 + time: 17:45:21.739282200]
Spring1消费者接收到消息: [{}hello, spring amqp!22 + time: 17:45:21.766085800]
Spring1消费者接收到消息: [{}hello, spring amqp!23 + time: 17:45:21.793910900]
Spring1消费者接收到消息: [{}hello, spring amqp!24 + time: 17:45:21.819754100]
Spring2消费者接收到消息: [{}hello, spring amqp!25 + time: 17:45:21.841787500]
Spring1消费者接收到消息: [{}hello, spring amqp!26 + time: 17:45:21.846494]
Spring1消费者接收到消息: [{}hello, spring amqp!27 + time: 17:45:21.872452100]
Spring1消费者接收到消息: [{}hello, spring amqp!28 + time: 17:45:21.899157900]
Spring1消费者接收到消息: [{}hello, spring amqp!29 + time: 17:45:21.926607200]
Spring1消费者接收到消息: [{}hello, spring amqp!30 + time: 17:45:21.954771600]
Spring1消费者接收到消息: [{}hello, spring amqp!31 + time: 17:45:21.982630200]
Spring1消费者接收到消息: [{}hello, spring amqp!32 + time: 17:45:22.008948800]
Spring1消费者接收到消息: [{}hello, spring amqp!33 + time: 17:45:22.035632500]
Spring2消费者接收到消息: [{}hello, spring amqp!34 + time: 17:45:22.043067600]
Spring1消费者接收到消息: [{}hello, spring amqp!35 + time: 17:45:22.062540100]
Spring1消费者接收到消息: [{}hello, spring amqp!36 + time: 17:45:22.090093800]
Spring1消费者接收到消息: [{}hello, spring amqp!37 + time: 17:45:22.117757200]
Spring1消费者接收到消息: [{}hello, spring amqp!38 + time: 17:45:22.145437600]
Spring1消费者接收到消息: [{}hello, spring amqp!39 + time: 17:45:22.172637900]
Spring1消费者接收到消息: [{}hello, spring amqp!40 + time: 17:45:22.202220500]
Spring1消费者接收到消息: [{}hello, spring amqp!41 + time: 17:45:22.228590600]
Spring2消费者接收到消息: [{}hello, spring amqp!42 + time: 17:45:22.244310400]
Spring1消费者接收到消息: [{}hello, spring amqp!43 + time: 17:45:22.254897800]
Spring1消费者接收到消息: [{}hello, spring amqp!44 + time: 17:45:22.281717900]
Spring1消费者接收到消息: [{}hello, spring amqp!45 + time: 17:45:22.308125900]
Spring1消费者接收到消息: [{}hello, spring amqp!46 + time: 17:45:22.335242200]
Spring1消费者接收到消息: [{}hello, spring amqp!47 + time: 17:45:22.362898300]
Spring1消费者接收到消息: [{}hello, spring amqp!48 + time: 17:45:22.389071200]
Spring1消费者接收到消息: [{}hello, spring amqp!49 + time: 17:45:22.415360]
```

#### 总结

Work模型的使用:

- 多个消费者绑定到一个队列, 可以加快消息处理速度
- 同一条消息只会被一个消费者处理
- 通过设置prefetch来控制消费者预获取的消息数量, 处理完一条再处理吓一跳, 实现能者多劳

### Fanout交换机

#### Fanout交换机

交换机的作用主要是**接收**发送者发送的消息, 并将消息**路由**到与其绑定的队列

常见交换机的类型有以下三种:

- Fanout: 广播
- Direct: 定向
- Topic: 话题

Fanout Exchange 会将接收到的消息路由到每一个跟其绑定的queue, 所以也叫广播模式

![image-20240722211527935](./图片/image-20240722211527935.png)

#### 案例

利用SpringAMQP演示FanoutExchange的使用

实现思路如下: 

- 在RabbitMQ控制台中, 声明队列fanout.queue1和fanout.queue2

  ![image-20240722212047376](./图片/image-20240722212047376.png)

- 在RabbitMQ控制台中, 声明交换机hmall.fanout, 将两个队列与其绑定

  ![image-20240722212019352](./图片/image-20240722212019352.png)

- 在consumer服务中, 编写两个消费者方法, 分别监听fanout.queue1和fanout.queue2

  ```java
  @RabbitListener(queues = "fanout.queue1")
  public void listenFanoutQueueMessage1(String message) {
      System.err.println("FanoutQueue1消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
  }
  
  @RabbitListener(queues = "fanout.queue2")
  public void listenFanoutQueueMessage2(String message) {
      System.out.println("FanoutQueue2消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
  }
  ```

- 在publisher中编写测试方法, 向hmall.fanout发送消息

  ```java
  @Test
  public void testFanoutQueue() {
      // 交换机名称
      String exchangeName = "hmall.fanout";
      // 消息
      String message = "hello, spring amqp!";
      // 发送消息
      rabbitTemplate.convertAndSend(exchangeName,"", message);
  }
  ```

  > 结果

  ![image-20240722213143803](./图片/image-20240722213143803.png)

![image-20240722212633714](./图片/image-20240722212633714.png)

#### 总结

交换机的作用是什么?

- 接收publisher发送的消息
- 将消息按照规则路由到与之绑定的队列
- FanoutExchange交换机会将消息路由到每个绑定的队列

发送消息到交换机的API是怎样的?

```java
@Test
public void testFanoutQueue() {
    // 交换机名称
    String exchangeName = "hmall.fanout";
    // 消息
    String message = "hello, spring amqp!";
    // 发送消息, 参数分别是: 交换机名称, RoutingKey(暂时为空), 消息内容
    rabbitTemplate.convertAndSend(exchangeName,"", message);
}
```

### Direct交换机

#### Direct交换机

Direct Exchange 会将接收到的消息根据规则路由到指定的Queue, 因此称为**定向**路由. 

- 每一个Queue都与Exchange设置一个BindingKey
- 发布者发送消息时, 指定消息的RoutingKey
- Exchange将消息路由到BindingKey与消息RoutingKey一致的队列

![image-20240722213948853](./图片/image-20240722213948853.png)

#### 案例

利用SpringAMQP演示DirectExchange的使用

需求如下:

- 在RabbitMQ控制台中, 声明队列direct.queue1和direct.queue2

  ![image-20240722214621969](./图片/image-20240722214621969.png)

- 在RabbitMQ控制台中, 声明交换机hamll.direct, 将两个队列与其绑定

  ![image-20240722214638657](./图片/image-20240722214638657.png)

- 在consumer服务中, 编写两个消费者方法, 分别监听direct.queue1和direct.queue2

  ```java
  @RabbitListener(queues = "direct.queue1")
  public void listenDirectQueueMessage1(String message) {
      System.err.println("DirectQueue1消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
  }
  
  @RabbitListener(queues = "direct.queue2")
  public void listenDirectQueueMessage2(String message) {
      System.out.println("DirectQueue2消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
  }
  ```

- 在publisher中编写测试方法, 利用不同的RoutingKey向hmall.direct发送消息

  ```java
  @Test
  public void testDirectQueue() {
      // 交换机名称
      String exchangeName = "hmall.direct";
      // 消息
      String message = "hello, spring amqp!";
      // 发送消息
      rabbitTemplate.convertAndSend(exchangeName,"blue", message + "blue");
      rabbitTemplate.convertAndSend(exchangeName,"yellow", message + "yellow");
  }
  ```

> 结果

![image-20240722215037248](./图片/image-20240722215037248.png)

- 将两个队列的BindingKey都设置为red之后

  ![image-20240722215256153](./图片/image-20240722215256153.png)

  ```java
  @Test
  public void testDirectQueue() {
      // 交换机名称
      String exchangeName = "hmall.direct";
      // 消息
      String message = "hello, spring amqp!";
      // 发送消息
      rabbitTemplate.convertAndSend(exchangeName,"red", message + "red");
  }
  ```

  ![image-20240722215341872](./图片/image-20240722215341872.png)

#### 总结

描述下Direct交换机与Fanout交换机的差异?

- Fanout交换机将消息路由给每一个与之绑定的队列
- Direct交换机根据RoutingKey判断路由给哪个队列
- 如果多个队列具有相同RoutingKey, 则与Fanout功能类似

### Topic交换机

#### Topic交换机

TopicExchange也是基于RoutingKey做消息路由, 但是RoutingKey通常是多个单词的组合, 并且以**`.`**分割

Queue与Exchange指定BindingKey时可以使用通配符:

- `#`: 代指0个或多个单词
- `*`: 代指一个单词

![image-20240722220947043](./图片/image-20240722220947043.png)

#### 案例

利用SpringAMQP演示TopicExchange的使用

需求如下:

1. 在RabbitMQ控制台中, 声明队列topic.queue1和topic.queue2

   ![image-20240722221335030](./图片/image-20240722221335030.png)

2. 在RabbitMQ控制台中, 声明交换机hamll.topic, 将两个队列与其绑定

   ![image-20240722222455805](./图片/image-20240722222455805.png)

3. 在consumer服务中, 编写两个消费者方法, 分别监听topic.queue1和topic.queue2

   ```java
   @RabbitListener(queues = "topic.queue1")
   public void listenTopicQueueMessage1(String message) {
       log.info("TopicQueue1消费者接收到消息: {}", message);
   }
   
   @RabbitListener(queues = "topic.queue2")
   public void listenTopicQueueMessage2(String message) {
       log.info("TopicQueue2消费者接收到消息: {}", message);
   }
   ```

4. 在publisher中编写测试方法, 利用不同的RoutingKey向hmall.topic发送消息

   ```java
   @Test
   public void testTopicQueue() {
       // 交换机名称
       String exchangeName = "hmall.topic";
       // 消息
       String chinaNews = "chinaNews: Hello, China!";
       String chinaWeather = "chinaWeather: Very Good!";
       String JapanNews = "JapanNews: Fuck!";
       // 发送消息
       rabbitTemplate.convertAndSend(exchangeName, "china.news", chinaNews);
       rabbitTemplate.convertAndSend(exchangeName, "china.weather", chinaWeather);
       rabbitTemplate.convertAndSend(exchangeName, "japan.news", JapanNews);
   }
   ```

> 结果

![image-20240722222649717](./图片/image-20240722222649717.png)

#### 总结

描述下Topic交换机相比Direct交换机的差异?

- Topic的RoutingKey和BindingKey可以是多个单词, 以`.`分割
- Topic交换机与队列绑定时的BindingKey可以指定通配符
- `#`: 代表0个或多个词
- `*`: 代表1一个词

### 声明队列和交换机

#### 声明队列和交换机

SpringAMQP提供了几个类, 用来声明队列、交换机及其绑定关系:

- Queue: 用于声明队列, 可以用工厂类QueueBuilder构建

- Exchange: 用于声明交换机, 可以用工厂类ExchangeBuilder构建

  ![image-20240723085720798](./图片/image-20240723085720798.png)

- Binding: 用户声明队列和交换机的绑定关系, 可以用工厂类BindingBuilder构建

声明一个Fanout类型的交换机, 并且创建队列与其绑定:

```java
@Configuration
public class FanoutConfig {
    // 声明FanoutExchange交换机
    @Bean
    public FanoutExchange fanoutExchange(){
        return new FanoutExchange("hmall.fanout");
    }
    // 声明第一个队列
    @Bean
    public Queue fanoutQueue1() {
        return new Queue("fanout.queue1");
    }
    // 绑定队列和交换机
    @Bean
    public Binding bindingQueue1(Queue fanoutQueue1, FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueue1).to(fanoutExchange);
    }
}
```

使用Builder方式声明交换机和队列

```java
@Configuration
public class FanoutConfig {
    // 声明FanoutExchange交换机
    @Bean
    public FanoutExchange fanoutExchange(){
        return ExchangeBuilder
            .fanoutExchange("hmall.fanout")
            .build();
    }
    // 声明第一个队列
    @Bean
    public Queue fanoutQueue1() {
        return QueueBuilder
            .durable("fanout.queue1")
            .build();
    }
}
```

> 示例

```java
@Configuration
public class FanoutConfig {
    // 声明交换机
    @Bean
    public FanoutExchange fanoutExchange() {
        //return new FanoutExchange("hmall.fanout");
        return ExchangeBuilder.fanoutExchange("hmall.fanout").build();
    }
    // 声明队列
    @Bean
    public Queue fanoutQueue1() {
        //return new Queue("fanout.queue1");
        return QueueBuilder.durable("fanout.queue1").build();
    }
    // 绑定队列到交换机
    @Bean
    public Binding bindingQueue1(Queue fanoutQueue1, FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueue1).to(fanoutExchange);
    }

    // 声明队列
    @Bean
    public Queue fanoutQueue2() {
        //return new Queue("fanout.queue2");
        return QueueBuilder.durable("fanout.queue2").build();
    }
    // 绑定队列到交换机
    @Bean
    public Binding bindingQueue2(Queue fanoutQueue2, FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueue2).to(fanoutExchange);
    }
}
```

![image-20240723092130967](./图片/image-20240723092130967.png)

### 基于注解声明队列和交换机

#### 案例

利用SpringAMQP声明DirectExchange并与队列绑定

需求如下:

1. 在consumer服务中, 声明队列direct.queue1和direct.queue2

   ```java
   // 声明队列1
   @Bean
   public Queue directQueue1() {
       return new Queue("direct.queue1");
   }
   // 声明队列2
   @Bean
   public Queue directQueue2() {
       return new Queue("direct.queue2");
   }
   ```

2. 在consumer服务中, 声明交换机hmall.direct, 将两个队列与其绑定

   ```java
   // 声明交换机
   @Bean
   public DirectExchange directExchange() {
       return new DirectExchange("hmall.direct");
   }
   ```

3. 在consumer服务中, 编写两个消费者方法, 分别监听direct.queue1和direct.queue2

   ```java
   @RabbitListener(queues = "direct.queue1")
   public void listenDirectQueueMessage1(String message) {
       System.err.println("DirectQueue1消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
   }
   
   @RabbitListener(queues = "direct.queue2")
   public void listenDirectQueueMessage2(String message) {
       System.out.println("DirectQueue2消费者接收到消息: [" + message + " + time: " + LocalTime.now() + "]");
   }
   ```

> 问题: 利用这种方式如果要绑定两个及以上的RoutingKey, 就需要写多个绑定的bean方法, 代码过于复杂, 麻烦

SpringAMQP还提供了基于`@RabbitListener`注解来声明队列和交换机的方式:

```java
@RabbitListener(bindings = @QueueBinding(
    value = @Queue(name = "direct.queue1"),
    exchange = @Exchange(name = "hmall.direct", type = ExchangeTypes.DIRECT),
    key = {"red", "blue"}
))
public void listenDirectQueue1(String msg){
    System.out.println("DirectQueue1消费者接收到消息: [" + message + "]");
}
```

> 示例

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "direct.queue1"),
        exchange = @Exchange(name = "hmall.direct"),
        key = {"red", "blue"}
))
public void listenDirectQueueMessage1(String message) {
    System.err.println("DirectQueue1消费者接收到消息: [" + message + "]");
}

@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "direct.queue2"),
        exchange = @Exchange(name = "hmall.direct"),
        key = {"red", "yellow"}
))
public void listenDirectQueueMessage2(String message) {
    System.out.println("DirectQueue2消费者接收到消息: [" + message + "]");
}
```

> 结果

![image-20240723094957375](./图片/image-20240723094957375.png)

### 消息转换器

#### 案例

需求: 测试利用SpringAMQP发送对象类型的消息

1. 声明一个队列, 名为object.queue

   ![image-20240723101057377](./图片/image-20240723101057377.png)

2. 编写单元测试, 向队列中直接发送一条消息, 消息类型为Map

   ```java
   @Test
   public void testObjectQueue(){
       // 队列名称
       String queueName = "object.queue";
       // 消息
       Map map = new HashMap();
       map.put("name", "tom");
       map.put("age", 12);
       // 发送消息
       rabbitTemplate.convertAndSend(queueName, map);
   }
   ```

3. 在控制台查看消息, 总结问题

   ![image-20240723101340351](./图片/image-20240723101340351.png)


#### 消息转换器

Spring对详细对象的处理是由org.springframework.amqp.support.converter.MessageConverter来处理的. 而默认实现是SimpleMessageConverter, 基于JDK的ObjectOutputStream完成序列化.

存在下列问题:

- JDK的序列化有安全风险
- JDK序列化的消息太大
- JDK序列化的消息可读性差

建议采用JSON序列化代替默认的JDK序列化, 要做两件事情:

在publisher和consumer中都要引入jackson依赖:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```

在publisher和consumer中都要配置MessageConverter:

```java
@Bean
public MessageConverter messageConverter(){
    return new Jackson2JsonMessageConverter();
}
```

![image-20240724071534805](./图片/image-20240724071534805.png)

## 业务改造

### 案例

业务改造

需求: 改造余额支付功能, 不再同步调用交易服务的OpenFeign接口, 而是采用异步MQ通知交易服务更新订单状态

![image-20240724081933985](./图片/image-20240724081933985.png)

> com.hmall.pay.service.impl.PayOrderServiceImpl

```java
@Override
@Transactional
public void tryPayOrderByBalance(PayOrderFormDTO payOrderFormDTO) {
    // 1.查询支付单
    ...
    // 2.判断状态
	...
    // 3.尝试扣减余额
	...
    // 4.修改支付单状态
	...
    // 5.修改订单状态
    log.info("支付成功，订单号：{}", po.getBizOrderNo());
    try {
        rabbitTemplate.convertAndSend("hmall.direct", "pay.success", po.getBizOrderNo());
    } catch (AmqpException e) {
        log.error("发送支付成功消息失败, 订单号: {}", po.getBizOrderNo());
        throw new RuntimeException(e);
    }
}
```

> com.hmall.trade.listener.PayStatusListener

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class PayStatusListener {
    private final IOrderService orderService;
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "trade.pay.success.queue", durable = "true"),
            exchange = @Exchange(name = "hmall.direct"),
            key = "pay.success"
    ))
    public void listenerPaySuccess(Long bizOrderNo){
        log.info("收到支付成功消息,订单号:{}",bizOrderNo);
        orderService.markOrderPaySuccess(bizOrderNo);
    }
}
```

# MQ高级

## 消息可靠性问题

- 发送者的可靠性
- MQ的可靠性
- 消费者的可靠性
- 延迟消息

## 发送者的可靠性

### 发送者重连

#### 发送者重连

有的时候由于网络波动, 可能会出现发送者连接MQ失败的情况. 通过配置我们可以开启连接失败后的重连机制:

```yaml
spring:
	rabbitmq: 
		connection-timeout: 1s # 设置MQ的连接超时时间
		template:
			retry:
				enabled: true # 开启超时重连机制
				initial-interval: 1000ms # 失败后的初始等待时间
				multiplier: 1 # 失败后下次的等待时长倍数, 下次等待时长 = initial-intervol * multiplier
				max-attempts: 3 # 最大重连次数
```

<font color='red'>**注意:**</font>

当网络不稳定的时候, 利用重连机制可以有效提高消息发送的成功率. 不过SpringAMQP提供的重连机制是<font color='red'>**阻塞式**</font>的重连, 也就是说多次重连等待的过程中, 当前线程是被阻塞的, 会影响业务性能.

如果对于业务性能有要求, 建议<font color='red'>**禁用**</font>重连机制. 如果一定要使用, 请合理配置等待时长和重试次数, 当然也可以考虑使用<font color='red'>**异步**</font>线程来执行发送消息的代码.

### 发送者确认

#### 发送者确认

SpringAMQP提供了**Publisher Confirm**和**Publisher Return**两种确认机制. 开启确认机制后, 当发送者发送消息给MQ后, MQ会返回确认结果给发送者. 返回的结果有以下几种情况: 

- 消息投递到了MQ, 但是路由失败. 此时会通过PublisherReturn返回路由异常原因, 然后通过PublisherConfirm返回<font color='green'>**ACK**</font>, 告知投递成功
- 临时消息投递到了MQ, 并且入队成功, 返回<font color='green'>**ACK**</font>, 告知投递成功.
- 持久消息投递到了MQ, 并且入队完成持久化, 返回<font color='green'>**ACK**</font>, 告知投递成功
- 其他情况都会返回<font color='red'>**NACK**</font>, 告知投递失败

![image-20240724101819096](./图片/image-20240724101819096.png)

#### SpringAMQP实现发送者确认

1. 在publisher这个微服务的application.yml中添加配置:

   ```yaml
   spring: 
   	rabbitmq: 
   		publisher-confirm-type: correlated # 开启publisher confirm机制, 并设置confirm类型
   		publisher-returns: true # 开启publisher return机制
   ```

配置说明:

- 这里publisher-confirm-type有三种模式可选:
  - none: 关闭confirm机制
  - simple: 同步阻塞等待MQ的回执消息
  - correlated: MQ异步回调方式返回回执消息

2. 每个RabbitTemplate只能配置一个ReturnCallback, 因此需要再项目启动过程中配置:

   ```java
   @Slf4j
   @AllArgsConstructor
   @Configuration
   public class MqConfig {
       private final RabbitTemplate rabbitTemplate;
       
       @PostConstruct
       public void init(){
           rabbitTemplate.setReturnsCallback(new RabbitTemplate.ReturnsCallback() {
               @Override
               public void returnedMessage(ReturnedMessage returned) {
                   log.error("触发return callback,");
                   log.debug("exchage: {}", returned.getExchange());
                   log.debug("routingKey: {}", returned.getRoutingKey());
                   log.debug("message: {}", returned.getMessage());
                   log.debug("replyCode: {}", returned.getReplyCode());
                   log.debug("replyText: {}", returned.getReplyText());
               }
           });
       }
   }
   ```

3. 发送消息, 指定消息ID、消息ConfirmCallback

   ```java
   @Test
   void testPublisherConfirm() throws InterruptedException {
       // 1.创建CorrelationData
       CorrelationData cd = new CorrelationData();
       // 2.给Future添加ConfirmCallback
       cd.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
           @Override
           public void onFailure(@NotNull Throwable ex) {
               // 2.1Future发生异常时的处理逻辑, 基本不会触发
               log.error("handle message ack fail", ex);
           }
   
           @Override
           public void onSuccess(CorrelationData.Confirm result) {
               // 2.2Future接收到回执的处理逻辑, 参数中的result就是回执内容
               if (result.isAck()){ // result.isAck(), boolean类型, true代表ack回执, false代表nack回执
                   log.debug("发送消息成功, 收到 ACK!");
               }else{ // result.getReason(), String类型, 返回nack时的异常描述
                   log.error("发送消息失败, 收到 NACK, reason: {}", result.getReason());
               }
           }
       });
       // 3.发送消息
       rabbitTemplate.convertAndSend("hmall.direct","pay.success","Hello!",cd);
   }
   ```

## MQ的可靠性

### MQ的可靠性

在默认情况下, RabbitMQ会将接收到的信息保存在内存中以降低消息收发的延迟. 这样会导致两个问题: 

- 一旦MQ宕机, 内存中的消息会丢失
- 内存空间有限, 当消费者故障或处理过慢时, 会导致消息积压, 引发MQ阻塞

### 数据持久化

RabbitMQ实现数据持久化包括三个方面:

- 交换机持久化

  ![image-20240724114517866](./图片/image-20240724114517866.png)

- 队列持久化

  ![image-20240724114608899](./图片/image-20240724114608899.png)

- 消息持久化

  ![image-20240724114723227](./图片/image-20240724114723227.png)

### Lazy Queue

#### Lazy Queue

从RabbitMQ的3.6.0版本开始, 就增加了Lazy Queue的概念, 也就是<font color='red'>惰性队列</font>.

惰性队列的特征如下:

- 接收到消息后直接存入磁盘, 不再存储到内存
- 消费者要消费消息时才会从磁盘中读取并加载到内存(可以提前缓存部分消息到内存, 最多2048条)

在3.12版本后, 所有队列都是Lazy Queue模式, 无法更改

要设置一个队列为惰性队列, 只需要在声明队列时, 指定x-queue-mode属性为lazy即可:

![image-20240724145935742](./图片/image-20240724145935742.png)

> 代码添加

```java
@Bean 
public Queue lazyQueue(){
    return QueueBuilder
        .durable("lazy.queue")
        .lazy() // 开启Lazy模式
        .build();
}
```

> 注解设置

```java
@RabbitListener(queuesToDeclare = @Queue(
    name = "lazy.queue",
    durable = "true",
    arguments = @Argument(name = "x-queue-mode", value = "lazy")
))
public void listenLazyQueue(String msg) {
    log.info(...)
}
```

### 总结

RabbitMQ如何保证消息的可靠性

- 首先通过配置可以让交换机、队列、以及发送的消息都持久化. 这样队列中的消息会持久化到磁盘, MQ重启消息依然存在
- RabbitMQ在3.6版本引入了LazyQueue, 并且在3.12版本后会成为队列的默认模式. LazyQueue会将所有消息都持久化.
- 开启持久化和生产者(发送者)确认时, RabbitMQ只有在消息持久化完成后才会给生产者返回ACK回执

## 消费者的可靠性

### 消费者确认机制

#### 消费者确认机制

消费者确认机制(**Consumer Acknowledgement**)是为了确认消费者是否成功处理消息. 当消费者处理消息结束后, 应该向RabbitMQ发送一个回执, 告知RabbitMQ自己消息处理状态: 

![image-20240725092406787](./图片/image-20240725092406787.png)

- <font color='green'>**ack**</font>: 成功处理消息, RabbitMQ从队列中删除该消息
- <font color='red'>**nack**</font>: 消息处理失败, RabbitMQ需要再次投递消息
- <font color='red'>**reject**</font>: 消息处理失败并拒绝该消息, RabbitMQ从队列中删除消息

SpringAMQP已经实现了消息确认功能. 并允许我们通过配置文件选择ACK处理方式, 有三种方式:

- **none**: 不处理. 即消息投递给消费者后立刻<font color='green'>ack</font>, 消息会立刻从MQ删除. 非常不安全, 不建议使用
- **manual**: 手动模式. 需要自己在业务代码中调用API, 发送<font color='green'>ack</font>或<font color='green'>reject</font>, 存在业务入侵, 但更灵活
- **auto**: 自动模式. SpringAMQP利用AOP对我们的消息处理逻辑做了环绕增强, 当业务正常执行时则自动返回<font color='green'>ack</font>. 当业务出现异常时, 根据异常判断返回不同结果:
  - 如果是业务异常, 会自动返回<font color='red'>nack</font>
  - 如果是消息处理或校验异常, 自动返回<font color='red'>reject</font>

> 配置

```yaml
spring:
	rabbitmq:
		listener:
			simple:
				prefetch: 1
				acknowledge-mode: none # none, 关闭ack; manual, 手动ack; auto, 自动ack
```

### 失败重试机制

#### 失败重试机制

SpringAMQP提供了消费者失败重试机制, 在消费者出现异常时利用本地重试, 而不是无限的requeue到mq. 我们可以通过在application.yaml文件中添加配置来开启重试机制:

```yaml
spring:
	rabbitmq:
		listener:
			simple:
				prefetch:1
				retry:
					enabled: true # 开启消费者失败重试
					initial-interval: 1000ms # 初始的失败等待时长为1秒
					multiplier: 1 # 下次失败的等待时长倍数, 下次等待时长 = multiplier * last-interval
					max-attempts: 3 # 最大重试次数
					stateless: true # true无状态; false有状态. 如果业务中包含事务, 这里改为false	
```

#### 失败消息处理策略

在开启重试模式后, 重试次数耗尽, 如果消息依然失败, 则需要有MessageRecoverer接口来处理, 它包含三种不同的实现:

- RejectAndDontRequeueRecoverer: 重试耗尽后, 直接reject, 丢弃消息. 默认就是这种方式
- ImmediateRequeueMessageRecoverer: 重试耗尽后, 返回nack, 消息重新入队
- RepublishMessageRecoverer: 重试耗尽后, 将失败消息投递到指定的交换机

将失败处理策略改为RepublishMessageRecoverer:

1. 首先, 定义接收失败消息的交换机、队列及其绑定关系

2. 然后, 定义RepublishMessageRecoverer

   ```java
   @Bean
   public MessgeRecoverer republishMessageRecoverer(RabbitTemplate rabbitTemplate){
       return new RepublishMessageRecoverer(rabbitTemplate, "error.direct", "error");
   }
   ```

#### 总结

如何开启消费者失败重试机制?

```yaml
spring:
	rabbitmq:
		listener:
			simple:
				prefetch:1
				retry:
					enabled: true # 开启消费者失败重试
					initial-interval: 1000ms # 初始的失败等待时长为1秒
					multiplier: 1 # 下次失败的等待时长倍数, 下次等待时长 = multiplier * last-interval
					max-attempts: 3 # 最大重试次数
					stateless: true # true无状态; false有状态. 如果业务中包含事务, 这里改为false
```

如何配置失败重试处理策略?

```java
@Bean
public MessgeRecoverer republishMessageRecoverer(RabbitTemplate rabbitTemplate){
    return new RepublishMessageRecoverer(rabbitTemplate, "error.direct", "error");
}
```

### 业务幂等性

#### 业务幂等性

**幂等**是一个数学概念, 用函数表达式来描述是这样的: f(x)=f(f(x)). 在程序开发中, 则是指同一个业务, 执行一次或多次对业务状态的影响是一致的.

#### 唯一消息ID

方案一, 是给每个消息都设置一个**唯一ID**, 利用ID区分是否是重复消息:

1. 每一条消息都生成一个唯一的ID, 与消息一起投递给消费者.
2. 消费者接收到消息后处理自己的业务, 业务处理成功后将消息ID保存到数据库
3. 如果下次又收到相同消息, 去数据库查询判断是否存在, 存在则为重复消息放弃处理

```java
@Bean
public MessageConverter messageConverter(){
    // 1.定义消息转换器
    Jackson2JsonMessageConverter jjmc = new Jackson2JsonMessageConverter();
    // 2.配置自动创建消息ID, 用于识别不同消息, 也可以在业务中基于ID判断是否是重复消息
    jjmc.setCreateMessageIds(true);
    return jjmc;
}
```

#### 业务判断

方案二, 是结合业务逻辑, 基于业务本身做判断. 以余额支付业务为例:

![image-20240726084320138](./图片/image-20240726084320138.png)

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "trade.pay.success.queue", durable = "true"),
        exchange = @Exchange(name = "hmall.direct"),
        key = "pay.success"
))
public void listenerPaySuccess(Long bizOrderNo){
    log.info("收到支付成功消息,订单号:{}",bizOrderNo);
    // 1.查询订单
    Order order = orderService.getById(bizOrderNo);
    // 2.判断订单状态, 是否是未支付
    if (order == null || order.getStatus() != 1){
        // 不做处理
        return;
    }
    // 3.标记订单状态为已支付
    orderService.markOrderPaySuccess(bizOrderNo);
}
```

#### 总结

如何保证支付服务与交易服务之间的订单状态一致性?

- 首先, 支付服务会在用户支付成功以后利用MQ消息通知交易服务, 完成订单状态同步
- 其次, 为了保证MQ的可靠性, 我们采用了生产者确认机制、消费者确认、消费者失败重试等策略, 确保消息投递和处理的可靠性. 同时也开启了MQ的持久化, 避免因服务宕机导致消息丢失.
- 最后, 我们还在交易服务更新订单状态时做了业务幂等判断, 避免因消息重复消费导致订单状态异常.

如果交易服务消息处理失败, 有没有什么兜底方案?

### 延迟消息

#### 延迟消息

**延迟消息**: 发送者发送消息时指定一个时间, 消费者不会立刻收到消息, 而是在指定时间之后才收到消息.

**延迟任务**: 设置在一定时间之后才执行的任务

#### 死信交换机

当一个队列中的消息满足下列情况之一时, 就会成为<font color='red'>死信(dead letter)</font>:

- 消费者使用basic.reject或basic.nack声明消费失败, 并且消息的requeue(重新入队)参数设置为false
- 消息是一个过期消息(达到了队列或消息本身设置的过期时间), 超时无人消费
- 要投递的队列消息堆积满了, 最早的消息可能成为死信

如果队列通过dead-letter-exchange属性指定了一个交换机, 那么该队列中的死信就会投递到这个交换机中. 这个交换机称为<font color='red'>死信交换机</font>(Dead Letter Exchange, 简称DLX).

![image-20240726090947493](./图片/image-20240726090947493.png)

<font color='red'>**两个交换机和队列的BindingKey/RoutingKey要保持一致!**</font>

normal队列不要绑定消费者, 而是直接绑定死信交换机

> normal

```java
@Configuration
public class NormalConfiguration {
    // 1.定义交换机
    @Bean
    public DirectExchange normalExchange() {
        return new DirectExchange("normal.direct");
    }

    // 2.定义队列
    @Bean
    public Queue normalQueue() {
        return QueueBuilder.durable("normal.queue")
                .deadLetterExchange("dlx.direct")
                .build();
    }

    // 3.定义队列绑定关系
    @Bean
    public Binding binding(Queue normalQueue, DirectExchange normalExchange) {
        return BindingBuilder.bind(normalQueue).to(normalExchange).with("hi");
    }
}
```

> dlx

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "dlx.queue"),
        exchange = @Exchange(name = "dlx.direct"),
        key = "hi"
))
public void listenerDLXMessage(Message message){
    log.info("死信交换机接收到消息: {}", new String(message.getBody()));
}
```

> 发送延迟消息

```java
void testSendDelayMessage(){
    rabbitTemplate.convertAndSend("normal.direct", "hi", "hello", message -> {
        message.getMessageProperties().setExpiration("10000");
        return message;
    });
}
```

#### 延迟消息插件

[‍⁠﻿‍⁠⁠‌‍⁠﻿‍⁠‌‌‍﻿‬﻿‬⁠‌‌⁠﻿⁠‬﻿‬‬﻿‍day07-MQ高级 - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/A9SawKUxsikJ6dk3icacVWb4n3g)

这个插件可以将普通交换机改造为支持延迟消息功能的交换机, 当消息投递到交换机后可以暂存一定时间, 到期后再投递到队列

![image-20240726100125372](./图片/image-20240726100125372.png)

> 基于注解方式声明延迟交换机

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "delay.queue", durable = "true"),
        exchange = @Exchange(name = "delay.direct", delayed = "true"),
        key = "delay"
))
public void listenerDelayMessage(Message message){
    log.info("接收到delay.queue的延迟消息: {}", Arrays.toString(message.getBody()));
}
```

> 基于Bean方式声明延迟交换机

```java
@Bean
public DirectExchange delayExchange(){
    return ExchangeBuilder
            .directExchange("delay.direct")
            .delayed() //设置delayed属性为true
            .durable(true) // 持久化
            .build();
}
```

发送消息时需要通过消息头x-delay来设置过期时间:

```java
void testPubilsherDelayMessage(){
    // 1.创建消息
    String msg = "Hello, Delayed Message!";
    // 2.发送消息, 利用消息后置处理器添加消息头
    rabbitTemplate.convertAndSend("delay.direct", "delay", msg, message -> {
        // 添加延迟消息属性
        message.getMessageProperties().setDelay(5000);
        return message;
    });
}
```

#### 取消超时订单

用户下单完成后, 发送15分钟延迟消息, 在15分钟后接收消息, 检查支付状态:

- 已支付: 更新订单状态为已支付
- 未支付: 更新订单状态为关闭订单, 恢复商品库存

![image-20240726102657221](./图片/image-20240726102657221.png)

![image-20240726102935169](./图片/image-20240726102935169.png)

> com.hmall.trade.service.impl.OrderServiceImpl

```java
@Override
@GlobalTransactional
public Long createOrder(OrderFormDTO orderFormDTO) {
    // 1.订单数据
    Order order = new Order();
    // 1.1.查询商品
    List<OrderDetailDTO> detailDTOS = orderFormDTO.getDetails();
    // 1.2.获取商品id和数量的Map
    Map<Long, Integer> itemNumMap = detailDTOS.stream()
            .collect(Collectors.toMap(OrderDetailDTO::getItemId, OrderDetailDTO::getNum));
    Set<Long> itemIds = itemNumMap.keySet();
    // 1.3.查询商品
    //List<ItemDTO> items = itemService.queryItemByIds(itemIds);
    List<ItemDTO> items = itemClient.queryItemByIds(itemIds);
    if (items == null || items.size() < itemIds.size()) {
        throw new BadRequestException("商品不存在");
    }
    // 1.4.基于商品价格、购买数量计算商品总价：totalFee
    int total = 0;
    for (ItemDTO item : items) {
        total += item.getPrice() * itemNumMap.get(item.getId());
    }
    order.setTotalFee(total);
    // 1.5.其它属性
    order.setPaymentType(orderFormDTO.getPaymentType());
    order.setUserId(UserContext.getUser());
    order.setStatus(1);
    // 1.6.将Order写入数据库order表中
    save(order);

    // 2.保存订单详情
    List<OrderDetail> details = buildDetails(order.getId(), items, itemNumMap);
    detailService.saveBatch(details);

    // 3.清理购物车商品
    //cartService.removeByItemIds(itemIds);
    cartClient.deleteCartItemByIds(itemIds);
    // 4.扣减库存
    try {
        //itemService.deductStock(detailDTOS);
        itemClient.deductStock(detailDTOS);
    } catch (Exception e) {
        throw new RuntimeException("库存不足！");
    }
    // 发送延迟消息, 检测订单支付状态
    rabbitTemplate.convertAndSend(
            MQConstants.DELAY_EXCHANGE_NAME,
            MQConstants.DELAY_ORDER_KEY,
            order.getId(),
            message -> {
                message.getMessageProperties().setDelay(30000);
                return message;
            }
    );
    return order.getId();
}
```

> com.hmall.trade.listener.OrderDelayMessageListener

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class OrderDelayMessageListener {
    private final IOrderService orderService;
    private final PayClient payClient;

    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = MQConstants.DELAY_ORDER_QUEUE_NAME, durable = "true"),
            exchange = @Exchange(name = MQConstants.DELAY_EXCHANGE_NAME, delayed = "true"),
            key = MQConstants.DELAY_ORDER_KEY
    ))
    public void listenerOrderDelayMessage(Long orderId) {
        log.info("订单延迟消息监听器收到延迟消息，订单id:{}", orderId);
        // 1.查询订单
        Order order = orderService.getById(orderId);
        // 2.检测订单状态, 判断是否已支付
        if (order == null || order.getStatus() != 1){
            return;
        }
        // 3.未支付, 需要查询支付流水状态
        PayOrderDTO payOrderDTO = payClient.queryPayOrderByBizOrderNo(orderId);
        // 4.判断是否已支付
        if (payOrderDTO != null && payOrderDTO.getStatus() == 3){
            // 4.1 已支付, 标记订单状态为已支付
            orderService.markOrderPaySuccess(orderId);
        }else{
            // 4.2 未支付, 取消订单, 恢复库存
            orderService.cancelOrder(orderId);
        }
    }
}
```

> com.hmall.trade.service.impl.OrderServiceImpl

```java
@Override
public void cancelOrder(Long orderId) {
    // 1.标记订单为已关闭
    this.lambdaUpdate()
            .set(Order::getStatus, 3)
            .eq(Order::getId, orderId)
            .update();
    // 2.恢复库存
    List<OrderDetail> orderDetail = detailService.lambdaQuery()
            .eq(OrderDetail::getOrderId, orderId)
            .list();
    List<OrderDetailDTO> orderDetailDTOS = BeanUtils.copyList(orderDetail, OrderDetailDTO.class);
    orderDetailDTOS.forEach(detail -> {
        detail.setNum(-detail.getNum());
    });
    try {
        //itemService.deductStock(detailDTOS);
        itemClient.deductStock(orderDetailDTOS);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
```

# ElasticSearch01

<center>高性能分布式搜索引擎</center>

## ElasticSearch

- 搜索引擎技术排名:

  1. Elasticsearch: 开源的分布式搜索引擎

  2. Splunk: 商业项目

  3. Solr: Apache的开源搜索引擎

### 初识ElasticSearch

#### 认识和安装

[‍‬‍⁠‬‬‍﻿‬﻿⁠⁠﻿‬﻿⁠‍day08-Elasticsearch - 飞书云文档 (feishu.cn)](https://b11et3un53m.feishu.cn/wiki/LDLew5xnDiDv7Qk2uPwcoeNpngf)

Lucene是一个java语言的搜索引擎类库, 是Apache公司的顶级项目, 由DougCutting于1999年研发. 官网地址: https://lucene.apache.org/

Lucene的优势:

- 易扩展
- 高性能(基于倒排索引)

---

- 2004年的Shay Banon基于Lucene开发了Compass
- 2010年Shay Banon重写了Compass, 取名为ElasticSearch.
- 官网地址: https://www.elastic.co/cn/
- ElasticSearch具备下列优势:
  - 支持分布式, 可水平扩展
  - 提供Restful接口, 可被任何语言调用

ElasticSearch结合kubana、Logstash、Beats, 是一整套技术栈, 被叫做ELK. 被广泛应用在日志数据分析、实时监控等领域

![image-20240726142338776](./图片/image-20240726142338776.png)

> Docker部署ES

```Bash
docker run -d \
  --name es \
  -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
  -e "discovery.type=single-node" \
  -v es-data:/usr/share/elasticsearch/data \
  -v es-plugins:/usr/share/elasticsearch/plugins \
  --privileged \
  --network hmall \
  -p 9200:9200 \
  -p 9300:9300 \
  elasticsearch:7.12.1
```

> Docker部署Kibana

```Bash
docker run -d \
--name kibana \
-e ELASTICSEARCH_HOSTS=http://es:9200 \
--network=qqzj-news \
-p 5601:5601  \
kibana:7.12.1
```

### 倒排索引

#### 倒排索引

传统数据库(如MySQL)采用正向索引, 例如给下表(tb_goods)中的id创建索引

![image-20240726144642984](./图片/image-20240726144642984.png)

ElasticSearch采用倒排索引:

- 文档(document): 每条数据就是一个文档

- 词条(term): 文档按照语义分成的词语

  ![image-20240727064211215](./图片/image-20240727064211215.png)

  ![image-20240727064508017](./图片/image-20240727064508017.png)

#### 总结

什么是文档和词条?

- 每一条数据就是一个文档
- 对文档中的内容分词, 得到的词语就是词条

什么是正向索引?

- 基于文档id创建索引. 根据id查询快, 但是查询词条时必须先找到文档, 而后判断是否包含词条

什么是倒排索引?

- 对文档内容分词, 对词条创建索引, 并记录词条所在文档的id. 查询时先根据词条查询到文档id, 而后根据文档id查询文档

### IK分词器

#### IK分词器

中文分词往往需要根据语义分析, 比较复杂, 这就需要用到中文分词器, 例如**IK分词器**. IK分词器是林良益在2006年开源发布的, 其采用的正向迭代最细粒度切分算法一直沿用至今

其安装的方式也比较简单, 只要将分词器放入ElasticSearch的插件目录即可:

![image-20240727065519022](./图片/image-20240727065519022.png)

在Kibana的DevTools中可以使用下面的语法来测试IK分词器:

```js
POST /_analyze
{
    "analyzer": "standard",
    "text": "青青子衿悠悠我心"
}
```

语法说明:

- `POST`: 请求方式
- `/_analyze`: 请求路径, 这里省略了http://192.168.11.128:9200, 有kibana帮我们补充
- 请求参数, JSON风格:
  - `analyzer`: 分词器类型, 这里是默认的standard分词器
  - `text`: 要分词的内容

> 返回结果

```json
{
  "tokens" : [
    {
      "token" : "青",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<IDEOGRAPHIC>",
      "position" : 0
    },
    {
      "token" : "青",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "子",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "衿",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "悠",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "悠",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "我",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    },
    {
      "token" : "心",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "<IDEOGRAPHIC>",
      "position" : 7
    }
  ]
}
```

> ik_smart分词器模式

```java
POST /_analyze
{
    "analyzer": "ik_smart",
    "text": "青青子衿悠悠我心"
}
```

> 返回结果

```json
{
  "tokens" : [
    {
      "token" : "青青",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "子",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "衿",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "CN_CHAR",
      "position" : 2
    },
    {
      "token" : "悠悠",
      "start_offset" : 4,
      "end_offset" : 6,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "我",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "CN_CHAR",
      "position" : 4
    },
    {
      "token" : "心",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "CN_CHAR",
      "position" : 5
    }
  ]
}
```

> ik_max_word模式

```java
POST /_analyze
{
    "analyzer": "ik_max-word",
    "text": "青青子衿悠悠我心"
}
```

> 返回结果

```json
{
  "tokens" : [
    {
      "token" : "青青",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "子",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "衿",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "CN_CHAR",
      "position" : 2
    },
    {
      "token" : "悠悠",
      "start_offset" : 4,
      "end_offset" : 6,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "我",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "CN_CHAR",
      "position" : 4
    },
    {
      "token" : "心",
      "start_offset" : 7,
      "end_offset" : 8,
      "type" : "CN_CHAR",
      "position" : 5
    }
  ]
}
```

> 换词

```json
POST /_analyze
{
    "analyzer": "ik_max_word",
    "text": ["我是大帅哥"]
}
```

```json
{
  "tokens" : [
    {
      "token" : "我",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "CN_CHAR",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "大帅",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "帅哥",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 3
    }
  ]
}
```

IK分词器允许我们配置拓展词典来增加自定义的词库:

![image-20240727072635325](./图片/image-20240727072635325.png)

![image-20240727072654743](./图片/image-20240727072654743.png)

#### 总结

分词器的作用是什么?

- 创建倒排索引时, 对文档分词
- 用户搜索时, 对输入的内容分词

IK分词器有几种模式?

- `ik_smart`: 智能切分, 粗粒度
- `ik_max_word`: 最细切分, 细粒度IK分词器

如何拓展分词器词库中的词条?

- 利用config目录的IkAnalyzer.cfg.xml文件添加扩展词典
- 在词典中添加扩展词条

### 基础概念

#### 基础概念

ElasticSearch中的文档数据会被序列化为json格式后存储在ElasticSearch中.

![image-20240727074357220](./图片/image-20240727074357220.png)

索引(index)/索引库: 相同类型的文档的集合

映射(mapping): 索引(库)中文档的字段约束信息, 类似表的结构约束

![image-20240727074435241](./图片/image-20240727074435241.png)

#### 关系对照

![image-20240727075001995](./图片/image-20240727075001995.png)

## 索引库操作

### Mapping映射属性

Mapping是对索引库中文档的约束, 常见的Mapping属性包括:

- `type`: 字段数据类型, 常见的简单类型有:
  - 字符串: text(可分词的文本)、keyword(精确值, 例如: 品牌、国家、IP地址)
  - 数值: long、integer、short、byte、double、float
  - 布尔: boolean
  - 日期: date
  - 对象: Object

- `index`: 是否创建索引, 默认为true
- `analyzer`: 使用哪种分词器(当数据类型为text时, 必须指定, 其他类型不指定)
- `properties`: 该字段的子字段(Object类型用到)

### 索引库操作

ElasticSearch提供的所有API都是Restful的接口, 遵循Restful的基本规范:

![image-20240727081503622](./图片/image-20240727081503622.png)

#### 新增

创建索引库和Mapping的请求语法如下:

![image-20240727081720487](./图片/image-20240727081720487.png)

```json
# 创建索引库并设置Mapping映射
PUT /qqzj
{
  "mappings": {
    "properties": {
      "info":{
        "type": "text",
        "analyzer": "ik_smart"
      },
      "age":{
        "type": "byte"
      },
      "email":{
        "type": "keyword",
        "index": false
      },
      "name":{
        "type": "object", 
        "properties": {
          "firstName":{
            "type": "keyword"
          },
          "lastName":{
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

> 返回结果

```java
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "qqzj"
}
```

#### 查询

```json
GET /qqzj
```

> 返回结果

```json
{
  "qqzj" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "byte"
        },
        "email" : {
          "type" : "keyword",
          "index" : false
        },
        "info" : {
          "type" : "text",
          "analyzer" : "ik_smart"
        },
        "name" : {
          "properties" : {
            "firstName" : {
              "type" : "keyword"
            },
            "lastName" : {
              "type" : "keyword"
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "qqzj",
        "creation_date" : "1722039872714",
        "number_of_replicas" : "1",
        "uuid" : "3mnxCh5rTjOrulIxYkoZzA",
        "version" : {
          "created" : "7120199"
        }
      }
    }
  }
}
```

#### 删除

```json
DELETE /qqzj
```

> 返回结果

```java
{
  "acknowledged" : true
}
```

> 再次查询结果

```json
{
  "error" : {
    "root_cause" : [
      {
        "type" : "index_not_found_exception",
        "reason" : "no such index [qqzj]",
        "resource.type" : "index_or_alias",
        "resource.id" : "qqzj",
        "index_uuid" : "_na_",
        "index" : "qqzj"
      }
    ],
    "type" : "index_not_found_exception",
    "reason" : "no such index [qqzj]",
    "resource.type" : "index_or_alias",
    "resource.id" : "qqzj",
    "index_uuid" : "_na_",
    "index" : "qqzj"
  },
  "status" : 404
}
```

#### 修改

索引库和Mapping一旦创建无法修改, 但是可以添加新的字段, 语法如下:

```json
PUT /索引库名/_mapping
{
    "properties": {
        "新字段名": {
            "type": ...
        }
    }
}
```

> 错误示例

```json
# 修改索引库
PUT /qqzj/_mapping
{
  "properties": {
    "info": {
      "type": "keyword"
    }
  }
}
```

> 返回结果

```json
{
  "error" : {
    "root_cause" : [
      {
        "type" : "illegal_argument_exception",
        "reason" : "mapper [info] cannot be changed from type [text] to [keyword]"
      }
    ],
    "type" : "illegal_argument_exception",
    "reason" : "mapper [info] cannot be changed from type [text] to [keyword]"
  },
  "status" : 400
}
```

> 添加字段

```json
# 修改索引库
PUT /qqzj/_mapping
{
  "properties": {
    "age": {
      "type": "byte"
    }
  }
}
```

> 返回结果

```json
{
  "acknowledged" : true
}
```

### 总结

索引库操作有哪些?

- 创建索引库: `PUT/索引库名`
- 查询索引库: `GET/索引库名`
- 删除索引库: `DELETE/索引库名`
- 添加字段: `PUT/索引库名/_mapping`

## 文档操作

### 文档CRUD

#### 增

新增文档的请求路径如下:

```json
POST /索引库名/_doc/文档id
{
    "字段1": "值1",
    "字段2": "值2",
    "字段3": {
        "子属性1": "值3",
        "子属性2": "值4"
    },
    ......
}
```

> 示例

```json
# 新增文档
POST /qqzj/_doc/1
{
  "info": "帅哥",
  "email": "12@",
  "age": 15,
  "name": {
    "firstName": "张",
    "lastName": "三"
  }
}
```

> 返回结果

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

#### 查

> 示例

```json
GET /qqzj/_doc/1
```

> 返回结果

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "info" : "帅哥",
    "email" : "12@",
    "age" : 15,
    "name" : {
      "firstName" : "张",
      "lastName" : "三"
    }
  }
}
```

#### 删

> 示例

```java
DELETE /qqzj/_doc/1
```

> 返回结果

```java
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
```

> 再次查询

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "found" : false
}
```

#### 改

##### 方式一: 全量修改, 会删除旧文档, 添加新文档(`DELETE` + `POST`)

```json
PUT /索引库名/_doc/文档id
{
    "字段1": "值1",
    "字段2": "值2",
    //........
}
```

> 示例

```json
PUT /qqzj/_doc/1
{
  "info": "美女",
  "email": "34@",
  "age": 18,
  "name": {
    "firstName": "李",
    "lastName": "四"
  }
}
```

> 返回结果

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}
```

> 再次查询

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 3,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "info" : "美女",
    "email" : "34@",
    "age" : 18,
    "name" : {
      "firstName" : "李",
      "lastName" : "四"
    }
  }
}
```

<font color='red'>**弊端**</font>: 因为底层是删除+新增操作, 所以如果id不存在, 会导致删除失败, 直接创建新的文档

<font color='red'>**注意**</font>: 全量修改一定要将所有字段全部写上, 因为会先删除, 如果字段不全, 会导致没有写的字段没有值

> 示例

```java
PUT /qqzj/_doc/2
{
  "info": "美女111",
  "email": "34@111",
  "age": 10,
  "name": {
    "firstName": "李",
    "lastName": "四"
  }
}
```

> 结果

```java
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
```

##### 方式二: 增量修改, 修改指定字段值

```json
POST /索引库名/_update/文档id
{
    "doc": {
        "字段名": "新的值",
    }
}
```

> 示例

```json
# 增量修改
POST /qqzj/_update/1
{
  "doc": {
    "age": 33
  }
}
```

> 返回结果

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 5,
  "_primary_term" : 3
}
```

> 查询

```json
{
  "_index" : "qqzj",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3,
  "_seq_no" : 5,
  "_primary_term" : 3,
  "found" : true,
  "_source" : {
    "info" : "美女",
    "email" : "34@",
    "age" : 33,
    "name" : {
      "firstName" : "李",
      "lastName" : "四"
    }
  }
}
```

#### 总结

文档操作有哪些?

- 创建文档: `POST /索引库名/_doc/文档id {json文档}`
- 查询文档: `GET/索引库名/_doc/文档id`
- 删除文档: `DELETE /索引库名/_doc/文档id`
- 修改文档:
  - 全量修改: `PUT /索引库名/_doc/文档id {json文档}`
  - 增量修改: `POST /索引库名/_update/文档id {"doc": {"字段": "值"}}`

### 批量处理

#### 批量处理

ElasticSearch中允许通过一次请求中携带多次文档操作, 也就是批量处理, 语法格式如下:

```json
POST /_bulk
# 批量新增文档
{"index": {"_index": "索引库名", "_id": "1"}}
{"字段1": "值1", "字段2": "值2"}
{"index": {"_index": "索引库名", "_id": "1"}}
{"字段1": "值1", "字段2": "值2"}
{"index": {"_index": "索引库名", "_id": "1"}}
{"字段1": "值1", "字段2": "值2"}
# 删除
{"delete": {"_index": "test", "_id": "2"}}
# 修改
{"update": {"_id": "1", "_index": "test"}}
{"doc": {"field2": "value2"}}
```

> 示例

```json
# 批量处理
POST /_bulk
{"index": {"_index": "qqzj", "_id": "3"}}
{"info": "aaaa", "email": "dajlfj", "age": 22, "name": {"firstName": "a", "lastName": "b"}}
{"index": {"_index": "qqzj", "_id": "4"}}
{"info": "bbbb", "email": "adsfasd", "age": 33, "name": {"firstName": "c", "lastName": "d"}}
{"index": {"_index": "qqzj", "_id": "5"}}
{"info": "cccc", "email": "agwef", "age": 44, "name": {"firstName": "e", "lastName": "f"}}
{"delete": {"_index": "qqzj", "_id": "2"}}
{"update": {"_id": "1", "_index": "qqzj"}}
{"doc": {"age": "66"}}
```

> 返回结果

```json
{
  "took" : 27,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "qqzj",
        "_type" : "_doc",
        "_id" : "3",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 6,
        "_primary_term" : 3,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "qqzj",
        "_type" : "_doc",
        "_id" : "4",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 7,
        "_primary_term" : 3,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "qqzj",
        "_type" : "_doc",
        "_id" : "5",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 8,
        "_primary_term" : 3,
        "status" : 201
      }
    },
    {
      "delete" : {
        "_index" : "qqzj",
        "_type" : "_doc",
        "_id" : "2",
        "_version" : 2,
        "result" : "deleted",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 9,
        "_primary_term" : 3,
        "status" : 200
      }
    },
    {
      "update" : {
        "_index" : "qqzj",
        "_type" : "_doc",
        "_id" : "1",
        "_version" : 4,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 10,
        "_primary_term" : 3,
        "status" : 200
      }
    }
  ]
}
```

## JavaRestClient

### JavaRestClient

ElasticSearch目前最新版本是8.0, 其Java客户端有很大变化. 不过大多数企业使用的还是8以下版本, 所以我们选择使用早期的JavaRestClient客户端来学习. 官方文档地址: https://www.elastic.co/guide/en/elasticsearch/client/index.html

![image-20240727190127635](./图片/image-20240727190127635.png)

### 客户端初始化

1. 引入es的RestHighLevelClient依赖:

   ```xml
   <dependency>
       <groupId>org.elasticsearch.client</groupId>
       <artifactId>elasticsearch-rest-high-level-client</artifactId>
   </dependency>
   ```

2. 因为SpringBoot默认的ES版本是7.17.0, 所以我们需要覆盖默认的ES版本:

   ```xml
   <properties>
       <elasticsearch.version>7.12.1</elasticsearch.version>
   </properties>
   ```

3. 初始化RestHighLevelClient:

   ```java
   RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(
       HttpHost.create("http://192.168.11.128:9200")
   ));
   ```

> 测试类代码

```java
public class ElasticTest {
    private RestHighLevelClient client;

    @Test
    void testConnection() {
        System.out.println("client: " + client);
    }

    @BeforeEach
    void setUp() {
        client = new RestHighLevelClient(RestClient.builder(
                HttpHost.create("http://192.168.11.128:9200")
        ));
    }

    @AfterEach
    void tearDown() throws IOException {
        if (client != null){
            client.close();
        }
    }
}
```

### 商品Mapping映射

我们要实现商品搜索, 那么索引库的字段肯定要满足页面搜索的需求:

![image-20240727192107463](./图片/image-20240727192107463.png)

![image-20240727192711716](./图片/image-20240727192711716.png)

```json
PUT /hmall
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name": {
        "type": "text",
        "analyzer": "ik_smart"
      },
      "price": {
        "type": "integer"
      }, 
      "image":{
        "type": "keyword",
        "index": false
      },
      "category":{
        "type": "keyword"
      },
      "brand":{
        "type": "keyword"
      },
      "sold":{
        "type": "integer"
      },
      "commentCount":{
        "type": "integer",
        "index": false
      },
      "isAD":{
        "type": "boolean"
      },
      "updataTime":{
        "type": "date"
      }
    }
  }
}
```

### 索引库操作

#### 增

创建索引库的JavaAPI与Restful接口API对比:

> Restful接口API

![image-20240728064436821](./图片/image-20240728064436821.png)

> JavaAPI

```java
@Test
void testCreateHoteIndex() throws IOException{
    // 1.创建Request对象
    CreateIndexRequest request = new CreateIndexRequest("items");
    // 2.请求参数, MAPPING_TEMPLATE是静态常量字符串, 内容是JSON格式请求体
    request.source(MAPPING_TEMPLATE, XContentType.JSON);
    // 3.发起请求
    client.indices().create(request, RequestOptions.DEFAULT);
}
```

![image-20240728065241176](./图片/image-20240728065241176.png)

#### 删

删除索引库:

```java
@Test
void testDeleteHoteIndex() throws IOException{
    // 1.创建Request对象
    DeleteIndexRequest request = new DeleteIndexRequest("indexName");
    // 2.发起请求
    client.indices().delete(request, RequestOptions.DEFAULT);
}
```

#### 查

查询索引库信息:

```java
@Test
void testExistsHoteIndex() throws IOException{
    // 1.创建Request对象
    GetIndexRequest request = new GetIndexRequest("indexName");
    // 2.发起请求
    client.indices().get(request, RequestOptions.DEFAULT);
}
```

#### 总结

索引库操作的基本步骤:

1. 创建`XxxIndexRequest`. Xxx可以是`Create`、`Get`、`Delete`
2. 准备请求参数(`Create`时需要)
3. 发送请求. 调用`RestHighLevelClient#indices().xxx()`方法, xxx可以是`create`、`get`/`exists`、`delete`

### 文档操作

#### 增

新增文档的JavaAPI如下:

```java
@Test
void testIndexDocument() throws IOException {
    // 1.创建Request对象
    IndexRequest request = new IndexRequest("indexName").id("1");
    // 2.准备JSON文档
    request.source("{\"name\": \"Jack\", \"age\": 21}", XContentType.JSON);
    // 3.发送请求
    client.index(request, RequestOptions.DEFAULT);
}
```

![image-20240728084530037](./图片/image-20240728084530037.png)

> 示例

```java
@Test
void testIndexDocument() throws IOException {
    // 0.准备文档数据
    Item item = itemService.getById(317578L);
    ItemDoc itemDoc = BeanUtil.copyProperties(item, ItemDoc.class);
    String json = JSONUtil.toJsonStr(itemDoc);
    // 1.创建Request对象
    IndexRequest request = new IndexRequest("items").id(itemDoc.getId());
    // 2.准备JSON文档
    request.source(json, XContentType.JSON);
    // 3.发送请求
    client.index(request, RequestOptions.DEFAULT);
}
```

> itemDoc

```java
@Data
@ApiModel(description = "索引库实体")
public class ItemDoc{

    @ApiModelProperty("商品id")
    private String id;

    @ApiModelProperty("商品名称")
    private String name;

    @ApiModelProperty("价格（分）")
    private Integer price;

    @ApiModelProperty("商品图片")
    private String image;

    @ApiModelProperty("类目名称")
    private String category;

    @ApiModelProperty("品牌名称")
    private String brand;

    @ApiModelProperty("销量")
    private Integer sold;

    @ApiModelProperty("评论数")
    private Integer commentCount;

    @ApiModelProperty("是否是推广广告，true/false")
    private Boolean isAD;

    @ApiModelProperty("更新时间")
    private LocalDateTime updateTime;
}
```

#### 删

删除文档的JavaAPI如下:

```java
@Test
void testIndexDocument() throws IOException {
    // 1.创建Request对象
    DeleteRequest request = new DeleteRequest("indexName", "1");
    // 2.删除文档
    client.delete(request, RequestOptions.DEFAULT);
}
```

> 示例

```java
@Test
void testDeleteDocument() throws IOException {
    // 1.创建Request对象
    DeleteRequest request = new DeleteRequest("items","317578");
    // 2.发送请求
    client.delete(request, RequestOptions.DEFAULT);
}
```

#### 查

查询文档包含查询和解析响应结果两部分. 对应的JavaAPI如下:

```java
@Test
void testIndexDocument() throws IOException {
    // 1.创建Request对象
    GetRequest request = new GetRequest("indexName", "1");
    // 2.发送请求, 得到结果
    GetResponse reponse = client.get(request, RequestOptions.DEFAULT);
    // 3.解析结果
    String json = response.getSourceAsString();
    
    System.out.println(json);
}
```

![image-20240728093405444](./图片/image-20240728093405444.png)

> 示例

```java
@Test
void testGetDocument() throws IOException {
    // 1.创建Request对象
    GetRequest request = new GetRequest("items","317578");
    // 2.发送请求
    GetResponse response = client.get(request, RequestOptions.DEFAULT);
    // 3.解析结果
    String json = response.getSourceAsString();
    ItemDoc itemDoc = JSONUtil.toBean(json, ItemDoc.class);
    System.out.println(itemDoc);
    System.out.println(json);
}
```

> 返回结果

```
ItemDoc(id=317578, name=RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4, price=28900, image=https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp, category=拉杆箱, brand=RIMOWA, sold=0, commentCount=0, isAD=false, updateTime=2023-05-06T11:06:17)
{"id":"317578","name":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":28900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","sold":0,"commentCount":0,"isAD":false,"updateTime":1683342377000}
```

#### 改

修改文档数据有两种方式:

- 方式一: 全量更新. 再次写入id一样的文档, 就会删除旧文档, 添加新文档. 与新增的JavaAPI一致
- 方式二: 局部更新. 只更新指定部分字段

```java
@Test
void testGetDocument() throws IOException {
    // 1.创建Request对象
    UpdateRequest request = new UpdateRequest("items","317578");
    // 2.准备参数
    request.doc(
        "age", 18,
        "name", "Rose"
    );
    // 3.更新文档
    client.update(request, RequestOptions.DEFAULT);
}
```

#### 总结

文档操作的基本步骤:

- 初始化`RestHighLevelClient`
- 创建`XxxRequest`. Xxx可以是`Index`、`Get`、`Update`、`Delete`
- 准备参数(`Index`和`Update`需要)
- 发送请求. 调用`RestHighLevelClient#.xxx()`方法, xxx可以是`index`、`get`、`update`、`delete`
- 解析结果(`Get`时需要)

### 批处理

#### 批处理

批处理代码流程与之前类似, 只不过构建请求会用到一个名为BulkRequest来封装普通的CRUD请求:

![image-20240728100031150](./图片/image-20240728100031150.png)

批处理的API示例:

```java
@Test
void testBulk() throws IOException {
    // 1.创建Bulk请求
    BulkRequest request = new BulkRequest();
    // 2.添加要批量提交的请求: 这里添加了两个新增文档的请求
    request.add(new IndexRequest("indexName")
                .id("101").source("json source1", XContentType.JSON));
    request.add(new IndexRequest("indexName")
                .id("102").source("json source2", XContentType.JSON));
    // 3.发起bulk请求
    client.bulk(request, RequestOptions.DEFAULT);
}
```

> 将商品表所有数据加入到ES中

```java
@Test
void testBulkDocument() throws IOException {
    // 准备数据
    int pageNo = 1;
    int pageSize = 500;
    int count = 0;
    while (true){
        Page<Item> page = itemService.lambdaQuery()
                .eq(Item::getStatus, 1)
                .page(Page.of(pageNo, pageSize));
        List<Item> records = page.getRecords();
        if (records == null || records.isEmpty()) {
            return;
        }
        List<ItemDoc> itemDocs = BeanUtil.copyToList(records, ItemDoc.class);
        // 1.创建Bulk请求
        BulkRequest request = new BulkRequest();
        // 2.添加要批量提交的请求
        for (ItemDoc itemDoc : itemDocs) {
            request.add(new IndexRequest("items").id(itemDoc.getId())
                    .source(JSONUtil.toJsonStr(itemDoc), XContentType.JSON));
            count++;
            System.out.println("count = " + count);
        }

        // 3.发起bulk请求
        client.bulk(request, RequestOptions.DEFAULT);

        pageNo++;
    }
}
```

# ElasticSearch02

## DSL查询

### DSL查询

ElasticSearch提供了DSL(Domain Specific Language)查询, 就是以JSON格式来定义查询条件. 类似这样:

```json
POST _search
{
    "query":{
        "bool":{
            "must":{
                "term": {"user_id": "kimchy"}
            },
            "filter": {
                "term": {"tags": "production"}
            },
            "must_not":{
                "range":{"age": {"gte":10, "lte": 20}}
            },
            "should":[
                {"term": {"tags": "env1"}},
        		{"term": {"tags": "deployed"}}
            ]
        }
    }
}
```

DSL查询可以分为两大类:

- **叶子查询(Leaf query Clauses)**: 一般是在特定的字段里查询特定值, 属于简单查询, 很少单独使用.
- **复合查询(Compound query clauses)**: 以逻辑方式组合多个叶子查询或者更改叶子查询的行为方式

在查询以后, 还可以对查询的结果做处理, 包括:

- **排序**: 按照一个或多个字段值做排序
- **分页**: 根据from和size做分页, 类似MySQL
- **高亮**: 对搜索结果中的关键字添加特殊样式, 使其更加醒目
- **聚合**: 对搜索结果做数据统计以形成报表

### 快速入门

基于DSL的查询语法如下:

```elm
GET /indexName/_search
{
    "query": {
        "查询类型": {
            "查询条件": "条件值"
        }
    }
}
```

> 示例

```elm
# 查询所有
GET /items/_search
{
    "query": {
        "match_all": {
        }
    }
}
```

> 响应结果

```elm
{
  "took" : 35,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "317578",
        "_score" : 1.0,
        "_source" : {
          "id" : "317578",
          "name" : "RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4",
          "price" : 28900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "RIMOWA",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1683342377000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "317580",
        "_score" : 1.0,
        "_source" : {
          "id" : "317580",
          "name" : "RIMOWA 26寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4",
          "price" : 28600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "RIMOWA",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1696644279000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "546872",
        "_score" : 1.0,
        "_source" : {
          "id" : "546872",
          "name" : "博兿（BOYI）拉杆包男23英寸大容量旅行包户外手提休闲拉杆袋 BY09186黑灰色",
          "price" : 27500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t3301/221/3887995271/90563/bf2cadb/57f9fbf4N8e47c225.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "博兿",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "561178",
        "_score" : 1.0,
        "_source" : {
          "id" : "561178",
          "name" : "RIMOWA 30寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4",
          "price" : 13000,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "RIMOWA",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1696644294000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "577967",
        "_score" : 1.0,
        "_source" : {
          "id" : "577967",
          "name" : "莎米特SUMMIT 旅行拉杆箱28英寸PC材质大容量旅行行李箱PC154 黑色",
          "price" : 71300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t30454/163/719393962/79149/13bcc06a/5bfca9b6N493202d2.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "莎米特",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "584382",
        "_score" : 1.0,
        "_source" : {
          "id" : "584382",
          "name" : "美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 25英寸海关锁DL7灰色",
          "price" : 36600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "584387",
        "_score" : 1.0,
        "_source" : {
          "id" : "584387",
          "name" : "美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 29英寸海关锁DL7灰色",
          "price" : 16200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "584391",
        "_score" : 1.0,
        "_source" : {
          "id" : "584391",
          "name" : "美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 20英寸海关锁DL7灰色",
          "price" : 29900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "584392",
        "_score" : 1.0,
        "_source" : {
          "id" : "584392",
          "name" : "美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 29英寸海关锁DL7灰色",
          "price" : 17000,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1696644299000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "584394",
        "_score" : 1.0,
        "_source" : {
          "id" : "584394",
          "name" : "美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 25英寸海关锁DL7灰色",
          "price" : 79400,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}

```

返回值解析:

- `took`: 本次操作耗时

- `timed_out`: 是否超时

- `_shards`: 分片信息

- `hits`: 命中的数据

  - `total`: 总条数
    - `value`: 总条数的值(ES默认单次查询最大数据上限为10000条)
    - `relation`: 关系(`gte`: 大于等于)

  - `max_score`: 得分, 相关度得分
  - `hits`: 真正的查询命中的数据(默认只返回10条文档数据)

### 叶子查询

#### 叶子查询

叶子查询还可以进一步细分, 常见的有:

- **全文检索(full text)**查询: 利用分词器对用户输入内容分词, 然后去词条列表中匹配. 例如:
  - `match_query`
  - `multi_match_query`

- **精确查询**: 不对用户输入内容分词, 直接精确匹配, 一般是查找keyword、数值、日期、布尔等类型. 例如:
  - `ids`(文档id比较)
  - `range`(范围比较)
  - `term`(字段值比较)
- **地理(geo)查询**: 用于搜索地理位置, 搜索方式很多. 例如:
  - `geo_distance`
  - `geo_bounding_box`

#### **`match`**查询

##### **`match`**查询

**`match`**查询: 全文检索查询的一种, 会对用户输入内容分词, 然后去倒排索引库检索, 语法:

```elm
GET /indexName/_search
{
    "query": {
    	"match":{
    		"FIELD": "TEXT"
    	}
    }
}
```

##### **`multi_match`**查询

**`multi_match`**: 与`match`查询类似, 只不过允许同时查询多个字段, 语法:

```java
GET /indexName/_search
{
    "query": {
    	"multi_match":{
    		"query": "TEXT"
                "fields":["FIELD1", "FIELD2"]
    	}
    }
}
```

##### 代码示例

```elm
GET /items/_search
{
    "query": {
        "match": {
          "name": "脱脂牛奶"
        }
    }
}
```

> 响应结果

```elm
{
  "took" : 70,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 960,
      "relation" : "eq"
    },
    "max_score" : 15.446136,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "30415947063",
        "_score" : 15.446136,
        "_source" : {
          "id" : "30415947063",
          "name" : "【6灌装两瓶减3元】德国原装进口（Weidendorf）德亚牛奶全脂纯牛奶脱脂牛奶 脱脂牛奶*12罐",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t27379/324/2136309393/332532/38bfbc43/5bf79104N3cec8ead.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "德亚",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "33449279171",
        "_score" : 15.385777,
        "_source" : {
          "id" : "33449279171",
          "name" : "意大利 进口牛奶 葛兰纳诺脱脂纯牛奶 成人牛奶  进口脱脂纯牛奶1Lx6盒",
          "price" : 3500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/25045/9/2656/164517/5c20699dE9b7f4c9c/1a05e9bdd2c5d59e.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "葛兰纳诺",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "12179607155",
        "_score" : 15.108821,
        "_source" : {
          "id" : "12179607155",
          "name" : """【沃尔玛】艾思达/ASDA纯牛奶 灭菌乳 早餐奶 牛奶 英国 进口 全脂\脱脂\部分脱脂 部分脱脂牛奶 1L*6""",
          "price" : 96300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t5977/320/3555243042/134144/38730483/59546920Nbe3ddc70.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "ASDA",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "12179607156",
        "_score" : 15.108821,
        "_source" : {
          "id" : "12179607156",
          "name" : """【沃尔玛】艾思达/ASDA纯牛奶 灭菌乳 早餐奶 牛奶 英国 进口 全脂\脱脂\部分脱脂 部分脱脂牛奶 1L*6""",
          "price" : 80900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t5977/320/3555243042/134144/38730483/59546920Nbe3ddc70.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "ASDA",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10110114392",
        "_score" : 14.971363,
        "_source" : {
          "id" : "10110114392",
          "name" : "1L*2瓶德国进口脱脂牛奶装甘蒂牧场 MUH牧牌牛奶脱脂进口纯牛奶",
          "price" : 29100,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t9958/316/1685860116/137786/e90b8396/59e5b8deN48db5664.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "甘蒂牧场",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "39417141576",
        "_score" : 14.971363,
        "_source" : {
          "id" : "39417141576",
          "name" : "德国进口好沃德脱脂无脂纯牛奶无脂肪牛奶整箱 脱脂牛奶1L12盒",
          "price" : 16500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/7639/1/8833/519114/5c10dad8E8a7e70a0/3b3215a3bc594537.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "萌妖小厨",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10554108383",
        "_score" : 14.929279,
        "_source" : {
          "id" : "10554108383",
          "name" : "德质牛奶德国进口牛奶脱脂牛奶成人高钙纯牛奶490ml（保质期19年9月） 490ml*15瓶/箱（赠脱脂6瓶）",
          "price" : 25600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t17965/337/720431810/159941/375edc2a/5aa23d85Nb612b051.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "德质",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "37227964859",
        "_score" : 14.764732,
        "_source" : {
          "id" : "37227964859",
          "name" : "【6灌装两瓶减3元】德国原装进口（Weidendorf）德亚牛奶全脂纯牛奶脱脂牛奶 脱脂牛奶*6罐（两份立减5元）",
          "price" : 77200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t27379/324/2136309393/332532/38bfbc43/5bf79104N3cec8ead.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "德亚",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "37227964860",
        "_score" : 14.764732,
        "_source" : {
          "id" : "37227964860",
          "name" : "【6灌装两瓶减3元】德国原装进口（Weidendorf）德亚牛奶全脂纯牛奶脱脂牛奶 脱脂牛奶*6罐（两份立减5元）",
          "price" : 31000,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t27379/324/2136309393/332532/38bfbc43/5bf79104N3cec8ead.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "德亚",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "37228712324",
        "_score" : 14.578689,
        "_source" : {
          "id" : "37228712324",
          "name" : "【包邮】德国进口Weidendorf德亚脱脂牛奶200ml*12盒进口早餐纯牛奶全脂 脱脂牛奶200ml*12盒",
          "price" : 29300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t30757/169/501371318/588248/ddbb022e/5bf4ef7bNe22a28fa.jpg!q70.jpg.webp",
          "category" : "牛奶",
          "brand" : "德亚",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}
```

<font color='red'>默认情况下, 根据匹配度排序</font>

#### 精确查询

精确查询, 英文是Term-level query, 顾名思义, 词条级别的查询. 也就是说不会对用户输入的搜索条件再分词, 而是作为一个词条, 与搜索的字段内容精确值匹配

因此推荐查找keyword、数值、日期、boolean类型的字段. 例如id、price、城市、地名、人名等作为一个整体才有含义的字段

##### term查询

```elm
// term查询
GET /indexName/_search
{
	"query":{
		"term":{
			"FIELD": {
				"value": "VALUE"
			}
		}
	}
}
```

> 示例

```elm
# term查询
GET /items/_search
{
  "query": {
    "term": {
      "price": {
        "value": "75800"
      }
    }
  }
}
```

> 返回结果

```elm
{
  "took" : 27,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 96,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "1780518",
        "_score" : 1.0,
        "_source" : {
          "id" : "1780518",
          "name" : "美旅AmericanTourister拉杆箱 休闲商务男女行李箱波浪纹时尚万向轮旅行箱 26英寸I20TSA海关锁玫红色",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t3700/35/474364855/272108/dcac53b2/5809f311Ne05cc27c.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "美旅箱包",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "5900003",
        "_score" : 1.0,
        "_source" : {
          "id" : "5900003",
          "name" : "莎米特SUMMIT拉杆箱18英寸PC材质万向轮旅行箱行李箱PC154T4A可扩容 紫色",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/23976/35/2907/274060/5c21e0aaEd5c3811f/3fe82f38ce55a6c5.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "莎米特",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "7582287",
        "_score" : 1.0,
        "_source" : {
          "id" : "7582287",
          "name" : "【新秀丽旗下】卡米龙Mapuna系列波点硬箱万向轮拉杆箱箱子密码箱行李箱女旅行箱AM6*60006红色24英寸",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t5545/3/867905746/103331/e69683a8/59083e7cNc14abb89.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "Kamiliant",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "1318817977",
        "_score" : 1.0,
        "_source" : {
          "id" : "1318817977",
          "name" : "OCSEE 时尚老花镜 舒适树脂 可配防蓝光老花镜眼镜美学花镜 白框黑腿+防蓝光镜片 老花300度",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t2695/284/3965832634/313034/d7acba88/57a418bdNebb53ab5.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "欧可眼镜",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "1417663821",
        "_score" : 1.0,
        "_source" : {
          "id" : "1417663821",
          "name" : "老爷子 老花镜男 时尚超轻老花眼镜 抗疲劳单光蛤蟆镜 7011 金框单光 350度（70岁以上）",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4138/5/2827/257241/5b9786e5Ee1b9b843/f07e19bdd17ac580.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "老爷子",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "1497354276",
        "_score" : 1.0,
        "_source" : {
          "id" : "1497354276",
          "name" : "捷纯女裤 春季九分牛仔裤女小脚高腰弹力显瘦铅笔裤 0027 靛蓝 25码",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t18703/303/2563183674/237524/2643f79f/5af6dcc7N9f62742e.jpg!q70.jpg.webp",
          "category" : "牛仔裤",
          "brand" : "捷纯jettrain",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10126359876",
        "_score" : 1.0,
        "_source" : {
          "id" : "10126359876",
          "name" : "人本帆布鞋一脚蹬帆布鞋女百搭小白鞋学生韩版内增高鞋子 白色 36",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/19124/26/3895/278667/5c2c85a8E2e36ab1f/10c112632266ec35.png!q70.jpg.webp",
          "category" : "休闲鞋",
          "brand" : "人本",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10235390810",
        "_score" : 1.0,
        "_source" : {
          "id" : "10235390810",
          "name" : "【一体化铝框 10万好评】EAZZ铝框拉杆箱万向轮行李箱男女士登机箱20/24寸29寸旅行箱图案定制 高端 铝框-银色 29英寸-全球飞",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/23200/1/3983/258649/5c2ce2fcEaa56d39d/6ff65b9420f2813f.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "EAZZ",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10395445763",
        "_score" : 1.0,
        "_source" : {
          "id" : "10395445763",
          "name" : "班亚奴新款H扣女士短款小钱包鳄鱼纹女式牛漆皮三折钱夹正品特价 红色",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t2224/74/1560332799/238464/81fac6c8/5665afeaN8960638e.jpg!q70.jpg.webp",
          "category" : "真皮包",
          "brand" : "班亚奴",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "10423176275",
        "_score" : 1.0,
        "_source" : {
          "id" : "10423176275",
          "name" : "卡片式老花镜 男女时尚迷你老花眼镜 可放钱包的超薄高清树脂花镜 便携夹鼻眼镜无腿 浅蓝 350度",
          "price" : 75800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t2665/47/2326530414/95504/e26442ff/5762679dN5fb48d22.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "冠豪",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}

```

##### range查询

```json
GET /indexName/_search
{
	"query":{
        "range":{
            "FIELD":{
                "gte": 10,
                "lte": 20
            }
        }
    }	
}
```

> 示例

```java
GET /items/_search
{
	"query":{
        "range":{
            "price":{
                "gte": 500000,
                "lte": 600000
            }
        }
    }	
}
```

> 返回结果

```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "5706773",
        "_score" : 1.0,
        "_source" : {
          "id" : "5706773",
          "name" : "华为 HUAWEI Mate 10 Pro 全面屏徕卡双摄游戏手机 6GB+128GB 银钻灰 全网通移动联通电信4G手机 双卡双待",
          "price" : 544000,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t11986/295/1484411523/155164/77795126/5a01503cN19d7f1a0.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1696644986000
        }
      }
    ]
  }
}
```

##### ids查询

```json
GET /indexName/_search
{
	"query":{
        "ids":{
            "values": ["613360",...]
        }
    }	
}
```

> 代码示例

```java
GET /items/_search
{
	"query":{
        "ids":{
            "values": ["613360"]
        }
    }	
}
```

> 响应结果

```json
{
  "took" : 23,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "613360",
        "_score" : 1.0,
        "_source" : {
          "id" : "613360",
          "name" : "莎米特SUMMIT 旅行拉杆箱28英寸PC材质大容量旅行行李箱PC154 黑色",
          "price" : 88000,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t30454/163/719393962/79149/13bcc06a/5bfca9b6N493202d2.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "莎米特",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}
```

#### 总结

`match`和`multi_match`的区别是什么?

- `match`: 根据一个字段查询
- `multi_match`: 根据多个字段查询, 参与查询字段越多, 查询性能越差

精确查询常见的有哪些?

- `term`查询: 根据词条精确匹配, 一般搜索keyword类型、数值类型、布尔类型、日期类型字段
- `range`查询: 根据数值范围查询, 可以是数值、日期的范围

### 复合查询

#### 复合查询

复合查询大致可以分为两类:

- 第一类: 基于逻辑运算组合叶子查询, 实现组合条件, 例如
  - `bool`
- 第二类: 基于某种算法修改查询时的文档相关性算分, 从而改变文档排名. 例如:
  - `function_score`
  - `dis_max`

#### `bool`查询

布尔查询是一个或多个查询子句的组合. 子查询的组合方式有:

- `must`: 必须匹配每个子查询, 类似“与”
- `should`: 选择性匹配子查询, 类似“或”
- `must_not`: 必须不匹配, 不参与算分, 类似“非”
- `filter`: 必须匹配, 不参与算分

> 示例代码

```json
GET /items/_search
{
  "query": {
    "bool": {
      "must": [
        {
            "match": {
          		"name": "手机"
        	}
        }
      ],
      "should": [
        { 
            "term": {
            	"brand": {
              		"value": "vivo"
            	}
          	}
        },
        {
          "term": {
            "brand": {
              "value": "小米"
            }
          }
        }
      ],
      "must_not": [
        {
          "range": {
            "price": {
              "gte": 2500
            }
          }
        }
      ],
      "filter": [
        {
          "range": {
            "price": {
              "lte": 1000
            }
          }
        }
      ]
    }
  }
}
```

> 响应结果

```json
{
  "took" : 71,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 113,
      "relation" : "eq"
    },
    "max_score" : 9.455136,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "8929561",
        "_score" : 9.455136,
        "_source" : {
          "id" : "8929561",
          "name" : "vivo X23 8GB+128GB 幻夜蓝 水滴屏全面屏 游戏手机 移动联通电信全网通4G手机",
          "price" : 0,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4612/28/6223/298257/5ba22d66Ef665222f/d97ed0b25cbe8c6e.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000183995",
        "_score" : 8.940575,
        "_source" : {
          "id" : "100000183995",
          "name" : "vivo Y81s 刘海全面屏 3GB+32GB 宝石红 移动联通电信4G手机",
          "price" : 0,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t20707/78/2349564629/130172/50a245d8/5b8e00e2Nf0bcd624.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "29244755574",
        "_score" : 8.57801,
        "_source" : {
          "id" : "29244755574",
          "name" : "vivo 【直降200】 Z1 新一代全面屏 双摄拍照手机 双卡双待 6GB+128GB 炫慕红",
          "price" : 100,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t22237/77/1169226319/175156/2dc4fbd1/5b20e599N03a7ce95.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "37597861533",
        "_score" : 8.336464,
        "_source" : {
          "id" : "37597861533",
          "name" : "vivo【直降100 领券减100】Z3 高通骁龙处理器 水滴全面屏 双摄拍照 游戏手机 极光蓝 6GB  64GB",
          "price" : 100,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t26914/306/1441231834/224805/20c2401e/5bc836acN8c2283a1.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "11265588270",
        "_score" : 6.915372,
        "_source" : {
          "id" : "11265588270",
          "name" : "小米（MI） 红米6A手机 樱花粉 全网通3G+32G",
          "price" : 200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t25285/170/1316599249/308687/5565c5f0/5b91fc58N9294a591.png!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "27598476417",
        "_score" : 6.915372,
        "_source" : {
          "id" : "27598476417",
          "name" : "小米（MI） 小米6X 手机 曜石黑 全网通 6G+128G",
          "price" : 900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t19318/352/1988594765/335794/c80dd3e5/5ae03df7N49e8e69d.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "28758228019",
        "_score" : 6.915372,
        "_source" : {
          "id" : "28758228019",
          "name" : "小米（MI） 小米8 手机 全网通 白色 全网通 6GB+128GB",
          "price" : 700,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t21235/356/699605352/211020/29d5b968/5b163223Nc89a265b.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "29207021958",
        "_score" : 6.915372,
        "_source" : {
          "id" : "29207021958",
          "name" : "小米（MI） 红米6A 手机 铂银灰 全网通(2G+16G)",
          "price" : 900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t23590/129/1424596003/352447/d39de63a/5b600b11N565c62a9.png!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "40470084749",
        "_score" : 6.915372,
        "_source" : {
          "id" : "40470084749",
          "name" : "小米（MI） 小米play 手机 梦幻蓝 6GB 128GB 全网通",
          "price" : 600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/6237/8/9218/73512/5c209758E9f1502c4/0ebcfb80aece10f8.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "29225133570",
        "_score" : 6.851416,
        "_source" : {
          "id" : "29225133570",
          "name" : "小米（MI） 红米6A 老人手机 樱花粉 2GB+16GB全网通",
          "price" : 300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t21550/281/1117076166/137040/dc431925/5b207b63N88e4a19b.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "小米",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}
```

#### 案例

需求: 我们要搜索“智能手机”, 但品牌必须是华为, 价格必须是900~1599

```json
GET /items/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "智能手机"
          }
        }
      ],
      "filter": [
        {
          "term": {
            "brand": "华为"
          }
        },
        {
          "range": {
            "price": {
              "gte": 900,
              "lte": 1599
            }
          }
        }
      ]
    }
  }
}
```

> 响应结果

```json
{
  "took" : 34,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 5.428427,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "14711554436",
        "_score" : 5.428427,
        "_source" : {
          "id" : "14711554436",
          "name" : "华为（HUAWEI） 华为 P10 Plus 全网通4G智能手机 曜石黑 6G+128G",
          "price" : 1200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t16873/258/2086056924/389581/6eb74d9c/5ae52af6N7e455833.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "28923730975",
        "_score" : 4.529688,
        "_source" : {
          "id" : "28923730975",
          "name" : "华为（HUAWEI） 荣耀V9 全网通 移动联通电信4G 双卡双待 智能手机 幻影黑 移动全网通4G【6GB+64GB】",
          "price" : 1100,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t20608/118/692564950/137597/1fd81096/5b160e11N5be56c0b.png!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34440585127",
        "_score" : 4.368948,
        "_source" : {
          "id" : "34440585127",
          "name" : "华为（HUAWEI） P9 全网通4G 移动联通电信4G手机 智能手机 双卡双待双通指纹解锁 金色 全网通 4GB+64GB",
          "price" : 1500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/5932/18/3236/95801/5b98b6a5Ee5aae64a/fbefe2ff8d9c9aee.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "40151919328",
        "_score" : 4.1481476,
        "_source" : {
          "id" : "40151919328",
          "name" : "华为（HUAWEI） 荣耀 畅玩7X 移动联通电信 全网通4G 全面屏智能手机 双卡双待 极光蓝 高配版（4G RAM+64G ROM）",
          "price" : 1400,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/20509/21/3503/102759/5c275f04E7b72e389/1af22ee29fe4051e.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        }
      }
    ]
  }
}
```

### 排序和分页

#### 排序

ElasticSearch支持对搜索结果排序, 默认是根据相关度算分(_score)来排序. 也可以指定字段排序. 可以排序字段类型有: keyword类型、数值类型、地理坐标类型、日期类型等.

语法:

```json
GET /indexName/_search
{
    "query":{
        "match_all": {}
    },
    "sort":[
        {
            "FIELD": "desc" //排序字段和排序方式ASC、DESC
        }
    ]
}
```

#### 案例

需求: 搜索商品, 按照销量排序, 销量一样则按照价格升序

```json
GET /items/_search
{
  "query": {
    "match_all": {}
  }
  , "sort": [
    {
      "sold": {
        "order": "desc"
      }
    },
    {
      "price": {
        "order": "asc"
      }
    }
  ]
}
```

> 响应结果

```java
{
  "took" : 15,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000011127",
        "_score" : null,
        "_source" : {
          "id" : "100000011127",
          "name" : "莎米特SUMMIT拉杆箱22英寸PC材质万向轮旅行箱行李箱PC154T4A可扩容 米白",
          "price" : 26600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/25363/12/2929/274060/5c21df3aE1789bda7/030af31afd116ae0.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "莎米特",
          "sold" : 45454,
          "commentCount" : 233324,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          45454,
          26600
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000004580",
        "_score" : null,
        "_source" : {
          "id" : "100000004580",
          "name" : "薇妮(viney)女士单肩包 时尚牛皮女包百搭斜挎包女士手提大包(经典黑)",
          "price" : 87900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t5590/64/5811657380/234462/5398e856/5965e173N34179777.jpg!q70.jpg.webp",
          "category" : "真皮包",
          "brand" : "viney",
          "sold" : 22233,
          "commentCount" : 2223232,
          "isAD" : false,
          "updateTime" : 1721967806000
        },
        "sort" : [
          22233,
          87900
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000003145",
        "_score" : null,
        "_source" : {
          "id" : "100000003145",
          "name" : "vivo X23 8GB+128GB 幻夜蓝 水滴屏全面屏 游戏手机 移动联通电信全网通4G手机",
          "price" : 95900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4612/28/6223/298257/5ba22d66Ef665222f/d97ed0b25cbe8c6e.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 11212,
          "commentCount" : 1231312,
          "isAD" : true,
          "updateTime" : 1556640000000
        },
        "sort" : [
          11212,
          95900
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000015166",
        "_score" : null,
        "_source" : {
          "id" : "100000015166",
          "name" : "华为 HUAWEI 麦芒7 6G+64G 亮黑色 全网通  前置智慧双摄  移动联通电信4G手机 双卡双待",
          "price" : 89200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t22642/312/2563982615/103706/1398b13d/5b865bb3N0409f0d0.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 6765,
          "commentCount" : 12,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          6765,
          89200
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030628",
        "_score" : null,
        "_source" : {
          "id" : "100000030628",
          "name" : "ARNO防蓝光老花镜女 远近两用智能自动变焦渐进多焦点老光眼镜A1005 300度 亮紫色",
          "price" : 11300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t25867/102/1274759920/233086/96f7fc46/5b909851N02396afd.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 4545,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4545,
          11300
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000015158",
        "_score" : null,
        "_source" : {
          "id" : "100000015158",
          "name" : "华为 HUAWEI 麦芒7 6G+64G 魅海蓝 全网通  前置智慧双摄  移动联通电信4G手机 双卡双待",
          "price" : 40800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t22642/312/2563982615/103706/1398b13d/5b865bb3N0409f0d0.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 4544,
          "commentCount" : 221,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4544,
          40800
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030800",
        "_score" : null,
        "_source" : {
          "id" : "100000030800",
          "name" : "ARNO渐进焦老花镜女 远近两用防蓝光优雅时尚老光眼镜A1007 100度 银间黑",
          "price" : 30500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t25957/363/1290162041/159777/41fe0e72/5b908f41N4884b765.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 4454,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4454,
          30500
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030632",
        "_score" : null,
        "_source" : {
          "id" : "100000030632",
          "name" : "ARNO防蓝光老花镜男 远近两用智能自动变焦渐进多焦点老光眼镜A1006 150度 亮黑色",
          "price" : 75300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4364/38/240/219619/5b909979E13d29469/7c908af4d61cf659.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 2132,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          2132,
          75300
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030074",
        "_score" : null,
        "_source" : {
          "id" : "100000030074",
          "name" : "夕阳红防蓝光老花镜男女通用款 清新灰色时尚大框镜架不易折断老花眼镜E9004G 200度 灰色",
          "price" : 99800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t26281/196/340140952/94362/c353173/5b8f6e63N56334c08.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "夕阳红",
          "sold" : 676,
          "commentCount" : 343,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          676,
          99800
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030942",
        "_score" : null,
        "_source" : {
          "id" : "100000030942",
          "name" : "ARNO防蓝光老花镜男 远近两用智能自动变焦舒适老光眼镜A1008 200度 枪间黑",
          "price" : 14500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t27001/327/415973578/197711/48dba286/5b908080Ncedd171b.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 665,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          665,
          14500
        ]
      }
    ]
  }
}
```

#### 分页

ElasticSearch默认情况下只返回top10的数据. 而如果要查询更多数据就需要修改分页参数了. ElasticSearch中通过修改from、size参数来控制要返回的分页结果:

- `from`: 从第几个文档开始
- `size`: 总共查询几个文档

语法:

```json
GET /indexName/_search
{
    "query":{
        "match_all": {}
    },
    "from": 0, //分页开始的位置, 默认为0
    "size": 10, //期望获取的文档总数
    "sort":[
        {
            "FIELD": "desc" //排序字段和排序方式ASC、DESC
        }
    ]
}
```

#### 案例

需求: 搜索商品, 查询出销量排名前十的商品, 销量一样时按照价格升序

```java
GET /items/_search
{
    "query":{
        "match_all": {}
    },
    "from": 0, 
    "size": 10, 
    "sort":[
      {
          "sold": "desc" 
      },
      {
        "price": "asc"
      }
    ]
}
```

> 响应结果

```java
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000011127",
        "_score" : null,
        "_source" : {
          "id" : "100000011127",
          "name" : "莎米特SUMMIT拉杆箱22英寸PC材质万向轮旅行箱行李箱PC154T4A可扩容 米白",
          "price" : 26600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/25363/12/2929/274060/5c21df3aE1789bda7/030af31afd116ae0.jpg!q70.jpg.webp",
          "category" : "拉杆箱",
          "brand" : "莎米特",
          "sold" : 45454,
          "commentCount" : 233324,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          45454,
          26600
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000004580",
        "_score" : null,
        "_source" : {
          "id" : "100000004580",
          "name" : "薇妮(viney)女士单肩包 时尚牛皮女包百搭斜挎包女士手提大包(经典黑)",
          "price" : 87900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t5590/64/5811657380/234462/5398e856/5965e173N34179777.jpg!q70.jpg.webp",
          "category" : "真皮包",
          "brand" : "viney",
          "sold" : 22233,
          "commentCount" : 2223232,
          "isAD" : false,
          "updateTime" : 1721967806000
        },
        "sort" : [
          22233,
          87900
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000003145",
        "_score" : null,
        "_source" : {
          "id" : "100000003145",
          "name" : "vivo X23 8GB+128GB 幻夜蓝 水滴屏全面屏 游戏手机 移动联通电信全网通4G手机",
          "price" : 95900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4612/28/6223/298257/5ba22d66Ef665222f/d97ed0b25cbe8c6e.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "vivo",
          "sold" : 11212,
          "commentCount" : 1231312,
          "isAD" : true,
          "updateTime" : 1556640000000
        },
        "sort" : [
          11212,
          95900
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000015166",
        "_score" : null,
        "_source" : {
          "id" : "100000015166",
          "name" : "华为 HUAWEI 麦芒7 6G+64G 亮黑色 全网通  前置智慧双摄  移动联通电信4G手机 双卡双待",
          "price" : 89200,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t22642/312/2563982615/103706/1398b13d/5b865bb3N0409f0d0.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 6765,
          "commentCount" : 12,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          6765,
          89200
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030628",
        "_score" : null,
        "_source" : {
          "id" : "100000030628",
          "name" : "ARNO防蓝光老花镜女 远近两用智能自动变焦渐进多焦点老光眼镜A1005 300度 亮紫色",
          "price" : 11300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t25867/102/1274759920/233086/96f7fc46/5b909851N02396afd.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 4545,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4545,
          11300
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000015158",
        "_score" : null,
        "_source" : {
          "id" : "100000015158",
          "name" : "华为 HUAWEI 麦芒7 6G+64G 魅海蓝 全网通  前置智慧双摄  移动联通电信4G手机 双卡双待",
          "price" : 40800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t22642/312/2563982615/103706/1398b13d/5b865bb3N0409f0d0.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "华为",
          "sold" : 4544,
          "commentCount" : 221,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4544,
          40800
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030800",
        "_score" : null,
        "_source" : {
          "id" : "100000030800",
          "name" : "ARNO渐进焦老花镜女 远近两用防蓝光优雅时尚老光眼镜A1007 100度 银间黑",
          "price" : 30500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t25957/363/1290162041/159777/41fe0e72/5b908f41N4884b765.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 4454,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          4454,
          30500
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030632",
        "_score" : null,
        "_source" : {
          "id" : "100000030632",
          "name" : "ARNO防蓝光老花镜男 远近两用智能自动变焦渐进多焦点老光眼镜A1006 150度 亮黑色",
          "price" : 75300,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/4364/38/240/219619/5b909979E13d29469/7c908af4d61cf659.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 2132,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          2132,
          75300
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030074",
        "_score" : null,
        "_source" : {
          "id" : "100000030074",
          "name" : "夕阳红防蓝光老花镜男女通用款 清新灰色时尚大框镜架不易折断老花眼镜E9004G 200度 灰色",
          "price" : 99800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t26281/196/340140952/94362/c353173/5b8f6e63N56334c08.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "夕阳红",
          "sold" : 676,
          "commentCount" : 343,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          676,
          99800
        ]
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "100000030942",
        "_score" : null,
        "_source" : {
          "id" : "100000030942",
          "name" : "ARNO防蓝光老花镜男 远近两用智能自动变焦舒适老光眼镜A1008 200度 枪间黑",
          "price" : 14500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t27001/327/415973578/197711/48dba286/5b908080Ncedd171b.jpg!q70.jpg.webp",
          "category" : "老花镜",
          "brand" : "ARNO FOCUS ON YOUR EYES",
          "sold" : 665,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "sort" : [
          665,
          14500
        ]
      }
    ]
  }
}
```

### 深度分页问题

#### 深度分页问题

ElasticSearch的数据一般会采用分片存储, 也就是把一个索引库中的数据分成N份, 存储到不同节点上. 查询数据时需要汇总各个分片的数据.

![image-20240728162214469](./图片/image-20240728162214469.png)

假如我们要查询第100页数据, 每页查10条:

```json
GET /hotel/_search
{
    "from": 990,
    "size": 10
}
```

实现思路:

1. 对数据排序(**筛选出每个分片上的前1000名数据, 聚合在一起然后排序得到全部的前1000名数据**)
2. 找出第990~1000名

![image-20240728162746543](./图片/image-20240728162746543.png)

**弊端: 查询的页码越深, 查询的数据量越大, 甚至导致内存崩溃, 降低性能**

针对深度分页, ES提供了两种解决方案, [官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html):

- `search after`: 分页时需要排序, 原理是从上一次的排序值开始, 查询下一页数据. 官方推荐
- `scroll`: 原理将排序数据形成快照, 保存在内存. 官方不推荐

`search after`模式:

- 优点: 没有查询上限, 支持深度分页
- 缺点: 只能向后逐页查询, 不能随机翻页
- 场景: 数据迁移, 手机滚动查询

### 高亮显示

**高亮显示**: 就是在搜索结果中把搜索关键字突出显示

```json
GET /items/_search
{
    "query": {
        "match": {
            "FIELD": "TEXT"
        }
    },
    "highlight": {
        "field": {// 指定要高亮的字段
            "FIELD": {
                "pre_tags": "<em>", // 高亮的前置标签
                "post_tags": "</em>" // 高亮的后置标签
            }
        }
    }
}
```

> 代码示例(标签不加, 默认`<em></em>`)

```json
GET /items/_search
{
  "query": {
    "match": {
      "name": "手机"
    }
  },
  "highlight": {
    "fields": {
      "name": {
        "pre_tags": "<em>",
        "post_tags": "</em>"
      }
    }
  }
}
```

> 响应结果

```json
{
  "took" : 301,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 9359,
      "relation" : "eq"
    },
    "max_score" : 3.604764,
    "hits" : [
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803399300",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803399300",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 64GB",
          "price" : 35800,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 64GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424701",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424701",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 128GB",
          "price" : 80400,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 128GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424702",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424702",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 128GB",
          "price" : 46400,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 128GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424703",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424703",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 256GB",
          "price" : 40900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 256GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424704",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424704",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 64GB",
          "price" : 72600,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 64GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424705",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424705",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 64GB",
          "price" : 15100,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 64GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424706",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424706",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 128GB",
          "price" : 84700,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 128GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424707",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424707",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 256GB",
          "price" : 53900,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 256GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424708",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424708",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 64GB",
          "price" : 63500,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 64GB"
          ]
        }
      },
      {
        "_index" : "items",
        "_type" : "_doc",
        "_id" : "34803424709",
        "_score" : 3.604764,
        "_source" : {
          "id" : "34803424709",
          "name" : "Apple 苹果 iPhone XR 手机 全网通4G手机 黑色 128GB",
          "price" : 91700,
          "image" : "https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/2706/35/9961/98599/5bc940f4Eecc2753b/22432d389910dec2.jpg!q70.jpg.webp",
          "category" : "手机",
          "brand" : "Apple",
          "sold" : 0,
          "commentCount" : 0,
          "isAD" : false,
          "updateTime" : 1556640000000
        },
        "highlight" : {
          "name" : [
            "Apple 苹果 iPhone XR <em>手机</em> 全网通4G<em>手机</em> 黑色 128GB"
          ]
        }
      }
    ]
  }
}
```

### 总结

搜索的完整语法:

```json
GET /items/_search
{
    "query": {
        "match": {
            "name": "华为"
        }
    },
    "from": 0, // 分页开始的位置
    "size": 20, // 期望获取的文档总数
    "sort": [
    	{"price": "asc"} // 普通排序
    ],
    "highlight": { 
        "fields": { //高亮字段
            "name": {
                "pre_tags": "<em>", // 高亮字段的前置标签
                "post_tags": "</em>" //高亮字段的后置标签
            }
        }
    }
}
```

## JavaRestClient查询

### 快速入门

数据搜索的Java代码我们分为两部分:

- 构建并发起请求
- 解析查询结果

#### 查询API

```java
@Test
void testMatchAll() throws IOException {
    // 1.准备Request
    SearchRequest request = new SearchRequest("indexName");
    // 2.组织DSL参数
    request.source()
        .query(QueryBuilders.matchAllQuery());
    // 3.发送请求, 得到响应结果
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    // ...解析响应结果
}
```

![image-20240729062634542](./图片/image-20240729062634542.png)

![image-20240729062707977](./图片/image-20240729062707977.png)![image-20240729062750297](./图片/image-20240729062750297.png)

#### 解析查询结果的API:

```java
@Test
void testMatchAll() throws IOException {
    // ...发送请求获取响应结果
    // 4.解析结果
    SearchHits searchHits = response.getHits();
    // 4.1.查询的总条数
    long total = searchHits.getTotalHits().value;
    // 4.2.查询的结果数组
    SearchHit[] hits = searchHits.getHits();
    for (SearchHit hit : hits) {
        // 4.3.得到source
        String json = hit.getSourceAsString();
        System.out.println(json);
    }
}
```

![image-20240729064303323](./图片/image-20240729064303323.png)

```java
@Test
void testSearchMatchALl() throws IOException {
    // 1.准备Request
    SearchRequest request = new SearchRequest("items");
    // 2.组织DSL参数
    request.source()
            .query(QueryBuilders.matchAllQuery());
    // 3.发送请求, 得到响应结果
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    // 4.解析结果
    SearchHits searchHits = response.getHits();
    // 4.1.查询的总条数
    long value = searchHits.getTotalHits() == null ? 0 : searchHits.getTotalHits().value;
    System.out.println("value = " + value);
    // 4.2.查询的结果数组
    SearchHit[] hits = searchHits.getHits();
    for (SearchHit hit : hits) {
        // 4.3.得到source
        String json = hit.getSourceAsString();
        // 4.4.转换ItemDoc
        //JSONUtil.toBean(json, ItemDoc.class)
        System.out.println("json = " + json);
    }
}
```

> 返回结果

```java
value = 10000
json = {"id":"317578","name":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":28900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","sold":0,"commentCount":0,"isAD":false,"updateTime":1683342377000}
json = {"id":"317580","name":"RIMOWA 26寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":28600,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","sold":0,"commentCount":0,"isAD":false,"updateTime":1696644279000}
json = {"id":"546872","name":"博兿（BOYI）拉杆包男23英寸大容量旅行包户外手提休闲拉杆袋 BY09186黑灰色","price":27500,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t3301/221/3887995271/90563/bf2cadb/57f9fbf4N8e47c225.jpg!q70.jpg.webp","category":"拉杆箱","brand":"博兿","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
json = {"id":"561178","name":"RIMOWA 30寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":13000,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","sold":0,"commentCount":0,"isAD":false,"updateTime":1696644294000}
json = {"id":"577967","name":"莎米特SUMMIT 旅行拉杆箱28英寸PC材质大容量旅行行李箱PC154 黑色","price":71300,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t30454/163/719393962/79149/13bcc06a/5bfca9b6N493202d2.jpg!q70.jpg.webp","category":"拉杆箱","brand":"莎米特","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
json = {"id":"584382","name":"美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 25英寸海关锁DL7灰色","price":36600,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp","category":"拉杆箱","brand":"美旅箱包","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
json = {"id":"584387","name":"美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 29英寸海关锁DL7灰色","price":16200,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp","category":"拉杆箱","brand":"美旅箱包","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
json = {"id":"584391","name":"美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 20英寸海关锁DL7灰色","price":29900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp","category":"拉杆箱","brand":"美旅箱包","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
json = {"id":"584392","name":"美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 29英寸海关锁DL7灰色","price":17000,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp","category":"拉杆箱","brand":"美旅箱包","sold":0,"commentCount":0,"isAD":false,"updateTime":1696644299000}
json = {"id":"584394","name":"美旅AmericanTourister拉杆箱 商务男女超轻PP行李箱时尚大容量耐磨飞机轮旅行箱 25英寸海关锁DL7灰色","price":79400,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/22734/21/2036/130399/5c18af2aEab296c01/7b148f18c6081654.jpg!q70.jpg.webp","category":"拉杆箱","brand":"美旅箱包","sold":0,"commentCount":0,"isAD":false,"updateTime":1556640000000}
```

### 构建查询条件

#### 构建查询条件

在JavaRestAPI中, 所有类型的query查询条件都是由QueryBuilders来构建的:

![image-20240729065420968](./图片/image-20240729065420968.png)

#### 全文检索

全文检索的查询条件构造API如下:

```java
// 单字段查询
QueryBuilders.matchQuery("name", "脱脂牛奶");
// 多字段查询
QueryBuilders.multiMatchQuery("脱脂牛奶", "name", "category");
```

#### 精确查询

精确查询的查询条件构造API如下:

```java
// 词条查询
QueryBuilders.termQuery("category", "牛奶");
// 范围查询
QueryBuilders.rangeQuery("price").gte(100).lte(150)
```

#### 布尔查询

布尔查询的查询条件构造API如下:

```java
// 创建布尔查询
BoolQueryBuilder boolQuery = QueryBuilders.boolQuery();
// 添加must条件
boolQuery.must(
		QueryBuilders.termQuery("brand", "华为"));
// 添加filter条件
boolQuery.filter(
		QueryBuilders.rangeQuery("price").lte(2500))
```

#### 案例

构建复杂查询条件的搜索

需求: 利用JavaRestClient实现搜索功能, 条件如下: 

- 搜索关键字为脱脂牛奶
- 品牌必须为德亚
- 价格必须低于300

```java
@Test
void testSearchBool() throws IOException {
    SearchRequest request = new SearchRequest("items");
    // 1.准备bool, 发送请求
    request.source()
        .query(
        QueryBuilders.boolQuery()
        .must(QueryBuilders.matchQuery("name", "脱脂牛奶"))
        // 1.2.添加filter
        .filter(QueryBuilders.termQuery("brand", "德亚"))
        .filter(QueryBuilders.rangeQuery("price").lt(30000))
    );
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    // 2.解析结果
    parseResponseResult(response);
}
```

### 排序和分页

#### 排序和分页

与`query`类似, 排序和分页参数都是基于`request.source()`来设置:

```java
// 查询
request.source().query(QueryBuilders.matchAllQuery());
// 分页
request.source().from(0).size(5);
// 价格排序
request.source().sort("price", SortOrder.ASC);
```

> 代码示例

```java
@Test
void testSortAndPage() throws IOException {
    int pageNo = 1;
    int pageSize = 5;
    SearchRequest request = new SearchRequest("items");
    request.source()
        // 构建查询条件
        .query(QueryBuilders.matchAllQuery())
        // 分页
        .from((pageNo - 1) * pageSize)
        .size(pageSize)
        // 排序
        .sort("price", SortOrder.DESC)
        .sort("sold", SortOrder.ASC);
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    parseResponseResult(response);
}
```

### 高亮显示

#### 高亮显示

高亮显示的条件构造API如下:

```java
request.source().highlighter(
    SearchSourceBuilder.highlight()
    	.field("name")
    	.preTags("<em>")
    	.postTags("</em>")
)
```

高亮结果解析: 

![image-20240729105606960](./图片/image-20240729105606960.png)

> 代码示例

```java
@Test
void testHighLight() throws IOException {
    SearchRequest request = new SearchRequest("items");
    request.source()
        .query(QueryBuilders.matchQuery("name","华为"))
        .highlighter(
        SearchSourceBuilder.highlight()
        .field("name")
        .preTags("<em>")
        .postTags("</em>")
    );
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    SearchHit[] hits = response.getHits().getHits();
    for (SearchHit hit : hits) {
        String json = hit.getSourceAsString();
        // 反序列化
        ItemDoc itemDoc = JSONUtil.toBean(json, ItemDoc.class);
        // 获取高亮结果
        Map<String, HighlightField> highlightFields = hit.getHighlightFields();
        if (CollUtils.isNotEmpty(highlightFields)){
            if (highlightFields.containsKey("name")){
                // 根据高亮字段名获取高亮结果
                Text[] fragments = highlightFields.get("name").getFragments();
                // 覆盖非高亮结果
                StringBuilder sb  = new StringBuilder();
                for (Text fragment : fragments) {
                    sb.append(fragment.string());
                }
                itemDoc.setName(sb.toString());
            }
        }
        log.info("itemDoc = {}",itemDoc);
    }
}
```

## 数据聚合

### 数据聚合

聚合(aggregations)可以实现对文档数据的统计、分析、运算. 聚合常见的有三类:

- 桶(Bucket)聚合: 用来对文档做分组
  - `TermAggregation`: 按照文档字段值分组
  - `Date Histogram`: 按照日期阶梯分组, 例如一周为一组, 或者一月为一组
- 度量(Metric)聚合: 用来计算一些值, 比如: 最大值、最小值、平均值等
  - `Avg`: 求平均值
  - `Max`: 求最大值
  - `Min`: 求最小值
  - `Stats`: 同时求`Max`、`Min`、`Avg`、`Sum`等
- 管道(pipeline)聚合: 以其他聚合的结果为基础做聚合

<font color='duckred'>**注意:**</font>参与聚合的字段必须是Keyword、数值、日期、布尔类型的字段

### DSL聚合

#### Bucket聚合

##### Term聚合

我们要统计所有商品中共有哪些商品分类, 其实就是以分类(category)字段对数据分组. category值一样的放在同一组, 数据Bucket聚合中的Term聚合

```json
GET /item/_search
{
    "query":{"match_all":{}}, //可省略
    "size": 0, //设置size为0, 结果中不包含文档, 只包含聚合结果
    "aggs": { //定义聚合
    	"cateAgg": { // 给聚合起个名字
            "terms": { // 聚合的类型, 按照分类值聚合, 所以选择term
                "field": "category", // 参与聚合的字段
                "size": 20 //希望获取的聚合结果数量
            }
        }
    }
}
```

<font color='red'>**聚合三要素: 聚合名称(cateAgg)、聚合类型(terms)、聚合的字段(field)**</font>

> 响应结果

```json
{
  "took" : 38,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "cateAgg" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "休闲鞋",
          "doc_count" : 20612
        },
        {
          "key" : "牛仔裤",
          "doc_count" : 19611
        },
        {
          "key" : "老花镜",
          "doc_count" : 16222
        },
        {
          "key" : "拉杆箱",
          "doc_count" : 14347
        },
        {
          "key" : "手机",
          "doc_count" : 10100
        },
        {
          "key" : "真皮包",
          "doc_count" : 3064
        },
        {
          "key" : "拉拉裤",
          "doc_count" : 1706
        },
        {
          "key" : "牛奶",
          "doc_count" : 1296
        },
        {
          "key" : "曲面电视",
          "doc_count" : 1219
        },
        {
          "key" : "硬盘",
          "doc_count" : 298
        }
      ]
    }
  }
}
```

默认情况下, Bucket聚合是对索引库的所有文档做聚合, 我们可以限定要聚合的文档范围, 只要添加query条件即可.

例如, 我想知道价格高于3000元的手机品牌有哪些?

```json
GET /items/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "match": {
            "category": "手机"
          }
        },
        {
          "range": {
            "price": {
              "gte": 300000
            }
          }
        }
      ]
    }
  },
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 10
      }
    }
  }
}
```

#### Metric聚合

除了对数据分组(Bucket)以外, 我们还可以对每个Bucket内的数据进一步做数据计算和统计.

例如: 我想知道手机有哪些品牌, 每个品牌的价格最小值、最大值、平均值

```json
GET /items/_search
{
  "query": {
    "term": {
      "category": "手机"
    }
  },
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 10
      },
      "aggs": {
        "priceStats": {
          "stats": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

> 响应结果

```json
{
  "took" : 28,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "brandAgg" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 175,
      "buckets" : [
        {
          "key" : "华为",
          "doc_count" : 7145,
          "priceStats" : {
            "count" : 7145,
            "min" : 0.0,
            "max" : 544000.0,
            "avg" : 50073.561931420576,
            "sum" : 3.577756E8
          }
        },
        {
          "key" : "小米",
          "doc_count" : 1227,
          "priceStats" : {
            "count" : 1227,
            "min" : 200.0,
            "max" : 889400.0,
            "avg" : 51005.86797066015,
            "sum" : 6.25842E7
          }
        },
        {
          "key" : "Apple",
          "doc_count" : 577,
          "priceStats" : {
            "count" : 577,
            "min" : 100.0,
            "max" : 688000.0,
            "avg" : 57975.73656845754,
            "sum" : 3.3452E7
          }
        },
        {
          "key" : "OPPO",
          "doc_count" : 430,
          "priceStats" : {
            "count" : 430,
            "min" : 0.0,
            "max" : 99500.0,
            "avg" : 50212.558139534885,
            "sum" : 2.15914E7
          }
        },
        {
          "key" : "vivo",
          "doc_count" : 174,
          "priceStats" : {
            "count" : 174,
            "min" : 0.0,
            "max" : 99800.0,
            "avg" : 52264.36781609195,
            "sum" : 9094000.0
          }
        },
        {
          "key" : "一加",
          "doc_count" : 117,
          "priceStats" : {
            "count" : 117,
            "min" : 0.0,
            "max" : 98200.0,
            "avg" : 52961.54700854701,
            "sum" : 6196501.0
          }
        },
        {
          "key" : "三星",
          "doc_count" : 99,
          "priceStats" : {
            "count" : 99,
            "min" : 1100.0,
            "max" : 474200.0,
            "avg" : 52292.92929292929,
            "sum" : 5177000.0
          }
        },
        {
          "key" : "诺基亚",
          "doc_count" : 57,
          "priceStats" : {
            "count" : 57,
            "min" : 800.0,
            "max" : 97500.0,
            "avg" : 53033.333333333336,
            "sum" : 3022900.0
          }
        },
        {
          "key" : "魅族",
          "doc_count" : 57,
          "priceStats" : {
            "count" : 57,
            "min" : 1900.0,
            "max" : 99200.0,
            "avg" : 46685.9649122807,
            "sum" : 2661100.0
          }
        },
        {
          "key" : "360",
          "doc_count" : 42,
          "priceStats" : {
            "count" : 42,
            "min" : 500.0,
            "max" : 95100.0,
            "avg" : 48711.90476190476,
            "sum" : 2045900.0
          }
        }
      ]
    }
  }
}
```

### RestClient聚合

#### RestClient聚合

我们以品牌聚合为例:

```java
request.source().size(0);
request.source().aggregation(
    AggregationBuilders
    .terms("brand_agg")
    .field("brand")
    .size(20)
);
```

![image-20240729115335837](./图片/image-20240729115335837.png)

解析结果

```java
// 解析聚合结果
Aggregations aggregations = response.getAggregations();
// 根据名称获取聚合结果
Terms bucketTerms = aggregations.get(aggName);
// 获取桶
List<? extends Terms.Bucket> buckets = bucketTerms.getBuckets();
// 遍历
for (Terms.Bucket bucket : buckets) {
    // 获取Key, 也就是品牌信息
    String brandName = bucket.getKeyAsString();
    System.out.println("brandName = " + brandName);
}
```

![image-20240729122234748](./图片/image-20240729122234748.png)

> 获取嵌套在里面的Metric聚合

```java
@Test
void testMetricAgg() throws IOException {
    String aggName = "brandAgg";
    String subAggName = "priceStats";
    SearchRequest request = new SearchRequest("items");
    request.source()
        .query(
        QueryBuilders
        .boolQuery()
        .must(
            QueryBuilders
            .matchQuery("category", "手机")
        )
    )
        .size(0)
        .aggregation(
        AggregationBuilders
        .terms(aggName)
        .field("brand")
        .size(10)
        .subAggregation(
            AggregationBuilders
            .stats(subAggName)
            .field("price")
        )
    );
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    Terms brandTerms = response.getAggregations().get(aggName);
    List<? extends Terms.Bucket> buckets = brandTerms.getBuckets();
    for (Terms.Bucket bucket : buckets) {
        String brand = bucket.getKeyAsString();
        System.out.println("brand = " + brand);
        long docCount = bucket.getDocCount();
        System.out.println("docCount = " + docCount);
        Stats priceStats = bucket.getAggregations().get(subAggName);
        double avg = priceStats.getAvg();
        System.out.println("avg = " + avg);
        double max = priceStats.getMax();
        System.out.println("max = " + max);
        double min = priceStats.getMin();
        System.out.println("min = " + min);
        double sum = priceStats.getSum();
        System.out.println("sum = " + sum);
    }
}
```
