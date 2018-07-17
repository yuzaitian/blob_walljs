---
title: 虚拟DOM原理了解一下
date: 2018-03-05 12:08:25
tags:
---
我们都知道前端的DOM是一棵树，例如下面这个DOM:
```html
<div class="theme" id="test">
  <img src="http://baidu.jpg" alt="图片" title="图片" />
  <span>a test</span>
</div>
```
<!-- more -->
仔细思考一下，例如对上面这个DOM来说，我们所需要关注的就只有标签名、属性、子节点这三个信息。这三个东西我们完全可以用一个普通的js对象来表示，用一个tagName属性来存标签名，一个props属性来存所有的attributes，一个children属性来存子节点信息。
我们再回到前面那个DOM，转换成JavaScript对象可以是下面这样一个结构：
```javascript
const element = {
  tagName: 'div',
  props: {
  	class: 'theme',
  	id: 'test'
  },
  children: [
    {
      tagName: 'img',
      props: {
      	src: 'http://baidu.jpg',
      	alt: '图片',
      	title: '图片'
      },
      children: null
    },
    {
      tagName: 'span',
      props: {},
      children: 'a test'
    }
  ]
}
```
有了这个有JavaScript对象构成的DOM树（我们称之为虚拟DOM树），我们就可以根据这棵虚拟DOM树的结构来构建一颗真正的DOM树。接下来，当我们网页有状态变更时，我们重新渲染这个JavaScript对象，我们只要根据这个JavaScript对象重新渲染一次网页，界面就更新了。
虽说我们的网页是更新了，不过这样做是不可取的，设想我们只是做了一个很小很小的变动，改了几个文字什么的，仍是要重新渲染整个DOM，那不是浪费资源吗？
换一种思路，我们可以用刚刚这棵新渲染的虚拟DOM树去和旧的虚拟DOM树进行对比，记录下它们的差异，这些差异就是我们需要进行DOM操作更新界面的地方，只改变需要变动的地方，这样我们就可以做到：页面状态改变了视图就刷新，将视图和状态进行了一个绑定。
这就是传说中的Virtual DOM算法（好吧我承认有点简化，不过原理就是这样，React厉害的地方就是在这上面做了一系列的优化），主要包括下面几个步骤：

> 1. 用JavaScript对象结构表示DOM树的结构，我们称之为虚拟DOM树，然后我们用这棵虚拟DOM树来构建一个真正的DOM，插入文档中
> 2. 当网页的状态改变时，我们重新构造一颗虚拟DOM树，将这棵新的虚拟DOM树与旧的虚拟DOM树进行对比，并记录它们之间的差异
> 3. 把这些差异记录应用到真正的DOM中，视图更新完成

用一句话解释一下：既然DOM有点慢，那我们用JavaScript做一个缓存好了。