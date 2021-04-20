

## 1.你是如何学习[前端](https://www.nowcoder.com/jump/super-jump/word?word=%E5%89%8D%E7%AB%AF)的? 说一说你的学习经历 

个人优缺点、[职业规划](https://www.nowcoder.com/jump/super-jump/word?word=%E8%81%8C%E4%B8%9A%E8%A7%84%E5%88%92) 为什么选择[前端](https://www.nowcoder.com/jump/super-jump/word?word=%E5%89%8D%E7%AB%AF) 18.接下来的学习计划 

## 2.自己做的[项目](https://www.nowcoder.com/jump/super-jump/word?word=%E9%A1%B9%E7%9B%AE)中有没有组件封装，有没有落地应用？

组件的封装里面都做了什么，是自己用还是打包到npm了？

1.在使用 vue-cli 创建的项目中，组件的创建非常方便，只需要新建一个 .vue 文件，然后在 template 中写好 HTML 代码，一个简单的组件就完成了 一个完整的组件，除了 template 以外，还有 script和 style 

## 3.git常用命令@

**提交步骤**

1. ****`git init` 初始化git仓库
2. `git status` 查看文件状态
3. `git add 文件列表` 追踪文件（将工作目录中的文件全部添加到暂存区：`git add .`
4. `git commit -m 提交信息`  向仓库中提交代码
5. `git log` 查看提交记录

 **撤销**

- 用暂存区中的文件覆盖工作目录中的文件： `git checkout 文件`
- 将文件从暂存区中删除： `git rm --cached 文件`
- 将 git 仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录：`git reset --hard commitID` 

**分支命令**

- `git branch` 查看分支
- `git branch 分支名称` 创建分支
- `git checkout 分支名称` 切换分支（检出分支）、git checkout -b分支名称(基于当前分支末梢创建并检出新分支) 
- `git merge 来源分支` 合并分支
- `git branch -d 分支名称` 删除分支（分支被合并后才允许删除）（-D 强制删除）

 **暂时保存更改**

在git中，可以暂时提取分支上所有的改动并存储，让开发人员得到一个干净的工作副本，临时转向其他工作。使用场景：分支临时切换

- 存储临时改动：`git stash`
- 恢复改动：`git stash pop`

**将本地仓库推送到远程仓库**

1. git push 远程仓库地址 分支名称

2. git push 远程仓库地址别名 分支名称

3. git push -u 远程仓库地址别名 分支名称

   -u 记住推送地址及分支，下次推送只需要输入git push即可

4. git remote add 远程仓库地址别名 远程仓库地址

- 克隆远端数据仓库到本地：`git clone 仓库地址`

- 拉取远程仓库中最新的版本：`git pull 远程仓库地址 分支名称`

- 在多人同时开发一个项目时，如果两个人修改了同一个文件的同一个地方，就会发生冲突。冲突需要人为解决。

**跨团队协作**

1. 程序员 C fork仓库
2. 程序员 C 将仓库克隆在本地进行修改
3. 程序员 C 将仓库推送到远程
4. 程序员 C 发起pull reqest
5. 原仓库作者审核
6. 原仓库作者合并代码

**GIT忽略清单**

将不需要被git管理的文件名字添加到此文件中，在执行git命令的时候，git就会忽略这些文件。

git忽略清单文件名称：**.gitignore**

![](C:\Users\superssssss\Desktop\Interview\mj-images\2010072023345292.png)

## git merge/rebase作用git reset、git revert 和 git checkout？？？ 

1、git rebase 和 git merge 一样都是用于从一个分支获取并且合并到当前分支.
假设一个场景,就是我们开发的[feature/todo]分支要合并到 master 主分支

![1616770638721](C:\Users\superssssss\Desktop\Interview\mj-images\1616770638721.png)

marge 特点：自动创建一个新的 commit 如果合并的时候遇到冲突，仅需
要修改后重新 commit
o 优点：记录了真实的 commit 情况，包括每个分支的详情
o 缺点：因为每次 merge 会自动产生一个 merge commit，所以在使用一些
git 的 GUI tools，特别是 commit 比较频繁时，看到分支很杂乱。

rebase 特点：会合并之前的 commit 历史
o 优点：得到更简洁的项目历史，去掉了 merge commit
o 缺点：如果合并出现代码问题不容易定位，因为 re-write 了 history
因此,当需要保留详细的合并信息的时候建议使用 git merge，特别是需要将分支合并进入master 分支时；当发现自己修改某个功能时，频繁进行了 git commit 提交时，发现其实过多的提交信息没有必要时，可以尝试 git rebase.

2、git reset、git revert 和 git checkout 的共同点：用来撤销代码仓库中的某些更改。
然后是不同点：
（1）首先，从 commit 层面来说：
o git reset 可以将一个分支的末端指向之前的一个 commit。然后再下次 git
执行垃圾回收的时候，会把这个 commit 之后的 commit 都扔掉。git reset
还支持三种标记，用来标记 reset 指令影响的范围：
 --mixed：会影响到暂存区和历史记录区。也是默认选项
 --soft：只影响历史记录区
 --hard：影响工作区、暂存区和历史记录区
注意：因为 git reset 是直接删除 commit 记录，从而会影响到其他开发人员的分
支，所以不要在公共分支（比如 develop）做这个操作。
 git checkout 可以将 HEAD 移到一个新的分支，并更新工作目录。
因为可能会覆盖本地的修改，所以执行这个指令之前，你需要
stash 或者 commit 暂存区和工作区的更改。
o git revert 和 git reset 的目的是一样的，但是做法不同，它会以创建新的
commit 的方式来撤销 commit，这样能保留之前的 commit 历史，比较安
全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，
你需要 stash 或者 commit 暂存区和工作区的更改。
（2）然后，从文件层面来说：？？不对吧
o git reset 只是把文件从历史记录区拿到暂存区，不影响工作区的内容，而
且不支持 --mixed、--soft 和 --hard。
o git checkout 则是把文件从历史记录拿到工作区，不影响暂存区的内容。
o git revert 不支持文件层面的操作。

## 前端工程化

**模块化和组件化** 

用我自己的话来说就是模块是在文件层面上，对代码和资源的拆分，比如js模块，css模块。组件是在UI层面上的拆分，比如页头，页脚，评论区等 

模块化中的模块一般指js模块，比如一个用来格式花时间的模块。
 而从UI拆分下来的每个包含模板(HTML)+样式(CSS)+逻辑(JS)功能完备的结构单元，我们称之为**组件**。比如vue组件包含了template,style,script。它的script可以由许多js模块组成。

 ![img](C:\Users\superssssss\Desktop\Interview\mj-images\webp11) 

 **前端工程化** 

概念：前端工程化是使用**软件工程**的技术和方法来进行前端项目的开发、维护和管理 

前端工程化包含如下：
 1.代码规范: 保证团队所有成员以同样的规范开发代码。
 2.分支管理: 不同的开发人员开发不同的功能或组件，按照统一的流程合并到主干。
 3.模块管理: 一方面，团队引用的模块应该是规范的;另一方面，必须保证这些模块可以正确的加入到最终编译好的包文件中。（以上两点可以总结为模块化或者组件化开发。）
 4.自动化测试：为了保证和并进主干的代码达到质量标准，必须有测试，而且测试应该是自动化的，可以回归的。
 5.构建：主干更新以后，自动将代码编译为最终的目标格式，并且准备好各种静态资源，
 6.部署。 将构建好的代码部署到生产环境。

 

 

 

## Webpack

有试过webpack从0开始搭建 

### 谈谈你对webpack的看法

webpack是一个模块打包工具，可以使用它管理项目中的模块依赖，并编译输出模块所需的静态文件。它可以很好地管理、打包开发中所用到的HTML,CSS,JavaScript和静态文件（图片，字体）等，让开发更高效。对于不同类型的依赖，webpack有对应的模块加载器，而且会分析模块间的依赖关系，最后合并生成优化的静态资源。

### webpack的基本功能和工作原理？

- 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等等
- 文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等
- 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
- 模块合并：在采用模块化的项目有很多模块和文件，需要构建功能把模块分类合并成一个文件
- 自动刷新：监听本地源代码的变化，自动构建，刷新浏览器
- 代码校验：在代码被提交到仓库前需要检测代码是否符合规范，以及单元测试是否通过
- 自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

### webpack构建过程

- 从entry里配置的module开始递归解析entry依赖的所有module
- 每找到一个module，就会根据配置的loader去找对应的转换规则
- 对module进行转换后，再解析出当前module依赖的module
- 这些模块会以entry为单位分组，一个entry和其所有依赖的module被分到一个组Chunk
- 最后webpack会把所有Chunk转换成文件输出
- 在整个流程中webpack会在恰当的时机执行plugin里定义的逻辑

### webpack打包原理

将所有依赖打包成一个bundle.js，通过代码分割成单元片段按需加载

### 什么是webpack，与gulp,grunt有什么区别

- webpack是一个模块打包工具，可以递归地打包项目中的所有模块，最终生成几个打包后的文件。

- 区别：三者都是前端构建工具，grunt和gulp在早期比较流行，现在webpack相对来说比较主流，不过一些轻量化的任务还是会用gulp来处理，比如单独打包CSS文件等。

  [grunt](https://link.zhihu.com/?target=https%3A//www.gruntjs.net/)和[gulp](https://link.zhihu.com/?target=https%3A//www.gulpjs.com.cn/)是基于任务和流（Task、Stream）的。类似jQuery，找到一个（或一类）文件，对其做一系列链式操作，更新流上的数据， 整条链式操作构成了一个任务，多个任务就构成了整个web的构建流程。

  webpack是基于入口的。webpack会自动地递归解析入口所需要加载的所有资源文件，然后用不同的Loader来处理不同的文件，用Plugin来扩展webpack功能。

- webpack支持代码分割，模块化（AMD,CommonJ,ES2015），全局分析

### 什么是entry,output?

- entry 入口，告诉webpack要使用哪个模块作为构建项目的起点，默认为./src/index.js
- output 出口，告诉webpack在哪里输出它打包好的代码以及如何命名，默认为./dist

### 什么是loader，plugins?

- loader是。
- plugins(插件)作用更大，

### 什么是bundle,chunk,module?

bundle是webpack打包出来的文件，chunk是webpack在进行模块的依赖分析的时候，代码分割出来的代码块。module是开发中的单个模块

### webpack-dev-server和http服务器如nginx有什么区别？

webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，相比传统http服务器开发更加简单高效

### 什么是模块热更新？

webpack的一个功能，可以使代码修改后不用刷新浏览器就自动更新，高级版的自动刷新浏览器

### 配置项：	

```
	module.exports = {
        mode:"development",
        //设置入口文件路径
        entry: path.join(__dirname,"./src/xx.js"),
        //设置出口文件
        output:{
            //设置路径
            path:path.join(__dirname,"./dist"),
            //设置文件名
            filename:"res.js"
        }.
        plugins:[ htmlPlugin ],//打包期间用到的插件数组
        module : {//模块：例如解读CSS,图片如何转换，压缩
            rules:[
                {
                    //test设置需要匹配的文件类型，支持正则
                    test:/\.css$/,
                    //use表示该文件类型需要调用的loader
                    use:['style-loader','css-loader']
                },
                {
                    test:/\.less$/,
                    use:['style-loader','css-loader','less-loader']
                }
            ]
        }
    }
```



###  plugin和loader

**loader**：用来告诉webpack如何转换某一类型的文件，并且引入到打包出的文件中。让webpack拥有了加载和解析*非JavaScript文件*的能力 

- babel-loader: 将ES6+转移成ES5-  处理高级js语法的加载器
- css-loader,style-loader：解析css文件，能够解释@import url()等
- file-loader：直接输出文件，把构建后的文件路径返回，可以处理很多类型的文件
- url-loader：打包处理css中与url路径有关的文件，打包图片
- image-loader：加载并且压缩图片文件
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。

 **plugins**：可以扩展webpack的功能，可以打包优化，资源管理和注入环境变量

- html-webpack-plugin: 生成一个预览页面
- commons-chunk-plugin：提取公共代码

### 配置html-webpack-plugin生成一个预览页面

```
因为当我们访问默认的 http://localhost:8080/的时候，看到的是一些文件和文件夹，想要查看我们的页面
还需要点击文件夹点击文件才能查看，那么我们希望默认就能看到一个页面，而不是看到文件夹或者目录。
实现默认预览页面功能的步骤如下：（原理是将index。html复制到根目录下）
    A.安装默认预览功能的包:html-webpack-plugin
        npm install html-webpack-plugin -D
    B.修改webpack.config.js文件，如下：
        //导入包
        const HtmlWebpackPlugin = require("html-webpack-plugin");
        //创建对象
        const htmlPlugin = new HtmlWebpackPlugin({
            //设置生成预览页面的模板文件
            template:"./src/index.html",
            //设置生成的预览页面名称
            filename:"index.html"
        })
    C.继续修改webpack.config.js文件，添加plugins信息：
        module.exports = {
            ......
            plugins:[ htmlPlugin ]//打包期间用到的插件数组
        } 
```

### webpack中babel的实现

安装 `npm i -D @babel-preset-env @babel-core babel-loader`

- @babel-preset-env：可以让我们灵活设置代码目标执行环境
- @babel-core: babel核心库
- babel-loader: webpack的babel插件，让我们可以在webpack中运行babel

 `babel` 用过吗 ？？？



###  什么是长缓存？在webpack中如何做到长缓存优化？

- 浏览器在用户访问页面的时候，为了加快加载速度会对用户访问的静态资源进行存储，但是每一次代码升级或更新都需要浏览器下载新的代码，最简单方便的方式就是引入新的文件名称。

- webpack中可以在output中指定chunkhash，并且分离经常更新的代码和框架代码。通过NameModulesPlugin或HashedModuleIdsPlugin使再次打包文件名不变。

  

### 如何利用webpack来优化前端性能？（提高性能和体验）

用webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运行快速高效。

- 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的`UglifyJsPlugin`和`ParallelUglifyPlugin`来压缩JS文件， 利用`cssnano`（css-loader?minimize）来压缩css
- 利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于`output`参数和各loader的`publicPath`参数来修改资源路径
- 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数`--optimize-minimize`来实现
- 提取公共代码。

### 什么是Tree-shaking？CSS可以Tree-shaking？

Tree-shaking是指在打包中取出那些引入了但在代码中没有被用到的死代码。webpack中通过uglifysPlugin来Tree-shaking JS。CSS需要使用purify-CSS

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 







## 反问：

1、蚂蚁技术栈是什么？

2、node是否是一定要掌握的呢？

3、对我有什么评价和建议？