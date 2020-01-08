## 算法

### 数组

数组（Array）是一种线性表数据结构。它用一组连续的内存空间，来存储一组具有相同类型的数据。最大的特点就是支持**随机访问**，但插入、删除操作也因此变得比较低效，平均情况时间复杂度为 O(n)。

&emsp;

### 链表

~~~js
/**
 * 单链表中判断回文串字符串
 */

// 节点
class Node {
  constructor(element) {
    this.element = element
    this.next = null
  }
}
// 链表(带头链表)
class LinkedList {
  constructor() {
    this.head = new Node()
  }
  // 根据value查找节点
  findByValue(element) {
    let currentNode = this.head.next
    while(currentNode !== null && currentNode.element !== element) {
      currentNode = currentNode.next
    }
    return currentNode === null? -1: currentNode
  }

   // 向链表末尾追加节点
   appned(newElement) {
    const newNode = new Node(newElement)
    let currentNode = this.head
    while(currentNode.next) {
      currentNode = currentNode.next
    }
    currentNode.next = newNode
   }
    
    // 根据值删除
    remove(element) {
      let prevNode = this.head
      let currentNode = this.head.next
      while(currentNode) {
        if(currentNode.element === element) {
          prevNode.next = currentNode.next
          return
        }
        prevNode = currentNode
        currentNode = currentNode.next
      }
    }

    // 遍历显示所有节点
    display() {
      let currentNode = this.head.next
      while(currentNode) {
        console.log(currentNode.element)
        currentNode = currentNode.next
      }
    }

    // 判断是否是回文串
    palindromic() {
      let slow = this.head.next
      let fast = this.head.next
      let mid = null
      // 找出中间节点
      while(fast.next && fast.next.next) {
        // 快指针前进两个节点
        fast = fast.next.next
        // 慢指针前进一个节点
        slow = slow.next
      }
      mid = slow
      // 后半部分逆序
      let currentNode = null
      let nextNode = null
      if(fast.next === null) { // 奇数回文
        currentNode = slow
        nextNode = slow.next
      } else { // 偶数回文
        currentNode = slow.next
        nextNode = slow.next.next
      }
      while(nextNode) {
        let next = nextNode.next
        nextNode.next = currentNode
        currentNode = nextNode
        nextNode = next
      }
      // 前后比较
      let s = this.head.next
      let e = currentNode
      while(s !== mid && s.element === e.element) {
        s = s.next
        e = e.next
      }
      if(s.next === mid) {
        return true
      } else {
        return false
      }
    }
    
    // 反转单链表 自己想的方法
    reverseList() {
      // 当前链表为空或者只有一个节点，就不作处理
      if(!this.head.next || !this.head.next.next) return 
      let cur = this.head.next
      let next = cur.next
      cur.next = null
      while(next.next) {
        let tmp = next.next
        next.next = cur
        cur = next
        next = tmp
      }
      next.next = cur
      this.head.next = next
    }

    // 尾插法
    reverseList1() {
      const root = new Node('head')
      let currentNode = this.head.next
      while (currentNode !== null) {
        const next = currentNode.next
        currentNode.next = root.next
        root.next = currentNode
        currentNode = next
      }
      this.head = root
    }

    // 增强尾插法可读性，便于初学者理解
    reverseList2() {
      let currentNode = this.head.next
      let previousNode = null
      while(currentNode !== null){
        let nextNode = currentNode.next
        currentNode.next = previousNode
        previousNode = currentNode
        currentNode = nextNode
      }
      this.head.next = previousNode
    }
    
    // 单链表中环的检测
    // 快慢指针法，慢指针每次前进一个节点，快指针每次前进两个节点
    // 若有环，快慢指针先后进环，且必定会在环中相遇
    checkCircle() {
      if(!this.head.next || !this.head.next.next) return false
      let slow = this.head
      let fast = this.head.next
      while(fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
        if(slow === fast) return true
      }
      return false
    }
    
    // 删除链表倒数第 n 个结点
    removeByIndexFromEnd(index) {
      if(this.checkCircle()) return
      this.reverseList2()
      let pos = 1
      let currentNode = this.head.next
      while(currentNode && pos < index) {
        currentNode = currentNode.next
        pos++
      }
      if (currentNode === null) {
        console.log('无法删除最后一个节点或者该节点不存在')
        return false
      }
      this.remove(currentNode.element)
      this.reverseList2()
    }
    
    // 求中间节点
    findMiddleNode() {
      if(!this.head.next) return
      let slow = this.head.next
      let fast = this.head.next
      while(fast.next && fast.next.next) {
        slow = slow.next
        fast = fast.next.next
      }
      console.log(slow.element)
      return slow
    }
}

// 两个有序的链表合并
const mergeSortedLists = (listA, listB) => {
  if(!listA || !listB) return null
  let a = listA.head.next
  let b = listB.head.next
  let listC = new LinkedList()
  let c = listC.head
  while(a && b) {
    if(a.element > b.element) {
      c.next = b
      b = b.next
    } else {
      c.next = a
      a = a.next
    }
    c = c.next
  }
  if(a) {
    c.next = a
  } else {
    c.next = b
  }
  return listC
}


const list = new LinkedList()
list.appned('a')
list.appned('b')
list.appned('a')

var result = list.palindromic()
~~~

&emsp;

### 栈

~~~js
class Node {
  constructor(element) {
    this.element = element
    this.next = null
  }
}

// 链式栈
class StackBasedLinkedList {
  constructor() {
    this.top = null
  }

  // 入栈
  push(element) {
    const node = new Node(element)
    node.next = this.top
    this.top = node
  }

  // 出栈
  pop() {
    if(!this.top) {
      return
    }
    const node = this.top
    this.top = node.next
    return node.element
  }

  display() {
    let node = this.top
    while(node) {
      console.log(node.element)
      node = node.next
    }
  }
}

/**
 * 使用前后栈实现浏览器的前进后退
 */
class Browser {
  constructor() {
    this.forwardStack = new StackBasedLinkedList()
    this.backwardStack = new StackBasedLinkedList()
  }
  
  // 浏览
  browse(element) {
    this.backwardStack.push(element)
    this.display()
  }

  // 前进
  forward() {
    const element = this.forwardStack.pop()
    if(element) {
      this.backwardStack.push(element)
    }
    this.display()
  }

  // 后退
  backward() {
    const element = this.backwardStack.pop()
    if(element) {
      this.forwardStack.push(element)
    }
    this.display()
  }

  display() {
    const node = this.backwardStack.top
    if(node) {
      console.log(node.element)
    }
  }
}

let browser = new Browser()
browser.browse('a')
browser.browse('b')
browser.browse('c')
browser.backward()
browser.forward()
~~~

&emsp;

### 队列

~~~js
class Node {
  constructor(element) {
    this.element = element
    this.next = null
  }
}

// 链式队列
class QueueBasedOnLinkedList {
  constructor() {
    this.head = this.tail = null
  }

  // 入队
  enQueue(element) {
    if(this.head === null) {
      this.head = this.tail = new Node(element)
      return
    }
    this.tail.next = new Node(element)
    this.tail = this.tail.next
  }

  // 出队
  deQueue() {
    if(this.head === null) {
      console.log('队空')
      return
    }
    let node = this.head
    this.head = this.head.next
    console.log(node.element)
  }
}

// 顺序循环队列
class CircleQueueBasedOnLinearList {
  constructor(n) {
    this.items = new Array(n)
    this.n = n
    this.head = this.tail = 0
  }

  enQueue(val) {
    if((this.tail + 1) % this.n == this.head) {
      console.log('队满')
      return
    }
    this.items[this.tail] = val
    this.tail = (this.tail + 1) % this.n

  }

  deQueue() {
    if(this.head === this.tail) {
      console.log('队空')
      return
    }
    let val = this.items[this.head]
    console.log(val)
    this.head = (this.head + 1) % this.n
  }
}

// 链式循环队列
class CircleQueueBasedOnLinkedList {
  constructor() {
    this.head = this.tail = null
  }

  // 入队
  enQueue(element) {
    if(this.head === null) {
      this.head = this.tail = new Node(element)
      this.tail.next = this.head
    } else {
      let newNode = new Node(element)
      this.tail.next = newNode
      this.tail = newNode
      this.tail.next = this.head
    }
  }

  // 出队
  deQueue() {
    if(this.head === null) {
      console.log('队空')
      return
    }
    if(this.head === this.tail) {
      let node = this.head
      console.log(node.element)
      this.head = this.tail = null
    } else {
      let node = this.head
      console.log(node.element)
      this.head = this.head.next
      this.tail.next = this.head
    }
  }
}
~~~



### 树

~~~js
function Node(data,left,right) {
    this.data = data;
    this.left = left;
    this.right = right;
}

function BST() {
    this.root = null;
    this.insert = insert;
    this.inOrder = inOrder;
    this.find = find;
    this.remove = remove;
}

function insert(data) {
    if(this.root == null) {
        this.root = new Node(data,null,null);
    } else {
        var node = this.root;
        while(true) {
            if(data < node.data) {
                if(node.left === null) {
                    node.left = new Node(data,null,null);
                    break;
                } else {
                    node = node.left;
                }
            } else if(data > node.data) {
                if(node.right === null) {
                    node.right = new Node(data,null,null);
                    break;
                } else {
                    node = node.right;
                }
            } else {
                break;
            }
        }
    }
}

function inOrder(node) {
    if(node != null) {
        console.log(node.data);
        inOrder(node.left);
        inOrder(node.right);
    }
}

function find(data) {
    var node = this.root;
    while(node) {
        if(data == node.data) {
            return node;
        } else if(data < node.data) {
            node = node.left;
        } else {
            node = node.right;
        }
    }
    return null;
}

function removeNode(node,data) {
    if(node == null) {
        return;
    }
    if(data == node.data) {
        if(node.left == null & node.right == null) {
            return null;
        } else if(node.left == null) {
            return node.right;
        } else if(node.right == null) {
            return node.left;
        } else {
            var temp = getMin(node.right);
            node.data = temp.data;
            node.right = removeNode(node.right,temp);
            return node;
        }
    } else if( data < node.data) {
        node.left = removeNode(node.left,data);
        return node;
    } else {
        node.right = removeNode(node.right,data);
        return node;
    }
}
~~~

&emsp;

### 二叉查找树

~~~js
class Node {
  constructor(data) {
    this.data = data
    this.left = null
    this.right = null
  }
}

class BST {
  constructor() {
    this.root = null
  } 

  // 插入
  insert(data) {
    if(this.root === null) {
      this.root = new Node(data)
      return
    }
    let p = this.root
    while(p !== null) {
      if(data < p.data) {
        if(p.left === null) {
          p.left = new Node(data)
          return
        }
        p = p.left
      } 
      else {
        if(p.right === null) {
          p.right = new Node(data)
          return
        }
        p = p.right
      } 
    }
  }

  // 中序遍历
  inOrder(node) {
    if(node === null) return
    this.inOrder(node.left)
    console.log(node.data)
    this.inOrder(node.right)
  }

  // 查询
  find(data) {
    let p = this.root
    while(p !== null) {
      if(data < p.data) p = p.left
      else if(data > p.data) p = p.right
      else return p
    }
    return null
  }

  // 删除(非递归)
  remove1(data) {
    let pp = null // 指向删除节点的父节点
    let p = this.root

    while(p !== null && p.data !== data) { // 查找被删除的节点
      pp = p
      if(data < p.data) p = p.left
      else p = p.right
    }
    if(p === null) return

    if(p.left !== null && p.right !== null) { // 如果被删除节点存在左右子树
      let minpp = p							  // 则用右子树的最小节点代替
      let minp = p.right
      while(minp.left !== null) {
        minpp = minp
        minp = minp.left
      }
      p.data = minp.data // 替换被删除节点
      pp = minpp // 同后续逻辑，删除右子树最小节点
      p = minp
    }
    
    let child // 保存被删除节点子树
    if(p.left !== null) { // 如果被删除节点只存在左子树
      child = p.left
    }
    else if(p.right !== null) { // 如果被删除节点只存在右子树
      child = p.right
    } else { // 如果被删除节点不存在左右子树
      child = null
    }

    if(pp === null) this.root = child // 删除根节点
    if(pp.left === p) pp.left = child
    else pp.right = child
  }

  // 删除（递归）
  remove(node, data) {
    if(data === node.data) {
      if(node.left === null && node.right === null) {
        return null
      } else if(node.left !== null && node.right === null) {
        return node.left
      } else if(node.right !== null && node.left === null) {
        return node.right
      } else {
        let p = node.right
        while(p.left !== null) {
          p = p.left
        }
        node.data = p.data
        node.right = this.remove(node.right, p.data)
        return node
      }
    } else if(data < node.data) {
      node.left = this.remove(node.left, data)
      return node
    } else {
      node.right = this.remove(node.right, data)
      return node
    }
  }
}

let bst = new BST()

bst.insert(16)
bst.insert(13)
bst.insert(15)
bst.insert(18)
bst.insert(17)
bst.insert(25)
bst.insert(19)
bst.insert(27)

bst.remove(bst.root, 18)
bst.inOrder(bst.root)
~~~

&emsp;

### 排序

![algorithm-sort](assets/algorithm-sort.jpg)

&emsp;

![algorithm-sort1](assets/algorithm-sort1.jpg)

##### 冒泡排序

~~~js
function bubbleSort(array) {
    let len = array.length;
    for(var i = 0; i < len-1; i++) {
        let flag = false // 提前退出冒泡循环的标志位
        for(let j = 0; j < len - 1 - i; j++) {
            if(array[j] > array[j+1]) {
                let tmp = array[j]
                array[j] = array[j+1]
                array[j+1] = tmp
                flag = true // 表示有数据交换
            }
        }
        if(!flag) break // 没有数据交换，提前退出
    }
}
~~~

&emsp;

##### 插入排序

~~~js
function insertionSort(array) {
    const len = array.length;
    for(let i = 1; i < len; i++) {
        const value = array[i]
        let j = i - 1
        for(; j >= 0; j--) {
            if(array[j] > value) {
                array[j+1] = array[j]
            } else {
                break
            }
        }
        array[j+1] = value
    }
}
~~~

&emsp;

##### 希尔排序

~~~js
function shellSort(array) {
    for(let gap = Math.floor(array.length/2); gap > 0; gap = Math.floor(gap/2)) {
      for(let i = gap; i < array.length; i++) {
        let value = array[i]
        let j = i - gap
        for(; j >= 0; j -= gap) {
          if(array[j] > value) {
            array[j+gap] = array[j]
          } else {
            break
          }
        }
        array[j+gap] = value
      }
    }
}
~~~

希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，也称为**缩小增量排序**。

&emsp;

##### 选择排序

~~~js
function selectionSort(array) {
    const len = array.length;
    for(let i = 0; i < len-1; i++) {
        var min = i;
        for(let j = i+1; j < len; j++) {
            if(array[j] < array[min]) {
                min = j;
            }
        }
        const tmp = array[i]
        array[i] = array[min]
        array[min] = tmp
    }
}
~~~

&emsp;

##### 快速排序

~~~js
function quickSort(array, left, right) {
    if(left < right) {
        var index = patition(array,left,right);
        quickSort(array,left,index-1);
        quickSort(array,index+1,right);
    }
}

function patition(array,left,right) {
    var begin = left, end = right, key = array[right];
    while(begin < end) {
        while(begin < end && array[begin] <= key) {
            begin++;
        }
        while(begin < end && array[end] >= key) {
            end--;
        }
        var temp = array[end];
        array[end] = array[begin];
        array[begin] = temp;
    }
    var temp = array[right];
    array[right] = array[begin];
    array[begin] = temp;
    return begin;
}

var a = [1,2,3,5,4];
quickSort(a,0,4);

// 第K大的数——分区思想——时间复杂度O(n)
function kthNumber(array, k) {
  if(k > array.length) {
    return -1
  }
  let left = 0, right = array.length -1
  while(left <= right) {
    let index = patition(array, left, right)
    if(index + 1 === k) {
      return array[index]
    } else if(index + 1 < k) {
      left = index  + 1
    } else {
      right = index - 1
    }
  }
}

function patition(array, left, right) {
  let begin = left, end = right, key = array[right]
  while(begin < end) {
    while(begin < end && array[begin] >= key) begin++
    while(begin < end && array[end] <= key) end--
    let tmp = array[begin]
    array[begin] = array[end]
    array[end] = tmp
  }
  let tmp = array[begin]
  array[begin] = array[right]
  array[right] = tmp
  return begin
}
~~~

&emsp;

##### 归并排序

~~~js
function mergeSort(array, left, right) {
  if(left >= right) return
  let mid = Math.floor((left + right) / 2)
  mergeSort(array, left, mid)
  mergeSort(array, mid + 1, right)
  merge(array, left, mid, right)
}

function merge(array, left, mid, right) {
  let arr = [], i = left, j = mid + 1
  while(i <= mid && j <= right) {
    if(array[i] <= array[j]) {
      arr.push(array[i++])
    } else {
      arr.push(array[j++])
    }
  }
  while(i <= mid) {
    arr.push(array[i++])
  }
  while(j <= right) {
    arr.push(array[j++])
  }
  for(let z = left; z <= right; z++) {
    array[z] = arr[z-left]
  }
}
~~~

&emsp;

##### 堆排序

~~~js
class Heap {
  constructor() {
    this.heap = [] // 从下标1开始存储数据
    this.n = 0
  }

  insert(data) { // 插入元素
    let n = ++this.n
    let heap = this.heap
    heap[n] = data
    let i = n
    while(Math.floor(i/2) > 0 && heap[i] > heap[Math.floor(i/2)]) {
      this.swap(i, Math.floor(i/2))
      i = Math.floor(i/2)
    }
  }

  remove() { // 删除堆顶元素
    if(this.n === 0) return
    this.heap[1] = this.heap[this.n]
    this.n--
    this.heapify(this.n, 1)
  }

  heapify(n, i) { // 自上往下堆化
    let heap = this.heap
    while(true) {
      let max = i
      if(2*i <= n && heap[i] < heap[2*i]) max = 2*i
      if(2*i+1 <= n && heap[max] < heap[2*i+1]) max = 2*i+1
      if(max === i) return
      this.swap(i,max)
      i = max
    }
  }

  swap(i, j) {
    let tmp = this.heap[i]
    this.heap[i] = this.heap[j]
    this.heap[j] = tmp 
  }
}

class HeapSort {
  constructor(array) {
    this.heap = array // 从下标1开始存储数据
    this.n = array.length - 1
  }

  buildHeap() {
    let i = Math.floor(this.n/2)
    while(i) {
      this.heapify(this.n, i)
      i--
    }
  }

  heapify(n, i) { // 自上往下堆化
    let heap = this.heap
    while(true) {
      let max = i
      if(2*i <= n && heap[i] < heap[2*i]) max = 2*i
      if(2*i+1 <= n && heap[max] < heap[2*i+1]) max = 2*i+1
      if(max === i) return
      this.swap(i,max)
      i = max
    }
  }

  sort() {
    this.buildHeap() // 先建堆
    let i = this.n
    while(i) {
      this.swap(1,i)
      i--
      this.heapify(i, 1)
    }
  }

  swap(i, j) {
    let tmp = this.heap[i]
    this.heap[i] = this.heap[j]
    this.heap[j] = tmp 
  }
}
~~~

&emsp;

#### 二分查找

~~~js
function binarySearch(array, key) {
    var low = 0, high = array.length, mid;
    while(low <= high) {
        mid = (low + high) / 2;
        if(key < array[mid]) {
            high = mid - 1;
        } else if(key > array[mid]) {
            low = mid + 1;
        } else {
            return mid;
        }
    }
    return -1;
}

var a = [1,2,4,6,8,9];
binarySearch(a, 4);

// 递归写法
function binarySearch(array, low, high, value) {
  let mid = Math.floor(low + (high - low) / 2)
  if(array[mid] === value) {
    return mid
  } else if(array[mid] < value) {
    return binarySearch(array, mid + 1, high, value)
  } else {
    return binarySearch(array, low, mid - 1, value)
  }
}
~~~

&emsp;

### 图

~~~js
class Graph {
  constructor(v) {
    this.v = v
    this.pre = [] // 记录搜索路径
    this.adj = [] // 邻接表
    for(let i = 0; i < this.v; i++) {
      this.adj[i] = []
    }
  }

  addEdge(s, t) {
    this.adj[s].push(t)
    this.adj[t].push(s)
  }

  bfs(s) { // 广度优先搜索
    let visited = [] // 标记已被访问的顶点
    let queue = []   // 队列存储已被访问，但相邻顶点还没被访问的顶点
    for(let i = 0; i < this.v; i++) {
      visited[i] = 0
      this.pre[i] = -1
    }
    queue.push(s)
    visited[s] = 1
    while(queue.length) {
      let v = queue.shift()
      for(let w of this.adj[v]) {
        if(!visited[w]) {
          queue.push(w)
          visited[w] = 1
          this.pre[w] = v
        }
      }
    }
  }

  dfs(s) { // 深度优先搜索
    let visited = [] // 标记已被访问的顶点
    for(let i = 0; i < this.v; i++) {
      visited[i] = 0
      this.pre[i] = -1
    }
    this.dfsRecursion(s, visited)
  }

  dfsRecursion(s, visited) {
    visited[s] = 1
    for(let w of this.adj[s]) {
      if(!visited[w]) {
        this.pre[w] = s
        this.dfsRecursion(w, visited)
      }
    }
  }

  print(s, t) { // 递归打印 s->t 的路径
    if(this.pre[t] !== -1 && t !== s) {
      this.print(s, this.pre[t])
    }
    console.log(t + ' ')
  }
}
~~~

&emsp;

### 字符串匹配算法

##### BF 算法

BF（Brute Force），暴力匹配算法，又叫朴素匹配算法

实现思路：拿模式串与主串中是所有子串匹配，看是否有能匹配的子串。时间复杂度是 O(n*m)

&emsp;

##### RK 算法

RK（Rabin-Karp）

实现思路：对每个子串分别求哈希值，然后拿子串的哈希值与模式串的哈希值比较。时间复杂度是 O(n)

&emsp;

##### BM 算法

BM（Boyer-Moore）

核心思想：利用模式串本身的特点，在模式串中某个字符与主串不能匹配的时候，将模式串往后多滑动几位，以此来减少不必要的字符比较，提高匹配的效率。BM 算法构建的规则有两类，坏字符规则和好后缀规则。好后缀规则可以独立于坏字符规则使用。因为坏字符规则的实现比较耗内存，为了节省内存，我们可以只用好后缀规则来实现 BM 算法。

&emsp;

### 动态规划

#### 最长公共子串

~~~js
function lcs(word1,word2) {
    var len1 = word1.length;
    var len2 = word2.length;
    var lcs = new Array(len1+1);
    for(var i = 0; i <= len1; i++) {
        lcs[i] = new Array(len2+1);
        for(var j = 0; j < len2; j++) {
            lcs[i][j] = 0;
        }
    }
    var max = 0, index = -1;
    for(i = 0; i <= len1; i++) {
        for(j = 0; j <= len2; j++) {
            if(i==0 || j==0) {
                lcs[i][j] = 0;
            }
            if(word1[i] == word2[j]) {
                lcs[i][j] = lcs[i-1][j-1] + 1;
            } else {
                lcs[i][j] = 0;
            }
            if(lcs[i][j] > max) {
                index = i;
                max = lcs[i][j];
            }
        }
    }
    var str = "";
    for(i = max-index; i < index; i++) {
        str += word1[i];
    }
}
~~~

&emsp;

#### 背包

~~~js
function dknapsack(capacity,size,value,n) {
    var k = [];
    for(var i = 0; i <= n; i++) {
        k[i] = [];
    }

    for(var i = 0; i <= n; i++) {
        for(var w = 0; w <= capacity; w++) {
            if(i == 0 || w == 0) {
                k[i][w] = 0;
            } else if(w >= size[i-1]) {
                k[i][w] = max(k[i-1][w-size[i-1]] + value[i-1],k[i-1][w]);
            } else {
                k[i][w] = k[i-1][w];
            }
        }
    }
}
~~~

