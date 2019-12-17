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
}


const list = new LinkedList()
list.appned('a')
list.appned('b')
list.appned('a')

var result = list.palindromic()
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
                if(node.left = null) {
                    node.left = new Node(data,null,null);
                    break;
                } else {
                    node = node.left;
                }
            } else if(data > node.data) {
                if(node.right = null) {
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

### 排序

#### 二分排序

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
~~~

&emsp;

#### 快速排序

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
~~~

&emsp;

#### 冒泡排序

~~~js
function bubbleSort(array) {
    var len = array.length;
    for(var i = 0; i < len-1; i++) {
        for(var j = 0; j < len-1-i; j++) {
            if(array[j] > array[j+1]) {
                swap(array[j],array[j+1]);
            }
        }
    }
}
~~~

&emsp;

#### 选择排序

~~~js
function selectionSort(array) {
    var len = array.length;
    for(var i = 0; i < len-1; i++) {
        var min = i;
        for(var j = i+1; j < len; j++) {
            if(array[j] < array[min]) {
                min = j;
            }
        }
        swap(array[i],array[min]);
    }
}
~~~

&emsp;

#### 插入排序

~~~js
function insertionSort(array) {
    var len = array.length;
    for(var i = 1; i < len; i++) {
        for(var j=i; j > 0; j--) {
            if(array[j] < array[j-1]) {
                swap(array[j], array[j-1])
            } 
        }
    }
}
~~~

&emsp;

#### 希尔排序

~~~js
function shellSort(array) {
    var len = array.length;
    var gap = [5,3,1];
    for(g = 0; g < gap.length; g++) {
        for(var i = gap[g]; i < len; i++) {
            for(var j=i; j >= gap[g]; j=j-gap[g]) {
                if(array[j] < array[j-gap[g]]) {
                    swap(array[j], array[j-gap[g]])
                } 
            }
        }
    }
}
~~~

 注意和插入排序的区别！ 

&emsp;

### 图

#### 广搜

~~~js
function bfs(s) {
    this.marked[s] = true;
    var queue = [];
    queue.push(s);
    while(queue.length) {
        var v = queue.shift();
        for(var w of this.adj[v]) {
            if(!this.marked[w]) {
                queue.push(w);
                this.EdgeTo[w] = v;
                this.marked[w] = true;
            }
        }
    }
}

function pathTo(s,t) {
    for(var v = t; v !=s; v = this.EdgeTo[v]) {
        path.push(v);
    }
}
~~~

&emsp;

#### 深搜

~~~js
function dfs(v) {
    this.marked[v] = true;
    for(var w of this.adj[v]) {
        if(!this.marked[w]) {
            dfs(w);
        }
    }
}
~~~

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

