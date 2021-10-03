项目地址：[https://github.com/siromomo/pbl-mm](https://github.com/siromomo/pbl-mm)

# 对象属性说明

## User

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

@Column(unique = true)

private String username;

private int age;

private String region;

private String gender;

private String email;

private String password;

private String fullname;

private String headProfilePath;

@Column(columnDefinition = "int(11) default 0")

private int modelId;

@ManyToMany(cascade = CascadeType.*MERGE*, fetch = FetchType.*EAGER*)

private Set<Authority> authorities;

@ManyToMany(fetch = FetchType.*EAGER*)

private Set<GameRecord> gameRecords;

private int knowledgeNum;

private int gameNum;

## Position

private int x;

private int y;

private int z;

private float rotation;

private float rotationY;

private float rotationZ;

private int lastRand; //用于随机更新位置

## Cell

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

private static final int *NUM_OF_TYPES *= 8;

@Column(unique = true)

private String nickname;

private String type;

private int level;

@Column(columnDefinition = "boolean default true")

private boolean active;

@Column(columnDefinition = "int(11) default 100")

private int hp;

private int initLevel;

@OneToOne

Pack pack;

@ManyToOne

@JoinColumn(name = "user_id",referencedColumnName="id")

private User user;

## Virus

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

private int level;

private int radius; //半径，判断碰撞用，默认为5

## CellInfo

//用于记录细胞的相关信息，关系到之前用背包收集信息的玩法

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

@Column(unique = true)

private String type;

private String info;

@ManyToMany(fetch = FetchType.*EAGER*, mappedBy = "cellInfoSet")

private Set<Pack> packSet;

*/**

*注：现在有的细胞信息类型*

*B         *

*nerve     *

*phagocyte*

*platelet  *

*red       *

*skin      *

*stem      *

*T   *

**/*

## Knowledge

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

private String content;

@ManyToMany(mappedBy = "knowledgeSet")

@JsonIgnore

private Set<Pack> packSet;

## Pack

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

@ManyToMany(fetch = FetchType.*EAGER*)

private Set<CellInfo> cellInfoSet;

@OneToMany

@JsonIgnore

private Set<Cell> cells;

@Column(columnDefinition = "int(11) default 100")

private int hp;

private final static int *DROP_NUM *= 10;

## ChoiceQuestion

@Id

@GeneratedValue(strategy = GenerationType.*AUTO*)

private Long id;

private String keyword;

@Column(unique = true)

private String stem; //题干

private String choiceA;

private String choiceB;

private String choiceC;

private String choiceD;

private String correctChoice;

## ChatMessage

private long toId;

private long fromId;

private String content;

private String toName;

private String fromName;

## StartGameResponse

private Pack pack;

private Map<User, Position> cellPositionMap;

private Map<Virus, Position> virusPositionMap;

private User newUser;

private Set<User> userSet;

## GameRecord

private Long id;

private String status;

private int score;

@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")

@Temporal(TemporalType.TIMESTAMP)

private Date time;

## Room

private int id;

private String name;

private String date;

private String status;

private int playerNum;

private int maxPlayNum = 2;

# 用户模块

## 登录

**接口URL**：/login

**请求方法**：POST

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| username   | 用户名   | String   | 是   | -   |
| password   | 密码   | String   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：登录成功  其他：失败   | int   | -   |
| message   | 登录成功则为token  其余字符串：异常信息在字符串中返回   | String   | 在请求/login和/register以外的api时需要以jwt_token的名字携带于header中   |
| content   | json序列化的User信息   | User   | 属性有username，password，fullname，age，region，gender，authorities（Set类型）   |

## 注册

**接口URL**：/register

**请求方法**：POST

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| username   | 用户名   | String   | 是   | -   |
| password   | 密码   | String   | 是   | -   |
| email   | 邮箱   | String   | 是   | -   |
| modelId   | 模型id   | int   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：注册成功  其他：失败   | int   | -   |
| message   | token：注册成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的User信息   | User   | 属性有username，password，email，fullname，age，region，gender，authorities（Set类型）   |

## 上传头像

**接口URL**：/setProfilePhoto

**请求方法**：POST

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| file   | 头像文件   | MultipartFile   | 是   | 这个MultipartFile挺玄学的，具体可以搜一下orz，用ajax的话需要模拟表单才能发送   |
| jwt_token   | 记录用户信息的token   | String   | 是   | header   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的User信息   | User   | 属性有username，password，fullname，age，region，gender，authorities（Set类型），headProfilePath  （头像文件URL）   |

## 获得用户信息

**接口URL**：/getUserInfo

**请求方法**：POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 记录用户信息的token   | String   | 是   | header   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的User信息   | User   | 属性有username，password，fullname，age，region，gender，authorities（Set类型），headProfilePath  （头像文件URL）   |

## 根据用户ID返回用户名

**接口URL**：/findUserById

**请求方法**：POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 记录用户信息的token   | String   | 是   | header   |
| userId   | id   | Long   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | username   | String   | -   |

## 更改用户信息

**接口URL**：/modifyInformation

**请求方法**：POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|:----:|
| username   | 用户名   | String   | 是   | 不能改…  另外这个请求也要加header（jwt_token）来验证权限   |
| email   | 邮箱   | String   | 是   | -   |
| fullName | 全名 | String | 否 | - |
| age | 年龄 | int | 否 | - |
| region | 地区 | String | 否 | - |
| gender | 性别 | String | 否 | - |
| originalPassword   | 旧密码   | String   | 是   | -   |
| newPassword   | 新密码   | String   | 否   | 不填就是不改   |
| modelId   | 模型id   | int   | 是   | -      |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的User信息   | User   | 属性有username，password，email，fullname，age，region，gender，authorities（Set类型），headProfilePath  （头像文件URL）   |

## 游戏记录绑定

**接口URL**：/endGame

**请求方法：**POST

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |
| status   | 游戏状态   | String   | 是   | -   |
| score   | 游戏分数   | int   | 是   | -   |
| time   | 游戏时间   | String   | 是   | -   |
| userIds   | 参与游戏用户ID   | Set<Long>   | 是   | -   |
| packId   | 当前游戏背包ID   | long   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | -   | -   | -   |

## 用户ID获取已获得过的知识点

**接口URL**：/findKnowledgeByUserId

**请求方法：**GET/POST

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |
| userId   | 用户id   | long   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 知识点集合   | Set<Knowledge>   | Knowledge属性见上   |

## 用户ID获取玩家的历史战绩

**接口URL**：/findRecordsByUserId

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |
| userId | 用户id | long | 是 | - |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | Array of Record |    | [{  gameId: '1',  status: '成功',  score: '12',  players: 'alice, bob, curry',  time: '2019-01-01 12:00'  },...  ]   |

## 用户ID获取玩家是否是管理员

**接口URL**：/getIsAdmin

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |
| userId | 用户id | long | 是 | - |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | isAdmin | Boolean | True or False |

## 获取所有玩家进度列表

**接口URL**：/getUserProgresses

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | Array of Users(按照knowledgeNum降序排序)   |    | [      {      rank: '1',      name: 'Curry',      userId: '1',      knowledgeNum: 100,      gameNum: 5,      },  ]     |

## 获取玩家游戏次数

**接口URL**：/getUserGameTimes

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | timeRange  timeRangeNum   | Array | 两数组对应，获取每个时间范围（time_range中项）的所有玩家游戏次数(time_range_num中项)     |

## 获取所有玩家基本信息量化排序

**接口URL**：/getUserInfos

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | ageRange,  ageRangeNum,  genderRange,  genderRangeNum         | Arrays |      |

## 用户选择虚拟形象

**接口URL**：/changeModel

**请求方法：**POST

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----|:----|:----:|:----|
| modelId |    | Integer   | y |    |
| userId   |    |    |    |    |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
|    |        |    |      |

## 获得所有知识点

接口URL：/getAllKnowledge

**请求方法：**GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content |        | Arrays | [      {          id: '12987122',          type: '选择题', *// 选择题/细胞信息/知识点*  *        *desc: '1+1=2', *//知识点内容*  *    *},]       |

## 增加知识点

接口URL：/addKnowledge

**请求方法：POST**

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----|:----|:----|:----|:----|
| content   | 内容   |    |    |    |
| type   | 类型   |    |    | *选择题/细胞信息/知识点*   |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content |      |    |        |

## 删除知识点

接口URL：/deleteKnowledge

**请求方法：****POST**** **

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----|:----|:----|:----|:----|
| id   |    |    |    |    |
| type |    |    |    |    |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content |        |    |        |

## 修改知识点

接口URL：/modifyKnowledge

**请求方法：****P****OST**

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----|:----|:----|:----|:----|
| id   |    |    |    |    |
| content   |    |    |    |    |
| type |    |    |    |    |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content |        |    |        |

## 创建房间

接口URL：/initRoom

**请求方法：****POST**

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----|:----|:----|:----|:----|
| name   |    | String   |    |    |
| playerNum   |    | int   |    | 默认为2   |

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | 新创建的Room     | Room   |        |

## 获取房间信息

接口URL：/getRoomInfo

**请求方法：GET**

**请求参数**：

**返回参数**：

| 字段名 | 说明 | 类型 | 备注 |
|:----:|:----:|:----:|:----:|
| code | 200：成功其他：失败 | int | - |
| message | "success"：成功其他：失败（信息在字符串中有说明） | String | - |
| content | 房间信息     | Set<Room> |        |

# 学习场景界面

## 将用户与细胞绑定

**接口URL**：/addCellToUser

**请求方法：**POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |
| type   | 细胞类型   | String   | 是   |    |
| nickname   | 细胞昵称   | String   | 是   |    |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 新创建的细胞   | Cell   | Cell的属性见上   |

## 重启细胞

**接口URL**：/restartCell

**请求方法：**POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |
| cellId   | cell的ID   | String   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 重启的细胞   | Cell   | Cell的属性见上   |

## 找到所有该用户拥有的细胞

**接口URL**：/findCellsByUser

**请求方法：**POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 细胞列表   | List<Cell>   | Cell的属性见上   |

## 获得某类型细胞的信息

**接口URL**：/findCellInfoByType

**请求方法：**POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |
| type   | 类型   | String   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 细胞信息   | CellInfo   | CellInfo的属性见上   |

## 用户收集信息到背包

**接口URL**：/app/collectCellInfo

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| packId   | 收集信息的背包ID   | long   | 是   | -   |
| cellInfoType   | 收集到的细胞信息的细胞类型   | String   | 是   | -   |

**返回参数**：-

## 用户答题

**接口URL**：/app/answerQuestion

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| answer   | 用户的答案   | String   | 是   | A,B,C,D   |
| questionId   | 答的题目的ID   | long   | 是   | -   |
| userId   | 用户ID   | long   | 是   | -   |
| roomId   | 房间id   | int   | 是   | -   |

**返回参数**：-

## 用户收集知识点

**接口URL**：/app/collectKnowledge

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| packId   | 收集信息的背包ID   | long   | 是   | -   |
| knowledgeId   | 知识点的ID   | long   | 是   | -   |

**返回参数**：-

## 背包更新通知

**接口URL**：/topic/{roomId}/updatePack

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 当前背包信息   | Pack   | Pack的属性见上   |

注：在其余用户收集到信息时会广播给所有用户

## 答题错误通知

**接口URL**：/topic/{userId}/wrongAnswer

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 答题错误的题目ID   | Long   | -   |

## 答题正确通知

**接口URL**：/topic/{userId}/correctAnswer

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 答题正确的题目ID   | Long   | -   |

## 用户与NPC接触

**接口URL**：/app/clickVirus

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| cellType   | 可能获得的细胞信息类型   | String   | 是   | *nerve, red, platelet, skin, stem, T, B, phagocyte*   |
| virusId   | NPC的ID   | long   | 是   | -   |
| userId   | 用户ID   | long   | 是   | -   |

## 随机获得一道问题

**接口URL**：/topic/{userId}/getRandomQuestion

**请求方法：**websocket订阅该URL

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 问题   | ChoiceQuestion   | ChoiceQuestion的属性见上   |

## 随机获得一个知识点

**接口URL**：/topic/{roomId}/getRandomKnowledge

**请求方法：**websocket订阅该URL

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 知识点   | Knowledge   | Knowledge的属性见上   |

## 获得某类型细胞的信息

**接口URL**：/topic/{roomId}/findCellInfoByType

**请求方法：**websocket订阅该URL

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 细胞信息   | CellInfo   | CellInfo的属性见上   |

## 连接到服务器

**接口URL**：/app/connectToServer

**请求方法**：websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| x   |    | int   | 是   |    |
| y   |    | int   | 是   |    |
| z   |    | int   | 是   |    |
| rotation   |    | float   | 是   |    |
| objectId   |    | user的ID   | 是   |    |
| roomId   | 房间id   | int   | 是   | -   |

## 加载完毕

**接口URL**：/app/loadFinish

**请求方法**：websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| x   |    | int   | 是   |    |
| y   |    | int   | 是   |    |
| z   |    | int   | 是   |    |
| rotation   |    | float   | 是   |    |
| objectId   |    | user的ID   | 是   |    |

## 游戏开始

**接口URL**：/topic/{roomId}/startGame

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的当前游戏背包（所有人共享），用户位置，npc位置等   | StartGameResponse   | 具体属性见上   |

注：在接入游戏的用户数量大于等于2时会自动广播，目前设置人数上限为2，**所有用户加载完毕后还是这个接口会发一个content为“all users load finish”的信息**

## 当前游戏人数

**接口URL**：/topic/{roomId}/currentNumOfUser

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 当前游戏人数   | int   | -   |

## 获得当前背包信息

**接口URL**：/getCurrentPack

**请求方法：**POST/GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的当前游戏背包（所有人共享）   | Pack   | Pack的属性见上   |

## 单个用户退出游戏

**接口URL**：/app/quitGame

**请求方法**：websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| x   |    | int   | 是   |    |
| y   |    | int   | 是   |    |
| z   |    | int   | 是   |    |
| rotation   |    | float   | 是   |    |
| objectId   |    | user的ID   | 是   |    |

## 用户退出游戏通知

**接口URL**：/topic/{roomId}/userQuit

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 退出游戏的用户信息   | User   | User的属性见上   |

注：在其余用户退出游戏时会广播给所有用户

## 游戏结束通知

**接口URL**：/topic/{roomId}/gameOver

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 游戏是否胜利   | boolean   | -   |

注：这个事件会强制触发所有人离开现场（等价于调用cleanCurrentGame）

## 结束当前游戏

**接口URL**：/cleanCurrentGame

**请求方法：**POST/GET

**请求参数**：

| 字段名 | 说明 | 类型 | 是否必填 | 备注 |
|:----:|:----:|:----:|:----:|:----:|
| jwt_token | 用户token | String | 是 | header |
| roomId   | 房间id   | int   | 是   | -   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 游戏是否胜利   | boolean   | -   |

## 客户端更新位置

**接口URL**：/app/updatePosition

**请求方法**：websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| x   |    | int   | 是   |    |
| y   |    | int   | 是   |    |
| z   |    | int   | 是   |    |
| rotation   |    | float   | 是   |    |
| objectId   |    | user的ID   | 是   |    |

## 掉血请求

**接口URL**：/app/dropHp

**请求方法**：websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| dropNum   | 掉的血量   | int   | 是   | 还是包装成JSON传   |
| roomId   | 房间id   | int   | 是   | -   |

## 血量更新

**接口URL**：/topic/{roomId}/updateHp

**请求方法：**websocket（订阅该URL即可）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 当前血量   | int   | -   |

## 初始化所有病毒NPC位置

**接口URL**：/initVirus

**请求方法：**POST/GET

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| jwt_token   | 用户token   | String   | 是   | header   |

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的virusPositionMap   | Map<Virus, Position>   | Virus及Position的属性见上   |

## 获得所有当前页面上细胞及病毒的位置

**接口URL**：/topic/{roomId}/updateCellAndVirus

**请求方法：**websocket（注：订阅该URL后以1s的频率更新）

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | json序列化的cellPositionMap及VirusPositionMap   | content.virusPositionMap:  Map<Virus, Position>  content.cellPositonMap:  Map<User, Position>   | Virus，User及Position的属性见上   |

注：更新过程中会判断细胞与病毒是否碰撞，这里判断的是在碰撞的情况下，如果cell的背包已经收集全cellInfo，则病毒死亡，否则细胞死亡

# 聊天部分

## 发送与其余用户一对一聊天消息

**接口URL**：/app/chatMessageToOne

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| toId   | 要发送给的用户的ID   | long   | 是   | -   |
| fromId   | 来自的用户的ID   | long   | 是   | -   |
| content   | 聊天内容   | String   | 是   | -   |

**返回参数**：（订阅获得一对一聊天信息的URL获取发送给自己的消息）

## 接受与其余用户一对一聊天消息

**接口URL**：/topic/{userId}/toOne

**请求方法：**websocket

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 聊天信息   | ChatMessage   | ChatMessage的属性见上   |

## 发送与其余用户公开聊天消息

**接口URL**：/app/chatMessageToAll

**请求方法：**websocket

**请求参数**：

| 字段名   | 说明   | 类型   | 是否必填   | 备注   |
|:----|:----|:----|:----|:----|
| toId   | 要发送给的用户的ID   | long   | 是   | 填-1就行   |
| fromId   | 来自的用户的ID   | long   | 是   | -   |
| content   | 聊天内容   | String   | 是   | -   |
| roomId   | 房间id   | int   | 是   | -   |

**返回参数**：（订阅获得公开聊天信息的URL获取发送给自己的消息）

## 接受与其余用户公开聊天消息

**接口URL**：/topic/{roomId}/toAll

**请求方法：**websocket

**请求参数**：

**返回参数**：

| 字段名   | 说明   | 类型   | 备注   |
|:----|:----|:----:|:----|:----|
| code   | 200：成功  其他：失败   | int   | -   |
| message   | "success"：成功  其他：失败（信息在字符串中有说明）   | String   | -   |
| content   | 聊天信息   | ChatMessage   | ChatMessage的属性见上   |




