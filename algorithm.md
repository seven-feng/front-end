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

- 堆是一颗完全二叉树
- 堆中每一个节点的值都必须大于等于（或小于等于）其子树中每个节点的值

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

  addEdge(s, t) { // 无向图
    this.adj[s].push(t)
    this.adj[t].push(s)
  }
    
  addEdge1(s, t) { // 有向图
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
    
  topoSortByKahn() { // 拓扑排序——Kahn 算法
    let inDegree = new Array(this.v)
    for(let i = 0; i < this.v; i++) { // 初始化顶点的入度数组
      inDegree[i] = 0
    }
    for(let i = 0; i < this.v; i++) {
      for(let j = 0; j < this.adj[i].length; j++) { // 计算每个顶点的入度
        let w = this.adj[i][j]
        inDegree[w]++
      }
    }

    let queue = []
    for(let i = 0; i < this.v; i++) {
      if(inDegree[i] === 0) {
        queue.push(i)
      }
    }
    while(queue.length) {
      let v = queue.shift()
      console.log(' --> ' + v)
      for(let j = 0; j < this.adj[v].length; j++) {
        let w = this.adj[v][j]
        inDegree[w]--
        if(inDegree[w] === 0) {
          queue.push(w)
        }
      }
    }
  }
    
  topoSortByDFS() { // 拓扑排序——深度优先遍历
    // 算法应用场景：编译器通过源文件两两之间的局部依赖关系，确定一个全局的编译顺序
    // 邻接表中，边 s->t 表示 s 先于 t 执行
    // 逆邻接表中，边 s->t 表示 s 依赖于 t，s 后于 t 执行
    // 依赖关系需要构建逆邻接表
    let visited = []
    let inverseAdj = []
    for(let i = 0; i < this.v; i++) {
      visited[i] = false
      inverseAdj[i] = []
    }
    
    for(let i = 0; i < this.v; i++) {
      for(let w of this.adj[i]) {
        inverseAdj[w].push(i)
      }
    }
      

    for(let i  = 0; i < this.v; i++) {
      if(!visited[i]) {
        this.DFS(i, visited, inverseAdj)
      }
    }
  }

  DFS(v, visited, inverseAdj) {
    visited[v] = true
    for(let w of inverseAdj[v]) {
      if(!visited[w]) {
        this.DFS(w, visited, inverseAdj)
      }
    }
    // 先把顶点可达的所有顶点都打印出来之后，再打印它自己（使用逆邻接表）
    console.log(' --> ' + v)
  }
}
~~~

&emsp;

##### 单源最短路径算法——Dijkstra 算法

从优先级队列中取出 dist 最小的顶点 minVertex，然后考察这个顶点可达的所有顶点（代码中的 nextVertex）。如果 minVertex 的 dist 值加上 minVertex 与 nextVertex 之间边的权重 w 小于 nextVertex 当前的 dist 值，也就是说，存在另一条更短的路径，它经过 minVertex 到达 nextVertex。那就把 nextVertex 的 dist 更新为 minVertex 的 dist 值加上 w。然后，把 nextVertex 加入到优先级队列中。重复这个过程，直到找到终止顶点 t 或者队列为空。

~~~js
class Edge {
  constructor(sid, tid, w) {
    this.sid = sid
    this.tid = tid
    this.w = w
  }
}

class Vertex {
  constructor(id, dist) {
    this.id = id
    this.dist = dist
  }
}

class PriorityQueue {
  constructor(v) {
    this.heap = []
    this.n = 0
  }

  isEmtpy() {
    return this.n === 0? true: false
  }

  add(vertex) {
    let i = ++this.n
    this.heap[i] = vertex
    while(Math.floor(i/2) && this.heap[i].dist < this.heap[Math.floor(i/2)].dist) {
        this.swap(i, Math.floor(i/2))
        i = Math.floor(i/2)
    }
  }

  update(vertex) {
    for(let v of this.heap) {
      if(v.id === vertex.id) {
        v.dist = vertex.dist
      }
    }
  }

  poll() {
    if(this.n === 0) return
    let vertex = this.heap[1]
    this.heap[1] = this.heap[this.n]
    this.n--
    this.heapify(this.n, 1)
    return vertex
  }

  heapify(n, i) {
    while(true) {
      let max = i
      if(i*2 <=n && this.heap[i].dist > this.heap[i*2].dist) max = 2*i
      if(i*2+1 <=n && this.heap[i].dist > this.heap[i*2+1].dist) max = 2*i+1
      if(max === i) return
      this.swap(i, max)
      i = max
    }
  }

  swap(i, j) {
    let tmp = this.heap[i]
    this.heap[i] = this.heap[j]
    this.heap[j] = tmp
  }
}

class Graph {
  constructor(v) {
    this.v = v
    this.pre = [] // 记录搜索路径
    this.adj = [] // 邻接表
    for(let i = 0; i < this.v; i++) {
      this.adj[i] = []
    }
  }

  addEdge2(s, t, w) { // 有向有权图
    this.adj[s].push(new Edge(s, t, w))
  }

  dijkstra(s, t) { // 从顶点s到顶点t的最短路径
    let vertextes = []
    let inQueue = []
    let predecessor = [] 
    for(let i = 0; i < this.v; i++) {
      vertextes[i] = new Vertex(i, Number.MAX_VALUE)
      inQueue[i] = false
    }
    vertextes[s].dist = 0 // 起点距离为0
    let queue = new PriorityQueue()
    queue.add(vertextes[s])
    while(!queue.isEmtpy()) {
      let minVertex = queue.poll() // 取堆顶元素并删除
      if(minVertex.id === t) break // 最短路径产生了
      for(let e of this.adj[minVertex.id]) {
        let nextVertex = vertextes[e.tid]
        if(minVertex.dist + e.w < nextVertex.dist) {
          nextVertex.dist = minVertex.dist + e.w
          predecessor[nextVertex.id] = minVertex.id
          if(inQueue[nextVertex.id]) {
            queue.update(nextVertex)
          } else {
            queue.add(nextVertex)
            inQueue[nextVertex.id] = true
          }
        }
      }
    }
    this.print(s, t, predecessor)
  }

  print(s, t, predecessor) {
    if(t === s) {
      console.log(t)
      return
    }
    this.print(s, predecessor[t], predecessor)
    console.log(t)
  }
}
~~~





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

核心思想：利用模式串本身的特点，在模式串中某个字符与主串不能匹配的时候，将模式串往后多滑动几位，以此来减少不必要的字符比较，提高匹配的效率。BM 算法构建的规则有两类，坏字符规则和好后缀规则。坏字符规则，拿坏字符在模式串中从后往前查找相同的坏字符。好后缀规则，拿好后缀在模式串中从后往前查找子串，如果没有，拿好后缀的后缀子串匹配模式串的前缀子串。

&emsp;

##### KMP 算法

KMP（Knuth Morris Pratt）

核心思想：借鉴 BM 算法，总结为好前缀规则，拿好前缀的后缀子串与好前缀的前缀子串匹配。

&emsp;

##### Tire 树

Tire 树是一种多模式串匹配算法，优势在于查找前缀匹配的字符串，比如搜索引擎中的关键词提示功能，是个比较经典的应用场景

~~~js
// 字典树
class Node {
  constructor(data) {
    this.data = data
    this.children = new Array(26)
    this.isEndingChar  = false
  }
}

class Tire {
  constructor() {
    this.root = new Node('/')
  }

  insert(text) {
    let p = this.root
    let i = 0
    for(let c of text) {
      let index = c.charCodeAt() - 'a'.charCodeAt()
      if(!p.children[index]) {
        p.children[index] = new Node(c)
      }
      p = p.children[index]
    }
    p.isEndingChar = true
  }

  find(text) {
    let p = this.root
    for(let c of text) {
      let index = c.charCodeAt() - 'a'.charCodeAt()
      if(!p.children[index]) {
        return false
      } else {
        p = p.children[index]
      }
    }
    return p.isEndingChar
  }
}
~~~

&emsp;

##### AC 自动机

AC 自动机（Aho-Corasick），经典的多模式串匹配算法。整个 AC 自动机算法包含两个部分：第一部分是将多个模式串构建成 AC 自动机（1.将模式串构建成 Trie 树 2.在 Trie 树上构建失败指针），第二部分是在 AC 自动机中匹配主串。

~~~js
class ACNode {
  constructor(data) {
    this.data = data
    this.children = new Map()
    this.fail = null
    this.length = 0
    this.isEndingChar = false
  }
}

class ACTree {
  constructor() {
    this.root = new ACNode('/')
  }

  insert(text) { // 将多个模式串构建成 Trie 树
    let p = this.root
    for(let c of text) {
      if(!p.children.has(c)) {
        p.children.set(c, new ACNode(c))
      }
      p = p.children.get(c)
    }
    p.isEndingChar = true
    p.length = text.length
  }

  buildFailurePointer() { // 在 Trie 树上构建失败指针
    let root = this.root
    let queue = []
    queue.push(root)
    while(queue.length) {
      let p = queue.shift()
      for(let pc of p.children.values()) {
        if(!pc) return
        if(p === root) {
          pc.fail = root
        } else {
          let q = p.fail
          while(q) {
            let qc = q.children.get(pc.data)
            if(qc) {
              pc.fail = qc
              break
            }
            q = q.fail
          }
          if(!q) {
            pc.fail = root
          }
        }

        queue.push(pc)
      }
    }
  }

  match(text) { // 匹配主串
    let root = this.root
    let n = text.length
    let p = root
    for(let i = 0; i < n; i++) {
      let c = text[i]
      while(!p.children.get(c) && p !== root) {
        p = p.fail
      }

      p = p.children.get(c)
      if(!p) { // 如果没有匹配的，从root开始重新匹配
        p = root
      }

      let tmp = p
      while(tmp !== root) {
        if(tmp.isEndingChar === true) {
          console.log(`start from ${i - p.length + 1}, length: ${p.length}`)
        }
        tmp = tmp.fail
      }
    }
  }
}

function match(text, patterns) {
  let automata = new ACTree()

  for (let pattern of patterns) {
      automata.insert(pattern);
  }
  automata.buildFailurePointer()
  automata.match(text)
}

let patterns2 = ["Fxtec Pro1", "谷歌Pixel"];
let text2 = "一家总部位于伦敦的公司Fxtex在MWC上就推出了一款名为Fxtec Pro1的手机，该机最大的亮点就是采用了侧滑式全键盘设计。DxOMark年度总榜发布 华为P20 Pro/谷歌Pixel 3争冠";
match(text2, patterns2);
~~~

&emsp;

### 动态规划

##### 最长公共连续子串

~~~js
function lcs(word1,word2) {
  let len1 = word1.length
  let len2 = word2.length
  let lcs = []
  for(let i = 0; i <= len1; i++) {
      lcs[i] = []
      for(let j = 0; j <= len2; j++) {
          lcs[i][j] = 0
      }
  }
  let max = 0, index = 0
  for(let i = 0; i <= len1; i++) {
      for(let j = 0; j <= len2; j++) {
          if(i==0 || j==0) {
              lcs[i][j] = 0
          } else {
            if(word1[i-1] == word2[j-1]) {
              lcs[i][j] = lcs[i-1][j-1] + 1
            } else {
                lcs[i][j] = 0
            }
          }

          if(lcs[i][j] > max) {
              index = i
              max = lcs[i][j]
          }
      }
  }
  let str = ''
  for(let i = index-max; i < index; i++) {
      str += word1[i]
  }
}
~~~

&emsp;

##### 0-1 背包

~~~js
// weight：物品重量，value：物品价值，w：背包可承载重量，n：物品个数
// 一维
function knapsack(weight, value, w, n) {
  let k = []
  for(let i = 0; i <= w; i++) {
    k[i] = 0
  }

  for(let i = 0; i < n; i++) {
    for(let j = w; j >=weight[i]; j--) { // 从后往前，可以防止重复计算
      k[j] = Math.max(k[j-weight[i]] + value[i], k[j])
    }
  }
}

// 二维
function knapsack(weight, value, w, n) {
  let k = []
  for(let i = 0; i < n; i++) {
    k[i] = []
  }

  for(let i = 0; i < n; i++) 
    for(let j = 0; j <= w; j++) {
      k[i][j] = 0
    }

  k[0][weight[0]] = value[0] 
  for(let i = 1; i < n; i++) {
    for(let j = weight[i]; j <=w; j++) {
      k[i][j] = Math.max(k[i-1][j], k[i-1][j-weight[i]] + value[i])
    }
  }
}
~~~

&emsp;

##### 量化两个字符串的相似度

编辑距离（Edit Distance），将一个字符串转化成另一个字符串，需要的最少编辑操作次数（比如增加一个字符、删除一个字符、替换一个字符）。编辑距离越大，说明两个字符串的相似程度越小；相反，编辑距离就越小，说明两个字符串的相似程度越大。对于两个完全相同的字符串来说，编辑距离就是 0。

根据所包含的编辑操作种类的不同，编辑距离有多种不同的计算方式，比较著名的有**莱文斯坦距离**（Levenshtein distance）和**最长公共子串长度**（Longest common substring length）。其中，莱文斯坦距离允许**增加、删除、替换字符**这三个编辑操作，最长公共子串长度只允许**增加、删除字符**这两个编辑操作。

&emsp;

~~~js
// 莱温斯坦距离——可直接计算编辑距离
function lwst(a, b) {
  let m = a.length
  let n = b.length
  let minDist = []
  for(let i = 0; i < m; i++) {
    minDist[i] = []
    for(let j = 0; j < n; j++) {
      minDist[i][j] = 0
    }
  }

  for(let i = 0; i < m; i++) { // 初始化第0列:a[0..i]与b[0..0]的编辑距离
    if(b[0] === a[i]) {
      minDist[i][0] = i
    } else if(i !== 0){
      minDist[i][0] = minDist[i-1][0] + 1
    }
  }

  for(let j = 0; j < n; j++) { // 初始化第0行:a[0..0]与b[0..j]的编辑距离
    if(a[0] === b[j]) {
      minDist[0][j] = j
    } else if(j !== 0){
      minDist[0][j] = minDist[j-1][0] + 1
    }
  }
  
  for(let i = 1; i < m; i++)
    for(let j = 1; j < n; j++) {
      if(a[i] === b[j]) {
        minDist[i][j] = 
            Math.min(minDist[i-1][j]+1, minDist[i][j-1]+1, minDist[i-1][j-1])
      } else {
        minDist[i][j] = 
            Math.min(minDist[i-1][j]+1, minDist[i][j-1]+1, minDist[i-1][j-1]+1)
      }
    }

  console.log(minDist[m-1][n-1])  
}
~~~

&emsp;

~~~js
// 最长公共子串长度（不连续）——需要进一步计算，才能计算得到编辑距离
function lcs(a, b) {
  let m = a.length
  let n = b.length
  let lcs = []
  for(let i = 0; i < m; i++) {
    lcs[i] = []
    for(let j = 0; j < n; j++) {
      lcs[i][j] = 0
    }
  }

  for(let i = 0; i < m; i++) {
    if(a[i] === b[0]) {
      lcs[i][0] = 1
    } else if(i !== 0){
      lcs[i][0] = lcs[i-1][0]
    }
  }

  for(let i = 0; i < n; i++) {
    if(a[0] === b[i]) {
      lcs[0][i] = 1
    } else if(i !== 0){
      lcs[0][i] = lcs[i-1][0]
    }
  }
  
  for(let i = 1; i < m; i++)
    for(let j = 1; j < n; j++) {
      if(a[i] === b[j]) {
        lcs[i][j] = Math.max(lcs[i-1][j-1]+1, lcs[i-1][j], lcs[i][j-1])
      } else {
        lcs[i][j] = Math.max(lcs[i-1][j-1], lcs[i-1][j], lcs[i][j-1])
      }
    }
    
  console.log(lcs[m-1][n-1])  
}
~~~

