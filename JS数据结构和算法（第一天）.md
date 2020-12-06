## 一、栈（Stack）：后进先出

**1. 举例：叠碗碟、装电池。**

> 栈常见的操作
> 
>  - push() 添加一个新元素到栈顶位置。 
>  - pop() 移除栈顶的元素，同时返回被移除的元素。 
>  - peek()   返回栈顶的元素，不对栈做任何修改（该方法不会移除栈顶的元素，仅仅返回它）。
>  -  isEmpty() 如果栈里没有任何元素就返回true，否则返回 false。
>  - size() 返回栈里的元素个数。这个方法和数组的 length 属性类似。
>  - toString()  将栈结构的内容以字符串的形式返回。

**2. JavaScript 代码实现栈结构**
```javascript
<script>
        // 栈结构的封装
        class Stack {

            constructor() {   
                this.items = []; //items是实例stack的私有属性（因为定义在this变量上）
            }

            // push(item) 压栈操作，往栈里面添加元素
            push(element) {  
                //push方法是Stack.prototype属性（因为定义在Stack类上）,是实例的公有属性
                this.items.push(element);
            }


            // pop() 出栈操作，从栈中取出元素，并返回取出的那个元素
            pop() {
                return this.items.pop();
            }

            // peek() 查看栈顶元素
            peek() {
                return this.items[this.items.length - 1];
            }

            // isEmpty() 判断栈是否为空
            isEmpty() {
                return this.items.length === 0;
            }

            // size() 获取栈中元素个数
            size() {
                return this.items.length;
            }

            // toString() 返回以字符串形式的栈内元素数据
            toString() {
                let result = '';
                for (let item of this.items) {
                    result += item + ' ';
                }
                return result;
            }
        }

        let stack = new Stack();

        // push() 测试
        stack.push(1);
        stack.push(2);
        stack.push(3);
        console.log(stack.items); //--> [1, 2, 3]

        // pop() 测试
        console.log(stack.pop()); //--> 3

        // peek() 测试
        console.log(stack.peek()); //--> 2

        // isEmpty() 测试
        console.log(stack.isEmpty()); //--> false

        // size() 测试
        console.log(stack.size()); //--> 2

        // toString() 测试
        console.log(stack.toString()); //--> 1 2

    </script>
```



**3. 栈结构的简单应用：利用栈结构的特点封装实现十进制转换为二进制的方法。**

```javascript
    <script>
        // 栈结构的封装
        class Stack {

            constructor() {
                this.items = [];
            }

            // push(item) 压栈操作，往栈里面添加元素
            push(item) {
                this.items.push(item);
            }

            // isEmpty() 判断栈是否为空
            isEmpty() {
                return this.items.length === 0;
            }

            // pop() 出栈操作，从栈中取出元素，并返回取出的那个元素
            pop() {
                return this.items.pop();
            }
        }

        function dec2bin(dec) {
            // new 一个 Map，保存余数
            const stack = new Stack();

            // 当不确定循环次数时，使用 while 循环
            while (dec > 0) {
                // 除二取余法
                stack.push(dec % 2); // 获取余数，放入栈中
                dec = Math.floor(dec / 2); // 除数除以二，向下取整
            }

            let binaryString = '';
            // 不断地从栈中取出元素（0 或 1），并拼接到一起。
            while (!stack.isEmpty()) {
                binaryString += stack.pop();
            }

            return binaryString;
        }
        // dec2bin() 测试
        console.log(dec2bin(100)); //--> 1100100
        console.log(dec2bin(88)); //--> 1011000
        console.log(dec2bin(1000)); //--> 1111101000


        /**
        扩展：floor() 方法执行的是向下取整计算，它返回的是小于或等于函数参数，并且与之最接近的整数。
        console.log(Math.floor(0.60));//0
        console.log(Math.floor(0.40));//0
        console.log(Math.floor(5.1));//5
        console.log(Math.floor(-5.1));//-6
        console.log(Math.floor(-5.9));//-6
        */
    </script>
```

## 二、队列（Queue）：先进先出

**1. 举例：排队**

> 队列常见的操作 
> - enqueue(element) 向队列尾部添加一个（或多个）新的项。
>  - dequeue() 移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。 
>  - front() 返回队列中的第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息与 Map 类的 peek方法非常类似）。 
>  - isEmpty() 如果队列中不包含任何元素，返回 true，否则返回 false。 
>  - size() 返回队列包含的元素个数，与数组的 length 属性类似。
>  - toString() 将队列中的内容，转成字符串形式。

**2. JavaScript 代码实现队列结构**
```javascript
    <script>
        class Queue {

            constructor() {
                this.items = [];
            }

            // enqueue(item) 入队，将元素加入到队列中
            enqueue(item) {
                this.items.push(item);
            }

            // dequeue() 出队，从队列中删除队头元素，返回删除的那个元素
            dequeue() {
                return this.items.shift();
            }

            // front() 查看队列的队头元素
            front() {
                return this.items[0];
            }

            // isEmpty() 查看队列是否为空
            isEmpty() {
                return this.items.length === 0;
            }

            // size() 查看队列中元素的个数
            size() {
                return this.items.length;
            }

            // toString() 将队列中的元素以字符串形式返回
            toString() {
                let result = '';
                for (let item of this.items) {
                    result += item + ' ';
                }
                return result;
            }
        }
        
        const queue = new Queue();

        // enqueue() 测试
        queue.enqueue('a');
        queue.enqueue('b');
        queue.enqueue('c');
        queue.enqueue('d');
        console.log(queue.items); //--> ["a", "b", "c", "d"]

        // dequeue() 测试
        queue.dequeue();
        queue.dequeue();
        console.log(queue.items); //--> ["c", "d"]

        // front() 测试
        console.log(queue.front()); //--> c

        // isEmpty() 测试
        console.log(queue.isEmpty()); //--> false

        // size() 测试
        console.log(queue.size()); //--> 2

        // toString() 测试
        console.log(queue.toString()); //--> c d
    </script>
```
**3. 队列实现小游戏：击鼓传花。**

分析：传入一组数据集合和设定的数字 number，循环遍历数组内元素，遍历到的元素为指定数字 number 时将该元素删除，直至数组剩下最后一个元素。

```javascript
    <script>
        class Queue {

            constructor() {
                this.items = [];
            }

            // enqueue(item) 入队，将元素加入到队列中
            enqueue(item) {
                this.items.push(item);
            }

            // dequeue() 出队，从队列中删除队头元素，返回删除的那个元素
            dequeue() {
                return this.items.shift();
            }

            // front() 查看队列的队头元素
            front() {
                return this.items[0];
            }

            // size() 查看队列中元素的个数
            size() {
                return this.items.length;
            }

        }
        
        // 利用队列结构的特点实现击鼓传花游戏求解方法的封装
        function passGame(nameList, number) {
            // 1、new 一个 Queue 对象
            const queue = new Queue();

            // 2、将 nameList 里面的每一个元素入队
            for (const name of nameList) {
                queue.enqueue(name);
            }

            // 3、开始数数
            // 队列中只剩下 1 个元素时就停止数数
            while (queue.size() > 1) {
                // 不是 number 时，重新加入到队尾
                // 是 number 时，将其删除

                for (let i = 0; i < number - 1; i++) {
                    // number 数字之前的人重新放入到队尾（即把队头删除的元素，重新加入到队列中）
                    queue.enqueue(queue.dequeue());
                }

                // number 对应这个人，直接从队列中删除
                // 由于队列没有像数组一样的下标值不能直接取到某一元素，
                // 所以采用，把 number 前面的 number - 1 个元素先删除后添加到队列末尾，
                // 这样第 number 个元素就排到了队列的最前面，可以直接使用 dequeue 方法进行删除
                queue.dequeue();
            }

            // 4、获取最后剩下的那个人
            const endName = queue.front();

            // 5、返回这个人在原数组中对应的索引
            return nameList.indexOf(endName);
        }

        // passGame() 测试
        const names = ['lily', 'lucy', 'tom', 'tony', 'jack'];
        const targetIndex = passGame(names, 4);
        console.log(names[targetIndex]); //--> lily
    </script>
```

## 三、优先队列

**优先级队列主要考虑的问题：**

> - 每个元素不再只是一个数据，还包含优先级。 
> - 在添加元素过程中，根据优先级放入到正确位置。

```javascript
    <script>
        function PriorityQueue() {
            function QueueElement(element, priority) {
                this.element = element;
                this.priority = priority;
            }
            this.items = [];

            //实现插入方法
            PriorityQueue.prototype.enqueue = function (element, priority) {
                var queueElement = new QueueElement(element, priority);
                if (this.items.length === 0) {
                    this.items.push(queueElement);
                } else {
                    var added = false;
                    for (var i = 0; i < this.items.length; i++) {
                        // 让新插入的元素进行优先级比较，priority 值越小，优先级越大
                        if (queueElement.priority < this.items[i].priority) {
                            this.items.splice(i, 0, queueElement); // 在指定的位置插入元素
                            added = true;
                            break;
                        }
                    }


                    // 如果遍历完所有元素，优先级都大于新插入的元素，就将新插入的元素插入到最后
                    if (!added) {
                        this.items.push(queueElement)
                    }
                }
            }
            PriorityQueue.prototype.dequeue = function () {
                return this.items.shift();
            }
            PriorityQueue.prototype.front = function () {
                return this.items[0];
            }
            PriorityQueue.prototype.isEmpty = function () {
                return this.items.length === 0;
            }
            PriorityQueue.prototype.size = function () {
                return this.items.length;
            }
            PriorityQueue.prototype.toString = function () {
                var resultString = '';
                for (var i = 0; i < this.items.length; i++) {
                    resultString += this.items[i].element + '-' + this.items[i].priority + ' '
                }
                return resultString;
            }
        }

        var p = new PriorityQueue();

        p.enqueue('abc', 111);
        p.enqueue('cbc', 200);
        p.enqueue('nbc', 55);
        p.enqueue('nbc', 60);
        alert(p); // --> nbc-55 nbc-60 abc-111 cbc-200 

        // dequeue() 测试
        p.dequeue();
        console.log(p.items); // --> nbc-60 abc-111 cbc-200 

        // isEmpty() 测试
        console.log(p.isEmpty()); //--> false

        // size() 测试
        console.log(p.size()); //--> 3

        // toString() 测试
        console.log(p.toString()); //--> nbc-60 abc-111 cbc-200 
    </script>
```

