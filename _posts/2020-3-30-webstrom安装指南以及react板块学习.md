[toc]



# 1.webstrom安装

###### 1.下载webstrom

###### 2.安装webstrom

https://blog.csdn.net/liudongdong19/article/details/79921880

###### 3.获取激活码，激活webstrom

http://idea.medeming.com/jets/

###### 4.下载，安装nodejs

http://nodejs.cn/

###### 5.配置webstrom中的nodejs环境变量

https://www.cnblogs.com/dogharry/p/4335157.html

###### 6.打开webstrom中的terminal:local，命令行

# 2.react

###### 7.查看node版本

```js
node --version
```

C:\Code>node --version
v12.15.0

###### 8.更改为淘宝源

```js
npx nrm use taobao
```

C:\Code>npx nrm use taobao
npx: installed 494 in 52.261s

   Registry has been set to: https://registry.npm.taobao.org/

## 2.1.创建react文件

> ```js
> npx create-react-app demo
> ```

后续系统会自动下载安装react相关的插件



###### 10.根据系统提示，cd进入创建的项目，

```
例如：cd  demo
```

输入命令：ls/dir

windows的dir命令相当于linux的ls

###### 11.命令

```
npm start
```

浏览器自动弹出一个React App（localhost:3000）说明react环境变量配置成功。

## 2.2.安装组件

路由组件

npm install react-router-dom --save

axios安装

npm install axios

qs安装

npm install qs

mock安装

npm install mockjs --save

## 2.3.上下文使用方法

### 1、app顶部创建,这是初始值和方法

```js
export const AuthContext=React.createContext({
    user:{
        vip:'',
        token:''
    },
    setToken:()=>{},
    setVip:()=>{}
});
```

### 2、设置Provider，传入value

```js
render() {
    return (
        <AuthContext.Provider value={this.state.auth}>
                <HashRouter>
                    <Switch>
                        <Route exact path="/" component={ArticleList}/>
                        <Route path="/article" component={Article}/>
                        <Route path="/login" component={Login}/>
                        <Route path="/pay" component={Pay}/>
                    </Switch>
                </HashRouter>
        </AuthContext.Provider>
    );
}
```

### 3、其他组件导入AuthContext,使用value.值和value.function

```js
import {AuthContext} from '../app'
```

```js
return(
    <AuthContext.Consumer>
        {(value)=>{
            return(
                <div className="Pay">
                    <div className="Title"><span className='Return'/>用户名</div>
                    <div className="Cost">
                        <div className='Year'>
                            <input name="Cost" type="radio"
                                   onClick={setCost('￥498')}
                            />包年
                            <span>￥498</span>
                        </div>
                        <div className='Mouth'>
                            <input
                                name="Cost"
                                type="radio"
                                   onClick={setCost('￥58')}
                            />包月 <span>￥58</span>
                        </div>
                    </div>
                    <div className="Bottom">
                        <div className="Title"><span className="S1">实付：</span><span className="S2">{cost}</span><span className="S3">暂无可用优惠券</span></div>
                        <div className="Submit">
                            <button onClick={()=>{handleBuy(value)}}
                            >{value.user.token?'立即订阅':'登录订阅'}</button>
                        </div>
                    </div>
                </div>
            )
        }}
    </AuthContext.Consumer>
)
```

## 2.4.hook使用方法

1.class改为function方法。

2.设置userState。

```js
function Pay() {
    let [cost,setCost]=useState('￥498');
}
```

3.去掉render()

4.将state的值直接改为userstate的值，变量可以直接赋值。

```js
let [articles,setArticles]=useState([]);
```

```js
setArticles(response.data.articles);
```

5.组件挂载时执行，useEffect组件。页面取消挂载时执行，useEffect内加return

```js
useEffect(()=>{
    axios({
        method:"get",
        url:'/api/article',
    }).then((result)=>{
        console.log(result)
    }).catch((error)=>{
        console.log(error)
    })

    return ()=>{
        console.log('离开文章详情页')
    }
},[])
```



## 2.5.mock使用



### 1.moke安装

npm install moke --save

### 2.创建一个文件，引入mock

```js
import Mock from 'mockjs'
```

### 3.编写response内容

格式：Mock.mock(rurl,rtype,template)

#### 3.1.简单的内容

```js
Mock.mock('/api/buy',"post",{
    vip:true,
    status:200,
    data:"购买成功"
})
```

#### 3.2.复杂的内容

```js
Mock.mock('/api/login',"post",(options)=>{
    let result=qs.parse(options.body)
    if(result.username==='123456'&&result.password==='123456'){
        return{
            status:200,
            token:"123456789",
            data:"登录成功"
        }
    }else{
        return{
            status:401,
            data:'账号密码错误'
        }
    }
})
```

### 4.需要拦截axjx的地方，导入mock

import "../mocks"





# 报错：

<img src="/img/webStrom1.png"/>

解决方案：

环境变量加一条：

C:\Windows\System32;