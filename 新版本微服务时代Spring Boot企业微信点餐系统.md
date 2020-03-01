# 第1章 课程介绍

## 1-1 课程导学

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222351447.png" alt="image-20200227222351447" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222328617.png" alt="image-20200227222328617" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222409028.png" alt="image-20200227222409028" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222423547.png" alt="image-20200227222423547" style="zoom: 33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222450762.png" alt="image-20200227222450762" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227222502682.png" alt="image-20200227222502682" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200227235106470.png" alt="image-20200227235106470" style="zoom:33%;" />

# 第2章 项目设计

## 2-1 课程源码和文档

## 2-2 项目设计

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228092609418.png" alt="image-20200228092609418" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228092647926.png" alt="image-20200228092647926" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228092725200.png" alt="image-20200228092725200" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228092803448.png" alt="image-20200228092803448" style="zoom:33%;" />



## 2-3 架构和基础框架



<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228092958528.png" alt="image-20200228092958528" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228093026997.png" alt="image-20200228093026997" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228093130591.png" alt="image-20200228093130591" style="zoom:33%;" />

## 2-4 数据库设计

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228094340084.png" alt="image-20200228094340084" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228094420136.png" alt="image-20200228094420136" style="zoom:33%;" />

```sql
-- 类目
create table `product_category` (
    `category_id` int not null auto_increment,
    `category_name` varchar(64) not null comment '类目名字',
    `category_type` int not null comment '类目编号',
    `create_time` timestamp not null default current_timestamp comment '创建时间',
    `update_time` timestamp not null default current_timestamp on update current_timestamp comment '修改时间',
    primary key (`category_id`),
    UNIQUE KEY `uqe_category_type` (`category_type`)
);

-- 商品
create table `product_info` (
    `product_id` varchar(32) not null,
    `product_name` varchar(64) not null comment '商品名称',
    `product_price` decimal(8,2) not null comment '单价',
    `product_stock` int not null comment '库存',
    `product_description` varchar(64) comment '描述',
    `product_icon` varchar(512) comment '小图',
    `product_status` tinyint(3) DEFAULT '0' COMMENT '商品状态,0正常1下架',
    `category_type` int not null comment '类目编号',
    `create_time` timestamp not null default current_timestamp comment '创建时间',
    `update_time` timestamp not null default current_timestamp on update current_timestamp comment '修改时间',
    primary key (`product_id`)
);

-- 订单
create table `order_master` (
    `order_id` varchar(32) not null,
    `buyer_name` varchar(32) not null comment '买家名字',
    `buyer_phone` varchar(32) not null comment '买家电话',
    `buyer_address` varchar(128) not null comment '买家地址',
    `buyer_openid` varchar(64) not null comment '买家微信openid',
    `order_amount` decimal(8,2) not null comment '订单总金额',
    `order_status` tinyint(3) not null default '0' comment '订单状态, 默认为新下单',
    `pay_status` tinyint(3) not null default '0' comment '支付状态, 默认未支付',
    `create_time` timestamp not null default current_timestamp comment '创建时间',
    `update_time` timestamp not null default current_timestamp on update current_timestamp comment '修改时间',
    primary key (`order_id`),
    key `idx_buyer_openid` (`buyer_openid`)
);

-- 订单商品
create table `order_detail` (
    `detail_id` varchar(32) not null,
    `order_id` varchar(32) not null,
    `product_id` varchar(32) not null,
    `product_name` varchar(64) not null comment '商品名称',
    `product_price` decimal(8,2) not null comment '当前价格,单位分',
    `product_quantity` int not null comment '数量',
    `product_icon` varchar(512) comment '小图',
    `create_time` timestamp not null default current_timestamp comment '创建时间',
    `update_time` timestamp not null default current_timestamp on update current_timestamp comment '修改时间',
    primary key (`detail_id`),
    key `idx_order_id` (`order_id`)
);

-- 卖家(登录后台使用, 卖家登录之后可能直接采用微信扫码登录，不使用账号密码)
create table `seller_info` (
    `seller_id` varchar(32) not null,
    `username` varchar(32) not null,
    `password` varchar(32) not null,
    `openid` varchar(64) not null comment '微信openid',
    `create_time` timestamp not null default current_timestamp comment '创建时间',
    `update_time` timestamp not null default current_timestamp on update current_timestamp comment '修改时间',
    primary key (`seller_id`)
) comment '卖家信息表';
```

## 2-5 接口文档

~~~properties
# API

###商品列表

```
GET /sell/buyer/product/list
```

参数

```
无
```

返回

```
{
    "code": 0,
    "msg": "成功",
    "data": [
        {
            "name": "热榜",
            "type": 1,
            "foods": [
                {
                    "id": "123456",
                    "name": "皮蛋粥",
                    "price": 1.2,
                    "description": "好吃的皮蛋粥",
                    "icon": "http://xxx.com",
                }
            ]
        },
        {
            "name": "好吃的",
            "type": 2,
            "foods": [
                {
                    "id": "123457",
                    "name": "慕斯蛋糕",
                    "price": 10.9,
                    "description": "美味爽口",
                    "icon": "http://xxx.com",
                }
            ]
        }
    ]
}
```


###创建订单

```
POST /sell/buyer/order/create
```

参数

```
name: "张三"
phone: "18868822111"
address: "慕课网总部"
openid: "ew3euwhd7sjw9diwkq" //用户的微信openid
items: [{
    productId: "1423113435324",
    productQuantity: 2 //购买数量
}]

```

返回

```
{
  "code": 0,
  "msg": "成功",
  "data": {
      "orderId": "147283992738221" 
  }
}
```

###订单列表

```
GET /sell/buyer/order/list
```

参数

```
openid: 18eu2jwk2kse3r42e2e
page: 0 //从第0页开始
size: 10
```

返回

```
{
  "code": 0,
  "msg": "成功",
  "data": [
    {
      "orderId": "161873371171128075",
      "buyerName": "张三",
      "buyerPhone": "18868877111",
      "buyerAddress": "慕课网总部",
      "buyerOpenid": "18eu2jwk2kse3r42e2e",
      "orderAmount": 0,
      "orderStatus": 0,
      "payStatus": 0,
      "createTime": 1490171219,
      "updateTime": 1490171219,
      "orderDetailList": null
    },
    {
      "orderId": "161873371171128076",
      "buyerName": "张三",
      "buyerPhone": "18868877111",
      "buyerAddress": "慕课网总部",
      "buyerOpenid": "18eu2jwk2kse3r42e2e",
      "orderAmount": 0,
      "orderStatus": 0,
      "payStatus": 0,
      "createTime": 1490171219,
      "updateTime": 1490171219,
      "orderDetailList": null
    }]
}
```

###查询订单详情

```
GET /sell/buyer/order/detail
```

参数

```
openid: 18eu2jwk2kse3r42e2e
orderId: 161899085773669363
```

返回

```
{
    "code": 0,
    "msg": "成功",
    "data": {
          "orderId": "161899085773669363",
          "buyerName": "李四",
          "buyerPhone": "18868877111",
          "buyerAddress": "慕课网总部",
          "buyerOpenid": "18eu2jwk2kse3r42e2e",
          "orderAmount": 18,
          "orderStatus": 0,
          "payStatus": 0,
          "createTime": 1490177352,
          "updateTime": 1490177352,
          "orderDetailList": [
            {
                "detailId": "161899085974995851",
                "orderId": "161899085773669363",
                "productId": "157875196362360019",
                "productName": "招牌奶茶",
                "productPrice": 9,
                "productQuantity": 2,
                "productIcon": "http://xxx.com",
                "productImage": "http://xxx.com"
            }
        ]
    }
}
```

###取消订单

```
POST /sell/buyer/order/cancel
```

参数

```
openid: 18eu2jwk2kse3r42e2e
orderId: 161899085773669363
```

返回

```
{
    "code": 0,
    "msg": "成功",
    "data": null
}
```

###获取openid

```
重定向到 /sell/wechat/authorize
```

参数

```
returnUrl: http://xxx.com/abc  //【必填】
```

返回

```
http://xxx.com/abc?openid=oZxSYw5ldcxv6H0EU67GgSXOUrVg
```

###支付订单
```
重定向 /sell/pay/create
```

参数

```
orderId: 161899085773669363
returnUrl: http://xxx.com/abc/order/161899085773669363
```

返回

```
http://xxx.com/abc/order/161899085773669363
```
~~~

# 第3章 项目起步

## 3-1 开发环境搭建

### 虚拟机说明文档
#### 软件下载地址
* 建议使用迅雷下载，下载后查看镜像大小，是否完全下载
* [VirtualBox-5.2.26(Windows版)](https://file.mukewang.com/shizhan/file/117/VirtualBox-5.2.26-128414-Win.exe)
* [VirtualBox-5.2.26(Mac版)](https://file.mukewang.com/shizhan/file/117/VirtualBox-5.2.26-128414-OSX.dmg)
* [Centos镜像（直接导入，不要解压！）](https://file.mukewang.com/shizhan/file/117/centos7.3-a418.ova)

#### 虚拟机登录
* 账号 root
* 密码 123456

#### 虚拟机已安装下列软件
* jdk 1.8.0_111
* nginx 1.11.7
* mysql 5.7.17
* redis 3.2.8

##### jdk
* 路径 /usr/local/jdk1.8.0_111

##### nginx
* 路径 /usr/local/nginx
* 启动 nginx
* 重启 nginx -s reload

##### mysql
* 配置 /etc/my.conf
* 账号 root
* 密码 123456
* 端口 3306
* 启动 systemctl start mysqld
* 停止 systemctl stop mysqld

##### redis
* 路径 /usr/local/redis
* 配置 /etc/reis.conf
* 端口 6379
* 密码 123456
* 启动 systemctl start redis
* 停止 systemctl stop redis

##### tomcat
* 路径 /usr/local/tomcat
* 启动 systemctl start tomcat
* 停止 systemctl stop tomcat

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228104534938.png" alt="image-20200228104534938" style="zoom:33%;" />

## 3-2 日志的使用

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105020670.png" alt="image-20200228105020670" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105058776.png" alt="image-20200228105058776" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105321894.png" alt="image-20200228105321894" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105354187.png" alt="image-20200228105354187" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105458096.png" alt="image-20200228105458096" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228105657424.png" alt="image-20200228105657424" style="zoom:33%;" />

选用的日志

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228110100376.png" alt="image-20200228110100376" style="zoom:33%;" />



<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228110608121.png" alt="image-20200228110608121" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228110626479.png" alt="image-20200228110626479" style="zoom:33%;" />

**日志demo**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
//@Data
public class LoggerTest {

    @Test
    public void test1() {
        String name = "imooc";
        String password = "123456";
        log.debug("debug...");
        log.info("name: " + name + " ,password: " + password);
        log.info("name: {}, password: {}", name, password);
        log.error("error...");
        log.warn("warn...");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<configuration>

    <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>
                %d - %msg%n
            </pattern>
        </layout>
    </appender>

    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>DENY</onMatch>
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <encoder>
            <pattern>
                %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>/var/log/tomcat/sell/info.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>


    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <pattern>
                %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>/var/log/tomcat/sell/error.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>

    <logger name="org.hibernate.type.descriptor.sql.BasicBinder" level="TRACE"/>

    <root level="info">
        <appender-ref ref="consoleLog" />
        <appender-ref ref="fileInfoLog" />
        <appender-ref ref="fileErrorLog" />
    </root>

</configuration>
```



# 第4章 买家端类目

## 4-1 买家类目-dao（上）

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228140012312.png" alt="image-20200228140012312" style="zoom:33%;" />

### 依赖pom.xml

```xml
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
```

### 数据库配置application-dev.yml

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 123456
    url: jdbc:mysql://127.0.0.1:3306/sell?characterEncoding=utf-8&useSSL=false
  jpa:
    show-sql: true
```

### dataobject的ProductCategory

```java
@Entity
@DynamicUpdate
@Data
public class ProductCategory {

    /** 类目id. */
    @Id
    @GeneratedValue
    private Integer categoryId;

    /** 类目名字. */
    private String categoryName;

    /** 类目编号. */
    private Integer categoryType;

    private Date createTime;

    private Date updateTime;

    public ProductCategory() {
    }

    public ProductCategory(String categoryName, Integer categoryType) {
        this.categoryName = categoryName;
        this.categoryType = categoryType;
    }
}
```

### ProductCategoryRepository

```java
public interface ProductCategoryRepository extends JpaRepository<ProductCategory, Integer> {

    List<ProductCategory> findByCategoryTypeIn(List<Integer> categoryTypeList);
}
```

## 4-2 买家类目-dao

## 4-3 买家类目-service

### CategoryService

```java
public interface CategoryService {

    ProductCategory findOne(Integer categoryId);

    List<ProductCategory> findAll();

    List<ProductCategory> findByCategoryTypeIn(List<Integer> categoryTypeList);

    ProductCategory save(ProductCategory productCategory);
}
```
### CategoryServiceImpl

```java
@Service
public class CategoryServiceImpl implements CategoryService {

    @Autowired
    private ProductCategoryRepository repository;

    @Override
    public ProductCategory findOne(Integer categoryId) {
		/**
		 * 查不到返回null
		 * .get 抛异常
		 */
		return repository.findById(categoryId).orElse(null);
    }

    @Override
    public List<ProductCategory> findAll() {
        return repository.findAll();
    }

    @Override
    public List<ProductCategory> findByCategoryTypeIn(List<Integer> categoryTypeList) {
        return repository.findByCategoryTypeIn(categoryTypeList);
    }

    @Override
    public ProductCategory save(ProductCategory productCategory) {
        return repository.save(productCategory);
    }
}
```

# 第5章 买家端商品

## 5-1 买家商品-dao

### dataobject的ProductInfo

```java
@Entity
@Data
@DynamicUpdate
public class ProductInfo implements Serializable {

    private static final long serialVersionUID = 6399186181668983148L;

    @Id
    private String productId;

    /** 名字. */
    private String productName;

    /** 单价. */
    private BigDecimal productPrice;

    /** 库存. */
    private Integer productStock;

    /** 描述. */
    private String productDescription;

    /** 小图. */
    private String productIcon;

    /** 状态, 0正常1下架. */
    private Integer productStatus = ProductStatusEnum.UP.getCode();

    /** 类目编号. */
    private Integer categoryType;

    private Date createTime;

    private Date updateTime;

    @JsonIgnore
    public ProductStatusEnum getProductStatusEnum() {
        return EnumUtil.getByCode(productStatus, ProductStatusEnum.class);
    }

    /**
     * 图片链接加host拼接成完整 url
     * @param host
     * @return
     */
    public ProductInfo addImageHost(String host) {
        if (productIcon.startsWith("//") || productIcon.startsWith("http")) {
            return this;
        }

        if (!host.startsWith("http")) {
            host = "//" + host;
        }
        if (!host.endsWith("/")) {
            host = host + "/";
        }
        productIcon = host + productIcon;
        return this;
    }
}
```

### ProductInfoRepository

```java
public interface ProductInfoRepository extends JpaRepository<ProductInfo, String> {

    List<ProductInfo> findByProductStatus(Integer productStatus);
}
```

## 5-2 买家商品-service

### 接口ProductService

```java
public interface ProductService {

    ProductInfo findOne(String productId);

    /**
     * 查询所有在架商品列表
     * @return
     */
    List<ProductInfo> findUpAll();

    Page<ProductInfo> findAll(Pageable pageable);

    ProductInfo save(ProductInfo productInfo);

    //加库存
    void increaseStock(List<CartDTO> cartDTOList);

    //减库存
    void decreaseStock(List<CartDTO> cartDTOList);

    //上架
    ProductInfo onSale(String productId);

    //下架
    ProductInfo offSale(String productId);
}
```

### 实现ProductServiceImpl

```java
@Service
//@CacheConfig(cacheNames = "product")
public class ProductServiceImpl implements ProductService {

    @Autowired
    private ProductInfoRepository repository;

    @Autowired
    private UpYunConfig upYunConfig;

    @Override
//    @Cacheable(key = "123")
    public ProductInfo findOne(String productId) {
        Optional<ProductInfo> productInfoOptional = repository.findById(productId);
//        if (productInfoOptional.isPresent()) {
//            return productInfoOptional.get().addImageHost(upYunConfig.getImageHost());
//        }
//        return null;

        productInfoOptional.ifPresent(e -> e.addImageHost(upYunConfig.getImageHost()));
        return productInfoOptional.orElse(null);
    }

    @Override
    public List<ProductInfo> findUpAll() {
        return repository.findByProductStatus(ProductStatusEnum.UP.getCode()).stream()
                .map(e -> e.addImageHost(upYunConfig.getImageHost()))
                .collect(Collectors.toList());
    }

    @Override
    public Page<ProductInfo> findAll(Pageable pageable) {
        Page<ProductInfo> productInfoPage = repository.findAll(pageable);
        productInfoPage.getContent().stream()
                .forEach(e -> e.addImageHost(upYunConfig.getImageHost()));
        return productInfoPage;
    }

    @Override
//    @CachePut(key = "123")
    public ProductInfo save(ProductInfo productInfo) {
        return repository.save(productInfo).addImageHost(upYunConfig.getImageHost());
    }

    @Override
    @Transactional
    public void increaseStock(List<CartDTO> cartDTOList) {
        for (CartDTO cartDTO: cartDTOList) {
            ProductInfo productInfo = repository.findById(cartDTO.getProductId()).orElse(null);
            if (productInfo == null) {
                throw new SellException(ResultEnum.PRODUCT_NOT_EXIST);
            }
            Integer result = productInfo.getProductStock() + cartDTO.getProductQuantity();
            productInfo.setProductStock(result);

            repository.save(productInfo);
        }

    }

    @Override
    @Transactional
    public void decreaseStock(List<CartDTO> cartDTOList) {
        for (CartDTO cartDTO: cartDTOList) {
            ProductInfo productInfo = repository.findById(cartDTO.getProductId()).orElse(null);
            if (productInfo == null) {
                throw new SellException(ResultEnum.PRODUCT_NOT_EXIST);
            }

            Integer result = productInfo.getProductStock() - cartDTO.getProductQuantity();
            if (result < 0) {
                throw new SellException(ResultEnum.PRODUCT_STOCK_ERROR);
            }

            productInfo.setProductStock(result);

            repository.save(productInfo);
        }
    }

    @Override
    public ProductInfo onSale(String productId) {
        ProductInfo productInfo = repository.findById(productId).orElse(null);
        if (productInfo == null) {
            throw new SellException(ResultEnum.PRODUCT_NOT_EXIST);
        }
        if (productInfo.getProductStatusEnum() == ProductStatusEnum.UP) {
            throw new SellException(ResultEnum.PRODUCT_STATUS_ERROR);
        }

        //更新
        productInfo.setProductStatus(ProductStatusEnum.UP.getCode());
        return repository.save(productInfo);
    }

    @Override
    public ProductInfo offSale(String productId) {
        ProductInfo productInfo = repository.findById(productId).orElse(null);
        if (productInfo == null) {
            throw new SellException(ResultEnum.PRODUCT_NOT_EXIST);
        }
        if (productInfo.getProductStatusEnum() == ProductStatusEnum.DOWN) {
            throw new SellException(ResultEnum.PRODUCT_STATUS_ERROR);
        }

        //更新
        productInfo.setProductStatus(ProductStatusEnum.DOWN.getCode());
        return repository.save(productInfo);
    }
}
```

## 5-3 买家商品-api（上）

### BuyerProductController

```java
@RestController
@RequestMapping("/buyer/product")
public class BuyerProductController {

    @Autowired
    private ProductService productService;

    @Autowired
    private CategoryService categoryService;

    @GetMapping("/list")
//    @Cacheable(cacheNames = "product", key = "#sellerId", condition = "#sellerId.length() > 3", unless = "#result.getCode() != 0")
    public ResultVO list(@RequestParam(value = "sellerId", required = false) String sellerId) {
        //1. 查询所有的上架商品
        List<ProductInfo> productInfoList = productService.findUpAll();

        //2. 查询类目(一次性查询)
//        List<Integer> categoryTypeList = new ArrayList<>();
        //传统方法
//        for (ProductInfo productInfo : productInfoList) {
//            categoryTypeList.add(productInfo.getCategoryType());
//        }
        //精简方法(java8, lambda)
        List<Integer> categoryTypeList = productInfoList.stream()
                .map(e -> e.getCategoryType())
                .collect(Collectors.toList());
        List<ProductCategory> productCategoryList = categoryService.findByCategoryTypeIn(categoryTypeList);

        //3. 数据拼装
        List<ProductVO> productVOList = new ArrayList<>();
        for (ProductCategory productCategory: productCategoryList) {
            ProductVO productVO = new ProductVO();
            productVO.setCategoryType(productCategory.getCategoryType());
            productVO.setCategoryName(productCategory.getCategoryName());

            List<ProductInfoVO> productInfoVOList = new ArrayList<>();
            for (ProductInfo productInfo: productInfoList) {
                if (productInfo.getCategoryType().equals(productCategory.getCategoryType())) {
                    ProductInfoVO productInfoVO = new ProductInfoVO();
                    BeanUtils.copyProperties(productInfo, productInfoVO);
                    productInfoVOList.add(productInfoVO);
                }
            }
            productVO.setProductInfoVOList(productInfoVOList);
            productVOList.add(productVO);
        }

        return ResultVOUtil.success(productVOList);
    }
}
```

### 返回对象ResultVO

```java
@Data
public class ResultVO<T> implements Serializable {

    private static final long serialVersionUID = 3068837394742385883L;

    /** 错误码. */
    private Integer code;

    /** 提示信息. */
    private String msg;

    /** 具体内容. */
    private T data;
}
```

### ProductVO

```java
@Data
public class ProductVO implements Serializable {

    private static final long serialVersionUID = 7097863777546530545L;

    @JsonProperty("name")
    private String categoryName;

    @JsonProperty("type")
    private Integer categoryType;

    @JsonProperty("foods")
    private List<ProductInfoVO> productInfoVOList;
}
```

### ProductInfoVO

```java
@Data
public class ProductInfoVO implements Serializable {

    private static final long serialVersionUID = -3895834204864685262L;

    @JsonProperty("id")
    private String productId;

    @JsonProperty("name")
    private String productName;

    @JsonProperty("price")
    private BigDecimal productPrice;

    @JsonProperty("description")
    private String productDescription;

    @JsonProperty("icon")
    private String productIcon;
}
```

## 5-4 买家商品-api（下）


# 第6章 买家端订单

## 6-1 买家订单-dao(上)

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228181806256.png" alt="image-20200228181806256" style="zoom:33%;" />

### dataobject OrderMaster

```java
@Entity
@Data
@DynamicUpdate
public class OrderMaster {

    /** 订单id. */
    @Id
    private String orderId;

    /** 买家名字. */
    private String buyerName;

    /** 买家手机号. */
    private String buyerPhone;

    /** 买家地址. */
    private String buyerAddress;

    /** 买家微信Openid. */
    private String buyerOpenid;

    /** 订单总金额. */
    private BigDecimal orderAmount;

    /** 订单状态, 默认为0新下单. */
    private Integer orderStatus = OrderStatusEnum.NEW.getCode();

    /** 支付状态, 默认为0未支付. */
    private Integer payStatus = PayStatusEnum.WAIT.getCode();

    /** 创建时间. */
    private Date createTime;

    /** 更新时间. */
    private Date updateTime;

}
```

### 枚举OrderStatusEnum

```java
@Getter
public enum OrderStatusEnum implements CodeEnum {
    NEW(0, "新订单"),
    FINISHED(1, "完结"),
    CANCEL(2, "已取消"),
    ;

    private Integer code;

    private String message;

    OrderStatusEnum(Integer code, String message) {
        this.code = code;
        this.message = message;
    }
}
```

### dataobject OrderDetail

```java
@Entity
@Data
public class OrderDetail {

    @Id
    private String detailId;

    /** 订单id. */
    private String orderId;

    /** 商品id. */
    private String productId;

    /** 商品名称. */
    private String productName;

    /** 商品单价. */
    private BigDecimal productPrice;

    /** 商品数量. */
    private Integer productQuantity;

    /** 商品小图. */
    private String productIcon;
}
```

### OrderMasterRepository

```java
public interface OrderMasterRepository extends JpaRepository<OrderMaster, String> {

    Page<OrderMaster> findByBuyerOpenid(String buyerOpenid, Pageable pageable);
}
```

### OrderDetailRepository
```java
public interface OrderDetailRepository extends JpaRepository<OrderDetail, String> {

    List<OrderDetail> findByOrderId(String orderId);
}
```

## 6-2 买家订单-dao(下)

## 6-3 买家订单-service创建_A

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228184138165.png" alt="image-20200228184138165" style="zoom:33%;" />

### interface OrderService

```java
public interface OrderService {

    /** 创建订单. */
    OrderDTO create(OrderDTO orderDTO);

    /** 查询单个订单. */
    OrderDTO findOne(String orderId);

    /** 查询订单列表. */
    Page<OrderDTO> findList(String buyerOpenid, Pageable pageable);

    /** 取消订单. */
    OrderDTO cancel(OrderDTO orderDTO);

    /** 完结订单. */
    OrderDTO finish(OrderDTO orderDTO);

    /** 支付订单. */
    OrderDTO paid(OrderDTO orderDTO);

    /** 查询订单列表. */
    Page<OrderDTO> findList(Pageable pageable);

}
```

### OrderServiceImpl

```java
@Service
@Slf4j
public class OrderServiceImpl implements OrderService {

    @Autowired
    private ProductService productService;

    @Autowired
    private OrderDetailRepository orderDetailRepository;

    @Autowired
    private OrderMasterRepository orderMasterRepository;

    @Autowired
    private PayService payService;

    @Autowired
    private PushMessageService pushMessageService;

    @Autowired
    private WebSocket webSocket;

    @Override
    @Transactional
    public OrderDTO create(OrderDTO orderDTO) {

        String orderId = KeyUtil.genUniqueKey();
        BigDecimal orderAmount = new BigDecimal(BigInteger.ZERO);

//        List<CartDTO> cartDTOList = new ArrayList<>();

        //1. 查询商品（数量, 价格）
        for (OrderDetail orderDetail: orderDTO.getOrderDetailList()) {
            ProductInfo productInfo =  productService.findOne(orderDetail.getProductId());
            if (productInfo == null) {
                throw new SellException(ResultEnum.PRODUCT_NOT_EXIST);
//                throw new ResponseBankException();
            }

            //2. 计算订单总价
            orderAmount = productInfo.getProductPrice()
                    .multiply(new BigDecimal(orderDetail.getProductQuantity()))
                    .add(orderAmount);

            //订单详情入库
            orderDetail.setDetailId(KeyUtil.genUniqueKey());
            orderDetail.setOrderId(orderId);
            BeanUtils.copyProperties(productInfo, orderDetail);
            orderDetailRepository.save(orderDetail);

//            CartDTO cartDTO = new CartDTO(orderDetail.getProductId(), orderDetail.getProductQuantity());
//            cartDTOList.add(cartDTO);
        }


        //3. 写入订单数据库（orderMaster和orderDetail）
        OrderMaster orderMaster = new OrderMaster();
        orderDTO.setOrderId(orderId);
        BeanUtils.copyProperties(orderDTO, orderMaster);
        orderMaster.setOrderAmount(orderAmount);
        orderMaster.setOrderStatus(OrderStatusEnum.NEW.getCode());
        orderMaster.setPayStatus(PayStatusEnum.WAIT.getCode());
        orderMasterRepository.save(orderMaster);

        //4. 扣库存
        List<CartDTO> cartDTOList = orderDTO.getOrderDetailList().stream().map(e ->
                new CartDTO(e.getProductId(), e.getProductQuantity())
        ).collect(Collectors.toList());
        productService.decreaseStock(cartDTOList);

        //发送websocket消息
        webSocket.sendMessage(orderDTO.getOrderId());

        return orderDTO;
    }

    @Override
    public OrderDTO findOne(String orderId) {

        OrderMaster orderMaster = orderMasterRepository.findById(orderId).orElse(null);
        if (orderMaster == null) {
            throw new SellException(ResultEnum.ORDER_NOT_EXIST);
        }

        List<OrderDetail> orderDetailList = orderDetailRepository.findByOrderId(orderId);
        if (CollectionUtils.isEmpty(orderDetailList)) {
            throw new SellException(ResultEnum.ORDERDETAIL_NOT_EXIST);
        }

        OrderDTO orderDTO = new OrderDTO();
        BeanUtils.copyProperties(orderMaster, orderDTO);
        orderDTO.setOrderDetailList(orderDetailList);

        return orderDTO;
    }

    @Override
    public Page<OrderDTO> findList(String buyerOpenid, Pageable pageable) {
        Page<OrderMaster> orderMasterPage = orderMasterRepository.findByBuyerOpenid(buyerOpenid, pageable);

        List<OrderDTO> orderDTOList = OrderMaster2OrderDTOConverter.convert(orderMasterPage.getContent());

        return new PageImpl<OrderDTO>(orderDTOList, pageable, orderMasterPage.getTotalElements());
    }

    @Override
    @Transactional
    public OrderDTO cancel(OrderDTO orderDTO) {
        OrderMaster orderMaster = new OrderMaster();

        //判断订单状态
        if (!orderDTO.getOrderStatus().equals(OrderStatusEnum.NEW.getCode())) {
            log.error("【取消订单】订单状态不正确, orderId={}, orderStatus={}", orderDTO.getOrderId(), orderDTO.getOrderStatus());
            throw new SellException(ResultEnum.ORDER_STATUS_ERROR);
        }

        //修改订单状态
        orderDTO.setOrderStatus(OrderStatusEnum.CANCEL.getCode());
        BeanUtils.copyProperties(orderDTO, orderMaster);
        OrderMaster updateResult = orderMasterRepository.save(orderMaster);
        if (updateResult == null) {
            log.error("【取消订单】更新失败, orderMaster={}", orderMaster);
            throw new SellException(ResultEnum.ORDER_UPDATE_FAIL);
        }

        //返回库存
        if (CollectionUtils.isEmpty(orderDTO.getOrderDetailList())) {
            log.error("【取消订单】订单中无商品详情, orderDTO={}", orderDTO);
            throw new SellException(ResultEnum.ORDER_DETAIL_EMPTY);
        }
        List<CartDTO> cartDTOList = orderDTO.getOrderDetailList().stream()
                .map(e -> new CartDTO(e.getProductId(), e.getProductQuantity()))
                .collect(Collectors.toList());
        productService.increaseStock(cartDTOList);

        //如果已支付, 需要退款
        if (orderDTO.getPayStatus().equals(PayStatusEnum.SUCCESS.getCode())) {
            payService.refund(orderDTO);
        }

        return orderDTO;
    }

    @Override
    @Transactional
    public OrderDTO finish(OrderDTO orderDTO) {
        //判断订单状态
        if (!orderDTO.getOrderStatus().equals(OrderStatusEnum.NEW.getCode())) {
            log.error("【完结订单】订单状态不正确, orderId={}, orderStatus={}", orderDTO.getOrderId(), orderDTO.getOrderStatus());
            throw new SellException(ResultEnum.ORDER_STATUS_ERROR);
        }

        //修改订单状态
        orderDTO.setOrderStatus(OrderStatusEnum.FINISHED.getCode());
        OrderMaster orderMaster = new OrderMaster();
        BeanUtils.copyProperties(orderDTO, orderMaster);
        OrderMaster updateResult = orderMasterRepository.save(orderMaster);
        if (updateResult == null) {
            log.error("【完结订单】更新失败, orderMaster={}", orderMaster);
            throw new SellException(ResultEnum.ORDER_UPDATE_FAIL);
        }

        //推送微信模版消息
        pushMessageService.orderStatus(orderDTO);

        return orderDTO;
    }

    @Override
    @Transactional
    public OrderDTO paid(OrderDTO orderDTO) {
        //判断订单状态
        if (!orderDTO.getOrderStatus().equals(OrderStatusEnum.NEW.getCode())) {
            log.error("【订单支付完成】订单状态不正确, orderId={}, orderStatus={}", orderDTO.getOrderId(), orderDTO.getOrderStatus());
            throw new SellException(ResultEnum.ORDER_STATUS_ERROR);
        }

        //判断支付状态
        if (!orderDTO.getPayStatus().equals(PayStatusEnum.WAIT.getCode())) {
            log.error("【订单支付完成】订单支付状态不正确, orderDTO={}", orderDTO);
            throw new SellException(ResultEnum.ORDER_PAY_STATUS_ERROR);
        }

        //修改支付状态
        orderDTO.setPayStatus(PayStatusEnum.SUCCESS.getCode());
        OrderMaster orderMaster = new OrderMaster();
        BeanUtils.copyProperties(orderDTO, orderMaster);
        OrderMaster updateResult = orderMasterRepository.save(orderMaster);
        if (updateResult == null) {
            log.error("【订单支付完成】更新失败, orderMaster={}", orderMaster);
            throw new SellException(ResultEnum.ORDER_UPDATE_FAIL);
        }

        return orderDTO;
    }

    @Override
    public Page<OrderDTO> findList(Pageable pageable) {
        Page<OrderMaster> orderMasterPage = orderMasterRepository.findAll(pageable);

        List<OrderDTO> orderDTOList = OrderMaster2OrderDTOConverter.convert(orderMasterPage.getContent());

        return new PageImpl<>(orderDTOList, pageable, orderMasterPage.getTotalElements());
    }
}
```

## 6-4 买家订单-service创建_B

### 枚举ResultEnum

```java
@Getter
public enum ResultEnum {

    SUCCESS(0, "成功"),

    PARAM_ERROR(1, "参数不正确"),

    PRODUCT_NOT_EXIST(10, "商品不存在"),

    PRODUCT_STOCK_ERROR(11, "商品库存不正确"),

    ORDER_NOT_EXIST(12, "订单不存在"),

    ORDERDETAIL_NOT_EXIST(13, "订单详情不存在"),

    ORDER_STATUS_ERROR(14, "订单状态不正确"),

    ORDER_UPDATE_FAIL(15, "订单更新失败"),

    ORDER_DETAIL_EMPTY(16, "订单详情为空"),

    ORDER_PAY_STATUS_ERROR(17, "订单支付状态不正确"),

    CART_EMPTY(18, "购物车为空"),

    ORDER_OWNER_ERROR(19, "该订单不属于当前用户"),

    WECHAT_MP_ERROR(20, "微信公众账号方面错误"),

    WXPAY_NOTIFY_MONEY_VERIFY_ERROR(21, "微信支付异步通知金额校验不通过"),

    ORDER_CANCEL_SUCCESS(22, "订单取消成功"),

    ORDER_FINISH_SUCCESS(23, "订单完结成功"),

    PRODUCT_STATUS_ERROR(24, "商品状态不正确"),

    LOGIN_FAIL(25, "登录失败, 登录信息不正确"),

    LOGOUT_SUCCESS(26, "登出成功"),
    ;

    private Integer code;

    private String message;

    ResultEnum(Integer code, String message) {
        this.code = code;
        this.message = message;
    }
}
```

### KeyUtil

```java
public class KeyUtil {

    /**
     * 生成唯一的主键
     * 格式: 时间+随机数
     * @return
     */
    public static synchronized String genUniqueKey() {
        Random random = new Random();
        Integer number = random.nextInt(900000) + 100000;

        return System.currentTimeMillis() + String.valueOf(number);
    }
}
```

## 6-5 买家订单-service创建_C

## 6-6 买家订单-service创建_D

## 6-7 买家订单-service查询

## 6-8 买家订单-service取消

## 6-9 买家订单-service finish和paid

## 6-10 买家订单-api_A

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228202432920.png" alt="image-20200228202432920" style="zoom:33%;" />

### BuyerOrderController

```java
@RestController
@RequestMapping("/buyer/order")
@Slf4j
public class BuyerOrderController {

    @Autowired
    private OrderService orderService;

    @Autowired
    private BuyerService buyerService;

    //创建订单
    @PostMapping("/create")
    public ResultVO<Map<String, String>> create(@Valid OrderForm orderForm,
                                                BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            log.error("【创建订单】参数不正确, orderForm={}", orderForm);
            throw new SellException(ResultEnum.PARAM_ERROR.getCode(),
                    bindingResult.getFieldError().getDefaultMessage());
        }

        OrderDTO orderDTO = OrderForm2OrderDTOConverter.convert(orderForm);
        if (CollectionUtils.isEmpty(orderDTO.getOrderDetailList())) {
            log.error("【创建订单】购物车不能为空");
            throw new SellException(ResultEnum.CART_EMPTY);
        }

        OrderDTO createResult = orderService.create(orderDTO);

        Map<String, String> map = new HashMap<>();
        map.put("orderId", createResult.getOrderId());

        return ResultVOUtil.success(map);
    }

    //订单列表
    @GetMapping("/list")
    public ResultVO<List<OrderDTO>> list(@RequestParam("openid") String openid,
                                         @RequestParam(value = "page", defaultValue = "0") Integer page,
                                         @RequestParam(value = "size", defaultValue = "10") Integer size) {
        if (StringUtils.isEmpty(openid)) {
            log.error("【查询订单列表】openid为空");
            throw new SellException(ResultEnum.PARAM_ERROR);
        }

        PageRequest request = PageRequest.of(page, size);
        Page<OrderDTO> orderDTOPage = orderService.findList(openid, request);

        return ResultVOUtil.success(orderDTOPage.getContent());
    }


    //订单详情
    @GetMapping("/detail")
    public ResultVO<OrderDTO> detail(@RequestParam("openid") String openid,
                                     @RequestParam("orderId") String orderId) {
        OrderDTO orderDTO = buyerService.findOrderOne(openid, orderId);
        return ResultVOUtil.success(orderDTO);
    }

    //取消订单
    @PostMapping("/cancel")
    public ResultVO cancel(@RequestParam("openid") String openid,
                           @RequestParam("orderId") String orderId) {
        buyerService.cancelOrder(openid, orderId);
        return ResultVOUtil.success();
    }
}
```

### OrderForm

```java
@Data
public class OrderForm {

    /**
     * 买家姓名
     */
    @NotEmpty(message = "姓名必填")
    private String name;

    /**
     * 买家手机号
     */
    @NotEmpty(message = "手机号必填")
    private String phone;

    /**
     * 买家地址
     */
    @NotEmpty(message = "地址必填")
    private String address;

    /**
     * 买家微信openid
     */
    @NotEmpty(message = "openid必填")
    private String openid;

    /**
     * 购物车
     */
    @NotEmpty(message = "购物车不能为空")
    private String items;
}
```

### OrderForm2OrderDTOConverter

```java
@Slf4j
public class OrderForm2OrderDTOConverter {

    public static OrderDTO convert(OrderForm orderForm) {
        Gson gson = new Gson();
        OrderDTO orderDTO = new OrderDTO();

        orderDTO.setBuyerName(orderForm.getName());
        orderDTO.setBuyerPhone(orderForm.getPhone());
        orderDTO.setBuyerAddress(orderForm.getAddress());
        orderDTO.setBuyerOpenid(orderForm.getOpenid());

        List<OrderDetail> orderDetailList = new ArrayList<>();
        try {
            orderDetailList = gson.fromJson(orderForm.getItems(),
                    new TypeToken<List<OrderDetail>>() {
                    }.getType());
        } catch (Exception e) {
            log.error("【对象转换】错误, string={}", orderForm.getItems());
            throw new SellException(ResultEnum.PARAM_ERROR);
        }
        orderDTO.setOrderDetailList(orderDetailList);

        return orderDTO;
    }
}
```

## 6-11 买家订单-api_B

### Date2LongSerializer

```java
public class Date2LongSerializer extends JsonSerializer<Date> {

    @Override
    public void serialize(Date date, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException, JsonProcessingException {
        jsonGenerator.writeNumber(date.getTime() / 1000);
    }
}
```

## 6-12 买家订单-api_C

### BuyerService

```java
public interface BuyerService {

    //查询一个订单
    OrderDTO findOrderOne(String openid, String orderId);

    //取消订单
    OrderDTO cancelOrder(String openid, String orderId);
}
```

### BuyerServiceImpl

```java
@Service
@Slf4j
public class BuyerServiceImpl implements BuyerService {

    @Autowired
    private OrderService orderService;

    @Override
    public OrderDTO findOrderOne(String openid, String orderId) {
        return checkOrderOwner(openid, orderId);
    }

    @Override
    public OrderDTO cancelOrder(String openid, String orderId) {
        OrderDTO orderDTO = checkOrderOwner(openid, orderId);
        if (orderDTO == null) {
            log.error("【取消订单】查不到改订单, orderId={}", orderId);
            throw new SellException(ResultEnum.ORDER_NOT_EXIST);
        }

        return orderService.cancel(orderDTO);
    }

    private OrderDTO checkOrderOwner(String openid, String orderId) {
        OrderDTO orderDTO = orderService.findOne(orderId);
        if (orderDTO == null) {
            return null;
        }
        //判断是否是自己的订单
        if (!orderDTO.getBuyerOpenid().equalsIgnoreCase(openid)) {
            log.error("【查询订单】订单的openid不一致. openid={}, orderDTO={}", openid, orderDTO);
            throw new SellException(ResultEnum.ORDER_OWNER_ERROR);
        }
        return orderDTO;
    }
}
```

## 6-13 买家订单-api_D

# 第7章 微信授权

## 7-1 不看错过一个亿！

第 7 章 微信授权
第 8 章 微信支付和退款
适合有自己有微信支付账号的同学。

**没有微信支付账号**的同学按下面的顺序学习。看到第7章时有疑问没关系，第16章里都有解答。

### 观看顺序

有账号的同学：7->8->9…
借用账号的同学：7 ->16 -> 8 -> 9…

### 另有2篇手记助你一臂之力。

- 微信授权 https://www.imooc.com/article/70497
- 微信支付（账号借用） https://www.imooc.com/article/31607

## 7-2 设置域名

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228222625759.png" alt="image-20200228222625759" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228222735624.png" alt="image-20200228222735624" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228222809340.png" alt="image-20200228222809340" style="zoom:33%;" />

## 7-3 获取code

### WeixinController

```java
@RestController
@RequestMapping("/weixin")
@Slf4j
public class WeixinController {

    @GetMapping("/auth")
    public void auth(@RequestParam("code") String code) {
        log.info("进入auth方法。。。");
        log.info("code={}", code);

        String url = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=wxd898fcb01713c658&secret=29d8a650db31472aa87800e3b0d739f2&code=" + code + "&grant_type=authorization_code";
        RestTemplate restTemplate = new RestTemplate();
        String response = restTemplate.getForObject(url, String.class);
        log.info("response={}", response);
    }
}
```

## 7-4 换取access_token

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200228224930027.png" alt="image-20200228224930027" style="zoom:33%;" />

## 7-5 使用sdk方式（上）

### WechatController

```java
@Controller
@RequestMapping("/wechat")
@Slf4j
public class WechatController {

    @Autowired
    private WxMpService wxMpService;

    @Autowired
    private WxMpService wxOpenService;

    @Autowired
    private ProjectUrlConfig projectUrlConfig;

    @GetMapping("/authorize")
    public String authorize(@RequestParam("returnUrl") String returnUrl) {
        //1. 配置
        //2. 调用方法
        String url = projectUrlConfig.getWechatMpAuthorize() + "/sell/wechat/userInfo";
        String redirectUrl = wxMpService.oauth2buildAuthorizationUrl(url, WxConsts.OAUTH2_SCOPE_BASE, URLEncoder.encode(returnUrl));
        return "redirect:" + redirectUrl;
    }

    @GetMapping("/userInfo")
    public String userInfo(@RequestParam("code") String code,
                         @RequestParam("state") String returnUrl) {
        WxMpOAuth2AccessToken wxMpOAuth2AccessToken = new WxMpOAuth2AccessToken();
        try {
            wxMpOAuth2AccessToken = wxMpService.oauth2getAccessToken(code);
        } catch (WxErrorException e) {
            log.error("【微信网页授权】{}", e);
            throw new SellException(ResultEnum.WECHAT_MP_ERROR.getCode(), e.getError().getErrorMsg());
        }

        String openId = wxMpOAuth2AccessToken.getOpenId();

        return "redirect:" + returnUrl + "?openid=" + openId;
    }

    @GetMapping("/qrAuthorize")
    public String qrAuthorize(@RequestParam("returnUrl") String returnUrl) {
        String url = projectUrlConfig.getWechatOpenAuthorize() + "/sell/wechat/qrUserInfo";
        String redirectUrl = wxOpenService.buildQrConnectUrl(url, WxConsts.QRCONNECT_SCOPE_SNSAPI_LOGIN, URLEncoder.encode(returnUrl));
        return "redirect:" + redirectUrl;
    }

    @GetMapping("/qrUserInfo")
    public String qrUserInfo(@RequestParam("code") String code,
                             @RequestParam("state") String returnUrl) {
        WxMpOAuth2AccessToken wxMpOAuth2AccessToken = new WxMpOAuth2AccessToken();
        try {
            wxMpOAuth2AccessToken = wxOpenService.oauth2getAccessToken(code);
        } catch (WxErrorException e) {
            log.error("【微信网页授权】{}", e);
            throw new SellException(ResultEnum.WECHAT_MP_ERROR.getCode(), e.getError().getErrorMsg());
        }
        log.info("wxMpOAuth2AccessToken={}", wxMpOAuth2AccessToken);
        String openId = wxMpOAuth2AccessToken.getOpenId();

        return "redirect:" + returnUrl + "?openid=" + openId;
    }
}
```

### WechatMpConfig

```java
@Component
public class WechatMpConfig {

    @Autowired
    private WechatAccountConfig accountConfig;

    @Bean
    public WxMpService wxMpService() {
        WxMpService wxMpService = new WxMpServiceImpl();
        wxMpService.setWxMpConfigStorage(wxMpConfigStorage());
        return wxMpService;
    }

    @Bean
    public WxMpConfigStorage wxMpConfigStorage() {
        WxMpInMemoryConfigStorage wxMpConfigStorage = new WxMpInMemoryConfigStorage();
        wxMpConfigStorage.setAppId(accountConfig.getMpAppId());
        wxMpConfigStorage.setSecret(accountConfig.getMpAppSecret());
        return wxMpConfigStorage;
    }
}
```

### 配置application-dev.yml

```yaml
wechat:
  # 公众账号, 授权
  mpAppId: wxd898fcb01713c658
  mpAppSecret:

  # 开放平台,  卖家扫码登录用
  openAppId: wx6ad144e54af67d87
  openAppSecret: 91a2ff6d38a2bbccfb7e9xxxxxx

  # 支付/商户号
  mchId: 1483469312
  mchKey: 06C56A89949D617xxxxxxxxxxx
  # 发起支付不需要证书, 退款需要
  keyPath:
  notifyUrl: http://sell.natapp4.cc/sell/pay/notify
  templateId:
    orderStatus: e-Cqq67QxD6YNI41iRiqawEYdFavW_7pc7LyEMb-yeQ
```

#### WechatAccountConfig

```java
@Data
@Component
@ConfigurationProperties(prefix = "wechat")
public class WechatAccountConfig {

    /**
     * 公众平台id
     */
    private String mpAppId;

    /**
     * 公众平台密钥
     */
    private String mpAppSecret;

    /**
     * 开放平台id
     */
    private String openAppId;

    /**
     * 开放平台密钥
     */
    private String openAppSecret;

    /**
     * 商户号
     */
    private String mchId;

    /**
     * 商户密钥
     */
    private String mchKey;

    /**
     * 商户证书路径
     */
    private String keyPath;

    /**
     * 微信支付异步通知地址
     */
    private String notifyUrl;

    /**
     * 微信模版id
     */
    private Map<String, String> templateId;
}
```

## 7-6 使用sdk方式（下）

## 7-7 微信网页授权前端调试

# 第8章 微信支付和退款

## 8-1 发起微信支付-后端（上）

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229010921515.png" alt="image-20200229010921515" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229011204820.png" alt="image-20200229011204820" style="zoom:33%;" />

### PayController

```java
/**
 * 支付
 * Created by 廖师兄
 * 2017-07-04 00:49
 */
@Controller
@RequestMapping("/pay")
public class PayController {

    @Autowired
    private OrderService orderService;

    @Autowired
    private PayService payService;

    @GetMapping("/create")
    public ModelAndView create(@RequestParam("orderId") String orderId,
                               @RequestParam("returnUrl") String returnUrl,
                               Map<String, Object> map) throws UnsupportedEncodingException {
        //1. 查询订单
        OrderDTO orderDTO = orderService.findOne(orderId);
        if (orderDTO == null) {
            throw new SellException(ResultEnum.ORDER_NOT_EXIST);
        }

        //2. 发起支付
        PayResponse payResponse = payService.create(orderDTO);

        map.put("payResponse", payResponse);
        map.put("returnUrl", returnUrl.startsWith("http://") ? returnUrl : URLEncoder.encode(returnUrl, "utf-8"));

        return new ModelAndView("pay/create", map);
    }

    /**
     * 微信异步通知
     * @param notifyData
     */
    @PostMapping("/notify")
    public ModelAndView notify(@RequestBody String notifyData) {
        payService.notify(notifyData);

        //返回给微信处理结果
        return new ModelAndView("pay/success");
    }
}
```

### PayService

```java
public interface PayService {

    PayResponse create(OrderDTO orderDTO);

    PayResponse notify(String notifyData);

    RefundResponse refund(OrderDTO orderDTO);
}
```

### PayServiceImpl

```java
@Service
@Slf4j
public class PayServiceImpl implements PayService {

    private static final String ORDER_NAME = "微信点餐订单";

    @Autowired
    private BestPayServiceImpl bestPayService;

    @Autowired
    private OrderService orderService;

    @Override
    public PayResponse create(OrderDTO orderDTO) {
        PayRequest payRequest = new PayRequest();
        payRequest.setOpenid(orderDTO.getBuyerOpenid());
        payRequest.setOrderAmount(orderDTO.getOrderAmount().doubleValue());
        payRequest.setOrderId(orderDTO.getOrderId());
        payRequest.setOrderName(ORDER_NAME);
        payRequest.setPayTypeEnum(BestPayTypeEnum.WXPAY_H5);
        log.info("【微信支付】发起支付, request={}", JsonUtil.toJson(payRequest));

        PayResponse payResponse = bestPayService.pay(payRequest);
        log.info("【微信支付】发起支付, response={}", JsonUtil.toJson(payResponse));
        return payResponse;
    }

    @Override
    public PayResponse notify(String notifyData) {
        //1. 验证签名
        //2. 支付的状态
        //3. 支付金额
        //4. 支付人(下单人 == 支付人)

        PayResponse payResponse = bestPayService.asyncNotify(notifyData);
        log.info("【微信支付】异步通知, payResponse={}", JsonUtil.toJson(payResponse));

        //查询订单
        OrderDTO orderDTO = orderService.findOne(payResponse.getOrderId());

        //判断订单是否存在
        if (orderDTO == null) {
            log.error("【微信支付】异步通知, 订单不存在, orderId={}", payResponse.getOrderId());
            throw new SellException(ResultEnum.ORDER_NOT_EXIST);
        }

        //判断金额是否一致(0.10   0.1)
        if (!MathUtil.equals(payResponse.getOrderAmount(), orderDTO.getOrderAmount().doubleValue())) {
            log.error("【微信支付】异步通知, 订单金额不一致, orderId={}, 微信通知金额={}, 系统金额={}",
                    payResponse.getOrderId(),
                    payResponse.getOrderAmount(),
                    orderDTO.getOrderAmount());
            throw new SellException(ResultEnum.WXPAY_NOTIFY_MONEY_VERIFY_ERROR);
        }

        //修改订单的支付状态
        orderService.paid(orderDTO);

        return payResponse;
    }

    /**
     * 退款
     * @param orderDTO
     */
    @Override
    public RefundResponse refund(OrderDTO orderDTO) {
        RefundRequest refundRequest = new RefundRequest();
        refundRequest.setOrderId(orderDTO.getOrderId());
        refundRequest.setOrderAmount(orderDTO.getOrderAmount().doubleValue());
        refundRequest.setPayTypeEnum(BestPayTypeEnum.WXPAY_H5);
        log.info("【微信退款】request={}", JsonUtil.toJson(refundRequest));

        RefundResponse refundResponse = bestPayService.refund(refundRequest);
        log.info("【微信退款】response={}", JsonUtil.toJson(refundResponse));

        return refundResponse;
    }
}
```

## 8-2 发起微信支付-后端（下）

### WechatPayConfig

```java
@Component
public class WechatPayConfig {

    @Autowired
    private WechatAccountConfig accountConfig;

    @Bean
    public BestPayServiceImpl bestPayService() {
        BestPayServiceImpl bestPayService = new BestPayServiceImpl();
        bestPayService.setWxPayH5Config(wxPayH5Config());
        return bestPayService;
    }

    @Bean
    public WxPayH5Config wxPayH5Config() {
        WxPayH5Config wxPayH5Config = new WxPayH5Config();
        wxPayH5Config.setAppId(accountConfig.getMpAppId());
        wxPayH5Config.setAppSecret(accountConfig.getMpAppSecret());
        wxPayH5Config.setMchId(accountConfig.getMchId());
        wxPayH5Config.setMchKey(accountConfig.getMchKey());
        wxPayH5Config.setKeyPath(accountConfig.getKeyPath());
        wxPayH5Config.setNotifyUrl(accountConfig.getNotifyUrl());
        return wxPayH5Config;
    }
}
```

## 8-3 在网页发起支付

## 8-4 动态注入参数发起支付

## 8-5 微信异步通知（上）

## 8-6 微信异步通知（下）

## 8-7 微信退款

## 8-8 补充：使用测试号实现授权

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229110643282.png" alt="image-20200229110643282" style="zoom:33%;" />

# 第9章 卖家端订单

## 9-1 卖家订单-service

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229111824107.png" alt="image-20200229111824107" style="zoom:33%;" />

## 9-2 卖家-订单-controller（上）

### SellerOrderController

```java
@Controller
@RequestMapping("/seller/order")
@Slf4j
public class SellerOrderController {

    @Autowired
    private OrderService orderService;

    /**
     * 订单列表
     * @param page 第几页, 从1页开始
     * @param size 一页有多少条数据
     * @return
     */
    @GetMapping("/list")
    public ModelAndView list(@RequestParam(value = "page", defaultValue = "1") Integer page,
                             @RequestParam(value = "size", defaultValue = "10") Integer size,
                             Map<String, Object> map) {
        PageRequest request = PageRequest.of(page - 1, size);
        Page<OrderDTO> orderDTOPage = orderService.findList(request);
        map.put("orderDTOPage", orderDTOPage);
        map.put("currentPage", page);
        map.put("size", size);
//        orderDTOPage.getTotalPages()
        return new ModelAndView("order/list", map);
    }

    /**
     * 取消订单
     * @param orderId
     * @return
     */
    @GetMapping("/cancel")
    public ModelAndView cancel(@RequestParam("orderId") String orderId,
                               Map<String, Object> map) {
        try {
            OrderDTO orderDTO = orderService.findOne(orderId);
            orderService.cancel(orderDTO);
        } catch (SellException e) {
            log.error("【卖家端取消订单】发生异常{}", e);
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/order/list");
            return new ModelAndView("common/error", map);
        }

        map.put("msg", ResultEnum.ORDER_CANCEL_SUCCESS.getMessage());
        map.put("url", "/sell/seller/order/list");
        return new ModelAndView("common/success");
    }

    /**
     * 订单详情
     * @param orderId
     * @param map
     * @return
     */
    @GetMapping("/detail")
    public ModelAndView detail(@RequestParam("orderId") String orderId,
                               Map<String, Object> map) {
        OrderDTO orderDTO = new OrderDTO();
        try {
            orderDTO = orderService.findOne(orderId);
        }catch (SellException e) {
            log.error("【卖家端查询订单详情】发生异常{}", e);
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/order/list");
            return new ModelAndView("common/error", map);
        }

        map.put("orderDTO", orderDTO);
        return new ModelAndView("order/detail", map);
    }

    /**
     * 完结订单
     * @param orderId
     * @param map
     * @return
     */
    @GetMapping("/finish")
    public ModelAndView finished(@RequestParam("orderId") String orderId,
                                 Map<String, Object> map) {
        try {
            OrderDTO orderDTO = orderService.findOne(orderId);
            orderService.finish(orderDTO);
        } catch (SellException e) {
            log.error("【卖家端完结订单】发生异常{}", e);
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/order/list");
            return new ModelAndView("common/error", map);
        }

        map.put("msg", ResultEnum.ORDER_FINISH_SUCCESS.getMessage());
        map.put("url", "/sell/seller/order/list");
        return new ModelAndView("common/success");
    }
}
```

## 9-3 卖家-订单-controller（下）

### CodeEnum

```java
public interface CodeEnum {
    Integer getCode();
}
```

### EnumUtil

```java
public class EnumUtil {

    public static <T extends CodeEnum> T getByCode(Integer code, Class<T> enumClass) {
        for (T each: enumClass.getEnumConstants()) {
            if (code.equals(each.getCode())) {
                return each;
            }
        }
        return null;
    }
}
```

## 9-4 卖家订单-controller-翻页

## 9-5 卖家订单-controller-取消订单

## 9-6 卖家订单-controller-订单详情

## 9-7 卖家订单-controller-完结订单

# 第10章 卖家端通用功能和上下架

## 10-1 关于模版的小技巧

## 10-2 实现边栏

## 10-3 实现商品列表

## 10-4 商品上下架-service

## 10-5 商品上下架-controller

### SellerProductController

```java
@Controller
@RequestMapping("/seller/product")
public class SellerProductController {

    @Autowired
    private ProductService productService;

    @Autowired
    private CategoryService categoryService;

    /**
     * 列表
     * @param page
     * @param size
     * @param map
     * @return
     */
    @GetMapping("/list")
    public ModelAndView list(@RequestParam(value = "page", defaultValue = "1") Integer page,
                             @RequestParam(value = "size", defaultValue = "10") Integer size,
                             Map<String, Object> map) {
        PageRequest request = PageRequest.of(page - 1, size);
        Page<ProductInfo> productInfoPage = productService.findAll(request);
        map.put("productInfoPage", productInfoPage);
        map.put("currentPage", page);
        map.put("size", size);
        return new ModelAndView("product/list", map);
    }

    /**
     * 商品上架
     * @param productId
     * @param map
     * @return
     */
    @RequestMapping("/on_sale")
    public ModelAndView onSale(@RequestParam("productId") String productId,
                               Map<String, Object> map) {
        try {
            productService.onSale(productId);
        } catch (SellException e) {
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/product/list");
            return new ModelAndView("common/error", map);
        }

        map.put("url", "/sell/seller/product/list");
        return new ModelAndView("common/success", map);
    }
    /**
     * 商品下架
     * @param productId
     * @param map
     * @return
     */
    @RequestMapping("/off_sale")
    public ModelAndView offSale(@RequestParam("productId") String productId,
                               Map<String, Object> map) {
        try {
            productService.offSale(productId);
        } catch (SellException e) {
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/product/list");
            return new ModelAndView("common/error", map);
        }

        map.put("url", "/sell/seller/product/list");
        return new ModelAndView("common/success", map);
    }

    @GetMapping("/index")
    public ModelAndView index(@RequestParam(value = "productId", required = false) String productId,
                      Map<String, Object> map) {
        if (!StringUtils.isEmpty(productId)) {
            ProductInfo productInfo = productService.findOne(productId);
            map.put("productInfo", productInfo);
        }

        //查询所有的类目
        List<ProductCategory> categoryList = categoryService.findAll();
        map.put("categoryList", categoryList);

        return new ModelAndView("product/index", map);
    }

    /**
     * 保存/更新
     * @param form
     * @param bindingResult
     * @param map
     * @return
     */
    @PostMapping("/save")
//    @Cacheable(cacheNames = "product", key = "123")
//    @Cacheable(cacheNames = "product", key = "456")
//    @CachePut(cacheNames = "product", key = "123")
    @CacheEvict(cacheNames = "product", allEntries = true, beforeInvocation = true)
    public ModelAndView save(@Valid ProductForm form,
                             BindingResult bindingResult,
                             Map<String, Object> map) {
        if (bindingResult.hasErrors()) {
            map.put("msg", bindingResult.getFieldError().getDefaultMessage());
            map.put("url", "/sell/seller/product/index");
            return new ModelAndView("common/error", map);
        }

        ProductInfo productInfo = new ProductInfo();
        try {
            //如果productId为空, 说明是新增
            if (!StringUtils.isEmpty(form.getProductId())) {
                productInfo = productService.findOne(form.getProductId());
            } else {
                form.setProductId(KeyUtil.genUniqueKey());
            }
            BeanUtils.copyProperties(form, productInfo);
            productService.save(productInfo);
        } catch (SellException e) {
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/product/index");
            return new ModelAndView("common/error", map);
        }

        map.put("url", "/sell/seller/product/list");
        return new ModelAndView("common/success", map);
    }
}
```

# 第11章 卖家端新增商品和类目

## 11-1 卖家商品-新增修改页面

## 11-2 卖家商品-修改表单提交

## 11-3 卖家商品-新增功能 

## 11-4 卖家类目功能开发

### SellerCategoryController

```java
/**卖家类目*/
@Controller
@RequestMapping("/seller/category")
public class SellerCategoryController {

    @Autowired
    private CategoryService categoryService;

    /**
     * 类目列表
     * @param map
     * @return
     */
    @GetMapping("/list")
    public ModelAndView list(Map<String, Object> map) {
        List<ProductCategory> categoryList = categoryService.findAll();
        map.put("categoryList", categoryList);
        return new ModelAndView("category/list", map);
    }

    /**
     * 展示
     * @param categoryId
     * @param map
     * @return
     */
    @GetMapping("/index")
    public ModelAndView index(@RequestParam(value = "categoryId", required = false) Integer categoryId,
                              Map<String, Object> map) {
        if (categoryId != null) {
            ProductCategory productCategory = categoryService.findOne(categoryId);
            map.put("category", productCategory);
        }

        return new ModelAndView("category/index", map);
    }

    /**
     * 保存/更新
     * @param form
     * @param bindingResult
     * @param map
     * @return
     */
    @PostMapping("/save")
    public ModelAndView save(@Valid CategoryForm form,
                             BindingResult bindingResult,
                             Map<String, Object> map) {
        if (bindingResult.hasErrors()) {
            map.put("msg", bindingResult.getFieldError().getDefaultMessage());
            map.put("url", "/sell/seller/category/index");
            return new ModelAndView("common/error", map);
        }

        ProductCategory productCategory = new ProductCategory();
        try {
            if (form.getCategoryId() != null) {
                productCategory = categoryService.findOne(form.getCategoryId());
            }
            BeanUtils.copyProperties(form, productCategory);
            categoryService.save(productCategory);
        } catch (SellException e) {
            map.put("msg", e.getMessage());
            map.put("url", "/sell/seller/category/index");
            return new ModelAndView("common/error", map);
        }

        map.put("url", "/sell/seller/category/list");
        return new ModelAndView("common/success", map);
    }
}
```

# 第12章 买家和卖家端联通

## 12-1 分布式session理论(上) 

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229170253842.png" alt="image-20200229170253842" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229191546320.png" alt="image-20200229191546320" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229191616775.png" alt="image-20200229191616775" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229193609630.png" alt="image-20200229193609630" style="zoom: 33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229193837565.png" alt="image-20200229193837565" style="zoom:33%;" />

## 12-2 分布式session理论(下)

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229194218271.png" alt="image-20200229194218271" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229194237418.png" alt="image-20200229194237418" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229194252859.png" alt="image-20200229194252859" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229221634576.png" alt="image-20200229221634576" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200229222352663.png" alt="image-20200229222352663" style="zoom:33%;" />

## 12-3 卖家信息表-dao开发

### SellerInfoRepository 

```java
public interface SellerInfoRepository extends JpaRepository<SellerInfo, String> {
    SellerInfo findByOpenid(String openid);
}
```

## 12-4 卖家扫码登录service开发

### SellerService

```java
/**卖家端*/
public interface SellerService {

    /**
     * 通过openid查询卖家端信息
     * @param openid
     * @return
     */
    SellerInfo findSellerInfoByOpenid(String openid);
}
```

### SellerServiceImpl

```java
@Service
public class SellerServiceImpl implements SellerService {

    @Autowired
    private SellerInfoRepository repository;

    @Override
    public SellerInfo findSellerInfoByOpenid(String openid) {
        return repository.findByOpenid(openid);
    }
}
```

## 12-5 卖家扫码登录获取openid

### WechatOpenConfig

```java
@Component
public class WechatOpenConfig {

    @Autowired
    private WechatAccountConfig accountConfig;

    @Bean
    public WxMpService wxOpenService() {
        WxMpService wxOpenService = new WxMpServiceImpl();
        wxOpenService.setWxMpConfigStorage(wxOpenConfigStorage());
        return wxOpenService;
    }

    @Bean
    public WxMpConfigStorage wxOpenConfigStorage() {
        WxMpInMemoryConfigStorage wxMpInMemoryConfigStorage = new WxMpInMemoryConfigStorage();
        wxMpInMemoryConfigStorage.setAppId(accountConfig.getOpenAppId());
        wxMpInMemoryConfigStorage.setSecret(accountConfig.getOpenAppSecret());
        return wxMpInMemoryConfigStorage;
    }
}
```

ProjectUrlConfig

```java
@Data
@ConfigurationProperties(prefix = "project-url")
@Component
public class ProjectUrlConfig {

    /**
     * 微信公众平台授权url
     */
    public String wechatMpAuthorize;

    /**
     * 微信开放平台授权url
     */
    public String wechatOpenAuthorize;

    /**
     * 点餐系统
     */
    public String sell;
}
```

## 12-6 登录成功

### SellerUserController

```java
@Controller
@RequestMapping("/seller")
public class SellerUserController {

    @Autowired
    private SellerService sellerService;

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Autowired
    private ProjectUrlConfig projectUrlConfig;

    @GetMapping("/login")
    public ModelAndView login(@RequestParam("openid") String openid,
                              HttpServletResponse response,
                              Map<String, Object> map) {

        //1. openid去和数据库里的数据匹配
        SellerInfo sellerInfo = sellerService.findSellerInfoByOpenid(openid);
        if (sellerInfo == null) {
            map.put("msg", ResultEnum.LOGIN_FAIL.getMessage());
            map.put("url", "/sell/seller/order/list");
            return new ModelAndView("common/error");
        }

        //2. 设置token至redis
        String token = UUID.randomUUID().toString();
        Integer expire = RedisConstant.EXPIRE;

        redisTemplate.opsForValue().set(String.format(RedisConstant.TOKEN_PREFIX, token), openid, expire, TimeUnit.SECONDS);

        //3. 设置token至cookie
        CookieUtil.set(response, CookieConstant.TOKEN, token, expire);

        return new ModelAndView("redirect:" + projectUrlConfig.getSell() + "/sell/seller/order/list");

    }

    @GetMapping("/logout")
    public ModelAndView logout(HttpServletRequest request,
                       HttpServletResponse response,
                       Map<String, Object> map) {
        //1. 从cookie里查询
        Cookie cookie = CookieUtil.get(request, CookieConstant.TOKEN);
        if (cookie != null) {
            //2. 清除redis
            redisTemplate.opsForValue().getOperations().delete(String.format(RedisConstant.TOKEN_PREFIX, cookie.getValue()));

            //3. 清除cookie
            CookieUtil.set(response, CookieConstant.TOKEN, null, 0);
        }

        map.put("msg", ResultEnum.LOGOUT_SUCCESS.getMessage());
        map.put("url", "/sell/seller/order/list");
        return new ModelAndView("common/success", map);
    }
}
```

### pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### RedisConstant

```java
/**
 * redis常量
 */
public interface RedisConstant {

    String TOKEN_PREFIX = "token_%s";

    Integer EXPIRE = 7200; //2小时
}
```

### CookieUtil

```java
/**
 * cookie工具类
 */
public class CookieUtil {

    /**
     * 设置
     * @param response
     * @param name
     * @param value
     * @param maxAge
     */
    public static void set(HttpServletResponse response,
                           String name,
                           String value,
                           int maxAge) {
        Cookie cookie = new Cookie(name, value);
        cookie.setPath("/");
        cookie.setMaxAge(maxAge);
        response.addCookie(cookie);
    }

    /**
     * 获取cookie
     * @param request
     * @param name
     * @return
     */
    public static Cookie get(HttpServletRequest request,
                           String name) {
        Map<String, Cookie> cookieMap = readCookieMap(request);
        if (cookieMap.containsKey(name)) {
            return cookieMap.get(name);
        }else {
            return null;
        }
    }

    /**
     * 将cookie封装成Map
     * @param request
     * @return
     */
    private static Map<String, Cookie> readCookieMap(HttpServletRequest request) {
        Map<String, Cookie> cookieMap = new HashMap<>();
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            for (Cookie cookie: cookies) {
                cookieMap.put(cookie.getName(), cookie);
            }
        }
        return cookieMap;
    }
}
```

## 12-7 登出成功

## 12-8 AOP实现身份验证

### SellerAuthorizeAspect

```java
@Aspect
@Component
@Slf4j
public class SellerAuthorizeAspect {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Pointcut("execution(public * com.imooc.controller.Seller*.*(..))" +
    "&& !execution(public * com.imooc.controller.SellerUserController.*(..))")
    public void verify() {}

    @Before("verify()")
    public void doVerify() {
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();

        //查询cookie
        Cookie cookie = CookieUtil.get(request, CookieConstant.TOKEN);
        if (cookie == null) {
            log.warn("【登录校验】Cookie中查不到token");
            throw new SellerAuthorizeException();
        }

        //去redis里查询
        String tokenValue = redisTemplate.opsForValue().get(String.format(RedisConstant.TOKEN_PREFIX, cookie.getValue()));
        if (StringUtils.isEmpty(tokenValue)) {
            log.warn("【登录校验】Redis中查不到token");
            throw new SellerAuthorizeException();
        }
    }
}
```

### SellExceptionHandler

```java
@ControllerAdvice
public class SellExceptionHandler {

    @Autowired
    private ProjectUrlConfig projectUrlConfig;

    //拦截登录异常
    //http://sell.natapp4.cc/sell/wechat/qrAuthorize?returnUrl=http://sell.natapp4.cc/sell/seller/login
    @ExceptionHandler(value = SellerAuthorizeException.class)
//    @ResponseStatus(HttpStatus.FORBIDDEN)
    public ModelAndView handlerAuthorizeException() {
        return new ModelAndView("redirect:"
        .concat(projectUrlConfig.getWechatOpenAuthorize())
        .concat("/sell/wechat/qrAuthorize")
        .concat("?returnUrl=")
        .concat(projectUrlConfig.getSell())
        .concat("/sell/seller/login"));
    }

    @ExceptionHandler(value = SellException.class)
    @ResponseBody
    public ResultVO handlerSellerException(SellException e) {
        return ResultVOUtil.error(e.getCode(), e.getMessage());
    }

    @ExceptionHandler(value = ResponseBankException.class)
    @ResponseStatus(HttpStatus.FORBIDDEN)
    public void handleResponseBankException() {

    }
}
```

## 12-9 微信模版消息推送

### PushMessageService

```java
/**
 * 推送消息
 */
public interface PushMessageService {

    /**
     * 订单状态变更消息
     * @param orderDTO
     */
    void orderStatus(OrderDTO orderDTO);
}
```

### PushMessageServiceImpl

```java
@Service
@Slf4j
public class PushMessageServiceImpl implements PushMessageService {

    @Autowired
    private WxMpService wxMpService;

    @Autowired
    private WechatAccountConfig accountConfig;

    @Override
    public void orderStatus(OrderDTO orderDTO) {
        WxMpTemplateMessage templateMessage = new WxMpTemplateMessage();
        templateMessage.setTemplateId(accountConfig.getTemplateId().get("orderStatus"));
        templateMessage.setToUser(orderDTO.getBuyerOpenid());

        List<WxMpTemplateData> data = Arrays.asList(
                new WxMpTemplateData("first", "亲，请记得收货。"),
                new WxMpTemplateData("keyword1", "微信点餐"),
                new WxMpTemplateData("keyword2", "18868812345"),
                new WxMpTemplateData("keyword3", orderDTO.getOrderId()),
                new WxMpTemplateData("keyword4", orderDTO.getOrderStatusEnum().getMessage()),
                new WxMpTemplateData("keyword5", "￥" + orderDTO.getOrderAmount()),
                new WxMpTemplateData("remark", "欢迎再次光临！")
        );
        templateMessage.setData(data);
        try {
            wxMpService.getTemplateMsgService().sendTemplateMsg(templateMessage);
        }catch (WxErrorException e) {
            log.error("【微信模版消息】发送失败, {}", e);
        }
    }
}
```

## 12-10 webSocket消息推送

### WebSocket

```java
@Component
@ServerEndpoint("/webSocket")
@Slf4j
public class WebSocket {

    private Session session;

    private static CopyOnWriteArraySet<WebSocket> webSocketSet = new CopyOnWriteArraySet<>();

    @OnOpen
    public void onOpen(Session session) {
        this.session = session;
        webSocketSet.add(this);
        log.info("【websocket消息】有新的连接, 总数:{}", webSocketSet.size());
    }

    @OnClose
    public void onClose() {
        webSocketSet.remove(this);
        log.info("【websocket消息】连接断开, 总数:{}", webSocketSet.size());
    }

    @OnMessage
    public void onMessage(String message) {
        log.info("【websocket消息】收到客户端发来的消息:{}", message);
    }

    public void sendMessage(String message) {
        for (WebSocket webSocket: webSocketSet) {
            log.info("【websocket消息】广播消息, message={}", message);
            try {
                webSocket.session.getBasicRemote().sendText(message);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

# 第13章 项目优化

## 13-1 异常捕获

## 13-2 mybatis注解方式使用_A

### pom.xml

```xml
<dependency>
   <groupId>org.mybatis.spring.boot</groupId>
   <artifactId>mybatis-spring-boot-starter</artifactId>
   <version>1.2.0</version>
</dependency>
```

### ProductCategoryMapper

```java
public interface ProductCategoryMapper {

    @Insert("insert into product_category(category_name, category_type) values (#{categoryName, jdbcType=VARCHAR}, #{category_type, jdbcType=INTEGER})")
    int insertByMap(Map<String, Object> map);

    @Insert("insert into product_category(category_name, category_type) values (#{categoryName, jdbcType=VARCHAR}, #{categoryType, jdbcType=INTEGER})")
    int insertByObject(ProductCategory productCategory);

    @Select("select * from product_category where category_type = #{categoryType}")
    @Results({
            @Result(column = "category_id", property = "categoryId"),
            @Result(column = "category_name", property = "categoryName"),
            @Result(column = "category_type", property = "categoryType")
    })
    ProductCategory findByCategoryType(Integer categoryType);

    @Select("select * from product_category where category_name = #{categoryName}")
    @Results({
            @Result(column = "category_id", property = "categoryId"),
            @Result(column = "category_name", property = "categoryName"),
            @Result(column = "category_type", property = "categoryType")
    })
    List<ProductCategory> findByCategoryName(String categoryName);

    @Update("update product_category set category_name = #{categoryName} where category_type = #{categoryType}")
    int updateByCategoryType(@Param("categoryName") String categoryName,
                             @Param("categoryType") Integer categoryType);

    @Update("update product_category set category_name = #{categoryName} where category_type = #{categoryType}")
    int updateByObject(ProductCategory productCategory);

    @Delete("delete from product_category where category_type = #{categoryType}")
    int deleteByCategoryType(Integer categoryType);

    ProductCategory selectByCategoryType(Integer categoryType);
}
```

## 13-3 mybatis注解方式使用_B

### application-dev.yml

```yaml
projectUrl:
  wechatMpAuthorize: http://lsx888.natapp1.cc
  wechatOpenAuthorize: http://lsx888.natapp1.cc
  sell: http://lsx888.natapp1.cc
logging:
  level:
    com.imooc.dataobject.mapper: trace
mybatis:
  mapper-locations: classpath:mapper/*.xml
```

### ProductCategoryDao

```java
public class ProductCategoryDao {

    @Autowired
    ProductCategoryMapper mapper;

    public int insertByMap(Map<String, Object> map) {
        return mapper.insertByMap(map);
    }
}
```

## 13-4 mybatis xml方式使用

### ProductCategoryMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.imooc.dataobject.mapper.ProductCategoryMapper" >

    <resultMap id="BaseResultMap" type="com.imooc.dataobject.ProductCategory">
        <id column="category_id" property="categoryId" jdbcType="INTEGER" />
        <id column="category_name" property="categoryName" jdbcType="VARCHAR" />
        <id column="category_type" property="categoryType" jdbcType="INTEGER" />
    </resultMap>

    <select id="selectByCategoryType" resultMap="BaseResultMap" parameterType="java.lang.Integer">
        select category_id, category_name, category_type
        from product_category
        where category_type = #{category_type, jdbcType=INTEGER}
    </select>
</mapper>
```

## 13-5 jpa和mybatis的选择

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301142043771.png" alt="image-20200301142043771" style="zoom:33%;" />

## 13-6 ab压测介绍

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301142127294.png" alt="image-20200301142127294" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301142450333.png" alt="image-20200301142450333" style="zoom:33%;" />

## 13-7 synchronized处理并发

### SecKillController

```java
@RestController
@RequestMapping("/skill")
@Slf4j
public class SecKillController {

    @Autowired
    private SecKillService secKillService;

    /**
     * 查询秒杀活动特价商品的信息
     * @param productId
     * @return
     */
    @GetMapping("/query/{productId}")
    public String query(@PathVariable String productId)throws Exception
    {
        return secKillService.querySecKillProductInfo(productId);
    }


    /**
     * 秒杀，没有抢到获得"哎呦喂,xxxxx",抢到了会返回剩余的库存量
     * @param productId
     * @return
     * @throws Exception
     */
    @GetMapping("/order/{productId}")
    public String skill(@PathVariable String productId)throws Exception
    {
        log.info("@skill request, productId:" + productId);
        secKillService.orderProductMockDiffUser(productId);
        return secKillService.querySecKillProductInfo(productId);
    }
}
```

### SecKillService

```java
public interface SecKillService {

    /**
     * 查询秒杀活动特价商品的信息
     * @param productId
     * @return
     */
    String querySecKillProductInfo(String productId);

    /**
     * 模拟不同用户秒杀同一商品的请求
     * @param productId
     * @return
     */
    void orderProductMockDiffUser(String productId);

}
```

### SecKillServiceImpl

```java
@Service
public class SecKillServiceImpl implements SecKillService {

    private static final int TIMEOUT = 10 * 1000; //超时时间 10s

    @Autowired
    private RedisLock redisLock;

    /**
     * 国庆活动，皮蛋粥特价，限量100000份
     */
    static Map<String,Integer> products;
    static Map<String,Integer> stock;
    static Map<String,String> orders;
    static
    {
        /**
         * 模拟多个表，商品信息表，库存表，秒杀成功订单表
         */
        products = new HashMap<>();
        stock = new HashMap<>();
        orders = new HashMap<>();
        products.put("123456", 100000);
        stock.put("123456", 100000);
    }

    private String queryMap(String productId)
    {
        return "国庆活动，皮蛋粥特价，限量份"
                + products.get(productId)
                +" 还剩：" + stock.get(productId)+" 份"
                +" 该商品成功下单用户数目："
                +  orders.size() +" 人" ;
    }

    @Override
    public String querySecKillProductInfo(String productId)
    {
        return this.queryMap(productId);
    }

    @Override
    public void orderProductMockDiffUser(String productId)
    {
        //加锁

        //1.查询该商品库存，为0则活动结束。
        int stockNum = stock.get(productId);
        if(stockNum == 0) {
            throw new SellException(100,"活动结束");
        }else {
            //2.下单(模拟不同用户openid不同)
            orders.put(KeyUtil.genUniqueKey(),productId);
            //3.减库存
            stockNum =stockNum-1;
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            stock.put(productId,stockNum);
        }

        //解锁

    }
}
```

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301143432917.png" alt="image-20200301143432917" style="zoom:33%;" />

## 13-8 redis分布式锁

### RedisLock

```java
@Component
@Slf4j
public class RedisLock {

    @Autowired
    private StringRedisTemplate redisTemplate;

    /**
     * 加锁
     * @param key
     * @param value 当前时间+超时时间
     * @return
     */
    public boolean lock(String key, String value) {
        if(redisTemplate.opsForValue().setIfAbsent(key, value)) {
            return true;
        }
        //currentValue=A   这两个线程的value都是B  其中一个线程拿到锁
        String currentValue = redisTemplate.opsForValue().get(key);
        //如果锁过期
        if (!StringUtils.isEmpty(currentValue)
                && Long.parseLong(currentValue) < System.currentTimeMillis()) {
            //获取上一个锁的时间
            String oldValue = redisTemplate.opsForValue().getAndSet(key, value);
            if (!StringUtils.isEmpty(oldValue) && oldValue.equals(currentValue)) {
                return true;
            }
        }

        return false;
    }

    /**
     * 解锁
     * @param key
     * @param value
     */
    public void unlock(String key, String value) {
        try {
            String currentValue = redisTemplate.opsForValue().get(key);
            if (!StringUtils.isEmpty(currentValue) && currentValue.equals(value)) {
                redisTemplate.opsForValue().getOperations().delete(key);
            }
        }catch (Exception e) {
            log.error("【redis分布式锁】解锁异常, {}", e);
        }
    }
}
```

## 13-9 redis缓存的使用(上)

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301145306332.png" alt="image-20200301145306332" style="zoom:33%;" />

启动缓存，加注解

### SellApplication

```java
@SpringBootApplication
@MapperScan(basePackages = "com.imooc.dataobject.mapper")
@EnableCaching
public class SellApplication {

   public static void main(String[] args) {
      SpringApplication.run(SellApplication.class, args);
   }
}
```

## 13-10 redis缓存的使用(下) 

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301150843390.png" alt="image-20200301150843390" style="zoom:33%;" />

# 第14章 项目部署

## 14-1 项目部署

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301150921644.png" alt="image-20200301150921644" style="zoom:33%;" />

# 第15章 课程总结

## 15-1 总结 

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301151857162.png" alt="image-20200301151857162" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301151950598.png" alt="image-20200301151950598" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301152008013.png" alt="image-20200301152008013" style="zoom:33%;" />

# 第16章 SpringBoot2.x版

## 16-1 抓包

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301152422087.png" alt="image-20200301152422087" style="zoom:33%;" />

## 16-2 升级到2.1.1

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301153505213.png" alt="image-20200301153505213" style="zoom:33%;" />

## 16-3 支付理论

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301153530928.png" alt="image-20200301153530928" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301153554153.png" alt="image-20200301153554153" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301153616983.png" alt="image-20200301153616983" style="zoom:33%;" />

## 16-4 授权1

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301153736864.png" alt="image-20200301153736864" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301154229334.png" alt="image-20200301154229334" style="zoom:33%;" />

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301154140216.png" alt="image-20200301154140216" style="zoom:33%;" />

## 16-5 授权2

## 16-6 网络不通调试方法

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301155003404.png" alt="image-20200301155003404" style="zoom:33%;" />

## 16-7 支付1

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301155025515.png" alt="image-20200301155025515" style="zoom:33%;" />

## 16-8 支付2

<img src="https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301155403414.png" alt="image-20200301155403414" style="zoom:33%;" />

![image-20200301155425546](https://gitee.com/woshiamiaojiang/image-hosting/raw/master/image-20200301155425546.png)