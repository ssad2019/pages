## 一. 使用ant-vue框架开发的优点：  
+ 提炼自企业级中后台产品的交互语言和视觉风格。  
+ 开箱即用的高质量Vue组件。  
+ 共享Ant Design of React设计工具体系。  

## 二. Vue - 渐进式JavaScript框架
+ MVC：
    + M: Model 数据模型（专门用来操作数据，数据的CRUD）
    + V：View 视图（对于前端来说，就是页面）
    + C：Controller 控制器（是视图和数据模型沟通的桥梁，用于处理业务逻辑）
+ Vue 中 数据绑定的方法
    + Mustache(插值语法)，也就是{{}}语法  
    ```html
            <p>{{text}}</p> 
    ```  
    + v-bind:
    ```html
            <div v-bind:class="classObject"></div>
    ```
+ Vue 中监听属性变化
```javascript
    <script>
    computed:{
            getFinishedSelected(){
                return this.$store.state.order.finished_selected;
            },
        },
        watch:{
            getFinishedSelected(val){
                this.finished_selected=val;
                if(val){
                    this.orderlist=this.finishedlist;
                }
                else{
                    this.orderlist=this.unfinishedlist;
                }
            },
        }
    </script>
```
+ Vue中设置路由
```javascript
    Vue.use(Router)
    export default new Router({
        mode: 'history',
        base: process.env.BASE_URL,
        routes: [
            {
            path: '/',
            name: 'home',
            component: Login
            },
            {
            path:'/login',
            name: 'login',
            component: Login
            },
            {
            path:'/register',
            name: 'register',
            component: Register
            },
            {
            path: '/content',
            name: 'content',
            component: Content,
            children:[
                {
                path:'info/login',
                component:Login
                },
                {
                path:'user',
                component:  StoreUserBlock,
                children:[
                    {
                    path:'store-info',
                    component:StoreInfo
                    },
                    {
                    path:'store-management',
                    component:StoreManagement,
                    children:[
                        {
                        path:'profit',
                        component:Profit
                        },
                        {
                        path:'dim2Code',
                        component:Dim2Code
                        },
                        {
                        path:'storeInfo-modify',
                        component:storeInfoModify
                        }
                    ]
                    },
                    {
                    path:'goods',
                    component:Menu
                    },
                    {
                    path:'type',
                    component:Type
                    }
                ]     
                },
                {
                path: 'order',
                component: StoreOrderBlock
                }
            ]
            }
        ]
    });
```
+ Vue中发送http请求  
```javascript
  var xhr = new XMLHttpRequest(); 
  var formData = new FormData();
  formData.append('file',file);//send file object
  xhr.open("POST",api,true);
  xhr.setRequestHeader("Authorization",token)
  xhr.send(formData);
  xhr.onreadystatechange=function(){
      if(this.readyState == 4){
        var responseText = this.responseText;
        var obj = JSON.parse(responseText);
        var status=obj['status'];
        var token = "";
        if(status==200){
            //response ok
        }
        else{
            //other response
        }
      }
      else{
          //no response received yet
      }
  }
```
+ Vue localStorage的使用
```javascript
    var token = window.localStorage.getItem('token');
    window.localStorage.setItem('token',token);
```
+ Vue 组件之间的通信  
多个组件可以通过下面代码调用同一个变量，通过监听变量或者修改变量实现组件之间的通信。
```javascript
    this.$store.state.order.listening=true;
```
+ Vue 定时任务和周期任务
```javascript
    mounted(){
        requestOrderList(this);
        if(this.$store.state.order.listening)return;//avoid multiple interval created
        var interval=3000;//every 3000ms flash the orderlist
        var that=this;
        var timer=setInterval(function(){
            requestOrderList(that);
            console.log("request order list");
        },interval);
        this.$store.state.order.listening=true;
    },
    //......
    var timer=setTimeout(()=>{
                window.location.href="/login";
    },1500);
```
