# 物流信息管理系统

开发周期：6天

技术栈：Vue3+Springboot+Mysql。

组件库：ElmentPlus+vant-ui

版本：jdk17，mysql8.0.30，springboot2.7.15，vant4，vue3

开发工具：IDEA2023.2+VScode+ApiPost7

<hr>

## 项目部分页面展示

### 用户篇

#### 桌面端

**用户查询页面**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919171342582.png" alt="image-20230919171342582" />

**用户登录完成的查询页面**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919172228178.png" alt="image-20230919172228178" />

#### 移动端适配

**用户查询页面适配**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919172634900.png" alt="image-20230919172634900" style="zoom:50%;" />

**用户登录完成的查询页面适配：**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173011443.png" alt="image-20230919173011443" style="zoom:50%;" />

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173058540.png" alt="image-20230919173058540" style="zoom:50%;" />

**登录页面适配**



<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173305875.png" alt="image-20230919173305875" style="zoom:50%;" />

### 管理员篇

**普通管理员与超级管理员模块页面相差不大，仅选取超级管理员的部分展示**

**superAdmin主页**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173806279.png" alt="image-20230919173806279" />



**更新表单**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173923249.png" alt="image-20230919173923249" />

**查询全部物流信息**



<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919173956912.png" alt="image-20230919173956912" />

**查看所有员工**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919174123193.png" alt="image-20230919174123193" />

**更新客户信息**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919174211743.png" alt="image-20230919174211743" />

## 项目亮点

1. 采用jwt令牌技术，根据不同类型的用户生成不同类型的令牌返回给前端，前端采用Vue路由守卫实现不同权限用户的管理
2. 采用事务管理，保证了数据的一致性和完整性
3. 数据库使用逻辑外键关联，避免强耦合
4. 使用springmvc拦截器技术，降低服务器压力。

## 前端页面使用细节

### 总体项目介绍图

**项目功能基础流程图**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919180827235.png" alt="image-20230919180827235" />



**项目结构图：**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919182856954.png" alt="image-20230919182856954" />

**项目架构图：**

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20230919181112170.png" alt="image-20230919181112170" />

- **约束**：
  - 用户名和密码为5-12位字母+数字的组合。
  - 用户名和单号需唯一
  - 所有字段除备注外均为非空（但未做空串处理，意思就是添加“  ”这样的空串也会添加到数据库）
  - 昵称默认为不知名用户，最多20个字符
  - 单号最多100个字符，国家，数据，系统单号，转单号，客户单号最多255个字符。备注最多200个字符。

### 根据单号查询物流信息页面（首页）

- 进入页面自动聚焦输入框，输入框内应避免输入空格。查询成功显示对应单号的物流详细信息，查询失败则显示**未知错误（一般是数据输入不合规）**。
- 点击右上角的超链接按钮可以跳转到登录页面。

#### 登录页面

- 输入用户名和密码（字母+数字5-12位，应避免输入空格）。登录成功定向到对应页面，登录失败则提示用户名或密码错误
- 勾选记住我可以把用户名和密码持久化本地进行保存。
- 点击忘记密码跳转到提示页面。

#### 用户已登录页面

- 桌面端：

  - 左上角是一个超链接，跳转到根据单号查询页面。
  - 右上角是退出按钮，确认退出后本地会自动把Authorization值清空，这样就可以切换其他权限用户的账号进行登录。
  - 中间区域为展示物流信息的区域

- 移动端：

  - 功能类似，把用户名放到了中间展示。

  - 进入页面后用户可以看到所有单号信息，点击某一单号跳转到当前单号的详情页面。

#### 超级管理员已登录页面。

​	**下面的所有文本框内容均不允许输入空格（提交时候会自动去除首尾和中间的空格）且并未对输入的数据进行限制/校验处理**

- 物流信息管理

  1. 添加物流信息

     - 点击添加物流信息按钮，弹出添加信息的弹框。在下拉框中选择用户账号（这就意味着需要先添加客户账号才能添加物流信息），输入对应单号，选择状态，等等。点击添加，提示**添加成功**

     - 输入单号重复，弹出弹框**未知异常（一般是输入数据不合规）**。未添加所有数据点击添加，提示**添加成功**，但并未添加进去任何数据。

  2. 更新物流信息

     - 点击更新物流信息按钮，弹出更新物流信息的弹框。首先选择用户账号，程序自动查询出当前账号对应的单号，选择单号后会把对应物流数据回显到表单中，只需输入物流信息即可。点击更新，提示**更新成功**

     - 对用户下没有单号的单号不能进行更新操作，点击更新，弹出弹框**未知异常（一般是输入数据不合规）**

  3. 查询物流信息

     - 点击查询物流信息按钮，输入对应单号，会查询出相应订单的物流信息。提示**查询成功**
     - 单号输入错误会弹出弹框**未知异常（一般是输入数据不合规）**

  4. 查询全部物流信息

     点击查询全部物流信息按钮，查询出全部物流信息。（物流信息会根据所属用户和所属订单自动分组展示）

  5. 删除物流信息

     - 点击删除物流信息按钮，输入相应单号，可以删除相应的物流信息。
     - 单号输入错误会弹出弹框**未知异常（一般是输入数据不合规）**

- 员工管理

  1. 添加员工
     - 点击添加员工按钮，弹出对话框，输入用户名，密码和昵称。提示添加成功
     - 用户名重复，用户名或密码不合约束，弹出弹框**未知异常（一般是输入数据不合规）**
  2. 查看所有员工
     - 点击查看所有员工按钮，下方显示一个表格。表格列出员工的用户名和密码
     - 若查询不出来数据有两种清空，1：服务器未启动。（弹框**未知异常（一般是输入数据不合规）**）2：数据库没有数据
  3. 添加客户
     - 点击添加客户按钮，弹出对话框，输入用户名，密码和昵称。提示添加成功
     - 用户名重复，用户名或密码不合约束，弹出弹框**未知异常（一般是输入数据不合规）**
  4. 查看所有客户信息
     - 点击查看所有员工按钮，下方显示一个表格。表格列出员工的用户名和密码
     - 若查询不出来数据有两种清空，1：服务器未启动。（弹框**未知异常（一般是输入数据不合规）**）2：数据库没有数据
  5. 编辑员工（客户信息）
     - 点击编辑按钮，弹出对话框。不可修改用户名，输入需要修改的密码和昵称。提示修改成功
     - 密码不合约束，弹出弹框**未知异常（一般是输入数据不合规）**
  6. 删除员工（客户）
     - 点击删除按钮，弹出确认对话框，点击确认，提示删除成功
     - 服务器未启动，弹出弹框**未知异常（一般是输入数据不合规）**
  
- 退出

  点击退出按钮，弹出确认退出对话框，点击确认，清除本地存储的Authorization并跳转到登录页面

1. 管理员已登录页面。     

   功能细节与超级管理员类似，但没有删除物流信息和员工管理模块。在此不在赘述

## 接口文档

下述接口设计大致符合RESTful风格

统一说明：

| 名称 | 类型   | 是否必须 | 备注                     |
| ---- | ------ | -------- | ------------------------ |
| code | number | 必须     | 响应码, 1 成功 ; 0  失败 |
| msg  | string | 非必须   | 提示信息                 |
| data | string | 必须     | 返回的数据 ,             |

### 登录管理

1. 登录请求

   >请求路径：/login
   >
   >请求方式：POST
   >
   >接口描述：接口用于登录系统，根据返回的不同信息给用户下发不同的jwt和定位到不同的页面

   请求参数：

   ```json
   {
    "username":"xiaohei666",
    "password":"123456"
   }
   ```

   响应参数：

   ```json
   {
       "code":1,
       "msg":"success",
       "data":{
           "type":"superAdmin",
           "nickname":"张狗蛋",
           "jwt":"asabsabds.hjbhsjsa.saj182292282029.hshsuahsak"          
       }
   }
   
   ```

设计细节：

​		用户点击登录按钮，系统会去数据库查询用户的类型和昵称，并存储到相应的userAuth对象里面，根据不同的用户类型和用户名下发不同类型的jwt令牌，在后端添加拦截器，每次请求都需要携带jwt令牌（token），使用拦截器实现，在前端实现不同用户类型访问对应页面的逻辑。

不同用户访问逻辑如下

- 超级管理员：
  - 点击登录按钮登录。登录完毕以后再次访问网站不需登录，如果需要更换管理员/顾客账号，需要点击退出登录清除数据。
  - 访问权限：超级管理员添加用户页面，超级管理员人员管理页面，根据单号查询信息页面，404页面
- 管理员：
  - 点击登录按钮登录。登录完毕以后再次访问网站不需登录，如果需要更换其他账号，需要点击退出登录清除数据。
  - 访问权限：管理员添加用户页面，根据单号查询信息页面，404页面
- 用户：
  - 点击登录按钮登录。登录完毕以后再次访问网站不需登录，如果需要更换其他账号，需要点击退出登录清除数据。
  - 访问权限：根据单号查询信息页面，404页面

注：访问不允许访问的页面会出现**权限不够被禁止的页面**，这时只需要重新访问原始页面或者点击浏览器的左上角返回键即可

### 物流信息管理

1. 添加物流信息

   >请求路径：/goods/add/goods
   >
   >请求方式：POST
   >
   >接口描述：根据前端传过来的JSON数据进行物流信息的添加

   请求参数：

   ```JSON
   {
       "userAccount": "testUser12",
       "code": "XYZ789",
       "state": 1,
       "country": "US",
       "sysCode": "SYS001",
       "removeCode": "RMV456",
       "postCode": "90210",
       "customerCode": "CST1001",
       "remark": "Sample remark for testing.",
       "transportInfo": "Truck 001 via Route 5"
   }
   
   ```

   响应参数：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

2. 查询账号信息

      > 请求路径：/goods/query/account
      >
      > 请求方式：GET
      >
      > 接口描述：用于添加/更新时下拉框数据来源的收集

      请求参数：无

      响应参数：

      ```json
      {
      	"code": 1,
      	"msg": "success",
      	"data": [
      		"user001",
      		"user004",
      		"user005",
      		"user007",
      		"user008",
      		"user010",
      		"user012",
      		"user013",
      		"user015",
      		"user016",
      		"user018",
      		"user019",
      		"testUser12"
      	]
      }
      ```

3. 根据账号查询单号信息

     > 请求路径：/goods/query/number
     >
     > 请求方式：GET
     >
     > 接口描述：该接口用于在选择完账号以后，更新数据时选择对应单号下拉框的数据获取

     请求参数：**查询参数**

     | 参数名  | 参数值  | 参数描述 |
     | ------- | ------- | -------- |
     | account | user001 | 用户账号 |

     响应参数：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": [
     		"code001",
     		"code02333"
     	]
     }
     ```

4. 根据单号查询对应的物流信息

     >请求路径：/goods/query/goods
     >
     >请求方式：GET
     >
     >接口描述：该接口用于在选择完账号和单号以后，更新数据弹框中的其他字段（物流信息除外）的数据回显

     请求参数：**查询参数**

     | 参数名 | 参数值  | 参数描述 |
     | ------ | ------- | -------- |
     | number | code002 | 订单编号 |

     响应参数：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": {
     		"id": 2,
     		"userAccount": "user002",
     		"code": "code002",
     		"state": 0,
     		"country": "美国",
     		"sysCode": "sys002",
     		"removeCode": "rem002",
     		"postCode": "200002",
     		"customerCode": "cust002",
     		"remark": "备注2",
     		"transportInfo": null,
     		"transportInfos": null
     	}
     }
     ```

5. 更新物流信息

     >请求路径：/goods/update/goods
     >
     >请求方式：PUT
     >
     >接口描述：根据收集的数据发送到后端进行数据表的更新

     请求参数：

     ```json
     {
         "userAccount": "testUser12",
         "code": "XYZ789",
         "state": 1,
         "country": "US",
         "sysCode": "SYS001",
         "removeCode": "RMV456",
         "postCode": "90210",
         "customerCode": "CST1001",
         "remark": "这是备注",
         "transportInfo": "这是更新的物流信息"
     }
     
     ```

     响应参数：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": null
     }
     ```

6. 根据单号查询物流信息

     >请求路径：/goods/query/transport
     >
     >请求方式：GET
     >
     >接口描述：该接口用于实现根据单号查询物流信息的功能

     请求参数：**查询参数**

     | 参数名 | 参数值  | 参数描述 |
     | ------ | ------- | -------- |
     | code   | code001 | 订单编号 |

     响应数据：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": {
     		"id": 1,
     		"userAccount": "user001",
     		"code": "code001",
     		"state": 0,
     		"country": "中国",
     		"sysCode": "sys001",
     		"removeCode": "rem001",
     		"postCode": "100001",
     		"customerCode": "cust001",
     		"remark": "备注1",
     		"transportInfo": null,
     		"transportInfos": [
     			{
     				"transportInfo": "哈哈哈",
     				"createTime": "2023-09-15T19:40:17"
     			}
     		]
     	}
     }
     ```

7. 查询所有物流信息

     >请求路径：/goods/query/transport/all
     >
     >请求方式：GET
     >
     >接口描述：该接口用于实现查询全部信息的功能，三张表联查查询出来用户的所有信息

     请求参数：无

     响应数据：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": [
     		{
     			"userAccount": "user001",
     			"nickname": "nickname",
     			"transportInfos": [
     				{
     					"code": "code02333",
     					"state": 0,
     					"country": "中国",
     					"sysCode": "sys001",
     					"removeCode": "rem001",
     					"postCode": "100001",
     					"customerCode": "cust001",
     					"remark": "备注2",
     					"transportInfos": [
     						{
     							"transportInfo": "这23号的物流信息",
     							"createTime": "2023-09-15T17:28:22"
     						}
     					]
     				},
     				{
     					"code": "code001",
     					"state": 0,
     					"country": "中国",
     					"sysCode": "sys001",
     					"removeCode": "rem001",
     					"postCode": "100001",
     					"customerCode": "cust001",
     					"remark": "备注1",
     					"transportInfos": [
     						{
     							"transportInfo": "信息1",
     							"createTime": "2023-09-15T10:12:16"
     						},
     						{
     							"transportInfo": "code0012222",
     							"createTime": "2023-09-15T13:08:18"
     						},
     						{
     							"transportInfo": "user002222222222",
     							"createTime": "2023-09-15T17:23:19"
     						}
     					]
     				}
     			]
     		}
     ```

8. 根据单号删除物流信息

     >请求路径：/goods/delete/transport
     >
     >请求方式：Delete
     >
     >接口描述：该接口用于实现根据单号删除物流信息功能，点击删除，订单的所有信息将都会删除（多表删除）

     请求参数：查询参数

     | 参数名 | 参数值  | 参数描述 |
     | ------ | ------- | -------- |
     | code   | code020 | 订单编号 |

     响应参数：

     ```json
     {
     	"code": 1,
     	"msg": "success",
     	"data": null
     }
     ```


### 人员管理

1. 添加员工

   >请求路径：/person/add/emp
   >
   >请求方式：POST
   >
   >接口描述：该接口用于实现系统中添加员工的功能，用户名需满足约束且唯一

   请求参数：

   ```json
   {
       "username":"xiaosan1",
       "password":"xiaosan666",
       "nickname":"小狗蛋"
   }
   ```

   响应数据

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

2. 查看所有员工

   >请求路径：/person/query/emps
   >
   >请求方式：GET
   >
   >接口描述：该接口用于实现系统中查看所有员工的功能，封装成一定的数据格式返回给前端显示

   请求参数：无

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": [
   		{
   			"username": "user002",
   			"password": "pass002",
   			"nickname": "昵称2",
   			"flag": 1
   		},
   		{
   			"username": "user006",
   			"password": "pass006",
   			"nickname": "昵称6",
   			"flag": 1
   		},
   		{
   			"username": "user009",
   			"password": "pass009",
   			"nickname": "昵称9",
   			"flag": 1
   		}]
   }
   ```

3. 修改员工信息

   >请求路径：/person/update/emp
   >
   >请求方式：PUT
   >
   >接口描述：该接口用于实现修改员工信息功能，根据用户名修改昵称和密码

   请求参数：

   ```json
   {
       "username":"xiaosan",
       "password":"xiaosan666",
       "nickname":"张三0"
   }
   ```

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

4. 删除员工

   >请求路径：/person/delete/emp/{username}
   >
   >请求方式：DELETE
   >
   >接口描述：根据传来的用户名删除员工信息

   请求参数：**路径参数**

   | 参数名   | 参数值   | 参数说明   |
   | -------- | -------- | ---------- |
   | username | xiaosan1 | 员工用户名 |

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

5. 添加客户

   >请求路径：/person/add/customer
   >
   >请求方式：POST
   >
   >接口说明：该接口用于实现添加客户的功能

   请求参数：

   ```json
   {
       "username":"cus0011",
       "password":"cus001666",
       "nickname":"王老板"
   }
   ```

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

6. 查看所有客户

   >请求路径：/person/query/customers
   >
   >请求方式：GET
   >
   >接口说明：该接口用于实现查看所有客户信息的功能

   请求参数：无

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": [
   		{
   			"username": "user001",
   			"password": "pass001",
   			"nickname": "昵称1",
   			"flag": 0
   		},
   		{
   			"username": "user004",
   			"password": "pass004",
   			"nickname": "昵称4",
   			"flag": 0
   		}]
   }
   ```

7. 修改客户信息

   >请求路径：/person/update/customer
   >
   >请求方式：PUT
   >
   >接口描述：该接口用于实现修改客户信息的功能，根据用户名修改密码和昵称

   请求参数：

   ```json
   {
       "username":"laowang",
       "password":"laowang666",
       "nickname":"王老板"
   }
   ```

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

8. 删除客户：

   >请求路径：/person/delete/customer/cus001
   >
   >请求方式：DELETE
   >
   >接口描述：根据用户名删除客户，并进行逻辑外键的关联，把其他表中的对应数据也一并删除

   请求参数：**路径参数**

   | 参数名   | 参数值  | 参数描述   |
   | -------- | ------- | ---------- |
   | username | cus0011 | 顾客用户名 |

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": null
   }
   ```

### 顾客查询

1. 根据顾客用户名查询对应全部物流信息

   >请求路径：/query/query/transport/user019
   >
   >请求方式：GET
   >
   >接口描述：该接口用于实现用户登录后可以查看所有物流信息的功能

   请求参数：**路径参数**

   | 参数名   | 参数值  | 参数描述   |
   | -------- | ------- | ---------- |
   | username | user019 | 顾客用户名 |

   响应数据：

   ```json
   {
   	"code": 1,
   	"msg": "success",
   	"data": {
   		"id": 19,
   		"userAccount": "user019",
   		"nickname": "昵称19",
   		"code": "code019",
   		"state": 0,
   		"country": "马来西亚",
   		"sysCode": "sys019",
   		"removeCode": "rem019",
   		"postCode": "190019",
   		"customerCode": "cust019",
   		"remark": "备注19",
   		"transportInfos": [
   			{
   				"transportInfo": "信息19",
   				"createTime": "2023-09-15T19:40:17"
   			},
   			{
   				"transportInfo": "这是一个重复信息",
   				"createTime": "2023-09-15T20:30:10"
   			}
   		]
   	}
   }
   ```

   



## 数据库表设计

鉴于这个项目结构简单，因此对数据库相应设计进行了聚合处理。虽然只满足第一范式，但也是相对而言的较优解

用户表User

| 字段名   | 备注                                            | 约束条件                    |
| -------- | ----------------------------------------------- | --------------------------- |
| id       | 用户ID                                          | 主键且自增                  |
| username | 用户名                                          | 非空且唯一，英文+数字5-12位 |
| password | 用户密码                                        | 非空，英文+数字5-12位       |
| nickname | 用户昵称                                        | Varchar（20）               |
| flag     | 标识用户的类型（0普通客户,1管理员,2超级管理员） | 默认值0，（0,1,2）          |

货物信息表goods(根据c_id和客户信息表关联，一个客户有多个货物)

| 字段名        | 备注                                             | 约束                   |
| ------------- | ------------------------------------------------ | ---------------------- |
| id            | 货物id                                           | 主键且自增             |
| u_username    | 客户用户名，关联客户表，用于查询                 | 非空且唯一             |
| code          | 单号                                             | 非空且唯一，<100个字符 |
| state         | 状态，0：已下单，1：已发货，2：在途中，3：已签收 | 默认0，（0,1,2,3）     |
| country       | 所属国家                                         | 非空，<255个字符       |
| post_code     | 邮编（后更改成数据）                             | 非空，<255个字符       |
| sys_code      | 系统单号                                         | 非空，<255个字符       |
| remove_code   | 转单号                                           | 非空，<255个字符       |
| customer_code | 客户单号                                         | 非空，<255个字符       |
| remark        | 备注                                             | <200个字符             |

物流信息表transport(根据g_id和货物信息表关联，一个货物id对应很多次货物信息)

| 字段名         | 备注                               | 约束                             |
| -------------- | ---------------------------------- | -------------------------------- |
| id             | 货物信息id                         | 主键，自增                       |
| g_id           | 货物id，关联货物表，用于添加和查询 | 非空且唯一                       |
| transport_info | 详细货物信息                       | 非空                             |
| create_time    | 创建时间，根据这个时间排序         | DateTime格式,YYYY-MM-DD HH:mm:ss |

## 后续可拓展的点

1. 对每个添加员工，客户，订单，物流信息都加上日志。记录是谁在何时操作了这些数据

2. 对于添加的时候每个单号的唯一性我没有做提示，如果提示说遇到未知错误很可能是因为添加的单号或者用户名等在表里面已经存在了。如果需要更详细的提示，比如**查找时候单号不存在**，**单号重复**，**用户名重复**，后续可以拓展。现在对于系统的错误有三个：
   1. **未知错误（一般是数据输入不合规）**。最大的错误，因为后端只是捕捉了一个最大的异常然后返回给前端
   2. **用户名或密码错误**。这个是因为用户名或密码错误，需要注意的是如果中间加上空格也会导致用户名或密码错误
   3. **服务器异常**。服务器端发生错误，服务器服务未开启

3. 添加上用户中心，可以上传头像，个人简介等等。

4. 给管理员和超级管理员也进行移动端的适配

5. 加载时给每个请求添加上合适的loading效果

6. 默认管理员页面输入的数据全部都是合法且合理的。后续如果需要校验输入的数据格式不对，可以在前端页面给每个表单都加上对应的校验效果。（如单号必须13位，备注不能有特殊字符）

7. 导入物流公司接口，实现数据的自动更新

8. 将网站升级成https

9. 给更新物理信息时添加的原子物流信息提供删除按钮
