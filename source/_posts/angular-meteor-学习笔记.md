title: Angular-Meteor 学习笔记
date: 2016-03-07 13:19:08
tags:
---
之前实习的公司官网使用Meteor开发的，用了三个月，感觉Meteor真的是可以很方便很快速的进行开发。正好现在有时间，把Angular和Meteor结合起来学习一下，记下一些心得吧~

- Meteor服务器开启时会扫描所有的HTML文件，然后把其中所有的`<html>`、`<head>`、`<body>`标签里的内容融合在一起。
- `helper`是reactiv的，函数内的数据一旦改变，函数就会重新运行一次。
- Meteor.startup函数可以在Meteor启动时执行内部的函数。
- `collection_name.find({}).fetch()`, `collection_name.insert()`, `db.parties.remove( {"_id": "N4KzMEvtm4dYvk2TF"});`
- JS里访问Collection是从`Parties`变量访问， Mongo Shell 里从`db.parties`访问。
```
Parties = new Mongo.Collection('parties');
```
- 使用 Controoler as 语法，先将 $scope 改为 this， 然后引入 $reactive 依赖。不使用的话不需要这样做。
```
$reactive(this).attach($scope);
```
- Angular去掉url的#
```
<!-- JS -->
$locationProvider.html5Mode(true);
<!-- HTML -->
<base href="/">
```
<!--more-->
- ui.router三个最重要的Provider
```
$stateParams.(:id)  //获取url里的参数
$urlRouterProvider.otherwise('')   //重定向
$stateProvider.state  //设置state
```
- 原来还可以这样跳转到其他state的，以前不知道可以直接传值。。
```
ui-sref="partyDetails({partyId: party._id})"
```
- findOne会找到符合条件的第一个元素
- update方法更新数据
```
Parties.update({_id: $stateParams.partyId}, {
            $set: {
              name: this.party.name,
              description: this.party.description
            }
          });
```
- 下面的error出现原因是`parties`这个Collection重复定义了。
```
Error: A method named '/parties/insert' is already defined
```
- `Meteor.users`是`meteor-accounts`定义的Collection
- 返回所有查询到的数据但是只包括这两个字段
```
return Meteor.users.find({}, {fields: {emails: 1, profile: 1}})
```
- 在哪个directive里subscribe了数据，就只能在这个di里访问到