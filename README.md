# DoubanMovie

创建vue项目的步骤

		安装node.js

		安装vue-cli 脚手架构建工具（必须在全局中进行安装）
		npm install -g vue-cli

		到项目文件夹安装vue
		npm install vue

		全局安装 vue-cli
		npm install --global vue-cli

		创建一个初始化的vue项目
		vue init webpack DoubanMovie
			Project name :项目名称 ，如果不需要更改直接回车就可以了。注意：这里不能使用大写，所以我把名称改成了vueclitest
			Project description:项目描述，默认为A Vue.js project,直接回车，不用编写。
			Author：作者，如果你有配置git的作者，他会读取。
			Install  vue-router? 是否安装vue的路由插件，我们这里需要安装，所以选择Y
			Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。我们这里不需要输入n（建议），如果你是大型团队开发，最好是进行配置。
			setup unit tests with  Karma + Mocha? 是否需要安装单元测试工具Karma+Mocha，我们这里不需要，所以输入n。
			Setup e2e tests with Nightwatch?是否安装e2e来进行用户行为模拟测试，我们这里不需要，所以输入n

命令行跳转到项目文件夹里运行项目
	npm run dev

vue项目打包,命令行跳转到项目文件夹里打包项目，打包后的dist文件夹直接放到服务器
	npm run build

用npm安装jquery
  npm install jquery
用npm安装better-scroll
  npm install better-scroll --save


为了解决打包后dist文件夹的index.html空白页的问题，在config文件夹的index.js中修改，参考[22](https://blog.csdn.net/jasons_xie/article/details/80901589)


提交Github后可以根据[33](https://1729176996.github.io/DoubanMovie/dist/index.html)来查看效果
