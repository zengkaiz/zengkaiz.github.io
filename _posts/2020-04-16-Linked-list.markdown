---
layout:     post
title:      "数据结构与算法-链表（Linked-list）-javascript描述"
subtitle:   ""
date:       2020-04-16 19:31:22
author:     "zack"
header-img: "img/post-bg-js-rwd.jpg"
catalog: true
tags:
    - 算法
---

### 写在前面

大家在写`javascript`的时候可能用的最多的数据结构就是数组，比如遇到某个需求操作数据可能第一时间就想到数组去存储操作等，但是数组不是
最佳的数据结构。在Javascript中数组被实现成了对象，与其他语言（比如C—++ 和 java）的数组相比，效率很低。如果你发现数组实际使用时很慢，
你就可以使用链表来代替它。。。。

### 单向链表

链表是一组节点组成的集合

![图片1](/img/linked-list/1.png)
从上图中可以看出，链表元素是靠相互之间的关系进行的引用，比如我们说bread是跟在milk后面，而不说bread是链表中的第二个元素，因为数组才是靠它们的位置进行引用的。
另外观察图中的链表可以看到最前面有个特殊节点，叫做`头节点`。链表的尾元素指向了一个null的节点。

##### 向链表中插入元素

通过上面总结链表的特点不难发现，要往链表中插入一个节点，需要修改它前面的节点（前驱），使其指向新加入的节点，而新加入的节点则指向原来前驱指向的节点，就完成了一次节点插入。

##### 从链表中删除元素

从链表中删除一个元素也需要通过将待删除的前驱节点指向待删除元素的后继节点，同时将待删除元素指向null，元素就成功删除了。

##### 设计一个基于对象的链表

设计两个类：
1. `Node`类用来表示节点
2. `LinkedList`类提供插入节点、删除节点、显示列表元素等一些其他辅助方法

Node类：
```
function Node(element) {
    this.element = element  // element用来保存节点上的数据
    this.next = null        // 用来保存指向下一个节点的链接
}
```

LinkedList类：
```
    function LList() {
        this.head = new Node("head")
        this.find = find
        this.insert = insert
        this.remove = remove
        this.display = display
    }
```

两个类定好了，首先我们来分析第一个方法`insert`,该方法向链表中插入一个新节点。
要插入一个新节点首先要知道在哪个节点的前面或者后面插入，所以先得找到这个节点。
假如要在一个已知的节点后面插入，先要找到“后面”的节点。所以先写个`find()`的辅助方法。
```
function find(item) {
    let currNode = this.head
    while (currNode.element !== item) {
        currNode = currNode.next
    }
    return currNode
}
```
理解一下这个方法，首先创建了一个节点，并将链表的头节点指给这个新创建的节点，然后在链表上循环去找，如果不符合，就将当前节点移动到下一个节点继续查找，直到成功后返回这个节点。否则，返回null。
那么找到这个“后面”的节点，下面就来定义`insert`插入的方法：

```
function insert(newElement, item) {
    let newNode = new Node(newElement)
    let currNode = this.find(item)
    newNode.next = currNode.next
    currNode.next = newNode
}
```
接着继续分析定义从链表中删除一个节点的方法`remove`，从链表中删除节点时，需要先找到待删除节点前面的节点。找到这个节点后，修改它的next属性，使其不再指向待删除节点，而是指向
待删除节点的下一个节点。我们需要先定义一个方法`findPrevious()`找到待删除元素的前一个元素,并且返回它。
```
function findPrevious(item) {
    let previousNode = this.head
    while (previousNode.next.element !== item && !(previousNode.next == null)) {
        previousNode = previousNode.next()
    }
    return previousNode
}
```
找到这个节点我们就来定义`remove`方法：
```
function remove(item) {
    let previousNode = this.findPrevious(item)
    while (!(previous.next == null)) {
        previousNode.next = previousNode.next.next
    }
}
```
这里跳过了待删除节点，让“前一个”节点指向待删除节点的后一个元素。
以下包含完整代码，包括Node类，LList类和测试代码
```
 function Node(element) {
    this.element = element
    this.next = null
 }

 function LLits() {
     this.head = new Node("head")
     this.find = find
     this.insert = insert
     this.findPrevious = findPrevious
     this.remove = remove
     this.display = display
 }

 function find(item) {
    let currNode = this.head
    while (currNode.element != item) {
        currNode = currNode.next 
    }
    return currNode
 }

 function insert(newElement, item) {
    let newNode = new Node(newElement)
    let currNode = this.find(item)
    newNode.next = currNode.next
    currNode.next = newNode
 }

 function findPrevious(item) {
    let currNode = this.head
    while (!(currNode.next == null) && currNode.next.element != item) {
        currNode = currNode.next
    }
    return currNode
 }

 function remove(item) {
    let previousNode = this.findPrevious(item)
    if(previousNode.next != null){
        previousNode.next = previousNode.next.next
    } 
 }

 function display() {
     var currNode = this.head
     while (!(currNode.next == null)) {
        console.log(currNode.next.element)
        currNode = currNode.next
     }
 }

 let cars = new LLits()
 cars.insert('bmw', 'head')
 cars.insert('tuolaji', 'bmw')
 cars.insert('benz', 'tuolaji')
 cars.display()
 console.log('----------')
 cars.remove('tuolaji')
 cars.display()
```

### 双向链表

