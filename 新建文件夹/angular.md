#### Angular脚手架 !

npm install -g @angular/cli

ng v (判断是否搭建成功)



#### 下载angular插件 !

angular 7...

angular language service 语言服务插件



#### **创建项目** !

ng new 项目名称



#### **运行项目** !

ng serve --open



#### 创建组件 !

ng g component components/header



#### 项目文档结构 !

```js
e2e -端到端测试目录
src -源文件(开发目录)
.editorconfig -编辑器统一风格工具配置文件
.gitignore -git忽略文件
angular.json -Angular CLI 脚手架配置
README.md -说明文件
package.json -npm配置文件
tsconfig.json -TypeScript编译器配置
tslint.json -TypeScript语法检查器配置

`src`
app -项目源文件
assets -存放图片等资源文件
browserslist -浏览器支持列表
environments -运行环境配置,开发或生产
favicon.ico -应用图标
index.html -项目首页
karma.conf.js -karma测试运行器的配置
main.js -项目入口
polyfills.ts -导入js,兼容老版本浏览器
style.css -全局样式
test.ts -测试入口
tsconfig.app.json -TypeScript编译器配置
tsconfig.spec.json -单元测试文件
tslint.json -额外的TypeScript语法检查器配置

`app`
app.component.css -app组件样式
app.component.html -app组件模板
app.component.spec.css -app组件单元测试
app.component.ts -app组件js代码
app.module.ts -根模块
```



#### @NgModule装饰器的元数据对象

```
declarations - 该模板拥有的组件
imports - 该模板依赖的模块
providers - 该模板拥有的服务提供商
bootstrap - 指定根组件,只有根模块需要该配置项
wxports - 公开该模块的其中一部分,以便外部模块使用
```

#### 数据绑定

##### 1- 插值表达式 {{}}

ts文件中  public student:string="小红"

##### 声明属性的几种方式

public ,共有(默认) , 可以在这个类里面使用,也可以在类外面使用

protected ,保护类型 , 只有在当前类和它的子类里面可以访问

private , 私有 , 只有在当前类才可以访问这个属性

#####  2-属性绑定

```
<div [title]="msg">哈哈哈</div>
```

##### 3-绑定html

```
<span [innerHTML]="msg"></span>
```

##### 4-绑定事件  指令(click)=”getData()”

```html
<button class="button" (click)="getData()"> 
    点击按钮触发事件 
</button> 
<button class="button" (click)="setData()"> 
    点击按钮设置数据 
</button>

getData(){ 
	alert(this.msg); 
}
setData(){ 
	this.msg='这是设置的值'; 
}

如果是a标签,则可加上事件对象,阻止默认行为
<a (click)="get($event)">点击</a>
get(event){
	event.preventDefault(); // 阻止浏览器默认行为
}
```

##### 5-**双向数据绑定**  指令[(ngModel)]

```html
1-
注意在app模块中引入：FormsModule 
import { FormsModule } from '@angular/forms';

2-
imports: [ 
	BrowserModule, 
	FormsModule 
]

3-
<input type="text" [(ngModel)]="inputValue"/>
{{inputValue}}
```

###### 1 表单例子

```html
<div class="people_list">
<ul>
    <li>姓 名：<input type="text" id="username" [(ngModel)]="peopleInfo.username" value="fonm_input" /></li>
    
    <li>性 别：
      <input type="radio" value="1" name="sex" id="sex1" [(ngModel)]="peopleInfo.sex"> <label for="sex1">男 </label>　　　
      <input type="radio" value="2" name="sex"  id="sex2" [(ngModel)]="peopleInfo.sex"> <label for="sex2">女 </label>
    </li>
    
    <li>    
    城 市：
      <select name="city" id="city" [(ngModel)]="peopleInfo.city">
      <option [value]="item" *ngFor="let item of peopleInfo.cityList">{{item}}</option>
      </select>
    </li>
    
    <li>
    爱 好：        
      <span *ngFor="let item of peopleInfo.hobby;let key=index;">
      <input type="checkbox"  [id]="'check'+key" [(ngModel)]="item.checked"/> <label [for]="'check'+key"> {{item.title}}</label>
        &nbsp;&nbsp;
      </span>         
     </li>
    
     <li>     
     备 注：              
     <textarea name="mark" id="mark" cols="30" rows="10" [(ngModel)]="peopleInfo.mark"></textarea>           
     </li>    
</ul>
    
<button (click)="doSubmit()" class="submit">获取表单的内容</button>
</div>

// ts文件
public peopleInfo:any={
    username:'',
    sex:'2',
    cityList:['北京','上海','深圳'],
    city:'上海',
    hobby:[{
          title:'吃饭',
          checked:false
      },{
            title:'睡觉',
            checked:false
        },{
          title:'敲代码',
          checked:true
      }],
      mark:''
  }
```

###### 2 todoList例子

```html
<div class="todolist">
  <input class="form_input" type="text" [(ngModel)]="keyword" (keyup)="doAdd($event)" />
  <hr>
  <h3>待办事项</h3> 
    <ul>
      <li *ngFor="let item of todolist;let key=index;" [hidden]="item.status==1">               <input type="checkbox" [(ngModel)]="item.status" />  
          {{item.title}}   ------ <button (click)="deleteData(key)">X</button>           
      </li>
    </ul>


  <h3>已完成事项</h3>
    <ul>
      <li *ngFor="let item of todolist;let key=index;" [hidden]="item.status==0">
      <input type="checkbox" [(ngModel)]="item.status" />  
          {{item.title}}   ------ <button (click)="deleteData(key)">X</button>    
        </li>
      </ul>
</div>


doAdd(e){    
    if(e.keyCode==13){               
        if(!this.todolistHasKeyword(this.todolist,this.keyword)){
          this.todolist.push({
            title:this.keyword,
            status:0                   //0表示代办事项  1表示已完成事项
          });
          this.keyword='';
        }else{
          alert('数据已经存在');
          this.keyword='';
        }
     }   
  }

deleteData(key){
    this.todolist.splice(key,1);
  }

// 如果数组里面有keyword返回true  否则返回false
todolistHasKeyword(todolist:any,keyword:any){   
    if(!keyword) return false;
    for(var i=0;i<todolist.length;i++){
      if(todolist[i].title==keyword){
          return true;
      } 
    }
    return false;
  }
```

#### 指令 [ngClass]、[ngStyle]

```html
[ngClass] -动态添加或移除css类
<div [ngClass]="{'red': true, 'blue': false}">   // 是一个对象
	这是一个 div 
</div>

public arr = [1, 3, 4, 5, 6]; 
<ul>
    <li *ngFor="let item of arr, let i = index"> 
        <span [ngClass]="{'red': i==0}">{{item}}</span> 
    </li> 
</ul>

如果只加一个类
<p [class.red]="isRed"></p>

----------

[ngStyle]
<div [ngStyle]="{'background-color':'green'}">
    你好 ngStyle
</div> 

public attr='red'; 
<div [ngStyle]="{'background-color':attr}">
    你好 ngStyle
</div>
```

#### 条件判断 *ngIf !

```html
<p *ngIf="list.length > 3">这是 ngIF 判断是否显示</p>
```

#### 数据循环 *ngFor !

```html
<ul>
	<li *ngFor="let item of list; let key = index; let odd = odd" [class.red]='odd'>
		{{item}}--{{key}}--{{odd?'奇数':'偶数}}
	</li> 
</ul>

-------

*ngFor添加trackBy (提升渲染对象数组的性能)       // 只有在渲染对象时才会有性能问题
<p *ngFor="let item of colors; trackBy:trackById"></p>

// ts
colors = [
	{id:1,name:'red'},
	{id:2,name:'green'},
]
trackById(index,color){        // 索引号,数组每一项
	return color.id            // 告诉angular按数据id跟踪渲染数据
}
```

#### 组件交互/父子传值

```js
`父传子`
父组件不仅可以给子组件传递简单的数据，还可把自己的方法以及整个父组件传给子组件

父组件给子组件 传数据-------------------------------------------------------
1-在子组件ts文件中导入input装饰器,并公开属性
// 导入装饰器,子组件公开属性
import { Component, OnInit, Input } from '@angular/core'
// 公开属性
  @Input() skill

2-父组件html中
<div>
  <h1>父组件：</h1>
  <app-child [skill]="parentData"></app-child>
</div>
  
// 子组件html
<p>子组件，接受到父组件传递过来的数据：{{ skill }}</p>

父组件给子组件 传方法------------------------------------------------------
<app-header [run]='run'></app-header>
@Input() run:any; 
getParentRun(){
	this.run();
  }

把整个父组件 传给 子组件---------------------------------------------------
<app-header [home]='this'></app-header>
getParentRun(){
    alert(this.home.msg);
    this.home.run();
  }

=========================================================================
    
`子传父`
方法一: 通过ViewChild

方法二:
四步: 
子组件创建事件
父组件绑定事件
子组件触发事件
父组件调用事件

`1-在子组件ts文件中导入Output(子组件暴露事件),EventEmitter(创建事件),并创建事件`
import { Component, OnInit, Output, EventEmitter } from '@angular/core'
// 1 创建事件
  @Output() getMoney = new EventEmitter()

`2-在父组件ts文件中提供方法,在父组件html中绑定事件`
// 2 提供方法
  giveMoney(data) {
    console.log('父组件中的方法调用了', data)
    this.msg = data
  }

<div>
  <h1>父组件：{{ msg }}</h1>
// 2 绑定事件 
  <app-child (getMoney)="giveMoney($event)"></app-child>
</div>

`3-子组件触发事件`
<button (click)="handleClick()">触发子组件的事件</button>
// 3 触发事件
handleClick() {    
    this.getMoney.emit('孩子问爸爸要1000块钱')
  }
`4-父组件调用事件`

=========================================================================
    
`非父子组件通讯`
1、公共的服务 
2、Localstorage (推荐) 
3、Cookie
```

#### 如何展示todo模块

```js
一,在主模块中导入todo模块      
  import { TodosModule } from './todos/todos.module'
  imports: [BrowserModule, TodosModule]

二,在todo模块中通过export导出todo组件
  exports: [TodoComponent]

三,在根组件中使用app-todo组件
  <app-todo></app-todo>
```

#### typeScript

```js
1 类型注解	为函数或变量添加约束
2 接口		对值所具有的结构进行类型检查   interface
  (若接口在多个地方用到,可抽离出来,避免后期修改时需要改多个地方,方便维护,先暴露后导入使用)
3 泛型		<> 泛型类EventEmitter约定参数类型
4 类成员修饰符	public 公共（默认） 、 private 私有
```

#### 服务 !

```js
组件只用于处理跟视图相关的内容,服务提供业务逻辑

服务的作用
组件应该只提供用于数据绑定的属性和方法
组件不应该定义任何诸如从服务器获取数据、验证用户输入等操作
应该把各种处理任务定义到可注入的服务中

服务的作用：处理业务逻辑，供组件使用
服务和组件的关系：组件是服务的消费者

1 通过  @Injectable()装饰器 来表示一个服务
2 服务需要注册提供商才可以使用
3 Angular通过依赖注入（DI）来为组件提供服务
4 DI使得在使用服务时，只提供要使用的服务即可。不需要手动创建服务实例
5 推荐在 constructor 中提供组件中用到的服务

`1-创建服务命令`
ng g service myService 

创建到指定目录下面 
ng g service services/storage

`2-使用的页面引入服务，注册服务`
import { StorageService } from '../../services/storage.service';
constructor(private storage: StorageService) {}

`2.1-注册提供商`

`3-在服务里写方法`
get(){ }

`4-调用该方法`
this.stotage.get()

注册提供商的三种方式
1 通过 @Injectable 的 providedIn: 'root' 注册为 根级 提供商
2 通过 @NgModule 的 providers: [StorageService] 注册为 模块 内可用的提供商
3 通过 @Component 的 providers: [StorageService] 注册为 组件 的提供商
  (只有在当前组件及当前组件的子组件中可以使用,在兄弟组件中不可用)
```

#### HttpClient !

```js
作用：发送Http请求
封装了浏览器提供的 XMLHttpRequest 接口
使用基于可观察（Observable）对象的 API 
提供了请求和响应拦截器
流式错误处理机制...

// 1 在app.module.ts中导入HttpClient模块
import { HttpClientModule } from '@angular/common/http'
imports: [BrowserModule, HttpClientModule]

// 2 组件中/服务中 导入HttpClient
import { HttpClient } from '@angular/common/http'

// 3 在组件中/服务中 创建一个 http 属性 (注入http服务)
constructor(private http: HttpClient) {}

// 4 发请求
interface Todo {      // 接口
  name: string
  description: string
}

this.http.get<Todo>('../assets/todos.json').subscribe((res: Todo) => {
      this.name = res.name
})

-------------------
    
// 获取完整的http响应 
import { HttpClient, HttpResponse } from '@angular/common/http'
this.http
      .get<Todo>('../assets/todos.json', { observe: 'response' })
      .subscribe((res: HttpResponse<Todo>) => {  // 由于这里使用了HttpResponse,所以上面得导入
        console.log(res.headers.get('content-type'), res.body)
        this.name = res.body.name
})
1-要想获取响应头中的内容,只需要给其加 { observe: 'response' } 即可
2-加类型检查的两种方式
  一.<Todo>
  二.HttpResponse<Todo>

// 在HttpClient中处理错误
this.http
      .get<Todo>('../assets/todos.json', { observe: 'response' })
      .subscribe((res: HttpResponse<Todo>) => { 
        this.name = res.body.name
},err => {
    console.log(err)
})

---------------------

// GET 获取数据
  getData() {
    this.http.get(this.url).subscribe(res => {
      console.log(res)
    })
  }

  // POST 添加数据
  addData() {
    this.http
      .post(this.url, {
        name: '测试添加',
        done: true
      })
      .subscribe(res => {
        console.log('POST成功：', res)
      })
  }

  // DELETE 删除数据
  delData() {
    this.http.delete(`${this.url}/2`).subscribe(res => {
      console.log('DELETE 成功：', res)
    })
  }

  // PATCH 修改数据
  updateData() {
    this.http
      .patch(`${this.url}/3`, {
        name: '好好学习 天天向上'
      })
      .subscribe(res => {
        console.log('PATCH 成功：', res)
      })
  }

--------------

在项目中使用方式
// 在service中
getTodos() {
    return this.http.get<Todo[]>(this.todosUrl)
  }
// 在组件中
this.todosService.getTodos().subscribe((todos: Todo[]) => {
      // this.todos = todos
      this.todos = [...todos]
  })
```

#### HttpClient 拦截器

```js
问题：每个请求都要在请求头中添加 Authorization，太繁琐
解决方式：使用 HttpClient 拦截器
作用：用它们监视和转换从应用发送到服务器的HTTP请求

步骤:
1 手动创建拦截器服务 auth.interceptor
  新建http-interceptor文件夹 - 新增auth.interceptor.ts
2 导入Injectable
  import { Injectable } from '@angular/core'
3 通过Injectable装饰类
  @Injectable()
  export class AuthInterceptors {}
4 导入HttpInterceptor接口
  import { HttpInterceptor } from '@angular/common/http'
5 继承接口 HttpInterceptor 并实现 intercept() 方法
  export class AuthInterceptors implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req)
  }
}
6 导入
  import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http'
  import { Observable } from 'rxjs'
7 在根模块app.module中注册当前服务的服务提供商
  (由于HttpClientModule在根模块中导入, 所以是在根模块去注册)
  // 导入 
  import { AuthInterceptors } from './http-interceptors/auth.interceptors'
  import { HTTP_INTERCEPTORS } from '@angular/common/http';
  // 在providers中写
  providers: [{
    provide: HTTP_INTERCEPTORS,
    useClass: AuthInterceptors,
    multi: true  // 为true表示可能有多个拦截器
  }]

8 添加token
  (修改请求方式：1 先克隆 2 修改这个克隆体 3 然后把克隆体传给 next.handle())

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = localStorage.getItem('itcast-token')
    const authReq = req.clone({
      headers: req.headers.set('Authorization', `Bearer ${token}`)
    })
    return next.handle(authReq)
  }
9 排除登录页
  // 给登录页请求中加一个标识
  login(loginForm: LoginForm) {
    return this.http.post(`${URL}/tokens`, loginForm, {
      headers: { 'No-Auth': 'TRUE'}
    })
  }
  // 在AuthInterceptors.ts中
  if (req.headers.get('No-Auth') === 'TRUE') {
      return next.handle(req)
    }

   const token = localStorage.getItem('itcast-token')
   const authReq = req.clone({
      headers: req.headers.set('Authorization', `Bearer ${token}`)
    })
   return next.handle(authReq)
10 加错误处理
   导入import { tap } from 'rxjs/operators'
      import { Router } from '@angular/router'
	  constructor(private router: Router) {}
   ----
   return next.handle(authReq).pipe(
      tap(
        // 成功的回调
        ok => {},
        // 失败的回调
        error => {
          if (error.status === 401) {
            localStorage.removeItem('itcast-token')
            this.router.navigate(['/login'])
          }
        }
      )
    )

-------------------------------------
参数一：req: HttpRequest<any> 请求头对象
参数二：next: HttpHandler 下一个拦截器，如果只有一个，则会把请求发给服务器，并接收服务器的响应
```



#### 路由

```js
路由是实现 SPA（单页应用程序） 的基础设施
作用：让用户从一个视图导航到另一个视图
路由是：URL和组件的对应规则
使用：HTML5风格（history.pushState）的导航
支持：重定向、路由高亮、通配符路由、路由参数
支持：子路由、路由模块、路由守卫、异步路由...

步骤
1 在index.html中设置<base href="/">
2 导入RouterModule模块及路由规则
3 配置路由规则 appRoutes
4 将 RouterModule.forRoot(appRoutes) 模块配置在根模块中
5 使用<router-outlet> 指定路由出口
6 使用 routerLink=”/home”指定导航链接

// 1-在主模块中导入路由模块 及 路由规则约束
import { RouterModule, Routes } from '@angular/router'

// 2 配置路由规则
const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  }
]

// 3 在@NgModule中配置路由模块 (配置路由模块，作为根模块的依赖项)
imports: [BrowserModule, RouterModule.forRoot(appRoutes)]

// 4 在根组件中指定路由出口
<router-outlet></router-outlet>

// 5 指定路由的导航链接
<a routerLink="/home">首页</a>

----------------

`forRoot的说明`
问题说明：服务应该是单例的，某些场景下会造成服务多次注册，破坏服务的单例特性
比如：路由懒加载的情况
解决方式：使用模块的 forRoot() 方法导入模块
RouterModule的 forRoot() 保证项目中只有一个Router服务

-----------------

// 默认路由
{
   path: '',
   redirectTo: '/home',
   pathMatch: 'full'
}

// 通配符路由   注意：应该出现在路由规则的最后！！！
{
   path: '**',
   component: NotfoundComponent
}

------------------

// 编程式导航
1-导入路由提供的服务
import { Router } from '@angular/router'
constructor(private router: Router) {}
2-用编程式导航来实现跳转
this.router.navigate(['/home'])

// 路由参数
{
    path: 'car/:id',
    component: CarComponent
}
1-在组件中导入路由服务
import { ActivatedRoute } from '@angular/router'
constructor(private route: ActivatedRoute) {}
2-获取参数
ngOnInit() {
    this.route.paramMap.subscribe(param => {
      // console.log('路由参数为：', param)
      const id = param.get('id')                  // 拿到的id是一个字符串
    })
  }

// 子路由
1-子路由出口
<router-outlet></router-outlet>
2-配置子路由
{
    path: 'home',
    component: HomeComponent,
    children: [
      {
        path: 'employee',
        component: EmployeeComponent
      }
    ]
  }

// 路由激活高亮
<a routerLink="/home" routerLinkActive="actived">首页</a>
<a routerLink="/about" routerLinkActive="actived">关于</a>

如果子组件高亮,不想其父组件也高亮,可给其加 精确匹配
<a routerLink="/home" routerLinkActive="actived" [routerLinkActiveOptions]="{ exact: true }">首页</a>
```



#### 表单 !

```js
两种表单
1 响应式表单（重点）
2 模板驱动表单（基于模板语法，[(ngModel)] ）

`响应式表单`
优势：让开发人员完全掌控表单
表单数据的初始化
表单值的获取
表单验证和提交...

具体的优势
它提供了一种模型驱动的方式来处理表单输入
简单理解：数据驱动视图的思想
同步的数据访问，保证数据和视图是一致的、可预测的
增强了可测试性，让测试变的更简单
内置表单验证器，可以自定义验证器...

步骤
// 1 在根模块中导入响应式表单模块
import { ReactiveFormsModule } from '@angular/forms'
// 2 配置为当前模块的依赖项
imports: [BrowserModule, ReactiveFormsModule]
// 3 在组件中导入表单控件
import { FormControl } from '@angular/forms'
// 4 
export class AppComponent {
  username = new FormControl('播客')
  password = new FormControl('')
  // 获取用户名
  getUserName() {
    console.log('当前用户名为：', this.username.value)
  }
  // 更新用户名
  setUserName() {
    this.username.setValue('博学谷')
  }
}
// 5 在组件html中
<div>
  <label>
    用户名：
    <input type="text" [formControl]="username">
  </label>
  <p>{{ username.value }}</p>   // 要通过value去拿到值

  <label>
    密码：
    <input type="text" [formControl]="password">
  </label>
  <p>{{ password.value }}</p>

  <button (click)="getUserName()">获取用户名</button>
  <button (click)="setUserName()">更新用户名</button>
</div>

---------------------------
      
`表单验证`
1 内置表单验证器：Validators
2 自定义验证器

表单验证常用属性
属性value：表示该表单控件的值
属性errors：表示描述错误的对象
属性dirty：表示是否编辑过表单
方法hasError()：用来获取错误信息

内置表单验证器步骤:----------------------------
1,导入Validators
import { FormControl, Validators } from '@angular/forms'

2,Validators.required 表示必填项
username = new FormControl('', [Validators.required])

3,在html中,展示错误信息
<p *ngIf="username.errors && username.errors.required">用户名为必填项</p>
// errors存在且required为true时显示错误信息,如果不加前面的条件,当errors为null时会报错

优化  <p *ngIf="username.errors?.required">用户名为必填项</p>
	 <p *ngIf="username.dirty && username.errors?.required">用户名为必填项</p> 
     // dirty表示是否有编辑过
	 <p *ngIf="username.dirty && username.hasError('required')">用户名为必填项</p>

eg:
验证规则: 要求最少长度为3
1-username = new FormControl('', [Validators.minLength(3)])
2-<p *ngIf="username.dirty && username.hasError('minlength')">用户名密码长度不能少于3位</p>
// 注意上面写的两个minlength是不同的,鼠标放在minLength上面可看到其在下方中的正确书写方式

eg:pattern 使用正则

    
自定义表单验证器-----------------------------
自定义验证器是个函数
参数类型：AbstractControl
验证成功：返回 null
验证失败：返回一个描述错误的对象（自己定义)

步骤:
1-
username = new FormControl('', [this.customValidate])
2-
    // 只要数据改变,就会执行自定义验证器  control即跟上面打印的username是同个对象
  customValidate(control: AbstractControl) {   // 加入AbstractControl
    // console.log(control.value) 拿到文本框的值
    if (/^[a-z]{3,6}$/.test(control.value)) {
      // 成功 返回 null
      return null
    }
    // 失败 返回 错误对象
    return { regerror: true }
  }
3-html
<p *ngIf="username.dirty && username.hasError('regerror')">用户名为小写字母,长度为3到6位</p>

补充:
import { FormControl, AbstractControl } from '@angular/forms'
// 加入AbstractControl,则会有提示信息

---------------------------------
    
`整个表单的控制`
步骤
1-导入FormGroup
import { FormControl, FormGroup, Validators } from '@angular/forms'

2-
loginForm = new FormGroup({
    username: new FormControl('传智播客'),
    password: new FormControl('', [Validators.required])
  })

3-用form标签进行包裹,并把里面的 [formControl]="password" 改成 formControlName="password"
<form [formGroup]="loginForm" (ngSubmit)="handleSubmit()">
    <label>
      用户名：
      <input type="text" formControlName="username">
    </label>
    <p *ngIf="loginForm.get('username').dirty && loginForm.get('username').hasError('required')">请输入用户名</p>  
	// 显示错误信息,如果只用error则不管是哪个错误都会提示,而用hasError可区分是哪一种错误

    <label>
      密码：
      <input type="text" formControlName="password">
    </label>
    <p>
      <button type="submit">提交表单</button>
    </p>
</form>

4-给表单加一个提交按钮,并绑定事件
<button type="submit">提交表单</button>
在form标签中绑定事件  (ngSubmit)="handleSubmit()" 

5-
handleSubmit() {
    // console.log(
    //   '表单提交了',
    //   this.loginForm.value,    可拿到表单所有值
    //   '表单是否验证成功:',
    //   this.loginForm.valid     可判断是否验证成功
    // )

    if (this.loginForm.valid) {
      console.log('表单验证成功, 发送请求提交表单')
    } else {
      console.log('表单验证失败, 提示用户表单验证失败')
    }
  }

------------------------------
    
`通过FormBuilder优化`
1-导入FormBuilder (构建form表单)
import { FormControl, FormGroup, Validators, FormBuilder } from '@angular/forms'
2-注入服务
constructor(private fb: FormBuilder) {}
3-修改原代码
loginForm = this.fb.group({
    username: [''],
    password: ['', Validators.required]
  })
```

#### 引入图片

```html
<img src="assets/images/01.png" alt="" />  (不需要../)
如果是外部链接要使用绑定属性进行引入图片
```

#### 条件判断 *ngSwitch

```html
<ul [ngSwitch]="score"> 
	<li *ngSwitchCase="1">已支付</li> 
	<li *ngSwitchCase="2">订单已经确认</li> 
	<li *ngSwitchCase="3">已发货</li> 
	<li *ngSwitchDefault>无效</li> 
</ul>
```

#### **管道** !

```html
public today=new Date();
<p>{{today | date:'yyyy-MM-dd HH:mm:ss' }}</p>
```

#### **表单事件**

```html
<input type="text" (keyup)="keyUpFn($event)"/>
keyUpFn(e){ 
	console.log(e) 
}
```

#### 操作dom

```js
`原生 js`
ngAfterViewInit(){  // 视图加载完成后触发的方法(生命周期函数)
	var boxDom:any=document.getElementById('box'); 
    boxDom.style.color='red'; }

`ViewChild`
1-
    <div #myattr></div>   // 起一个名字
2-
    import { Component ,ViewChild,ElementRef} from '@angular/core'; // 引入viewchild
3-
    @ViewChild('myattr') myattr: any;  // 通过装饰器获取到节点后赋值给myattr
4-
    ngAfterViewInit(){ 
    this.myattr.nativeElement.style.width="100px";  // 操作dom节点 
}
```

#### **父子组件中通过** **ViewChild** 调用子组件的方法/数据

```js
1.调用子组件给子组件定义一个名称 
<app-footer #footerChild></app-footer> 
2. 引入 ViewChild 
import { Component, OnInit ,ViewChild} from '@angular/core'; 
3. ViewChild 和刚才的子组件关联起来 
@ViewChild('footerChild') footer; 
4.调用子组件 
run(){this.footer.footerRun(); }
```

#### 生命周期函数

```js
// ngOnChanges() - 父组件给子组件传值时会触发
ngOnInit(){}  - 组件和指令初始化完成 (一般用于请求数据)
// ngDoCheck()
// ngAfterContentInit()
// ngAfterContentChecked()
ngAfterViewInit(){}  - 视图加载完成后触发 (一般用于进行dom操作)
// ngAfterViewChecked()
ngOnDestroy() - 销毁指令/组件之前调用
```

#### angular 补充

```js
1-定义一个变量要给其指定类型,否则有可能会报错
2-[hidden] 隐藏 (todoList例子)
3-数据持久化 (利用本地存储)
4-ng g m xxx  创建模块
5-CommonModule模块主要是提供了*ngif,*ngfor等指令
  BrowserModule实际上也是包含CommonModule,还有一些渲染功能,只需要主模块导入一次就好
6-不要在constructor中放一些逻辑代码,只应该提供属性或提供组件使用的服务 
7-:void (表示没有返回值)
8-findIndex方法 找到索引号
  const curIndex = this.todos.findIndex(todo => todo.id === id )
```



#### 抽离路由模块 !

```js
创建路由模块：ng g m app-routing --flat --module=app
--flat：在 src/app 中创建路由文件，而不是在单独的目录中
--module=app：将该模块注册到 AppModule 中
(把路由抽离为单独的模块)

// Bug
要在路由模块中的@NgModule中将RouterModule导出
exports: [RouterModule]

原因
由于<router-outlet>是依赖于RouterModule的,但在根模块中并没有导入RouterModule,只是导入了路由模块AppRoutingModule,因此要在路由模块中将RouterModule暴露出来
```



#### 路由守卫 !

```js
用CanActivate来处理导航到某路由的情况
用CanActivateChild来处理导航到某子路由的情况
用CanDeactivate来处理从当前路由离开的情况
用Resolve在路由激活之前获取路由数据
用CanLoad来处理异步导航到某特性模块的情况

步骤:
1 创建：ng generate guard auth

2 在路由模块中添加导入 auth.guard
import { AuthGuard } from './auth.guard'

3 给 home 路由添加 canActivate 守卫 (哪个路由需要就给哪个路由加守卫)
{
    path: 'home',
    component: HomeComponent,
    canActivate: [AuthGuard] }

4 在auth.guard中加逻辑判断
 canActivate(): boolean {
    const token = localStorage.getItem('itcast-token')
    if (!!token) {
      return true;
    }
    this.router.navigate(['/login'])
    return false
  }

注意 : 路由守卫对子路由也会起作用
```

#### 异步路由 !

```js
特点：用户请求某个模块的时候，才加载这个模块
优势：减小了初始加载包体积，提高了首屏加载速度

步骤:
1 创建带有路由的模块：ng g m employees --routing
2 创建员工列表组件：ng g c employees/employee-list
3 创建员工添加组件：ng g c employees/employee-add
4 在 employees-routing 中配置路由规则
5 在 app-routing 通过 loadChildren 异步加载employees模块
    // 异步加载路由,实际上加载的是模块
  {
    path: 'employee',
    // 要将异步加载模块的路径以及模块名称配置在此处
    // 语法: 异步加载模块的路径#模块名称
    loadChildren: './employees/employees.module#EmployeesModule'
  }
注意：不要在根模块中加载 employees 模块
```

#### angular 模板引用变量  !

```html
#变量名
模板引用变量就是给一个标签起一个命名，比如#Var，然后可以在当前模板中引用Var的值；
也可以将Var的值传入到ts中进行后续的处理

eg:
如果此DOM元素只是一个普通元素，则该变量包含对该元素的引用,可以通过以下变量访问该DOM元素的任何属性
<input type="text" name="phone" placeholder="请输入手机号" class="form-control" 
   #phone (blur)="showOne(phone.value);"><br>
<span class="lable lable-primary">{{phone.value}}</span>
// ts中
showOne(str: string) {
    console.log(str);
}

如果变量在组件中，则该变量将代表该组件的实例
<my-component #var [input]="'hello'"></my-component>  
{{ var.input }}

如果要访问某个指令的实例，则需要指定哪一个指令，因为DOM元素上可能存在多个指令 - ???
```





#### 项目 !

```
技术栈
1 Angular
2 UI组件库：Ant Design of Angular（NG-ZORRO）
3 server：json-server + JWT权限认证

搭建
1 创建项目：ng new itcast-hmr
2 cd itcast-hmr
3 使用antd：ng add ng-zorro-antd
4 ng serve --open
```

##### 1.登录模块 !

```js
`ts中`
export class LoginComponent implements OnInit {
  constructor(
    private fb: FormBuilder,
    private loginService: LoginService,
    private router: Router
  ) {}
    
  loginForm: FormGroup

  submitForm(): void {
    const loginForm = this.loginForm
    const { controls } = loginForm

    for (const i in controls) {  // controls 表单中的每一个formcontrol
      if (controls.hasOwnProperty(i)) {  // 排除原型中的属性
        controls[i].markAsDirty()   // 标记为已经编辑过
        controls[i].updateValueAndValidity()  // 重新计算控件的值及校验
      }
    }

    // 判断验证是否成功
    if (!loginForm.valid) {
      console.log('验证失败')
      return
    }

    const { userName, password } = loginForm.value
    const loginParams: LoginForm = {
      username: userName,
      password
    }

    this.loginService.login(loginParams).subscribe((res: Token) => {
      // 存储token
      localStorage.setItem('itcast-token', res.token)
      this.router.navigate(['/home'])   // 路由跳转也需要导入服务和注入服务
    })
  }

  ngOnInit(): void {
    this.loginForm = this.fb.group({
      userName: [
        'zqran',
        [Validators.required, Validators.minLength(3), Validators.maxLength(6)]
      ],
      password: [
        '123456',
        [Validators.required, Validators.pattern(/^[a-zA-Z0-9]{3,6}$/)]
      ]
    })
  }
}

`html中`
<nz-form-explain *ngIf="loginForm.get('userName').dirty && (loginForm.get('userName').hasError('minlength') || loginForm.get('userName').hasError('maxlength'))">用户名长度为3-6位</nz-form-explain>
(校验2个及以上)

-------------------------------------------

// 要发送请求
在根模块中导入HttpClientModule
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http'
// 把逻辑放在独立的服务中
ng g s login/login (创建服务)
// 在服务文件中导入HttpClient
import { HttpClient } from '@angular/common/http'
// 注入服务
constructor(private http: HttpClient) {}
// 写登录的方法
interface LoginForm {
  username: string
  password: string
}

  login(loginForm: LoginForm) {  // 加类型约束
    return this.http.post(`${URL}/tokens`, loginForm, {
      headers: {
        'No-Auth': 'TRUE'
      }
    })
  }
// 回到组件中导入登录服务 并 在constructor中注入服务
import { LoginService } from './login.service'
private loginService: LoginService
    
// 在ts中调用方法...
    
// 路由跳转导入及注入服务
import { Router } from '@angular/router'
private router: Router

// 抽离类型约束 login.type.ts     export & import

// 抽离基地址 config.ts      export & import

// 路由守卫实现访问控制思路
1 登录成功后，将 token 存储在 localStorage 中
2 在路由守卫CanActivate 中判断是否有token
3 如果有直接放行
4 如果没有，跳转到 login 页
注意点：不要校验 login 页
```

##### 2.优化 !

```
问：进入应用就加载所有功能模块是否合理？
不合理！！！
1 增加了初始加载包的体积
2 降低了首屏加载速度
3 长时间白屏，用户体验差
4 对服务器造成额外的压力

解决-异步路由
```

