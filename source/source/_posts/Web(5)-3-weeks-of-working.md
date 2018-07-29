---
title: 实习三周的一些想法
Date: 2018-7-29
Categories: Web
Tags: [JavaScript, CSS, 生活]
---

## 写在前面

这三周在实习, 发现自己真的是菜啊, 太多不会的了. 而且本来还说坚持更新博客的. 但是自己现在写的东西基本都是网上别人做过的, 再写一遍有什么意思呢. 可是这里是我的博客啊! 我记录我的东西. 可是有时候好多东西写起来真的费劲. 而且我又不是写的那种对好多人有用的东西. 为什么还要写呢. 有些迷茫了.

而且时间是真的不够用啊! 哪来的时间写博客啊. 我自己的电脑一周都不开机一次. 自己现在的状态只是在拼命的学而已. 

但是, 还是想坚持写下去. 我的生活挺多维的, 以后这里就只写技术相关的了, 甚至软技能方向的都不想写. 其他的东西我自己有再写, 但是闭源留着自己看了. 以后只写技术的, 而且是作为顺带着的写, 不会专门抽时间写博客. 除非自己能写出几千几万个小白都会看的教程...

不说了, 这篇博客算是欠的债补一下. 前三周本有需要写的博客,可是晚上一懒就没写(也不是懒啊, 突然不适应这样的生活). 哎, 我给自己的包袱也很多, 很多东西真的不重要. 我应该甩掉很多包袱多做点东西. 这半年逐渐从一个被大学这隐形的墙围起来的不知生活苦痛的大学生向社会人转变了. 自己的变化也挺多, 多到不想写博客, 都过去了. 

以后会好好写博客的, 这一篇给前三周一个简单的快速总结.

## 写在技术内容之前

这篇文章还是非技术多于技术. 我越来越明白以下道理. 

- 80%以上的事情都是不重要的, 需要抓住真正重要的东西去做.
- 欠债都是要还的. 早该学会的东西早晚都要学会的. 第一次遇到的时候就应该迎难而上做掉.
- 不要做没有意义的事情, 做事情就像是搬砖, 有的搬砖就像是搭梯子, 为自己积累, 有的只是纯属消耗体力, 没长远意义, 这些事少做.

哎, 好久没更新博客了. 想在博客上写的东西挺多, 自己的变化也挺多, 还是慢慢写吧.

以后别人写的好的博客, 我就不会再写一遍了. 我只会写我工作中遇到的问题和我解决的方法, 顺带着写. 和一些特别好的总结吧.

## 下面

偷个懒, 今天事情还挺多. 复制一下实习日结里头写的东西.

## Node-FTP

使用了GitHub上一个node-ftp客户端模块，在此总结一下本次项目用的API

#### 事件

- `ready()  `- 当连接建立且认证成功
- `end()` - 当连接关闭
- `error(<Error>err)` - 当发生错误时候

#### 方法

注：描述不全 只是今天用到的部分 完整描述查看[github](https://github.com/mscdex/node-ftp)

- `connect(<object>config)` - 按照连接配置进行连接 主要配置: host,port,user,password.

- `list([<string>path,][<boolean>useCompression,]<function>callback)` - 获取path下的目录与文件，默认是当前工作目录，回调函数有两个参数，err和`<array>list` 。 <u>注意，如果list了一个不存在的目录，一样会有返回值，不报错，但是list.length 为0.</u> list是一个array，每一项会有一下描述信息：

  ```
  - type
  - name
  - size
  - date
  ···
  ```

- `get(<string>path, [<boolean>useCompression,]<function>callback)`  获取文件，回调函数的第二个参数是一个可读文件流

- `put(input,destPath,callback)` 向destpath路径上传文件。回调函数只有一个Error参数

- `rename(oldPath,newPath,callback)` 移动文件，<u>注意，如果向不存在的路径移动会报错，不会自动新建路经，所以要先mkdir</u>

- `delete(path,callback)` 删除文件

- `mkdir([<string>path,][<boolean>recursive,]<function>callback)` 返回值void。path是要新建目录的路径。<u>注意，如果多次建立同一个文件夹不会报错，结果还是一个文件夹，但是没有测试如果里头有文件会不会造成文件丢失</u>

- `rmdir(path,recursive,callback)` 删除文件夹

## JS事件循环

今天详细地了解了一下JS Event Loop的机制，异步任务按什么样的准则执行。

简而言之：

- JS所有的任务都需要在主线程排队等待。任务又分同步任务和异步任务，基于Event Loop机制执行。

- 同步任务作为首要任务在主线程执行，异步任务被发配到另一个线程管理的任务队列中等待处理。异步任务符合条件（比如ajax请求到数据，setTimeout延时到期）后，会在任务队列中添加可执行“事件”，等待主线程中的同步任务执行完毕到任务队列里读取当前可执行的任务，将其加入主线程中执行，以此循环。（所以所有的异步任务都是在所有的同步任务完成之后才执行的，这一点是基本）

- JS中的异步任务：有setTimeout，setInterval等定时任务，Promise，MutationObserver监听DOM，process.nextTick（Node）. 

- 异步的实现有两个机智 macro-task和micro-task。

  - <u>macrotasks包括：script（整体代码）、setTimeout, setInterval, setImmediate, I/O, UI Rendering；</u>
  - <u>microtasks包括： process.nextTick, Promise, Object.observe, MutationObserver。</u>

  macrotasks, microtasks执行机制：

  <u>1.主线程执行完后会先到micro-task队列中读取可执行任务</u>

  <u>2.主线程执行micro-task任务</u>

  <u>3.主线程到macro-task任务队列中读取可执行任务</u>

  <u>4.主线程执行macro-task任务</u>

  <u>5....转到Step 1</u>

  这里注意的是，UI Rendering是在micro-task之后执行，需要在UI渲染之前执行的逻辑，一般采用micro-task异步回调方式进行调用。

  同样，micro-task队列不宜过长，给micro-task队列添加过多回调阻塞macro-task队列的任务执行是小事，重点是这有可能会阻塞UI Render，导致页面不能更新。浏览器也会基于性能方面的考虑，对micro-task中的任务个数进行限制。

- 可以配合这张图理解

  ![](../../../../images/js-event-loop.png)

- 所以，写的代码有一些注意点，昨天我把回调函数当成了会立刻执行（虽然之前了解过，没有写过就是印象不深），今天对于promise，也是要注意，老是下意识认为resolve状态就会像同步一样执行😶 现在区分清楚了感觉也挺清晰的。

- 网上有个例子

  ```javascript
  (function test() {
      setTimeout(function() {console.log(4)}, 0);
      new Promise(function executor(resolve) {
          console.log(1);
          for( var i=0 ; i<10000 ; i++ ) {
              i == 9999 && resolve();
          }
          console.log(2);
      }).then(function() {
          console.log(5);
      });
      console.log(3);
  })()
  //按照上面的原则逐步分析的话 输出顺序是: 1 2 3 5 4
  ```

## Promise

只记录今天用到的部分和注意点，不全。

- Promise新建就会立即执行； Promise如果不设置回调函数，内部抛出错误不会反映到外部。

- `Promise.prototype.then()` ， 第一个参数是resolve状态的回调函数，第二个参数（可选）是rejected状态的回调函数。 返回的是一个新的Promise实例，所以可以采用链式写法。 <u>注意，这个时候，如果有后面的代码需要依赖这里的结果，就可以直接return 这个Promise对象，结果自然就出去了，或者是必须要这个任务先彻底执行完毕，（有点感觉像是同步，指定顺序），也可以return 需要处理的Promise对象。 Promise对象总是要return出来的。</u> 

- `Promise.prototype.catch()` 是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

- `Promise.prototype.finally()` 用于指定不管 Promise 对象最后状态如何，都会执行的操作。

  ```javascript
  promise
  .then(result => {···})
  .catch(error => {···})
  .finally(() => {···});
  //这样的套路
  ```

- `Promise.all()` 用于将多个 Promise 实例，包装成一个新的 Promise 实例。方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例. 

## promise-ftp

GitHub上的一个用promise封装的node-ftp模块，提供promise-based API。 这现成的，就很提高开发速度。

### axios

axios是一个基于 promise 的 HTTP 库,可以用在浏览器和 node.js 中. 

遇到的问题是, 没有仔细看文档, post的时候需要加参数的, 即使是空也要加`{}` .

```javascript
return axios.post('/path',{},{
    //{}是要加的
    withCredentials: true
    //withCredentials 表示跨域请求时是否需要使用凭证
}).then((resp) =>{
    return resp.data.data;
})
//resp 查看文档请求响应的结构
{
    //服务器提供的响应
    data: {}
    ...其他信息
}
```

### 友好意识

比如要生成一张图片, 就这么让用户等着不太好, 可以做一个loading转圈圈的效果.

### 一个重复的错误

```javascript
xxxx(某promise对象).then(function(){
    异步回调操作A
})

又把操作A写到了外面. 我觉得下次应该没有这种错误了...
```

### HTTP状态码

今天一次请求, 遇到了状态码100. 不知道什么意思, 100是`继续. 客户端应继续请求` 的意思. 

状态码也是挺有用的, 用时查一下就行...浏览一下大致记得 太多了不写了...

还有 多看看chrome 开发者工具 network什么的 能自主发现,解决好多问题

### 预请求

今天看chrome devtools network发现同一个请求发了两次, 请求的method,一次是`post` 一次是`option` 那么这个`option` 是什么鬼呢. 

原因是浏览器对简单跨域请求和复杂跨域请求的处理区别. 这种叫`preflighted request`.

preflighted request在发送真正的请求前, 会先发送一个方法为OPTIONS的预请求(preflight request), 用于试探服务端是否能接受真正的请求, 如果options获得的回应是拒绝性质的, 比如404\403\500等http状态, 就会停止post put等请求的发出. 

### vm.$nextTick([callback])

在一次操作之后怎么都没有发现问题之后, 知道了这个方法. Vue状态更新了. 但是DOM树还没有渲染好. 这时候如果对DOM操作,得到的值就可能不是预期的了. 

所以`$nectTick` 用法是

> 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 `Vue.nextTick` 一样，不同的是回调的 `this` 自动绑定到调用它的实例上。

(操作可以理解, 但是如果不知道就有点方了, 还是要不断深入了解, 慢慢来,争取几个月后跟着注解看Vue源码解析什么的😶)

### .sync修饰符

`.sync` 修饰符是以 `update:my-prop-name` 的模式触发事件的缩写.

> 在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。
>
> 所以, 推荐以 `update:my-prop-name` 的模式触发事件取而代之。

## encodeURI

记一个今天用到的东西, 对URL进行Encode. 

有两个方法 `encodeURI` `encodeURIComponent` 

从功能上讲:

- `encodeURI`：不会进行编码的字符有82个 ：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z
- `encodeURIComponent`:不会进行编码的字符有71个：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z

使用角度上讲:

- `encodeURI` 主要用于直接赋值给地址栏时候：
- `encodeURIComponent` 主要用于url的query参数

但是这样还是感觉不够, 要知道为什么会有这种操作.

URL是可移植的(portable). 它要统一地命名因特网上所有的资源，这也就意味着要通过各种不同的协议来传送这些资源。这些协议在传输数据时都会使用不同的机制，所以，设计 URL，使其可以通过任意因特网协议安全地传输是很重要的。安全传输意味着 URL 的传输不能丢失信息。而且还希望可供人类阅读. 还得是完整的. 

于是, 就变成了US-ASCII 字符集+转义序列, 通过转义序列，就可以用 US-ASCII 字符集的有限子集对任意字符值或数据进行编码了，这样就实现了可移植性和完整性。

为了避开安全字符集表示法带来的限制，人们设计了一种编码机制，用来在 URL 中表示各种不安全的字符。这种编码机制就是通过一种“转义”表示法来表示不安全字符的，这种转义表示法包含一个百分号（%），后面跟着两个表示字符 ASCII 码的 十六进制数。

```javascript
> encodeURIComponent("https://twitter.com/cancan_z98");
'https%3A%2F%2Ftwitter.com%2Fcancan_z98'
```

## git

我觉得好的配置挺好用的:

```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"


all:
core.excludesfile=~/.gitignore
core.legacyheaders=false
core.quotepath=false
mergetool.keepbackup=true
push.default=simple
color.ui=auto
color.interactive=auto
repack.usedeltabaseoffset=true
alias.s=status
alias.a=!git add . && git status
alias.au=!git add -u . && git status
alias.aa=!git add . && git add -u . && git status
alias.c=commit
alias.cm=commit -m
alias.ca=commit --amend
alias.ac=!git add . && git commit
alias.acm=!git add . && git commit -m
alias.l=log --graph --all --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset'
alias.ll=log --stat --abbrev-commit
alias.lg=log --color --graph --pretty=format:'%C(bold white)%h%Creset -%C(bold green)%d%Creset %s %C(bold green)(%cr)%Creset %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
alias.llg=log --color --graph --pretty=format:'%C(bold white)%H %d%Creset%n%s%n%+b%C(bold blue)%an <%ae>%Creset %C(bold green)%cr (%ci)' --abbrev-commit
alias.d=diff
alias.master=checkout master
alias.spull=svn rebase
alias.spush=svn dcommit
alias.alias=!git config --list | grep 'alias\.' | sed 's/alias\.\([^=]*\)=\(.*\)/\1\     => \2/' | sort
include.path=~/.gitcinclude
include.path=.githubconfig
include.path=.gitcredential
diff.exif.textconv=exif
credential.helper=osxkeychain
core.excludesfile=/Users/eric/.gitignore_global
difftool.sourcetree.cmd=opendiff "$LOCAL" "$REMOTE"
difftool.sourcetree.path=
mergetool.sourcetree.cmd=/Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh "$LOCAL" "$REMOTE" -ancestor "$BASE" -merge "$MERGED"
mergetool.sourcetree.trustexitcode=true
alias.co=checkout
alias.ci=commit
alias.br=branch
alias.unstage=reset HEAD
alias.last=log -1
alias.lg=log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
alias.st=status
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/BayernSoft/pandora-proxy.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
(END)
```

## 写在最后

太菜了. 菜到不想写博客. 反正都有人写过了.

以后少写这种低质量的. 起码能写的是对细节的探究. 