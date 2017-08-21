### ng-route和ui-router的区别

#### ng-route
- ng-route是angularjs官方提供的一种简单的路由操作
- ng-route主要分为三个组成部分：
	- 路由指令: ng-view（ngView指令用于将路由指向的页面渲染到当前页面的布局中）
	- 路由服务: $routeProvider （内置服务对象$routeProvider主要用于进行路由配置）
	- 路由依赖: 路由服务的使用必须依赖ngRoute模块 （需要引入angular-route.js文件，并在定义模块时注入ngRoute依赖）

```
语法：
	作为属性使用
	<div ng-view [onload="string"][autoscroll="string"]></div>

	作为元素使用
	<ng-view [onload="string"][autoscroll="string"]></ng-view>
	- onload:当视图发生变化时执行属性值中的表达式
	- autoscroll：当视图发生变化时自动触发$anchorScroll事件

事件：
	路由视图一旦加载时，就会自动触发$viewContentLoaded事件


js中：
angular.module('myApp', ['ngRoute'])
	.config(['$routeProvider', function($routeProvider){
		//在这里定义路由
		$routeProvider
			.when('/', {
				templateUrl: 'views/home.html',
				controller: 'homeCtrl'
				})
			.when('/login', {
				templateUrl: 'views/login.html',
				controller: 'loginCtrl'
				})
			.otherwise({
				redirectTo: '/'
				});
		}]);

```

#### ui-router
- ui-router是第三方提供的一种强大的路由处理模块
- 和ng-route相比，ui-router的处理方式更加简洁和易用，尤其涉及到项目中大量路由嵌套时，使用ui路由能更快捷的完成项目中路由的跳转处理
