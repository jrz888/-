## JavaScript实现排序算法

**一、大O表示法**

> 大O表示法：
> 
> - 在计算机中采用粗略的度量来描述计算机算法的效率，这种方法被称为“大O”表示法
> - 在数据项个数发生改变时，算法的效率也会跟着改变。所以说算法A比算法B快两倍，这样的比较是没有意义的。
> - 因此我们通常使用算法的速度随着数据量的变化会如何变化的方式来表示算法的效率，大O表示法就是方式之一。

> 
> 
> 常见的大O表示形式：
> - O（1）	常数
> - O（log(n)）	对数
> - O（n）	线性
> - O（nlog(n)）	线性和对数乘积
> - O（n²）	平方
> - O（2^n^）	指数
> 

> 不同大O形式的时间复杂度：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118100454549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 可以看到效率从大到小分别是：O（1）> O（logn）> O（n）> O（nlog(n)）> O（n²）> O（2^n^）

> 推导大O表示法的三条规则：
> 
> - 规则一：用常量1取代运行时间中所有的加法常量。如7 + 8 = 15，用1表示运算结果15，大O表示法表示为O（1）；
> - 规则二：运算中只保留最高阶项。如N^3 + 3n +1，大O表示法表示为：O（N^3^）;
> - 规则三：若最高阶项的常数不为1，可将其省略。如4N^2^，大O表示法表示为：O（N^2^）;

**二、排序算法**

> 这里主要介绍几种简单排序和高级排序：
> 
> - 简单排序：冒泡排序、选择排序、插入排序； 
> - 高级排序：希尔排序、快速排序；

1.冒泡排序

> 思路：
> 
> - 对未排序的各元素从头到尾依次比较相邻的两个元素大小关系；
> - 如果左边的人员高，则将两人交换位置。比如1比2矮，不交换位置；
> - 向右移动一位，继续比较2和3，最后比较 length - 1 和 length - 2这两个数据；
> - 当到达最右端时，最高的人一定被放在了最右边；
> - 按照这个思路，重新从最左端开始，只需要走到倒数第二个位置即可；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118113330871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 



> 动态过程：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118113603149.gif#pic_center)
> 
代码实现：
```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script>
        // 封装列表
        function ArrayList() {
            // 属性
            this.array = [];

            // 方法
            // insert：将数据插入到数组中的方法
            ArrayList.prototype.insert = function (item) {
                this.array.push(item);
            }

            ArrayList.prototype.toString = function () {
                return this.array.join('-');
            }

            // 交换两个数的方法
            ArrayList.prototype.swap = function (m, n) {
                var temp = this.array[m];
                this.array[m] = this.array[n];
                this.array[n] = temp;
            }


            // 冒泡排序
            ArrayList.prototype.bubblesort = function () {
                for (var j = 0; j < this.array.length - 1; j++) { //一共只需比较length-1次
                    for (var i = 0; i < this.array.length; i++) {
                        if (this.array[i] > this.array[i + 1]) {
                            this.swap(i, i + 1);
                        }
                    }
                }
            }
        }

        // 测试类
        var list = new ArrayList();

        //插入元素
        list.insert(66);
        list.insert(88);
        list.insert(12);
        list.insert(87);
        list.insert(100);
        list.insert(5);
        list.insert(566);
        list.insert(23);
        // console.log(list.toString()); // 66-88-12-87-100-5-566-23

        //测试冒泡排序
        list.bubblesort();
        console.log(list.toString()); // 5-12-23-66-87-88-100-566
    </script>
</body>

</html>
```

冒泡排序的效率：

> 举例：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201118113436505.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 
> - 上面所讲的对于7个数据项，比较次数为：6 + 5 + 4 + 3 + 2 + 1;
> - 对于N个数据项，比较次数为：(N - 1) + (N - 2) + (N - 3) + ... + 1 = N * (N - 1) / 2；如果两次比较交换一次，那么平均交换次数为：N * (N - 1) / 4；
> - 使用大O表示法表示比较次数和交换次数分别为：O（ N * (N - 1) / 2）和O（ N * (N - 1) / 4），根据大O表示法的三条规则都化简为：**O（N^2）**;

2.选择排序

> 选择排序改进了冒泡排序：
> 
> - 将交换次数由O（N^2）减小到O（N）；
> - 但是比较次数依然是O（N^2）；

> 
> 
> 思路：
> 数据：66-88-12-87-100-5-566-23
> - 首先，令min = 0（min：用来记录索引的变量），记录第一个索引为0的位置，然后依次和后面的元素进行比较；
> - 索引为0的位置，值为66，小于索引为1所在的值，不进行任何操作，min仍为0；继续往后比较，发现66大于索引为2所在的值，则令min = 2；然后让索引为2所在的值和索引3的值进行比较，发现12小于87，不进行任何操作；同理，12小于100，不进行操作，此时min=2；直到12大于5，此时令min = 5；索引为5的值小于索引为6的值，不进行任何操作；同理。索引为5的值小于索引为7的值，不进行任何操作，min = 5。
> - 经过前面一轮的比较，我们已经找出了其中的最小值，即索引为5所在的值，接下来，执行this.swap(min, 0);让索引为5和索引为0的值进行交换，得到：5-88-12-87-100-66-566-23
> - 显然，只需要重复以上的循环操作，从88-12-87-100-66-566-23开始，再次找出其中的最小值，最后与88所在的值进行交换，便可以得到：5-12-88-87-100-66-566-23。
> - 如此看来，我们只需要加多一个外循环，使其一共循环length-1次，便可以完成选择排序。
> - 详细请看以下代码实现步骤。

代码实现：

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script>
        // 封装列表
        function ArrayList() {
            // 属性
            this.array = [];

            // 方法
            // insert：将数据插入到数组中的方法
            ArrayList.prototype.insert = function (item) {
                this.array.push(item);
            }

            ArrayList.prototype.toString = function () {
                return this.array.join('-');
            }

            // 交换两个数的方法
            ArrayList.prototype.swap = function (m, n) {
                var temp = this.array[m];
                this.array[m] = this.array[n];
                this.array[n] = temp;
            }



            // 选择排序
            ArrayList.prototype.selectionSort = function () {
                for (var j = 0; j < this.array.length - 1; j++) { //外层循环：一共循环length-1次
                    var min = j; // min：用来记录索引的变量
                    for (var i = min + 1; i < this.array.length; i++) {
                        if (this.array[min] > this.array[i]) {
                            min = i;
                        }
                    }
                    this.swap(min, j); // 每一轮找出最小的值之后，才进行交换
                }
            }
        }

        // 测试类
        var list = new ArrayList();

        //插入元素
        list.insert(66);
        list.insert(88);
        list.insert(12);
        list.insert(87);
        list.insert(100);
        list.insert(5);
        list.insert(566);
        list.insert(23);
        // console.log(list.toString()); // 66-88-12-87-100-5-566-23

        //测试选择排序
        list.selectionSort();
        console.log(list.toString()); // 5-12-23-66-87-88-100-566
    </script>
</body>

</html>
```

选择排序的效率：
> 
> - 选择排序的比较次数为：N * (N - 1) / 2，用大O表示法表示为：O（N^2）;
> - 选择排序的交换次数为：(N - 1) / 2，用大O表示法表示为：O（N）;
> - 所以选择排序的效率高于冒泡排序；

3.插入排序

> - 插入排序是简单排序中效率最高的一种排序。 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120204603184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 动态过程：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201120204827979.gif#pic_center)

代码实现：
```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script>
        // 封装列表
        function ArrayList() {
            // 属性
            this.array = [];

            // 方法
            // insert：将数据插入到数组中的方法
            ArrayList.prototype.insert = function (item) {
                this.array.push(item);
            }

            ArrayList.prototype.toString = function () {
                return this.array.join('-');
            }

            // 插入排序
            ArrayList.prototype.insertionSort = function () {
                // 外层循环：从第1个位置开始获取数据，向前面局部有序进行插入
                for (var i = 1; i < this.array.length; i++) {
                    // 内层循环：获取i位置的元素，和前面的元素依次进行比较
                    var temp = this.array[i];
                    while (this.array[i - 1] > temp && i > 0) {
                        this.array[i] = this.array[i - 1];
                        i--;
                    }
                    this.array[i] = temp;
                }
            }
        }

        // 测试类
        var list = new ArrayList();

        //插入元素
        list.insert(66);
        list.insert(88);
        list.insert(12);
        list.insert(87);
        list.insert(100);
        list.insert(5);
        list.insert(566);
        list.insert(23);
        // console.log(list.toString()); // 66-88-12-87-100-5-566-23

        //测试插入排序
        list.insertionSort();
        console.log(list.toString()); // 5-12-23-66-87-88-100-566
    </script>
</body>

</html>
```

插入排序的效率：

> - **比较次数**：第一趟时，需要的最大次数为1；第二次最大为2；以此类推，最后一趟最大为N-1次；所以，插入排序的总比较次数为N * (N - 1) / 2；但是，实际上每趟发现插入点之前，平均只有全体数据项的一半需要进行比较，所以比较次数为：N * (N - 1) / 4；
> 
> - **复制次数**：第一趟时，需要的最多复制次数为1；第二次最多复制次数2；以此类推，最后一趟最多为N-1次；所以，插入排序的总复制次数最多为N * (N - 1) / 2；平均次数为：N * (N - 1) / 4；
> 
>数据基本有序的情况：
> - 对于已经有序或基本有序的数据来说，插入排序要好很多；
> - 当数据有序的时候，while循环条件总是为假，所以他变成外层循环中一个简单地语句，执行N-1次。

4.希尔排序

> 希尔排序是插入排序的一种高效的改进版，效率比插入排序要高。
 
> 
> 希尔排序的历史背景：
> 
> - 希尔排序按其设计者希尔（Donald Shell）的名字命名，该算法由1959年公布；
> - 希尔算法首次突破了计算机界一直认为的**算法的时间复杂度都是O（N^2）**的大关，为了纪念该算法里程碑式的意义，用Shell来命名该算法；

> 
> 插入排序的问题：
> 
> - 假设一个很小的数据项在很靠近右端的位置上，这里本应该是较大的数据项的位置；
> - 将这个小数据项移动到左边的正确位置，所有的中间数据项都必须向右移动一位，这样效率非常低；
> - 如果通过某种方式，不需要一个个移动所有中间的数据项，就能把较小的数据项移到左边，那么这个算法的执行速度就会有很大的改进。

> 
> 希尔排序的实现思路：
> 
> - 希尔排序主要通过对数据进行分组实现快速排序；
> - 根据设定的增量（gap）将数据分为gap个组（组数等于gap），再在每个分组中进行局部排序；
> - 排序之后，减小增量，继续分组，再次进行局部排序，直到增量gap=1为止。随后只需进行微调就可完成数组的排序；

举例：

> - （1）排序之前的，储存10个数据的原始数组为：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122170411525.png#pic_center)
> - （2）设初始增量gap = length / 2 = 5，即数组被分为了5组，如图所示分别为：[8, 3]、[9, 5]、[1, 4]、[7, 6]、[2, 0]：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112217050158.png#pic_center)
> - （3）随后分别在每组中对数据进行局部排序，5组的顺序如图所示，变为：[3, 8]、[5, 9]、[1, 4]、[6, 7]、[0, 2]： ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112217054879.png#pic_center)
> - （4）然后缩小增量gap = 5 / 2 = 2，即数组被分为了2组，如图所示分别为：[3，1，0，9，7]、[5，6，8，4，2]： ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122170711302.png#pic_center)
> - （5）随后分别在每组中对数据进行局部排序，两组的顺序如图所示，变为：[0，1，3，7，9]、[2，4，5，6，8]： ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122170747765.png#pic_center)
> - （6）然后缩小增量gap = 2 / 1 = 1，即数组被分为了1组，如图所示为：[0，2，1，4，3，5，7，6，9，8]： ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112217082056.png#pic_center)
> - （7）最后只需要对该组数据进行插入排序即可完成整个数组的排序： ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122170847652.png#pic_center)
> 

> 增量的选择：
> 
> - 原稿中希尔建议的初始间距为N / 2，比如对于N = 100的数组，增量序列为：50，25，12，6，3，1，可以发现不能整除时向下取整。
> - Hibbard增量序列：增量序列算法为：2^k - 1，即1，3，5，7... ...等；这种情况的最坏复杂度为O(N^3/2)， 平均复杂度为O（N^5/4），但未被证明；
> - Sedgewcik增量序列： ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122171049616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 



代码实现（采用希尔排序原稿中建议的增量即N / 2）：

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script>
        // 封装列表
        function ArrayList() {
            // 属性
            this.array = [];

            // 方法
            // insert：将数据插入到数组中的方法
            ArrayList.prototype.insert = function (item) {
                this.array.push(item);
            }

            ArrayList.prototype.toString = function () {
                return this.array.join('-');
            }

            // 希尔排序
            ArrayList.prototype.shellSort = function () {
                // 1. 初始化增量
                var gap = Math.floor(this.array.length / 2); //向下取整
                // 2.while（将gap不断减小）
                while (gap >= 1) {
                    // 3.以gap为增量，进行分组，对分组进行插入排序
                    for (var i = gap; i < this.array.length; i++) {
                        var temp = this.array[i];
                        var j = i;
                        while (this.array[j - gap] > temp) {
                            this.array[j] = this.array[j - gap];
                            j -= gap;
                        }
                        // 4.将j位置的元素赋值temp
                        this.array[j] = temp;
                    }
                    gap = Math.floor(gap / 2);
                }
            }
        }

        // 测试类
        var list = new ArrayList();

        //插入元素
        list.insert(66);
        list.insert(88);
        list.insert(12);
        list.insert(87);
        list.insert(100);
        list.insert(5);
        list.insert(566);
        list.insert(23);
        // console.log(list.toString()); // 66-88-12-87-100-5-566-23

        //测试希尔排序
        list.shellSort();
        console.log(list.toString()); // 5-12-23-66-87-88-100-566
    </script>
</body>

</html>
```
希尔排序的效率：

> - 希尔排序的效率和增量有直接关系，即使使用原稿中的增量效率都高于简单排序。

5.快速排序

> 快速排序的介绍：
> 
> - 快速排序可以说是目前所有排序算法中，**最快**的一种排序算法。当然，没有任何一种算法是在任意情况下都是最优的。但是，大多数情况下快速排序是比较好的选择。
> 
> - 快速排序其实是**冒泡排序的升级版**；
> - 快速排序的核心思想是分而治之；
> 
> **快速排序的本质**：逐渐将每一个元素都转换成轴点元素。

举例：

> 有下面这样一组数据：13-81-92-43-65-31-57-26-75-0 需要进行排序。
> - 第一步：选出65（任意选择的，这里以65为例，这个65数据称为**枢纽**）；
> - 第二步：将所有比65小的都放在它的左边，比它大的数都放在它的右边；
> - 第三步：递归处理左边的数据（比如选择31处理），递归处理右边的数据（比如选择75）；
> - 第四步：排序完成。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122212536222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 
> 和冒泡排序的不同：
> 
> - 我们选择的65可以一次性将它放在最正确的位置，之后就不需要做任何移动；
> - 而冒泡排序即使已经找到最大值，也需要继续移动最大值，直到将它移动到最右边；

快速排序的枢纽：

> - **第一种方案**：直接选择第一个元素作为枢纽。但是，当第一个元素就是最小值的情况下，效率不高；
> - **第二种方案**：使用随机数。随机数本身十分消耗性能，不推荐；
> -  **第三种方案（优秀的解决方法）**：
> 		- a.取数列中index为头、中、尾的三个数据，如下图所示： ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122223117854.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 
> 		- b.按下标值取出的三个数据为：66，87，23，经排序后变为：23，66，87，取其中的中位数66作为枢纽： 	
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122223938691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70#pic_center)
> 
>	 - c.将center换到right-1的位置
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201122223811997.png#pic_center)



枢纽确定后接下来的操作：

> d.定义变量，用于记录当前找到的位置(指针)
> - var i = left; 
> - var j = right - 1;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128222223434.png)
> 
> 
> e.开始查找
> 
> - 从（++i）88的位置开始查找，当查找到比66大的数则停止查找；
> - 从（--j）5的位置开始查找，当查找到比66小的数则停止查找；
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112822231169.png)
> 
> f.满足条件，停止查找，接下来交换i和j两个数，如图所示：        
>                          ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128222443310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70)
> g.继续进行查找，直到 i >= j，循环条件结束
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128222805704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NDAzNzM0,size_16,color_FFFFFF,t_70)
> h.将枢纽放置在正确的位置： this.swap(i, right - 1);
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128222940220.png)
i.后面就是进行递归的操作了，枢纽左边和右边的数值都进行同样地操作----分而治之........................



快速排序代码实现：

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <script>
        // 封装列表
        function ArrayList() {
            // 属性
            this.array = [];

            // 方法
            // insert：将数据插入到数组中的方法
            ArrayList.prototype.insert = function (item) {
                this.array.push(item);
            }

            ArrayList.prototype.toString = function () {
                return this.array.join('-');
            }

            // 交换两个数的方法
            ArrayList.prototype.swap = function (m, n) {
                var temp = this.array[m];
                this.array[m] = this.array[n];
                this.array[n] = temp;
            }

            // 快速排序
            // 1.选择枢纽
            ArrayList.prototype.median = function (left, right) {
                // 1.1取出中间的位置
                var center = Math.floor((left + right) / 2);
                // 1.2判断大小，并且进行交换
                if (this.array[left] > this.array[center]) {
                    this.swap(left, center);
                }
                if (this.array[left] > this.array[right]) {
                    this.swap(left, right);
                }
                if (this.array[center] > this.array[right]) {
                    this.swap(center, right);
                }
                // 1.3将center换到right-1的位置
                this.swap(center, right - 1);

                // 1.4返回枢纽的数值
                return this.array[right - 1];
            }
            // 2.快速排序的实现
            ArrayList.prototype.quickSort = function () {
                this.quick(0, this.array.length - 1);
            }
            ArrayList.prototype.quick = function (left, right) {
                //结束条件
                if (left >= right) return;
                //获取枢纽
                var pivot = this.median(left, right);
                //定义变量，用于记录当前找到的位置(指针)
                var i = left;
                var j = right - 1;
                //开始进行交换
                while (i < j) {
                    while (this.array[++i] < pivot) {}
                    while (this.array[--j] > pivot) {}
                    if (i < j) {
                        this.swap(i, j);
                    } else {
                        break;
                    }
                }
                //将枢纽放置在正确的位置
                this.swap(i, right - 1);
                //分而治之
                this.quick(left, i - 1);
                this.quick(i + 1, right);
            }
        }

        // 测试类
        var list = new ArrayList();

        //插入元素
        list.insert(66);
        list.insert(88);
        list.insert(12);
        list.insert(87);
        list.insert(100);
        list.insert(5);
        list.insert(566);
        list.insert(23);
        console.log(list.toString()); // 66-88-12-87-100-5-566-23

        //测试快速排序
        list.quickSort();
        console.log(list.toString()); // 5-12-23-66-87-88-100-566
    </script>
</body>

</html>
```
快速排序的效率：

> - 快速排序最坏情况下的效率：每次选择的枢纽都是最左边或最右边的数据，此时效率等同于冒泡排序，时间复杂度为O（n^2）。可根据不同的枢纽选择避免这一情况；
> - 快速排序的平均效率：为O（N*logN），虽然其他算法效率也可达到O（N*logN），但是其中快速排序是最好的。



**【注】：本博客灵感来源于以下视频：
https://www.bilibili.com/video/BV1x7411L7Q7?p=1**





