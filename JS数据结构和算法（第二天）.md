## 一、链表

**链表和数组**

> 链表和数组一样，可以用于存储一系列的元素，但是链表和数组的实现机制完全不同。

数组

> - 存储多个元素，数组（或列表）可能是最常用的数据结构。
> - 几乎每一种编程语言都有默认实现数组结构，提供了一个便利的 [] 语法来访问数组元素。

数组缺点：

> - 数组的创建需要申请一段连续的内存空间(一整块内存)，并且大小是固定的，当前数组不能满足容量需求时，需要扩容。
> (一般情况下是申请一个更大的数组，比如 2 倍，然后将原数组中的元素复制过去)
> 
> - 在数组开头或中间位置插入数据的成本很高，需要进行大量元素的位移。

链表

> - 存储多个元素，另外一个选择就是使用链表。
> 
> - 不同于数组，链表中的元素在内存中不必是连续的空间。
> 
> - 链表的每个元素由一个存储元素本身的节点和一个指向下一个元素的引用(有些语言称为指针)组成。

链表优点：

> - 内存空间不必是连续的，可以充分利用计算机的内存，实现灵活的内存动态管理。
> 
> - 链表不必在创建时就确定大小，并且大小可以无限延伸下去。
> 
> - 链表在插入和删除数据时，时间复杂度可以达到 O(1)，相对数组效率高很多。

链表缺点：

> - 访问任何一个位置的元素时，需要从头开始访问。(无法跳过第一个元素访问任何一个元素)
> 
> - 无法通过下标值直接访问元素，需要从头开始一个个访问，直到找到对应的元素。
> 
> - 虽然可以轻松地到达下一个节点，但是回到前一个节点是很难的。

## **二、单向链表**

> 单向链表类似于火车，有一个火车头，火车头会连接一个节点，节点上有乘客，并且这个节点会连接下一个节点，以此类推。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201031193257487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)

链表的数据结构

> - head 属性指向链表的第一个节点。 
> - 链表中的最后一个节点指向 null。 当链表中一个节点也没有的时候，head 直接指向 null。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201031192913827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)

**链表中的常见操作**

>  - append(element) 向链表尾部添加一个新的项。 
>  - insert(position, element) 向链表的特定位置插入一个新的项。 
>  - get(position) 获取对应位置的元素。 
>  - indexOf(element)  返回元素在链表中的索引。如果链表中没有该元素就返回-1。 
>  - update(position, element) 修改某个位置的元素。
>  - removeAt(position) 从链表的特定位置移除一项。 
>  - remove(element) 从链表中移除一项。 
>  - isEmpty() 如果链表中不包含任何元素，返回 true，如果链表长度大于 0 则返回 false。 
>  - size() 返回链表包含的元素个数，与数组的 length 属性类似。 
>  - toString() 由于链表项使用了 Node 类，就需要重写继承自 JavaScript 对象默认的toString 方法，让其只输出元素的值。

JS实现单向链表的封装：
```javascript
        <script>
        function LinkedList() {
            function Node(data) {
                this.data = data;
                this.next = null;
            }

            this.head = null;
            this.length = 0;

            LinkedList.prototype.append = function (data) {
                var newNode = new Node(data);
                // 判断是否添加的是第一个节点
                if (this.length === 0) {
                    this.head = newNode;
                } else {
                    // 找到最后一个节点
                    var current = this.head;
                    while (current.next) {
                        current = current.next;
                    }
                    // 最后节点的next指向新的节点
                    current.next = newNode;
                }
                this.length += 1;
            }

            LinkedList.prototype.toString = function () {
                var current = this.head;
                var listString = '';

                // 循环获取一个个的节点
                while (current) {
                    listString += current.data + ''
                    current = current.next;
                }
                return listString;
            }

            LinkedList.prototype.insert = function (position, data) {

                // 对position进行越界判断
                if (position < 0 || position > this.length) return false;

                // 根据data创建newNode
                var newNode = new Node(data);

                // 判断插入位置是否为第一个
                if (position == 0) {
                    newNode.next = this.head;
                    this.head = newNode;
                } else {
                    var index = 0;
                    var current = this.head;
                    var previous = null;
                    while (index++ < position) {
                        previous = current;
                        current = current.next;
                    }
                    newNode.next = current;
                    previous.next = newNode;
                }
                this.lenght += 1
                return true;
            }

            LinkedList.prototype.get = function (position) {
                // 越界判断
                if (position < 0 || position >= this.lenght) return null;

                // 获取对应的data
                var current = this.head;
                var index = 0;
                while (index++ < position) {
                    current = current.next;
                }
                return current.data;
            }

            LinkedList.prototype.indexOf = function (data) {
                var current = this.head;
                var index = 0;

                // 开始查找
                while (current) {
                    if (current.data == data) {
                        return index;
                    } else {
                        current = current.next;
                        index++;
                    }
                }
                return -1;
            }

            LinkedList.prototype.update = function (position, newData) {
                // 越界判断
                if (position < 0 || position >= this.lenght) return false;

                // 查找正确的节点
                var current = this.head;
                var index = 0;
                while (index++ < position) {
                    current = current.next;
                }

                // 将position位置的节点的data改为newData
                current.data = newData;
                return true;
            }

            LinkedList.prototype.removeAt = function (position) {
                // 越界判断
                if (position < 0 || position >= this.lenght) return null;

                // 判断是否删除的为第一个节点
                var current = this.head;
                if (position == 0) {
                    this.head = this.head.next;
                } else {
                    var index = 0;
                    var previous = null;
                    while (index++ < position) {
                        previous = current;
                        current = current.next;
                    }
                    // 前一个节点的next指向current的next即可
                    previous.next = current.next;
                }
                this.lenght--;
                return current.data;
            }

            LinkedList.prototype.remove = function (data) {
                // 获取data在列表中的位置
                var position = this.indexOf(data);
                // 根据位置信息，删除节点
                return this.removeAt(position);
            }

            LinkedList.prototype.isEmpty = function () {
                return this.length == 0;
            }

            LinkedList.prototype.size = function () {
                return this.length;
            }
        }

        const linkedList = new LinkedList();

        // 测试 append 方法
        linkedList.append("A");
        linkedList.append("B");
        linkedList.append("C");
        console.log(linkedList);
        //-->输出：
        // head: Node
        // data: "A"
        // next: Node
        // data: "B"
        // next: Node
        // data: "C"
        // next: null


        // 测试toString方法
        console.log(linkedList.toString()); //--> ABC

        // 测试insert方法
        linkedList.insert(0, "156");
        linkedList.insert(5, "198");
        linkedList.insert(2, "178");
        console.log(linkedList.toString()); //--> 156A178BC

        // 测试get方法
        console.log(linkedList.get(0)); // -->156
        console.log(linkedList.get(3)); //-->B

        // 测试indexOf方法
        console.log(linkedList.indexOf('B')); //-->3
        console.log(linkedList.indexOf('156')); //-->0
        console.log(linkedList.indexOf('D')); //-->-1

        // 测试update方法
        console.log(linkedList.update(0, 'L')); // -->true
        console.log(linkedList.update(4, 'D')); //-->true
        // alert(linkedList); //-->LA178BD

        // 测试removeAt方法
        console.log(linkedList.removeAt(0)); //-->L
        // alert(linkedList); //-->A178BD
        console.log(linkedList.removeAt(3)); //-->D 
        // alert(linkedList); //-->A178B

        // 测试remove方法
        linkedList.remove('B');
        // alert(linkedList);//--> A178

        // 测试isEmpty方法
        console.log(linkedList.isEmpty());//-->false
        // 测试size方法
        console.log(linkedList.size());//-->3
    </script>
```

## **三、双向链表**

> - 既可以从头遍历到尾，也可以从尾遍历到头。
>  - 链表相连的过程是双向的。实现原理是一个节点既有向前连接的引用，也有一个向后连接的引用。
> - 双向链表可以有效的解决单向链表存在的问题。 
> - 双向链表缺点： 
> 1. 每次在插入或删除某个节点时，都需要处理四个引用，而不是两个，实现起来会困难些。
> 2. 相对于单向链表，所占内存空间更大一些。
> 3. 但是，相对于双向链表的便利性而言，这些缺点微不足道。

双向链表结构

> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201106192624780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> - 双向链表不仅有 head 指针指向第一个节点，而且有 tail 指针指向最后一个节点。 
> - 每一个节点由三部分组成：item 储存数据、prev指向前一个节点、next 指向后一个节点。 
> - 双向链表的第一个节点的 prev 指向 null。 
> - 双向链表的最后一个节点的 next 指向 null。

双向链表常见的操作

> - append(element) 向链表尾部追加一个新元素。
> - insert(position, element) 向链表的指定位置插入一个新元素。
> - getElement(position) 获取指定位置的元素。
> - indexOf(element) 返回元素在链表中的索引。如果链表中没有该元素就返回 -1。
> - update(position, element) 修改指定位置上的元素。
> - removeAt(position) 从链表中的删除指定位置的元素。
> - remove(element) 从链表删除指定的元素。
> - isEmpty() 如果链表中不包含任何元素，返回 true，如果链表长度大于 0 则返回 false。
> - size() 返回链表包含的元素个数，与数组的 length 属性类似。
> - toString() 由于链表项使用了 Node 类，就需要重写继承自 - JavaScript 对象默认的 toString 方法，让其只输出元素的值。
> - forwardString() 返回正向遍历节点字符串形式。
> - backwordString() 返回反向遍历的节点的字符串形式。

JS实现双向链表的封装：

```javascript
    <script>
        function DoublyLinkedList() {
            function Node(data) {
                this.data = data;
                this.next = null; // 新添加 this.next 属性，该属性用于指向下一个节点
                this.prev = null; // 新添加 this.prev 属性，该属性用于指向上一个节点。
            }

            this.head = null;
            this.tail = null; // 新添加 this.tail 属性，该属性指向末尾的节点
            this.length = 0;

            DoublyLinkedList.prototype.append = function (data) {
                var newNode = new Node(data);
                // 判断是否添加的是第一个节点
                if (this.length === 0) {
                    this.head = newNode;
                    this.tail = newNode;
                } else {
                    newNode.prev = this.tail;
                    this.tail.next = newNode;
                    this.tail = newNode;
                }
                this.length += 1;
            }

            DoublyLinkedList.prototype.toString = function () {
                return this.backwardString();
            }

            DoublyLinkedList.prototype.forwardString = function () {
                var current = this.tail;
                var listString = '';

                // 循环获取一个个的节点
                while (current) {
                    listString += current.data + ''
                    current = current.prev;
                }
                return listString;
            }

            DoublyLinkedList.prototype.backwardString = function () {
                var current = this.head;
                var listString = '';

                // 循环获取一个个的节点
                while (current) {
                    listString += current.data + ''
                    current = current.next;
                }
                return listString;
            }

            DoublyLinkedList.prototype.insert = function (position, data) { 

                // 对position进行越界判断
                if (position < 0 || position > this.length) return false;

                // 根据data创建newNode
                var newNode = new Node(data);

                // 判断插入位置是否为第一个
                if (this.length == 0) {
                    this.head = newNode;
                    this.tail = newNode;
                } else {
                    if (position == 0) {
                        this.head.prev = newNode;
                        newNode.next = this.head;

                        this.head = newNode;
                    } else if (position == this.length) {
                        newNode.prev = this.tail;
                        this.tail.next = newNode;                       
                        this.tail = newNode;
                    } else {
                        var current = this.head;
                        var index = 0;
                        while (index++ < position) {
                            current = current.next;
                        }
                        newNode.next = current;
                        newNode.prev = current.prev;
                        current.prev.next = newNode;
                        current.prev = newNode;
                    }
                }

                this.length += 1;
                return true;
            }

            DoublyLinkedList.prototype.get = function (position) { 

                // 对position进行越界判断
                if (position < 0 || position >= this.length) return null;

                var current = this.head;
                var index = 0;
                while(index++ < position){
                    current = current.next;
                }
                return current.data;
            }

            DoublyLinkedList.prototype.indexOf = function (data) { 
                var current = this.head;
                var index = 0;
                while(current){
                    if(current.data == data){
                        return index;          
                    }
                    current = current.next;
                    index++;
                }
                return -1;
            }

            DoublyLinkedList.prototype.update = function (position,newData) { 

                // 对position进行越界判断
                if (position < 0 || position >= this.length) return false;

                var current = this.head;
                var index = 0;
                while(index++ < position){
                    current = current.next;
                }
                current.data = newData;
                return true;
            }

            DoublyLinkedList.prototype.removeAt = function (position) { 
                if (position < 0 || position >= this.length) return null;

                var current = this.head;
                if(this.length==1){
                    this.head = null;
                    this.tail = null;
                }else {
                    if(position == 0){
                        this.head.next.prev = null;
                        this.head = this.head.next;
                    } else if(position == this.length - 1){
                        current = this.tail;
                        this.tail.prev.next = null;
                        this.tail = this.tail.prev;
                    } else {
                        var index = 0;
                        while(index++ < position){
                            current = current.next;
                        }
                        current.prev.next = current.next;
                        current.next.prev = current.prev;
                    }
                }
                this.length--;
                return current.data;
            }

            DoublyLinkedList.prototype.remove = function (data) { 
                // 根据data获取下标值
                var index = this.indexOf(data);
                // 根据index删除对应位置的节点
                return this.removeAt(index);
            }

            DoublyLinkedList.prototype.isEmpty = function () { 
                return this.length == 0;
            }

            DoublyLinkedList.prototype.size = function () { 
                return this.length;
            }

            // 获取链表的第一个元素
            DoublyLinkedList.prototype.getHead = function () { 
                return this.head.data;
            }

            // 获取链表的最后一个元素
            DoublyLinkedList.prototype.getTail = function () { 
                return this.tail.data;
            }

        }
        // 测试代码
        var list = new DoublyLinkedList();

        // 测试append方法
        list.append('aaa');
        list.append('bbb');
        list.append('ccc');

        // console.log(list); //-->aaabbbccc
        // console.log(list.forwardString()); //-->cccbbbaaa
        // console.log(list.backwardString()); //-->aaabbbccc

        // 测试insert方法
        list.insert(0, 'qqq'); 
        // alert(list); //-->qqqaaabbbccc
        list.insert(2, 'zzz');
        alert(list); //-->qqqaaazzzbbbccc

        // 测试get方法
        // console.log(list.get(0)); //-->qqq
        // console.log(list.get(2)); //-->zzz
        // console.log(list.get(4)); //-->ccc
        // console.log(list.get(5)); //-->null

        // 测试indexOf方法
        // alert(list.indexOf('aaa')); //-->1
        // alert(list.indexOf('ccc')); //-->4
        // alert(list.indexOf('www')); //-->-1
        
        // 测试update方法
        list.update(1, 'www'); 
        // alert(list); //-->qqqwwwzzzbbbccc
        list.update(4, 'ggg'); 
        // alert(list); //-->qqqwwwzzzbbbggg

        // 测试removeAt方法
        console.log(list.removeAt(0)); //-->qqq
        alert(list); //-->wwwzzzbbbggg
        console.log(list.removeAt(1)); //-->zzz
        alert(list); //-->wwwbbbggg
        console.log(list.removeAt(2)); //-->ggg
        alert(list); //-->wwwbbb

        // 测试remove方法
        console.log(list.remove('www')); //-->www
        alert(list); //-->bbb


        // 测试isEmpty方法
        console.log(list.isEmpty()); //--> false
        // 测试size方法
        console.log(list.size()); //--> 1

        
        list.insert(0,'aaa');
        list.append('ccc');
        alert(list); //--> aaabbbccc
        // 测试getHead方法
        console.log(list.getHead()); //-->aaa
        // 测试getTail方法
        console.log(list.getTail()); //-->ccc
    </script>
```

