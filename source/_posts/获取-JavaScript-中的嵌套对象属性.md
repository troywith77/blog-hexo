title: 获取 JavaScript 中的嵌套对象属性
date: 2018-03-15 06:52:24
tags:
---

JavaScript 是个很棒的语言，但是我们经常会碰到一些奇怪的场景让人想挠破头，其中一个就是获取嵌套对象的属性时可能会面对的错误。

## Cannot read property 'foo' of undefined

在使用 JavaScript 的时候，我们经常会需要到获取深层嵌套对象的里层元素，比如下面的例子：

```
const user = {
  id: 77,
  email: 'ruitang307@gmail.com',
  info: {
    name: 'troy',
    age: 23,
    address: {
      city: 'ShangHai'
    }
  }
};
```

为了获取用户的 `name` 或者 `city` ，我们会写到

```
const userName = user.info.name;

const userCity = user.info.address.city;
```

这看起来很简单也很直观。

但是有时候用户的 `info` 可能不存在，比如：

<!--more-->

```
const user = {
  id: 100,
  email: 'troy@google.com'
};
```

现在如果你仍然像之前获取 `name` ，你将会被抛出一个错误 `Cannot read property 'name' of undefined`，这是因为我们在试图从一个不存在的对象里获取 `name` 这个 key。常规的解决办法是：

```
const name = (user && user.info) ? user.info.name : undefined;
// or
const name = user && user.info && user.info.name
```

如果你的嵌套结构很简单，这样写是完全ok的。但是如果你的嵌套有5层6层甚至更深你还这样写的话，你的代码绝对会变得非常混乱而且难看。

我有几个小技巧可以让你的代码看起来干净并且简洁，第一种是像下面这样：

```
const name = ((user || {}).info || {}).name;
```

虽然这看起来很 tricky，但它确实解决了问题，cheers ~ 用这个方法，你将不会再碰到 `Cannot read property 'name' of undefined` 了。这样纯粹只是先check了一下获取的对象是否存在，如果不存在的话就创建一个空对象，这样的话，每一层的key对应的value都能从一个存在的对象或者一个空对象里取到了，永远不会 `undefined`。

如果嵌套的都是 object ，这个方法很棒，但是如果嵌套的结构里存在 array ，那这个方法就很鸡肋了。。

所以更好的办法是写一个 `safeGet` 函数来更好的handle这种问题，我们可以用 `Array.reduce` 来完成这个函数，`Array.reduce` 非常强大，以后可以专门写一篇文章来介绍它。

```
const safeGet = (obj, path) => (
  path.reduce((obj, key) => (
    (obj && (typeof obj[key] !== 'undefined')) ? obj[key] : undefined
  ), obj)
)

// 我们可以用 safeGet 函数来获取任意层次的嵌套对象了

const user = {
  id: 100,
  email: 'troy@google.com'
};

safeGet(user, ['id']) // 100
safeGet(user, ['email']) // 'troy@google.com'
safeGet(user, ['info']) // undefined
safeGet(user, ['info', 'name']) // undefined
safeGet(user, ['info', 'name', 'fly', 'whatever']) // undefined

// 我们同样能够用 safeGet 函数来获取嵌套对象里的数组元素

const anotherUser = {
  id: 101,
  email: 'dev@google.com',
  info: ['Living in USA', 'she is 18']
};

safeGet(anotherUser, ['info', 0]) // 'Living in USA'
safeGet(user, ['info', 1]) // 'she is 18'
safeGet(user, ['info', 2]) // undefined
```

开源库比如 `lodash` 和 `underscore` 都提供了类似的方法，虽然这两个库都可以按需引入，不过自己写一个函数更轻量级更棒咯（笑哭）

Happy safeGeting ~