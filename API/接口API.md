# 顾客版

## 首页

### 搜索

​	URL:     /home/search?text=xxxxxx

​	方法:   GET

​	参数:  text = xxxxx

​	返回示例:      返回的跟商家列表的很像

```javascript
[
    {},
    {},
    ......
]
```

​	另外操作:    字符串去除首尾空格,  插入 search 表中，用于做 热搜

### 热搜

​	URL:  /home/hot

​	方法:  GET

​	参数: 无

​	返回示例:

```javascript
[ '鱼香肉丝', '自选餐', .... ]  
// 前端
```

​	逻辑思路: 

​	把每一次的搜索都放入到一个 搜索记录表中,   然后以 content 字段分组 求和  再desc（降序） limit



### 轮播图

​	静态资源路径

​	/public/img/home/bander1.jpg

​	........

​    餐厅图片



### 分类

​	URL:	/home/category?cate=xxxx

​	参数: 	cate=xxx

​	方法:	GET

​	返回示例:	

```javascript
[
    {
        商家列表中的属性
    },
    {},
    {},
    ......
]
```

​	逻辑提示:	去 shop_category表中 模糊查询，还有去



### 预定 TOP榜

​	URL:	/home/reservetop

​	方法: GET

​	参数： 无

​	返回示例：

​	待修改 ！！！

​			

### 商家列表

​	URL:	/home/shoplist

​	方法:	GET

​	参数： 无

​	返回示例:

```javascript
[
    {
        name: '商家名字',
        ......
    },
    {},
    {},
    {},
    ......
]
```



## 我的订单

​	这个应该 放到  我的, 我想做的显眼一点.   |    商讨一下吧



​	操作  shop_order 表

​	概括: 	这个路由是用来查看用户预定的有哪些菜

​	URL：	/order

​	参数:	uid=xxx 用户ID

​	方法: 	GET

​	返回示例:	结构类似于 商家版-订单管理 

```javascript
[	// 按照 name 菜名分类
	{
		name: '黑椒鸡柳饭',
		list: [
			{
                "id": 34,
                "price": 10,
                "time": "2020-10-08T01:34:00.000Z",
                "uname": "TEST用户名"
            },
            {},
            .....
		]
	},
	{
		name： ‘’
	}
]
```



### 取消预定

​	概括:	删除当前的订单,  需要做个判断,   大于**1小时** 不能删除.

​	URL:	/order/remove?_id=xxxx

​	参数	_id=xxx    _id 是那个  shop_order _id字段

​	方法:	GET

​	返回:   字符串1表示取消成功,      字符串0表示  时间超时 就是超过一个小时了.  





## 我的

​	





## 详情页

浏览数

### 	点赞功能

​		概括：	点赞。因为是 放入数据，所以用POST

​		URL：	/detail/like

​		方法：	POST

​		参数：	sid=xxxxlike=true|false&  商家ID

​		说明， like=true | false   true说明 是点赞.   false 说明取消点赞   。注意这时  字符串

​		

​		数据表：  新建一个 shop_like 表

​		还有一种思路是， 这个路由里，然后更新 商家表中的相关字段。

### 	头部

​		概括：	商家名、评分、评价人数、点赞人数、是否点赞、展示图片、餐厅。因为是拿数据  所以用GET，语义明确。

​		URL：	/detail/top?sid=xxxxx & uid=xxxx

​		方法：	GET

​		参数：	sid=xxxx & uid=xxx

​		依赖：    评论表、商家后台

​		返回示例： 

```javascript
[
    {
        shopname: '天下好面',
        score:   '评分'                // shop_comment 表
        commentNum: '评论人数'          // shop_comment
        isLike:  1|0                    // 是否点赞  // shop_like
        showList: ['http://', 'http://']    // shop_show
		canteen：  '哪个餐厅'
    }
]
```

可以在  商家表， 加些字段    评分、评价人数、点赞人数、展示图片字段

但是这样做， 就需要再其他路由 操作的时候，进行 修改。

综上考虑，我决定 不用这种 耦合度高的了。



### 菜品展示

​	概括：	用来获取 商家的菜品列表

​	URL：	/detail/menu？sid=xxx

​	方法：	GET

​	返回示例：	

```javascript
[
    {
        mname: '菜名',
        mprice: '价格',
        mimg: '菜的图片URL'
        mid： 
    }
]
```

​	注意：	只取 上架的菜品

​	

### 	预定[功能]

​	shop_order 表



### 取消预定

​	返回： 200，说明取消预约成功

​	返回： 400， 说明时间超时

​		返回 500，说明程序出错



### 评论展示

​	获取评论

### 评论[功能]





## 账号

### 登录

​	URL: 	/user/login

​	方法: 	POST

​	参数: uname=xxxx&upwd=xxxxx

```javascript
{
    uname: '',
    upwd: ''
}
```

​	返回示例: 

```javascript
1  代表登录成功
0  代表密码错误
-1  此用户不存在
```



### 注册

#### 验证重复

​	概括:   这个 接口用来验证   用户名是否重复	

​	URL:   /user/sigin?uname=xxxxx

​	方法:  POST

​	返回:  字符串 1代表用户名没有重复,    0代表用户名重复

​	

#### 插入数据库

​	概括:	就是注册功能的本质

​	URL:	/user/sigin

​	方法:	POST

​	参数:	uname=xxxxx&upwd=xxxxxx

​	返回:	字符串 1代表插入成功(注册成功),    0代表重新注册

​	安全：	为了安全起见， 把 uname 设置为 唯一约束



















# 商家版

## 注册用户名判断

​	URL： /user/sigincheck?mname

​	方法： GET

​	参数:    mname=xxxx

​	返回： 字符串1代表用户名可以使用。    0代表不能使用



## 注册

​	概括:   

​	URL:  /user/sigin

​	

​	方法: POST

​	返回:   字符串1代表插入成功(注册成功),   0反之



## 登录

​	URL： /user/login

​	方法： POST

​	返回： 字符串1代表登录成功，0反之



## 首页

高武杰做

### 查看操作

​	`今日订单数`、`今日预收入`、`点赞数`   有新增点赞数  显示小提示文字

![image-20201108164033126](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201108164033126.png)

​	交易额  -  预收入

​	订单数 、点赞数 、 浏览量

​	订单动态 - 



​	图表 - 每日预记收入 

​	商品预定



可以 优化.   传递 时间参数,   如果为空 , 那就是查询  所有

### 顶部

​	概括： 预收入、订单量、点赞数、浏览量   

​	需要:      shop_order表,  把id替换成 对应的吧  

​	URL： /home/base           base 基础信息

​	方法： GET

​	参数： sid=xxx

​	返回：

```
{
	income: ‘预收入’，
	Monthincome: '月预收入',
	totalOrderNum: '总订单量'，
	noworderNum: '今日订单量'，
	totalLikeNum： ‘总点赞数’，
	nowLikeNum： ‘今日’
	totalBrowseNum： ‘总浏览量’，
	nowBrowseNum： ‘今日’
}
```



### 实时订单状态

​	概括： 实时订单, 

​	URL： /home/order?sid=xxx

​	参数:  sid = xx

​	方法:  GET

​	返回： 

​		两个

​		查询数据库( shop_order )， 取出最近时间的两个订单   - MySQL排序 和 limit

```
[
	{
		time：   ’下单时间‘
		uname：  ’下单用户名‘
		mname：  ’商品名字‘ 
	},
	{
	
	}
]
```

​	

### 预收入图表

​	概括：  应该去查那个表?

​	URL：/home/income?sid=xxx

​	方法： GET

​	参数： sid=xxxx

​	返回： 

```
[
	{
		time: '2020-11-1',
		income: 290
	},
	{
		time: '2020-11-2',
		income: 290
	},
	{
	
	},
	......
]
```



### 菜品销售榜

​	概括： 

​	URL： /home/top?sid=xxxx

​	方法： GET

​	参数： sid=xxx

​	返回： 

​		显示5个

```
[
	{
		name: '商品名字'，
		reserveNum： ’预定量‘
	},
	{},
	{}
]
```





## 订单管理

陈梦佳,  shop_order   订单表是空的,   得自己手动模拟数据 .

currenttime,   这个

### 查看-默认显示今天的订单 

​	概括： 返回该商家的，按菜的名字分组 - 今天 - 昨天- 啥时候的订单都可以看

​				日期选择,   选择哪个日期, 就显示哪个日期的订单.  

   			 月选择 , 选择哪个月就显示哪个月的订单  

​				日期段查询 ，start=2020-12-1   end=2020-12-14

​	URL： /order?sid=xxxx&type=month&time=12

​				/order?sid=xxxx&type=date&time=2020-3-2

​				/order?sid=xxxxx&type=range & start=2020-10-12 23:00:00& end=2020-10-13 24:00:00

​	方法： GET

​	参数： sid=xxx&time=xxxx .  time可以省略, 则表示全部

​	返回： 

```javascript
[
	{	
		"id": 34,
        "price": 10,
        "time": "2020-10-08T01:34:00.000Z",
        "uname": "TEST用户名"
	},
	{},
	....
]


// 优先考虑
[	// 按照 name 菜名分类
	{
		name: '黑椒鸡柳饭',
		list: [
			{
                "id": 34,
                "price": 10,
                "time": "2020-10-08T01:34:00.000Z",
                "uname": "TEST用户名"
            },
            {},
            .....
		]
	},
	{
		name： ‘’
	}
]
```





## 菜品管理 

李小威

#### 查看

​	概括： 把所有的菜都查询出来

​	URL： /menu?sid

​	参数: 	sid=xxx

​	方法： GET

​	返回： 

```javascript
[
    {
        
    },
    {}
]
```



#### 添加

​	概括： 添加一个新的菜

​	URL：/menu/add

​	参数： 菜的图片、名称、价格、商家ID

​	方法： POST

​	返回： 字符串1， 添加成功，返回插入失败



#### 修改

​	概括： 修改价格、图片、名字等

​	URL： /menu/update

​	方法： POST

​	参数： newPrice= & newName & newImg 

​	返回： 字符串1，修改成功。 0反之



#### 删除

​	URL：/menu/remove

​	方法： GET

​	参数： sid=xxxx&mid=xxx

​	返回： 字符串1删除成功， 否则失败



## 评论管理

张磊

### 	查看

​		URL： /comment?sid=xxx

​		方法： GET

​		参数： sid=xxx

​		返回： 	

```javascript
[
    {
        unickname: '',  // 用户昵称,   -- 用户信息
        uimg: '用户头像URL'            -- 用户信息
        imgList: ['htp://', 'http://'], // 先不做图片 JSON
        score: 3.5,
       	content: 'xxxxxx',
        response: 'xxx'
        time: '自动的',
    },
    {
        
    },
    {},
]
```



​	**数据表**

```

```



### 	查看 - 前端需要新功能图片展示 和 回复



### 	回复

​		URL： /comment/response

​		方法： POST

​		参数： **id**=xxxx&response=xxxxxxxxxxxx    id 是 评论表 的主键ID,  字段是 _id   

​		返回： 字符串1 回复成功，0遇到错误

​		说明:	需要对 response 限制 50个字符.



## 商家信息

李阳做



#### 查看

​	概括：点进去显示商家的   各种信息，比如

​	`店名`，`餐厅`,  `标语` , `分类关键字`，`logo`，`展示图片`，

​	展示图片  3 - 6张

​	 数据渲染

​	URL： /info?sid=xxx

​	参数:  sid=xxx

​	方法： GET

​	返回： JSON对象

```javascript
{
    shopname: '天下好面'，
    。。。。。。
}
```



#### 修改

​	概括： /info， POST负责逻辑处理

​	方法： POST

​	参数： sid，控件name（对应后端表中的字段），值



​	`标语`, `展示图片` , `logo` , `分类关键字`

​	商家信息不涉及删除。
