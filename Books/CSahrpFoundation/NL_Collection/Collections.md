title: Collections
date: 2017-11-4 11:28:00
tag: CSharp

---

* 如何定义和使用集合
* 使用不同类型的集合
* 如何比较类型，如何使用is运算符
* 如何比较值，如何重载运算符
* 如何定义和转换
* 如何使用as运算符

<!--more-->

## 集合 ##

* 名称空间：Using System.Collections;
	* 提供基本的集合功能的接口：
		* IEnumerable
		* ICollection(Inherit IEnumerable)
		* IList(Inherit IEnumerable and ICollection)
		* IDictionary(Inherit IEnumerable and ICollection)
* 数组名称空间：Using System.Array; 
* 实现Foreach效果
	1. 继承IEnumerable接口，调用GetEnumerator()函数，返回一个迭代集合的枚举器
* Array类中，为长度属性是length
* Colletion集合中的类中，为长度属性是count，继承自ICollection接口
	* RemoveAt() Remove()  AddRange()

### 定义集合 ###

* 引用已定义好的类：System.Colletions.CollectionBase类

#### 强类型集合 ####

* ColletionBase类提供两个受保护的属性
	* List：通过IList接口访问项
	* InnerList：用于存储项的ArrayList对象
* 索引符（Indexer）是一种特殊类型的属性

`

	public Animal this[int index]
	{
		get{return List[index] as Animal;}
		set{List[index] as Animal = value;}
	}	

#### 键控集合和IDictionary ####

* 基类：DictionaryBase

`

	class Animals : DictionaryBase
	    {
	        public void Add(string identifyID,Animals animal)
	        {
	            Dictionary.Add(identifyID, animal);
	        }
	        
	        public Animals this[string identifyID]
	        {
	            get { return Dictionary[identifyID] as Animals; }
	            set { Dictionary[identifyID] = value; }
	        }
	    }

* 迭代器foreach的实现原理：
	1. 调用GetEnumerator(),返回值IEnumerator类型
	2. 调用IEnumerator类型的MoveNext()方法
	3. 如果MoveNext()返回为True，则使用当前迭代器的current属性，返回当前的值
	4. 重复前两步，知道MoveNex()返回false位置，此时循环结束
* 迭代器的定义：迭代器只是一个代码块，按照顺序提供foreach中要循环使用的所有值

* 如果要迭代一个类，可使用方法GetEnumerator(),其返回值类型为IEnumerator;
* 如果要迭代一个类成员，例如一个方法，则使用IEnumerable

## 重载运算符 ##

`

	class MyClassA
    {
        public int Val
        {
            get;
            set;
        }

        public static MyClassC operator +(MyClassA val1, MyClassB val2)
        {
            MyClassC returnVal = new MyClassC();
            returnVal.Val = val1.Val + val2.Val;
            return returnVal;
        }
    }

    class MyClassB
    {
        public int Val
        {
            get;
            set;
        }
    }

    class MyClassC
    {
        public int Val
        {
            get;
            set;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            MyClassA a = new MyClassA { Val = 1 };
            MyClassB b = new MyClassB { Val = 2 };
            Console.WriteLine((a + b).Val);



            Console.Read();
        }
    }

* 一元运算符：
	* +
	* -
	* !
	* ~
	* ++
	* --
	* true
	* false
* 二元运算符
	* +
	* -
	* *
	* /
	* %
	* &
	* |
	* ^
	* <<
	* >>
* 三元运算符
	* ==
	* !=
	* <
	* >
	* <=
	* >=

* 不能重载的运算符：
	* +=
	* &&
	* ||

## 比较（Compare） ##

* 名称空间：Using System.Collections;
* IComparable：在比较的对象的类中实现，可以比较该对象和另一个对象
* IComparer：在一个单独的类中实现，可以比较任意两个对象
* 快速比较方法：Comparer.Default.Compare(obj1,obj2);
	* 注意的事项：
		1. 检查传送的对象是否支持IComparable
		2. 允许使用null值，他表示“小于”其他任意对象
		3. 若不区分大小写的方式，则调用CaseInsensitveComparer类
* 对集合进行排序

`

	class Person : IComparable
    {
        public string Name { get; set; }
        public int Age { get; set; }

        public int CompareTo(Object other)
        {
            if (other.GetType() == typeof(Person))
            {
                return Age - ((Person)other).Age;
            }
            else
            {
                throw new ArgumentException("Inout Type is not typeof(Person).");
            }
        }
    }

    class PersonCompareName : IComparer
    {
        public static IComparer Default = new PersonCompareName();
        public int Compare(object x, object y)
        {
            if (x is Person && y is Person)
            {
                return (x as Person).Name.CompareTo((y as Person).Name);
            }
            else
            {
                throw new ArgumentException("```");
            }
        }
    }




    class Program
    {
        static void Main(string[] args)
        {
            ArrayList list = new ArrayList
            {
                new Person { Name="name1",Age=10},
                new Person {Name="name2",Age=20 },
                new Person { Name="name",Age=30},
                new Person { Name="name4",Age=5}
            };

            foreach (Person person in list)
            {
                Console.WriteLine($"This Person's name is {person.Name},And Age is {person.Age}.");
                Console.WriteLine("\n");
            }

            Console.WriteLine("\n--------------------\nUpside is sort list:");

            list.Sort();

            foreach (Person person in list)
            {
                Console.WriteLine($"This Person's name is {person.Name},And Age is {person.Age}.");
                Console.WriteLine("\n");
            }

            Console.WriteLine("\n--------------------\nsort by name is sort list:");

            list.Sort(PersonCompareName.Default);

            foreach (Person person in list)
            {
                Console.WriteLine($"This Person's name is {person.Name},And Age is {person.Age}.");
                Console.WriteLine("\n");
            }


            Console.Read();
        }
    }

## 转换 ##

* int可以隐式转换成long或double；采用相同的方法还可以把类型进行显性和隐性的转换
* 隐式转换(implicit)：小到大；int->long
* 显示转换(explicit)：大到小;long->int

`

	class myClassA
    {
        public int val;
        public static implicit operator myClassB(myClassA a)
        {
            myClassB returnVal = new myClassB();
            returnVal.val = a.val;
            return returnVal;
        }

    }

    class myClassB
    {
        public double val;

        public static explicit operator myClassA(myClassB b)
        {
            myClassA returnVal = new myClassA();
            returnVal.val = (int)b.val;
            return returnVal;
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            myClassA a = new myClassA();
            a.val = 10;
            myClassB b = new myClassB();

            b = a;

            Console.WriteLine(b.val);


            Console.Read();
        }
    }