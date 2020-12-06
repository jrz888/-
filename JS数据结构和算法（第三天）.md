## 一、集合
**1.集合特点**

> - 集合通常是由一组无序的、不能重复的元素构成。
> 
> - 数学中常指的集合中的元素是可以重复的，但是计算机中集合的元素不能重复
> - 集合是特殊的数组。
> 
> 【注】：特殊之处在于里面的元素没有顺序，也不能重复。 没有顺序意味着不能通过下标值进行访问，不能重复意味着相同的对象在集合中只会存在一份。

**2.封装集合**

> ES6 中的 Set 就是一个集合类，这里我们重新封装一个 Set 类，了解集合的底层实现。

**3.集合常见的操作**

> - add(value) 向集合添加一个新的项。
> - remove(value) 从集合移除一个值。
> - has(value) 如果值在集合中，返回 true，否则返回 false。
> - clear() 移除集合中的所有项。
> - size() 返回集合所包含元素的数量。与数组的 length 属性类似。
> - values() 返回一个包含集合中所有值的数组。 
> 还有其他的方法，用的不多，这里不做封装。

封装集合：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
		function Set(){
			// 属性
			this.items = {};

			// 方法
			Set.prototype.add = function(value){
				if(this.has(value)){
					return false;
				}
				this.items[value] = value;
				return true;
			}

			Set.prototype.has = function(value){
				return this.items.hasOwnProperty(value);
			}

			Set.prototype.remove = function(value){
				if(!this.has(value)){
					return false;
				}
				delete this.items[value];
				return true;
			}

			Set.prototype.clear = function(){
				this.items = {};
			}

			// 获取集合的长度
			Set.prototype.size = function(){
				return Object.keys(this.items).length;
			}
			
			// 获取集合中所有的值
			Set.prototype.values = function(){
				return Object.keys(this.items);
			}
		}

		// 测试代码
		var set = new Set();
		alert(set.add('a')); //true
		alert(set.add('b')); //true
		alert(set.add('c')); //true
		alert(set.add('a')); //false
		alert(set.values()); //-->a,b,c

		alert(set.remove('b')); //true
		alert(set.remove('b')); //false

		alert(set.has('a')); //true
		alert(set.size()); //2
		set.clear();
		alert(set.size()); //0
	</script>
</body>
</html>
```

**4.集合之间的操作**

> - 并集：对于给定的两个集合，返回一个包含两个集合中所有元素的新集合。
> - 交集：对于给定的两个集合，返回一个包含两个集合中共有元素的新集合。
> - 差集：对于给定的两个集合，返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合。
> - 子集：验证一个给定集合是否是另一个集合的子集。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201107175356957.png#pic_center)


并集的实现：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
	function Set(){
			// 属性
			this.items = {};

			// 方法
			Set.prototype.add = function(value){
				if(this.has(value)){
					return false;
				}
				this.items[value] = value;
				return true;
			}

			Set.prototype.values = function(){
				return Object.keys(this.items);
			}

			Set.prototype.has = function(value){
				return this.items.hasOwnProperty(value);
			}

			Set.prototype.union = function(otherSet){
				// 1.创建一个新的集合
				var unionSet = new Set();
				// 2.将A中所有的元素添加到新的集合中(这里的this是A集合)
				var valueA = this.values();
				for(var i = 0; i < valueA.length; i++){
					unionSet.add(valueA[i]);
				}
				// 3.取出B集合中的所有元素，判断是否需要加到新的集合
				var valueB = otherSet.values();
				for(var i = 0; i < valueB.length; i++){
					unionSet.add(valueB[i]);//add方法已经做去重处理，可以直接往大的集合添加
				}
				return unionSet;
			}
	}


	var setA = new Set();
	setA.add('aaa');
	setA.add('bbb');
	setA.add('ccc');
	var setB = new Set();
	setB.add('aaa');
	setB.add('qqq');
	setB.add('www');

	var unionSet = setA.union(setB);
	alert(unionSet.values()); //-->aaa,bbb,ccc,qqq,www
 
	</script>
</body>
</html>
```
交集的实现：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
	function Set(){

			this.items = {};

			Set.prototype.add = function(value){
				if(this.has(value)){
					return false;
				}
				this.items[value] = value;
				return true;
			}

			// 获取集合中所有的值
			Set.prototype.values = function(){
				return Object.keys(this.items);
			}

			Set.prototype.has = function(value){
				return this.items.hasOwnProperty(value);
			}

			Set.prototype.intersection = function(otherSet){
				// 1.创建一个新的集合
				var interSectionSet = new Set();
				// 2.从A中取出一个个元素，判断是否同时存在于集合B中，存在即放入新的集合中
				var valueA = this.values();
				for(var i = 0; i < valueA.length; i++){
					if(otherSet.has(valueA[i])){
						interSectionSet.add(valueA[i]);
					}				
				}
				return interSectionSet;
			}

	}


	var setA = new Set();
	setA.add('aaa');
	setA.add('bbb');
	setA.add('ccc');
	var setB = new Set();
	setB.add('aaa');
	setB.add('qqq');
	setB.add('www');

	alert(setA.values()); //-->aaa,bbb,ccc
	alert(setB.values()); //-->aaa,qqq,www

	var interSectionSet = setA.intersection(setB);
	alert(interSectionSet.values()); //-->aaa
 
	</script>
</body>
</html>
```
差集的实现（与交集正好相反）：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
	function Set(){

			this.items = {};

			Set.prototype.add = function(value){
				if(this.has(value)){
					return false;
				}
				this.items[value] = value;
				return true;
			}

			// 获取集合中所有的值
			Set.prototype.values = function(){
				return Object.keys(this.items);
			}

			Set.prototype.has = function(value){
				return this.items.hasOwnProperty(value);
			}

			Set.prototype.difference = function(otherSet){
				// 1.创建一个新的集合
				var differentSet = new Set();
				// 2.从A中取出一个个元素，判断是否同时存在于集合B中，不存在即放入新的集合中
				var valueA = this.values();
				for(var i = 0; i < valueA.length; i++){
					if(!otherSet.has(valueA[i])){
						differentSet.add(valueA[i]);
					}				
				}
				return differentSet;
			}

	}


	var setA = new Set();
	setA.add('aaa');
	setA.add('bbb');
	setA.add('ccc');
	var setB = new Set();
	setB.add('aaa');
	setB.add('qqq');
	setB.add('www');

	alert(setA.values()); //-->aaa,bbb,ccc
	alert(setB.values()); //-->aaa,qqq,www

	var differentSet = setA.difference(setB);
	alert(differentSet.values()); //-->bbb,ccc
 
	</script>
</body>
</html>
```

子集的实现：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script>
	function Set(){

			this.items = {};

			Set.prototype.add = function(value){
				if(this.has(value)){
					return false;
				}
				this.items[value] = value;
				return true;
			}

			// 获取集合中所有的值
			Set.prototype.values = function(){
				return Object.keys(this.items);
			}

			Set.prototype.has = function(value){
				return this.items.hasOwnProperty(value);
			}

			Set.prototype.subSet = function(otherSet){
				// 1.遍历集合A中的所有元素，若发现集合A中的元素，在集合B中不存在，则返回false
				// 2.弱如果遍历完A的整个元素，依然没有返回false，那么则返回true
				var valueA = this.values();
				for(var i = 0; i < valueA.length; i++){
					if(!otherSet.has(valueA[i])){
						return false;
					}				
				}
				return true;
			}
	}


	var setA = new Set();
	setA.add('aaa');
	setA.add('bbb');
	setA.add('ccc');
	var setB = new Set();
	setB.add('aaa');
	setB.add('bbb');
	setB.add('ccc');
	setB.add('ddd');
	setB.add('www');


	alert(setA.values()); //-->aaa,bbb,ccc
	alert(setB.values()); //-->aaa,bbb,ccc,ddd,www

	console.log(setA.subSet(setB)); //-->true(说明A为B的子集)

 
	</script>
</body>
</html>
```

## 二、字典
**字典特点**

> 字典存储的是键值对，主要特点是一一对应。比如保存一个人的信息
> - 数组形式：[19，‘Tom’，1.65]，可通过下标值取出信息。
> - 字典形式：{"age"：19，"name"："Tom"，"height"：165}，可以通过 key 取出 value。 此外，在字典中 key 是不能重复且无序的，而 Value 可以重复。

**字典和映射的关系**

> - 有些编程语言中称这种映射关系为字典，如 Swift 中的 Dictonary，Python 中的 dict。
> - 有些编程语言中称这种映射关系为 Map，比如 Java 中的 HashMap 和 TreeMap 等。

**字典常见的操作**

> - set(key,value) 向字典中添加新元素。
> - remove(key) 通过使用键值来从字典中移除键值对应的数据值。
> - has(key) 如果某个键值存在于这个字典中，则返回 true，反之则返回 false。
> - get(key) 通过键值查找特定的数值并返回。
> - clear() 将这个字典中的所有元素全部删除。
> - size() 返回字典所包含元素的数量。与数组的 length 属性类似。
> - keys() 将字典所包含的所有键名以数组形式返回。
> - values() 将字典所包含的所有数值以数组形式返回。

