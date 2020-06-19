# 《工作细胞》项目设计文档

## 项目背景

### web3d项目要求

- 模拟真实世界中的环境，实现一个完整的多人在线Web3D教学式场景。

- 用户拥有自己的虚拟形象，可以在这个环境中模拟真实世界中的操作，如行走、交流、或其他自定义的交互动作。

  因此我们选择模拟人体内器官环境，用户扮演各类细胞角色，拥有自己的细胞虚拟形象，在这个环境中模拟真实细胞的各类操作，比如漫游、拟人交流、被病毒攻击等等。

### 游戏背景

#### 市场背景

- 缺乏同类竞品，潜力巨大
- 没有主流平台
- 较为新颖

#### 需求背景

- 疫情之下 群众生物基础知识欠缺
- 青少年儿童对于生物学习兴趣下降
- 高中面临生物会考
- 家长如何向孩子解释新冠病毒

……

#### 游戏背景

同名动漫《工作细胞》



## 需求规格说明

### 《工作细胞》概述（完整系统）

“工作细胞”是一款面向青少年的，基于Web的多人在线生物学习游戏系统。主要由“游戏中心”、“个人中心”、“角色图鉴”、“管理后台”（仅对管理员开放）四个模块构成。

#### 游戏中心描述

游戏中心是本系统的核心子模块。玩家在登陆/注册进入系统首页之后，可以点击游戏中心创建房间/随机加入房间/加入指定房间。当房间人数达到所选择的人数时，游戏统一开始。

##### 游戏规则描述

- 作为多细胞合作游戏，所有玩家随机掉落于某一共同器官环境，开始漫游。

- 玩家通过wsad移动，鼠标控制视角旋转/瞄准，左键点击与对象交互。

- 按B打开背包查看收集到的信息。

- 点击NPC获得细胞和病毒信息/生物题。

- 玩家需要维持血条，并且搜集足够多的信息/刷足够多的题（进度条100%）才能集体获得游戏胜利。

- 所有玩家共享背包内收集到的信息。

- 血条降到0时，游戏失败结束。

- 玩家可以与其他玩家用文字的方式在下方聊天框中对话。

  

#### 个人中心描述

##### 登录注册

- 用户模块可分为两大类：普通玩家模块与管理员模块。

- 用户可分为两类：普通玩家用户（通过注册方式登陆）和管理员用户（通过内置账号/后台配置成为管理员）。

- 在刚进入系统之前，玩家需要有该系统下的普通玩家/管理员的账号和密码。

1. 登陆

   - 玩家可通过使用用户名和密码登陆系统。

   （管理员用户可以是后台配置文件指定的形式，也可以直接通过admin用户来登陆）

2. 注册
   - 玩家可通过输入用户名、密码、和邮箱来完成注册。
   - 玩家需要在注册时选定对应的虚拟形象，在进入系统之后仍然可以在多处修改。

##### 基本信息

- 玩家输入密码之后，可以修改他的个人信息（邮箱、性别、地区等基本信息，但注意用户名唯一且不可修改），并且修改他当前的虚拟形象。

##### 历史战绩

- 玩家可以查看自己过去的历史游戏记录，包括：
  - 游戏状态
  - 游戏分数
  - 玩家列表
  - 游戏时间

##### 知识清单

- 玩家可以查看自己历史游戏记录中获取的所有知识点，包括：
  - 知识点类别，例如：高中生物
  - 知识点详细内容

#### 角色图鉴描述

玩家可以查看所有系统内置的细胞模型，包括其对应的细胞类型、信息等，并且选择对应的细胞作为自己的虚拟形象。

#### 管理后台描述

当玩家是后台管理员时，显示这一界面。

##### 玩家进度统计

表格形式，以知识点解锁数从高到低给所有玩家进行排序，内容包括：

- 排名
- 用户名
- 用户ID
- 知识点解锁数
- 游戏次数

##### 游戏次数统计

折线图形式，将玩家游戏次数按天分布情况作可视化展示。

##### 玩家信息分析

柱状图形式，对玩家基本信息（年龄和性别）的分布情况作可视化展示。

##### 知识点管理

作为管理员可以查看系统当前所有的知识点。

## 设计思路

### 技术栈选取

Web3d没有限制所使用的技术栈。

经过多方调研，我们选择Vue + Three.js + Springboot + Hibernate 作为前后端分离架构的技术栈。



## 项目分工

**参与度与贡献度：1:1:1:1**

- 前端Vue：@黄佳妮

- 前端Three.js: @刘铭涵 @王嵩立

- 后端Springboot：@田嘉禾

  

## 项目组织

### 前端Vue

#### 外部组件说明

- axios

  这是一个基于浏览器XMLHttpRequest接口的客户端http API，使用方法如下：

  - 发送请求

  ```
  axios.request(config)
  axios.get(url[, config])
  axios.delete(url[, config])
  axios.head(url[, config])
  axios.options(url[, config])
  axios.post(url[, data[, config]])
  axios.put(url[, data[, config]])
  axios.patch(url[, data[, config]])
  ```

  - 处理相应

  ```
  axios.post('/login', {
    firstName: 'Finn',
    lastName: 'Williams'
  })
  .then((response) => {
    console.log(response);
  }, (error) => {
    console.log(error);
  });
  ```

  - 拦截器说明

  ```javascript
  axios.interceptors.request.use(
      config => {
        console.log("axios interceptor works...")
        if (Cookies.get('token')) {
          config.headers.jwt_token = Cookies.get('token');
          console.log('config header is ' + config.headers);
        }
        return config;
      },
      err => {
        return Promise.reject(err);
      });
  ```

  ```javascript
  axios.interceptors.response.use(
      response => {
        return response;
      },
      error => {
        if (error.response) {
          console.log('axios:' + error.response.status);
          switch (error.response.status) {
            case 403:
              router.replace({
                path: '/pages/Login',
                query: {redirect: router.currentRoute.fullPath}
              });
              break;
            case 400:
              router.replace( {
                path: '/pages/Login',
                query: {redirect:  router.currentRoute.fullPath}
              });break;
          }
        }
        return Promise.reject(error.response.data);
      }
  );
  ```

- echarts

  可视化图库

- element-ui

  我们挑选的UI组件库，语法类似bootstrap

- jquery

  在Vue中使用jquery进行一些常见操作

- js-cookie

  简化对于cookie的相关操作

- live2d4Vue

  引入live2d模型

- Vue3dModel

  引入threeJS模型

- sockjs-client + stompjs

  实现ws通信（Vue内实现聊天功能）

- three

  实现普通模型加载功能

- vee-validate

  常用表单验证

- vue-router

  路由控制

- vuex

  全局状态量维护

#### UI设计

- 配色

考虑到本游戏的游戏风格，我们考虑使用红色与蓝色结合的UI配色，以表达人体环境的阴阳平衡。基础UI使用了element-ui的基础配色蓝色，而图表展示使用了红色系配色。

- UX交互

考虑到本项目的针对性与适用人群，我们没有使用过于复杂的导航栏组件，而是一切从简，降低进入游戏的成本，方便用户的交互，增强用户的粘性。

- 风格综述

继承《工作细胞》的整体UI，整体偏向可爱二次元风，使较为低龄的同学们更有点击的欲望。

#### 模块划分

- Component：页面布局组件
- Api：统一规范定义后端接口
- Router： 定义路由
- Store：定义全局状态对象
- Views: 定义基本视图页面

#### 文件说明

- public 静态资源目录
- src 源代码
  - api
    - index.js 定义后端接口
  - components
    - Aside.vue 边栏组件
    - Container.vue 全局容器
    - Footer.vue 脚注组件
    - Header.vue 头组件
    - NavMenu.vue 导航栏组件
    - Top.vue 扉页组件
  - router
    - index.js 定义整体路由
  - store
    - modules
      - user.js 用户全局状态信息
    - getters.js 定义store的变量映射
    - store.js 规范引入所有module中的全局状态信息
  - utils
    - index.js 定义小工具函数
    - validate.js 表单校验函数
  - views
    - admin
      - AdminContainer.vue 管理页布局容器
      - AllKnowledges.vue 知识点管理页面
      - GameNum.vue 游戏次数页面
      - UserInfos.vue 用户信息统计页面
      - UserProgress.vue 用户进度页面
    - errorPages
      - 404.vue 
      - 500.vue
    - loin
      - Login.vue 登陆页面
    - personnel
      - Information.vue 个人信息页面
      - KnowledgeList.vue 知识列表页面
      - Records.vue 历史战绩页面
    - register
      - Register.vue 注册页面
    - Chatting.vue 游戏页面
    - Hall.vue 主页页面 
    - Models.vue 角色图鉴页面
    - Room.vue 房间页面
  - App.vue 全局App入口
  - main.js 全局路由与拦截

- index.html 入口文件
- package.json 配置文件

#### 难点阐述

- iframe内嵌页面

  ```
  <iframe src="game/main/src/main.html" frameborder="0" scrolling="auto" height="500px" width="1200px" style="margin: 30px auto;"></iframe>
  ```

- ws通信

  ```
    let socket = new SockJS('http://122.51.160.221:8080/ws');
    this.chat_socket = Stomp.over(socket);
  ```

  - 订阅示例：

    ```
     this.subscribe('/topic/' + Cookies.get('roomID') + '/userQuit', function(obj) {
                            console.log('enter the user quit method');
                            console.log('obj body is ' + obj.body);
                            let user_id = obj.content.id;
                            self.messageList.push({
                                type: 2,
                                msg: '用户 ' + self.onlineUserList[user_id]._username + ' 退出聊天',
                                msgUser: null
                            })
                            delete self.onlineUserList[user_id]
                            // let obj_ = JSON.parse(obj.body);
                            self.$message.success("本房间结束游戏！");
                        })
    
    ```

  - 发送示例：

    ```
     send () {
                    this.message = this.trim(this.inputValue)
                    let msg = {
                        'toId': -1,
                        'fromId': this.user_id,
                        'content':this.message,
                        'roomId': parseInt(this.roomID)
                    }
                    if (this.message.length > 0) {
                        this.chat_socket.send('/app/chatMessageToAll', {}, JSON.stringify(msg))
                        this.inputValue=''
                    }
                },
    ```

    

#### 亮点阐述

- live2d插件

- echarts数据可视化库

  

### 前端3D

#### 设计思路

《工作细胞》互动游戏整体建立在一个封闭沙盒中，该沙盒模拟了人体内部的微观环境，有凹凸的地形和流动的血液。多玩家扮演受病毒感染的人体内的一颗细胞（B淋巴细胞，T淋巴细胞，血红蛋白，血小板等），通过合作收集知识点，完成答题挑战，最终获取足够的积分使人体恢复健康。通过有趣的互动和阶段性的知识输入/检测，使得玩家在游戏过程中获取生物学和免疫学知识。

多名玩家进入游戏后组成队伍，共享收集到的资源和体力。当收集进度达到100%时，游戏成功；当生命值降为0时，游戏失败。

#### 模块划分

游戏主体共分为两大模块：漫游和NPC（Non-Player Character非玩家角色）交互。

##### 漫游模块

细胞进入游戏后会生成在地图的随机位置，玩家通过键盘WASD进行移动，鼠标移动转换视角。通过physijs物理引擎模拟了碰撞、摩擦、重力等真实的物理环境。如：上坡会受到重力的拉扯，在水中阻力较小移动变快，在粗糙的地面上会受到摩擦力的影响移动变得迟缓。同时，会遇到同一房间中的其他玩家和NPC，可以产生碰撞。

##### 交互模块

在玩家目视方向设置有虚拟准心，准心范围内有NPC的时候玩家可以通过点击鼠标与之进行交互。后台会随机提供给玩家反馈：①获得关于NPC的信息 ②获得一条生物学知识点 ③回答一道生物学常识题。其中，获取到的知识点和信息会即时存到玩家的背包中并且获得积分，之后通过按下B键随时查看背包内信息。如果玩家正确回答题目，则同样会获得积分奖励。在漫游的过程中，可能会有玩家不慎与NPC病毒发生了碰撞（非点击），此时的碰撞会导致玩家队伍的生命值降低。生命值降为0后游戏宣告失败。

#### 文件说明

- /models 目录下保存了所有游戏中形象的模型文件（.obj）和材质信息（.mtl）

- /src

  - /css 目录下有页面全局的样式单

  - /js    目录下包含玩家控制的逻辑实现javascript文件

    --main.html 游戏的主文件，包含场景渲染、前后端交互、和游戏的主逻辑

    --MathUtils.js 游戏内数学运算的工具类（主要用于运动计算）

    --Reflector.js / Refractor.js 模拟反射/折射效果的实现

    --Water2.js 游戏中模拟湖泊的效果实现

#### 难点阐述

##### Physijs物理引擎模拟移动旋转和后端位置同步的冲突

基于Physijs进行位置和旋转的模拟，同时允许后端发送的位置信息（Dirty Position）和旋转(Dirty Rotation)。同时，由于后端发送的位置信息存在延迟，可能会引发位置的跳变，所以使用插值算法来使模型的位置同步更平滑。

插值算法：

```javascript
this.update = function(){
    if(this.startMoving){
        //当开始接收到更新后的位置时开始插值计算位置更新
        this.position.x = this.position.x + (this.nextPosition.x - this.position.x) * rate;
        this.position.y = this.position.y + (this.nextPosition.y- this.position.y) * rate;
        this.position.z = this.position.z + (this.nextPosition.z - this.position.z) * rate;
        this.rotation.x = this.rotation.x + (this.nextRotation.x - this.rotation.x) * rate;
        this.rotation.y = this.rotation.y + (this.nextRotation.y - this.rotation.y) * rate;
        this.rotation.z = this.rotation.z + (this.nextRotation.z - this.rotation.z) * rate;
    }
    ...
}
```

##### 其他玩家名称显示

利用Physijs的Font库，根据用户昵称创建立体字对象，并将立体字对象添加到模型顶部，从而实现在玩家的角色模型上方实时显示玩家名称（位置同步）

示例：

```javascript
 geometry = new THREE.TextGeometry(nickName, {
     //设置字体属性
     font: idFont,
     size: 2,
     height: 0.2/*,
     curveSegments: 12,
     bevelEnabled: true,
     bevelThickness: 10,
     bevelSize: 8,
     bevelSegments: 5*/
 });
//创建法向量材质
var meshMaterial = new THREE.MeshNormalMaterial({
    flatShading: THREE.FlatShading,
    transparent: true,
    opacity: 0.9
});
var mesh = new THREE.Mesh(geometry, meshMaterial);
box.add(mesh);//将立体字对象添加到玩家模型
mesh.position.set( 0,0,0);
mesh.geometry.center();
mesh.position.y = 12;
```

##### 相机和碰撞体的结构关系

相机绑定到模型上，位于模型的坐标系中，同时，模型需要锁定旋转轴。碰撞体和模型逻辑分离，模型只负责显示，而碰撞体是包裹在在模型外部的不可见盒子

<img src="C:\Users\wangs\AppData\Roaming\Typora\typora-user-images\image-20200619145247662.png" alt="image-20200619145247662" style="zoom:67%;" />

##### 鼠标点击场景中物体判定

用RayCaster像准心方向发射线，然后判断最近的一个目标物体的属性，然后产生相应的反馈。

RayCaster示例：

```javascript
window.addEventListener('click',function (e) {
            ...
            var raycaster = new THREE.Raycaster();
            var mouse = new THREE.Vector2();
            var ev = e || window.event;
            // 通过鼠标点击位置,计算出 raycaster 所需点的位置,以屏幕为中心点,范围 -1 到 1
            mouse.x = 0;
            mouse.y = 0;
            //通过鼠标点击的位置(二维坐标)和当前相机的矩阵计算出射线位置
            raycaster.setFromCamera(mouse, camera);
            // 获取与raycaster射线相交的数组集合，其中的元素按照距离排序，越近的越靠前
            var intersects = raycaster.intersectObjects(scene.children);
            for(let i = 0; i < intersects.length;i++){
                if(intersects[i].object.oType == 'virus'){
                    console.log(intersects[i].object);
                    let sendMsg = {
                        'cellType':'T',
                        'userId':playerId,
                        'virusId': intersects[i].object.name.split("@")[1],
                        'roomId':roomId
                    };
                    console.log(sendMsg);
                    stompClient.send("/app/clickVirus", {},
                        JSON.stringify(sendMsg));
                    return;
                }
            }
        });
```



#### 亮点阐述

##### 游戏地形

通过unity地形建模后生成灰度图，然后再THREE.js中的HeightFieldMesh读取灰度图，蒋灰度作为高度数据渲染地图

```javascript
loadFloor=function () {
        var ready = false;
        var myImage = new Image();
        let arr,ccc;
    	//读取灰度图
        myImage.src = "../objects/terrain.jpg";
        myImage.onload = function(){
            var canvas = document.createElement('canvas');
            ccc= canvas;
            canvas.height = 250;
            canvas.width = 250;
            var ctx=canvas.getContext("2d");
            ctx.drawImage(myImage,0,0,250,250);
            arr = ctx.getImageData(0,0,250,250).data;

            loader = new THREE.TextureLoader();
            // 设置地形材质，略
            ground_material = Physijs.createMaterial(
                ...
            );
            ...
			// 插入高度数据
            ground_geometry = new THREE.PlaneGeometry( 500, 500, 250, 250 );
            let count = 0;
            for ( var i = 0; i < ground_geometry.vertices.length; i++ ) {
                var vertex = ground_geometry.vertices[i];
                count++;
                if(((vertex.x+250)/2*250+(250-vertex.y)/2)*4>=250*250*4)
                    vertex.z = 0;
                else
                    vertex.z = arr[((vertex.x+250)/2*250+(250-vertex.y)/2)*4]*0.5;
            }
            ground_geometry.computeFaceNormals();
            ground_geometry.computeVertexNormals();
			//生成地形
            ground = new Physijs.HeightfieldMesh(
                ground_geometry,
                ground_material,
                0, // mass
                250,
                250
            );
            ground.position.set(0, -20, 0);
            ground.rotation.x = Math.PI / -2;
            ground.receiveShadow = true;
            scene.add( ground );
        };
    };
```



##### 浮力湖泊（血液）

使用 jbouny / https://github.com/jbouny 提供的water类型库，生成流体。同时该库解助Reflector和Refractor赋予湖泊反射和折射效果，同时water设置了浮力和阻力属性，以更加符合现实场景。

```javascript
 var waterGeometry = new THREE.PlaneBufferGeometry( 500, 500 );
        var params = {
            color: '#ff9ea2',
            scale: 4,
            flowX: 1,
            flowY: 1
        };
        water = new THREE.Water( waterGeometry, {
            color: params.color,
            scale: params.scale,
            flowDirection: new THREE.Vector2( params.flowX, params.flowY ),
            textureWidth: 1024,
            textureHeight: 1024
        } );
        water.position.y = 1;
        water.rotation.x = Math.PI * - 0.5;
        scene.add( water );
```



##### 悬浮红细胞粒子效果

实际上是THREE.PointsMaterial实现的粒子效果，将红细胞作为粒子的贴图，实现红细胞悬浮的效果

初始化红细胞：

```javascript
		/*白细胞漂浮*/
        var geometry = new THREE.BufferGeometry();
        var vertices = [];
        var textureLoader = new THREE.TextureLoader();
        var sprite1 = textureLoader.load( '../images/WBC1.png' );
        var sprite2 = textureLoader.load( '../images/WBC1.png' );
        var sprite3 = textureLoader.load( '../images/WBC3.png' );
        var sprite4 = textureLoader.load( '../images/WBC4.png' );
        var sprite5 = textureLoader.load( '../images/WBC5.png' );
        for ( var i = 0; i < 50; i ++ ) {
            var x = Math.random() * 500 - 250;
            var y = Math.random() * 500 - 250;
            var z = Math.random() * 500 - 250;
            vertices.push( x, y, z );
        }
        geometry.setAttribute( 'position', new THREE.Float32BufferAttribute( vertices, 3 ) );
        parameters = [
            [[ 1.0, 0.2, 0.5 ], sprite2, 20 ],
            [[ 0.95, 0.1, 0.5 ], sprite3, 15 ],
            [[ 0.90, 0.05, 0.5 ], sprite1, 10 ],
            [[ 0.85, 0, 0.5 ], sprite5, 8 ],
            [[ 0.80, 0, 0.5 ], sprite4, 5 ]
        ];
        for ( var i = 0; i < parameters.length; i ++ ) {
            var color = parameters[ i ][ 0 ];
            var sprite = parameters[ i ][ 1 ];
            var size = parameters[ i ][ 2 ]*Math.random()*2;
            materials[ i ] = new THREE.PointsMaterial( { size: size, map: sprite, blending: THREE.AdditiveBlending, depthTest: false, transparent: true } );
            materials[ i ].color.setHSL( color[ 0 ], color[ 1 ], color[ 2 ] );
            var particles = new THREE.Points( geometry, materials[ i ] );
            particles.rotation.x = Math.random() * 6;
            particles.rotation.y = Math.random() * 6;
            particles.rotation.z = Math.random() * 6;
            scene.add( particles );
        }
```



##### 游戏动画背景

通过定义大量的div，结合css实现动态背景效果

##### 实时显示背包和收集进度

通过订阅和发送，后端检测到背包更新后同步广播到房间内各个玩家更新背包。

### 后端

#### 设计思路
##### 概念术语描述

######  细胞

* 每个用户对应一个细胞
* 每个细胞有属于自己的模型
* 细胞间合作收集信息，进行游戏
###### NPC

* 以病毒的形象出现
* 单击后能够进行交互，获得问题或知识点或细胞信息
###### 细胞信息

* 任意一条细胞的信息介绍
###### 问题

* 有四个选项，其中一个选项为正确的生物领域选择题
###### 知识点

* 任意一条生物领域的知识点
###### 背包

* 用户间互相合作，进行收集信息的背包
* 每次游戏开始时会创建新的背包
* 背包中存储信息，知识点及答对的问题
* 当次游戏中每个用户收集到的知识（含以上三种）都将加入背包
* 背包中存储的知识（含以上三种）达到一定数额时游戏结束

##### 基本设计描述

* 使用REST风格的接口进行前后端数据交互
* 使用jwt token进行权限判定
* SpringBoot通过stomp协议实现双向通信功能

#### 模块划分

整体分为用户信息模块及游戏逻辑模块。

#### 文件说明

遵循MVC架构及SpringBoot的一般设计模式，共有如下的几个包

##### config

用于配置跨域，websocket，上传文件地址映射

* UploadFilePathConfig：上传文件路径映射配置
* WebSocketConfig：websocket变量配置
* CorsConfig：跨域配置

##### controller

接受请求，交给service包中的类处理

* AuthController：用户信息相关的Controller
* WebSocketController：游戏主体逻辑相关的Controller
* PackController：背包相关的Controller
* ProblemController：问题相关的Controller

##### domain

各实体类

* User：用户
* Pack：背包
* Knowledge：知识点（游戏过程中可能呈现给用户的）
* Position：位置（由于具有实时性故不存入数据库）
* Virus：NPC
* GameRecord：游戏记录
* Authority：用户身份
* CellInfo：细胞信息（游戏过程中可能呈现给用户的）
* ChoiceQuestion：选择题（游戏过程中可能呈现给用户的）

##### repository

数据库接口，继承CrudRepository接口，运行时由SpringBoot自行注入（与除了Position的各实体类一一对应）

##### security

进行访问控制，对请求进行过滤及权限判断

##### service

对请求逻辑的实际处理

* JwtUserDetailsService：解析jwt_token获取用户信息
* ChoiceQuestionService：与数据库交互获取或存入选择题
* KnowledgeService：与数据库交互获取或存入知识点
* CellInfoService：与数据库交互获取或存入细胞信息
* PackService：背包相关的服务
* AuthService：用户信息相关的服务

##### exception

对异常的包装

##### 类图

总体结构如下：

![图片](https://uploader.shimo.im/f/kq4DBu3SPW6Bxn0c.png!thumbnail)

主要各包的具体结构如下

###### config

![图片](https://uploader.shimo.im/f/s6lb6L0jbsk6rb45.png!thumbnail)

###### controller

![图片](https://uploader.shimo.im/f/kyr18IPXyMYYy0WC.png!thumbnail)

###### service

![图片](https://uploader.shimo.im/f/IvXQMA0N1oV6wIk7.png!thumbnail)

###### repository

![图片](https://uploader.shimo.im/f/8sOFBNfFeAkNBMCa.png!thumbnail)

###### domain

![图片](https://uploader.shimo.im/f/EpAbN13wdpthFfDK.png!thumbnail)

###### security

![图片](https://uploader.shimo.im/f/ymZsOW7OBdbSQq6B.png!thumbnail)

#### 数据库设计

数据表由JPA自动生成，共有如下实体表：

* cell_info               
* choice_question          
* game_record              
* knowledge                
* pack                        
* user                       
* virus

UML图如下：

![图片](https://uploader.shimo.im/f/hTllk1AZwBvlcSK9.png!thumbnail)

#### 难点阐述

##### 利用token进行身份验证

利用token实现了系统的无状态，每次前端只需设置登录时发回的jwt_token在header中即可实现身份验证。

```java
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
    try{
        if(request.getHeader("jwt_token") != null){
            String token = request.getHeader("jwt_token");

            String username = jwtTokenUtil.getUsernameFromToken(token);
            User user = (User) jwtUserDetailsService.loadUserByUsername(username);
            if(jwtTokenUtil.validateToken(token, user)){
                setSecurityContext(new WebAuthenticationDetailsSource().buildDetails(request), token);
            }
        }
    }catch (IllegalArgumentException e){
        logger.warn(e.getLocalizedMessage());
    }
    filterChain.doFilter(request, response);
}
```

##### 应用JPA进行映射自动建表

每个实体中通过@OneToMany，@ManyToOne，@ManyToMany注解及MappedBy，JoinColumn设定实现了与其他实体的关联，并设置了cascade方便级联编辑，从而能够利用JPA自动生成表。
例如：

```Java
@ManyToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
@JsonIgnore
private Set<Knowledge> knowledgeSet;
```

#### 亮点阐述

##### 系统的可复用性

系统模块间具有隔离性，用户信息及websocket游戏部分互不干扰；

需要增加功能时只需增加对应的Entitiy-Controller-Service，逻辑明确；

整个系统的架构为分层次的。

Entitiy代表数据库中的实体，Repository类负责与数据库进行交互；

Service类为主要逻辑实现的类，注入Repository的实体，实现功能；

Controller类为接口实现的类，负责进行请求的映射，在Controller类中注入Service实体并调用Service实体的方法可以实现接口的功能；

系统的分层模块化使debug变得简洁，容易定位问题。

##### 设计模式的应用

###### MVC

Model：各实体类
Controller：各控制器类
与传统MVC不同的是，本项目的后端并不封装ViewObject，而是遵循Rest原则将数据传递给前端，由前端将数据封装成View。

###### 工厂模式

工厂模式的实质是由一个工厂类根据传入的参数，动态决定应该创建哪一个产品类。 
spring中的BeanFactory就是简单工厂模式的体现，根据传入一个唯一的标识来获得bean对象，但是否是在传入参数后创建还是传入参数前创建这个要根据具体情况来定。

###### 策略模式

在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。这种类型的设计模式属于行为型模式。Spring中在实例化对象的时候会用到策略模式。

###### 观察者模式

当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知它的依赖对象。观察者模式属于行为型模式。
在游戏主逻辑中，各细胞位置被修改或与NPC进行交互后，会自动连带背包，地图更新，并通知其余客户端，属于观察者模式的应用。

## *敏捷开发流程说明

| 阶段   | 任务                                     | 时间       |      |
| ------ | ---------------------------------------- | ---------- | ---- |
| 阶段一 | 初步demo构建，素材收集、汇总、联调、集成 | 4.1～4.19  | ✔️    |
| 阶段二 | 详细需求重构，素材收集、汇总、联调、集成 | 4.20～4.27 | ✔️    |
| 阶段三 | 详细需求完善，素材收集、汇总、联调、集成 | 5.1～6.1   | ✔️    |

- 结对编程

  前后端持续结对编程

- “站立”会议

  每日在微信群中汇报进度

- 小版本发布

  维持一个基本可用的版本，在此基础上逐步增加新特性，进行小版本发布

- 较少的文档

  尽管每份文档字数不多，但每个阶段都要相应的需求文档、进度文档、设计文档。在最后提交的时候，进行综合汇总。

- 以合作为中心，代码共享

  在github上进行代码协作

- 可调整计划

  随时机动安排人员

## 云部署说明

### 前端

#### 构建vue项目

![image-20200607143939745](/Users/bytedance/Library/Application Support/typora-user-images/image-20200607143939745.png)

```bash
vue-cli-service build
```

#### 使用Docker部署

- nginx配置

  ```
  server {
          listen 80;
          server_name localhost;
  
          access_log /var/log/nginx/host.access.log main;
          error_log /var/log/nginx/error.log error;
  
          location / {
                  root /usr/share/nginx/html;
                  index index.html index.htm;
          }
  }
  ```

- Dockerfile编写

  ```
  FROM nginx
  COPY dist/ /usr/share/nginx/html/
  COPY nginx/default.conf /etc/nginx/conf.d/default.conf
  ```

  自定义构建镜像的时候基于Dockerfile来构建。

  `FROM nginx` 命令的意思该镜像是基于 nginx:latest 镜像而构建的。

  `COPY dist/ /usr/share/nginx/html/` 命令的意思是将项目根目录下dist文件夹下的所有文件复制到镜像中 /usr/share/nginx/html/ 目录下。

  `COPY nginx/default.conf /etc/nginx/conf.d/default.conf` 命令的意思是将nginx目录下的default.conf 复制到 etc/nginx/conf.d/default.conf，用本地的 default.conf 配置来替换nginx镜像里的默认配置。

  

### 后端

采用Docker进行部署，将项目打包为jar后上传到服务器，在tomcat镜像的基础上新建项目镜像。

使用的Dockerfile分别如下：

#### tomcat基础镜像

```plain
FROM maven:3.6.3-jdk-8
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME
ENV TOMCAT_VERSION 8.5.54
ENV TOMCAT_TGZ_URL https://www.apache.org/dist/tomcat/tomcat-8/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz
COPY tomcat.tar.gz tomcat.tar.gz
RUN set -x \
&& tar -xvf tomcat.tar.gz --strip-components=1 \
&& rm bin/*.bat \
&& rm tomcat.tar.gz*
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

#### 项目镜像

```plain
FROM java:8
MAINTAINER bingo
COPY pj.jar pj.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","pj.jar"]
```



## 功能自检清单

### 基础功能自查

| 功能点                     | 详细说明                                           | 完成情况 | 完成度 |
| -------------------------- | -------------------------------------------------- | -------- | ------ |
| 前台页面                   | 【必选】提供用户登录注册                           | ✔️        | 100%   |
|                            | 【必选】选择相应的Web3D场景                        | ✔️        | 100%   |
| 后台页面                   | 【必选】记录用户信息、虚拟形象等                   | ✔️        | 100%   |
|                            | 【可选】根据场景可选提供历史行为查看和分析功能     | ✔️        | 100%   |
| 虚拟Web3D场景              | 【必选】正确显示一个可交互的3D场景，有教学意义     | ✔️        | 100%   |
| 支持多人加入同一个虚拟世界 | 【必选】相互可见，行为共享                         | ✔️        | 100%   |
|                            | 【必选】可以进行一定方式的交流                     | ✔️        | 100%   |
|                            | 【必选】环境中可以有一些可交互性的实体             | ✔️        | 100%   |
| 维护虚拟世界的一致性       | 【必选】同一时刻各个化身看到的场景是一致的且最新的 | ✔️        | 100%   |
|                            | 【可选】根据你的场景，可选提供实时聊天等附加功能   | ✔️        | 100%   |
| 云部署                     | 【必选】部署在云服务器上，提供可以访问的公网地址。 | ✔️        | 100%   |

### 附加功能自查

| 进阶功能项                 | 要求                                | 详细说明     | 是否完成 |
| -------------------------- | ----------------------------------- | ------------ | -------- |
| 虚拟形象选择5‘             | 用户可以选择、修改自己的虚拟形象    | 细胞形象选择 | ✔️        |
| 图表化展示后台记录的数据5’ | 历史性未查看与分析功能              | echarts库    | ✔️        |
| 人工智能因素10‘            | 增加智能虚拟人NPC，作简单响应       | Live2d看板娘 | ✔️        |
| 其他合理亮眼的附加功能15’  | 游戏中不同房间的创建与加入          | 游戏中心页面 | ✔️        |
|                            | 更精致、逼真的细胞模型与Threejs场景 | 游戏场景     | ✔️        |
|                            | 更好的开发部署流程                  | 详见文档     | ✔️        |
|                            | 更好的设计模式应用                  | 详见文档     | ✔️        |



## 参考目录

- Axios教程

  https://zhuanlan.zhihu.com/p/87836564

- echarts教程

  https://echarts.apache.org/zh/index.html

- Vue插件教程

  https://vuescrolljs.yvescoding.org/

- element-ui文档

  https://element.eleme.cn/#/zh-CN/component/table

  

