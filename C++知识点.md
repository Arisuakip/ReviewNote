<h2>C++知识点</h2>

<h3>指针和引用</h3>

___

**引用：**相当于给对象起别名，变量名用&来声明

+ 引用必须被初始化
+ 引用不能与字面值或表达式结果绑定，只能绑定对象
+ 引用绑定一个对
+ 象后，不能重写绑定其他对象
+ 对引用的操作等价于对对象的操作
+ 引用本身不是一个对象，因此不能被指针指向

**指针：**实现对其他对象的间接访问，变量名用*来声明

+ 指针本身是一个对象，允许对指针赋值和拷贝
+ 指针在定义时不强制初始化
+ 给指针赋值，可用**取地址符&**，例如：`int *p = &a`
+ 利用指针访问对象，可用**解引用符***，例如：`cout<<*p<<endl;`
+ **指针初始化**
  + 当指针初始化的指为另一个指针时，该指针会直接指向另一个指针指向的对象，而非指针的指针
  + 例如：`int a = 1024;int *p = &a; int *p1 = p;`此时p1指向a
  + 指针初始化后，单纯变量名代表地址，而*变量名代表地址指向的对象（带\*改的是值，不带改的是地址）
+ 可以通过指针修改指向对象的值
+ `int a = 3;int *p = &a; *p=5;`

+ **空指针：**c++11引入，使用nullptr。NULL也可以，但不推荐
+ 指针的==：三种情况下指针相同：均为空、均指向同一个对象、都指向了同一对象的下一个地址
+ void*：是一种特殊指针类型，可以存放任意类型对象的地址，但不能拿来操作
+ **指向指针的指针：**指针也是一个对象，因此有自己的地址，也可以被指针指向，通过*的个数来区分指针的级别
  + 例如：`int a = 1024; int *p = &a;int **pp = &a`
  + 对于指针的指针，解引用会得到一个指针，再次解引用才得到对象：即**p才是1024
+ **指向指针的引用：**`int a =1024;int *p;int *&r = p;`r是对指针的引用
  + 从右往左分析优先度，*&r，先是一个引用，然后是一个指针

**const限定符**

+ **常量**

+ const定义的变量为常量，不允许再修改，因此需要初始化好

+ 默认下，cosnt仅在文件内部有效，想文件使用，需要在定义和声明处，加extern关键字

+ **const的引用**

  + 可以把引用绑定到const对象上，称为**对常量的引用**
  + `const int a = 1024; const int &r = a;`
  + 此时不能再修改r了
  + 非常量引用不能引用常量，如：`int &r2 = a;//错误！`
  + 引用的类型必须与所引用的对象保持一致
    + 例外：在初始化常量引用时，允许用任意表达式作为初始值，只要该表达式能转换成引用的类型即可
    + `double val = 3.14;const int &ri = val;`
    + 当引用ri不是常量引用时，c++把这类行为归为非法
  + 对const的引用可以引用一个非const对象，但不能通过引用改变对象的值了
    + `int i = 5; const int &r = i;`合法
    + `r = 1;`不合法

+ **指针和const**

  + **指向常量的指针：**
    + 可以令指针指向一个常量，但**不能修改用于改变其所指对象的值**
    + 普通指针不能指向常量，指向常量必须用指向常量的指针
    + 指向常量的指针的类型必须与其所指的对象一致，但不一定是常量，但对不会修改指向的对象

  + ```c++
    const double pi = 3.14;
    double *ptr = &pi;   //不合法，因为ptr是个普通指针
    const double *cptr = &pi; //合法，cptr为指向常量的指针
    *cptr = 1; //非法，不能修改
    //指向常量的指针即可以指向常量，也可以不指向常量
    double dval = 3.33;
    cptr = &dval; //合法
    ```

  + **常量指针：**

    + 指针是对象，因 量，常量指针必须初始化，不允许再修改，把*放在const之前用来表示指针是一个常量
    + `int num = 0; int *const p= &num;  `
    + p1是一个指向常量的常量指针
    + `const double pi = 3.14; const double *const p1 = &pi;`
    + 常量指针允许通过指针修改指向对象的值，但不允许修改指针指向。

  + **顶层const和底层const**

    + 指针本身是个常量：顶层const
    + 指针指向的对象是一个常量：底层const
    + 一个指针可以即是顶层const又是底层const，如上小节的p1
    + 一般化：
      + 顶层const可以表示任意的对象是常量，适用于任何数据类型：const int...
      + 底层const与指针和引用等复合类型的基本类型有关
    + 对象拷贝操作中：顶层const的拷贝不受限制
    + 底层const的拷贝：拷入和拷出的对象对必须具有**相同的底层const资格**，或者两个对象的数据类型必须能够转换，非常量可以转换为常量，反之不行；

  + **常量表达式**

    + 常量表达式指：值不会改变，在编译过程中就能得到计算结果的表达式
    + 字面值属于常量表达式
    + c++11，新规定：可以将变量声明为**constexpr**类型，来由编译器验证表达式是否为常量表达式

  + **字面值类型**

    + 算数类型，引用，指针等，都属于字面值类型；自定义类，IO库，string等不属于字面值类型
    + 特点：值显而易见，容易得到

  + **指针和constexpr**

    + 如果constexpr中声明了一个指针，限定符就仅对指针有效，与指针所指的对象无关

    + ```cpp
      const int *p = nullptr; //指向常量的指针
      constexpr int *q = nullprt;//常量指针
      ```

<h3>处理类型</h3>

___

**类型别名**：让复杂类型的名字变得简单明了

+ 方法一语法：**typedef**

+ 例：`typedef double wages` 代表wages是double的同义词

+ 例：`typedef wages base,*p;`代表base是double的同义词，p是double*的同义词

+ 方法二语法：**using**

+ 新标准规定，可以用**别名声明**来定义类型的别名

+ 例如：`using SI = Sales_item;`代表SI是Sales_item的同义词

+ **指代复合类型的类型别名**

  + 如果某个类型别名指代的是复合类型或常量，会有不合理效果

  + 例如`typedef char *pstring;`

  + 此时等价于pstring 将作为 char*的别名,此时的含义如下，注意此时不能单纯替换了！

  + ```c++
    const pstring cstr = 0; //cstr 是一个指向char的常量指针
    consr pstring *ps; //ps是一个“指向char的常量指针”的指针
    //注意如果单纯替换，则会发生错误
    //例如将第一句替换为
    const char *cstr = 0;
    //就会变成cstr是指向char常量的指针
    
    ```

**auto类型说明符**

+ C++11新标准引入auto类型说明符，能让编译器自动分析表达式所属类型
  + 一行auto必须是同类型的。
  + auto会忽略顶层const特性，但会保留底层const，如果需要保留顶层const特性，需要使用const关键字
  + 可以将引用类型设置为auto，但常量引用依然要const

**decltype**

+ C++11新标准引入decltype类型说明符，推断类型但不用表达式的值初始化。

  + `decltpye(f()) sum = x;`sum的类型就是函数f的返回类型

  + 与auto不同：如果decltype使用的是一个变量，则decltype返回该变量的类型（包括顶层const和引用）

  + ```c++
    const int ci = 0,&cj = ci;
    decltype(ci) x = 0; //x是 const int
    decltype(cj) y = x; //y是 const int&
    ```

  + 如果decltype使用的表达式不是一个变量，则decltype返回表达式结果对应的类型。

  + 如果表达式的内容是解引用操作，则decltype将得到引用类型。

  + ```c++
    int i = 42 ,*p = &i, &r = i;
    decltype(r+0) b; //因为r是引用，但r+0结果是int，所以b也是int
    decltype(*p) c; //因为*p是解引用操作，所以得到的结果为int&， 必须初始化，所以不合法
    ```

  + 当变量名加上一对括号，不会得到变量类型，而会得到引用类型。

**类关键字**：**struct**，下文定义一个**结构体**

```c++
#ifndef SALES_DATA_H //头文件保护符
#define SALES_DATA_H
#include<iostream>
#include<string>
struct Sales_data{
    //数据成员	
	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0;
};
#endif
```

<h3

<h3>字符串、向量和数组</h3>

___

**命名空间的using声明**

+ `using namespace::name;`	
  + `example:using std::cin；`

**标准库string**

+ 表示可变长的字符序列

+ **初始化方法**

+ ```c++
  string s1;           //s1为空字符串
  string s2 = s1;		//s2是s1的副本
  string s3 = "diana";	//s3是该字符串字面值的副本  拷贝初始化
  string s4(10,'c')	//s4是cccccccccc  // 直接初始化
  ```

+ **string的操作**

+ ```
  string s;
  os<<s;将s写到输出流  cout<<s;
  is>>s;将s写到输入流  cin>>s; //自动忽略开头的空白，从第一个字符读起，直到遇到下一处空白
  s.empty() //为空返回true
  s.size() //返回字符个数 返回的是一个string.size_type类型的值，这是个无符号数
  s[n] //返回第n个字符的引用
  s1+s2 //返回s1和s2连接结果  两个至少有一个是string对象，不能2个字面值相加
  "hello" 是字面值，并不是string
  s1==s2 s1!=s2 //字符一样就相等，大小写敏感
  s1 += s2 //把s2追加到s1后面
  <,<=,>,>= //按字典序比大小
     
  ```

+ **字符判断：cctype头文件**

+ ```
  isalnum(c) //是否是字母或数字
  isalpha(c) //是否是字母
  isdigit(c) //是否是数字
  ispunct(c) //是否是标点符号
  islower(c) //是否是小写
  isupper(c) //是否是大写
  tolower(c) //大写转小写
  toupper(c) //小写转大写
  ```

+ c++11新特性**范围for循环**（增强for循环）

+ ```c++
  string str("some string")
  for(auto c:str) //取出str中的每一个字符
      cout<<c<<endl;
  //如果想改变里面的值，必须写引用
  for(auto &c:str)
      c = toupper(c);
  ```

**标准库vector**：可变数组

+ `#include <vecotr>`

+ vector是一个类模板，声明时提供vector内存放对象的类型

+ ```
  vector<int> ivec;
  vector<Sales_item> Sales_ivec;
  vector<vector<int>> vvec; //旧编译器要写成 vector<vector<int> >;
  
  //使用数组首尾指针初始化vector对象
  int arr[] = {0,1,2,3,4,5,6};
  vector<int> v(begin(arr),end(err))；
  
  //初始化vector对象的方法
  vector<T> v1;
  vector<T> v2(v1); //v2中包含v1所有的副本
  vector<T> v2 = v1; //与上等价
  vector<T> v3(n,val) //包含n个重复元素val
  vector<T> v4(n); //包含n个初始化的对象
  vector<T> v5{a,b,c} //包含初始值为abc三个元素 {}列表初始化
  vector<T> v5 = {a,b,c} //等价
  ```
  
+ vector常用方法

+ ```
  v.push_bacK(val) 添加元素
  v.empty() 判空
  v.size() 大小
  v.pop_back()删除最后一个
  v.remove(val) //删除容器中和指定值元素相等的元素 大小不变
  v.erase(idx) //删除指定下标元素 size-1
  v.clear() //清空
  ```

**迭代器**

+ 迭代器有自己的类型，同时有自己的成员，其中begin成员返回指向第一个元素的迭代器，end返回指向尾元素的下一个元素的迭代器（没啥用 ）

+ `vector<int> v; auto b = v.begin(),auot e = v.end()`

+ 迭代器的类型是？：例如vector是vector<int>::iteraotr v; string是

+ 迭代器运算

+ ```
  *iter 返回迭代器iter所指元素的引用
  修改值：*iter = xxxx
  iter->mem  解引用iter，并获取该元素名为mem的成员，等价于(*iter).mem
  ++iter 指下一个
  --iter 指上一个
  iter + n 移动n个元素
  iter1 == iter2 判断相等（指向同一个元素/都是尾后迭代器）
  iter != iter2 判断不等 
  iter1 - iter2 两个迭代器直接相差的距离，必须指向同一个容器
  ```

+ 对于常量，返回常量迭代器，常量迭代器只能读取不能修改，可以用cbegin()和cend()直接返回常量迭代器

+ 箭头运算符：->，对于迭代器

+ ```
  auto it = v.begin();
  访问元素中的empty方法时，
  应写成(*it).empty();
  而非it.empty() //错误 会先去找it的empty方法
  为简化，c定义运算符:->
  可写成it->empty(); //与第一个相同
  ```

+ 对容器容量的操作，如push_back()，会使迭代器失效！

**数组**

+ 数组的维度必须是常量表达式

+ ```c++
  constexpr unsigned 	sz = 42;
  int arr[10]; //10个整数的数组
  int *parr[sz]; //42个整数指针的数组
  int arr1[] = {1,2,3};
  int arr2[5] = {1,2,3} //等价于{1,2,3,0,0}
  int arr3[2] = {0,1,2} //错误！
  //字符数组可以用字面值赋值
  char a3[] = "hello";//注意不能超过长度
  char a4[6] = "Daniel" //虽然长度相等，但结尾还包含个\0,所以不合法
  ```

+ 正常情况下不允许数组拷贝和赋值

+ 复杂数组声明

+ ```
  int *ptrs[10];    //10个整形指针的数组
  int &refs[10] = .. //错误！，不存在引用的数组
  
  int (*Parray)[10] = &arr; //Parray指向一个10个整数的数组
  int (&Rarray)[10] = arr; //Rarray引用一个10个整数的数组
  
  ```

+ **数组和指针**

+ c++中，使用数组时，编译器一般会把其替换为一个指向数组首元素的指针

+ ```
  string nums[] = {"one","two","three"};
  string *p = nums; //等价于 p = &nums[0];
  因此auto ia(nums) 会获得一个指针类型；
  //但 decltype()时例外，decltype会正常返回一个数组
  ```

+ **指针也是迭代器：**迭代器支持的运算，数组指针全部支持

+ c++11新标准：引入**begin()**和**end()**函数，方便获得首尾指针

+ ```c++
  int a1[] = {1,2,3,4,5}
  int *b = begin(a1); //指向首元素的指针
  int *e = end(a1); //指向尾元素的指针
  ```

+ **下标和指针**

+ 对数组进行下标运算，其实就是对指向数组元素的指针进行下标运算，因此则有

+ ```
  int ia[] = {1,2,3,4,5,6};
  int *p = ia;
  int i = *(p+2);// 等价于i = ia[2]
  int *p = &ai[2]; //p指向索引为2的元素	
  int j = p[1];  //等价于ia[3];
  int k = p[-2]; //等价于ia[0]
  ```

+ **C风格字符串**

+ 包含在cstring头文件中

+ 方法如下:

+ ```
  strlen(s); //长度
  strcmp(s1,s2) //比较s1和s2
  strcat(s1,s2) //s2加到s1后面
  strcpy(s1,s2) //s2拷贝给s1
  ```

+ **多维数组：数组的数组**

+ ```
  int arr[3][4];
  int arr1[3][4] = {
  	{0,1,2,3},
  	{4,5,6,7},
  	{8,9,10,11}
  };
  int arr2[3][4] = {1,2,3,4} //只初始化全4个
  //二维数组增强for遍历
  int ia[a][b];
  for(auto &row:ia) //如果不写成引用，会被解析成指向数组首元素的指针，下一个for就不合法了
  	for(auto col:row) 
  		cout<<col<<endl;
  for(int (&row)[4]:arr) //不用auto
       for(int col:row)
          cout<<col<<endl;
  ```

+ **多维数组和指针**

+ 多维数组是数组的数组，所以由多维数组名转换成的指针实际上指向第一个内层数组的指针

+ ```
  int ia[3][4];
  int (*p)[4] = ia; //p指向一个含有4个元素的数组
  int *p[4]; //这个意思是有4个整型指针的数组
  p = &ia[2]; //p指向ia中的第三行
  ```

+ **类型别名简化多维数组指针**

+ ```
  //声明别名
  1：using int_array = int[4];
  2:typedef int int_array[4];
  此时的增强for
  for(int_array *p = ia)
  ```

+ 遍历多维数组的几种方式：

+ ```c++
  int arr[3][4] = {1,2,3,4,5,6,7,8,9,10,11,12};
  //迭代器
  for(auto i=begin(arr);i != end(arr);i++)
       for(auto j= begin(*i);j != end(*i);j++)
           cout<<*j<<endl;
  //范围for循环
  for(auto &row:arr)
      for(auto col:row)
          cout<<col<<endl;
  //范围for 不用auto
  for(int (&r)[4]:arr)
      for(int col:row)
          cout<<col<<endl;
  //指针
  for(int (*p)[4]=arr;p<arr+3;p++)
       for(int *j= *p;j<*p+4;j++)
           cout<<*j<<endl;
  //指针auto
  for(auto *p = arr;p<arr+3;p++)
      for(auto *j= *p;j<*p+4;j++)
        	cout<<*j<<endl;
  //下标 比较简单	
  ```

**指向数组的指针**

```
声明一个指向数组的指针;
int arr[3]{1,2,3};
int (*p)[3] = &arr; //p指向数组arr
此时
*p 是 数组——首元素的地址
**p 是 数组首元素
```



<h3>表达式</h3>

___

**取余和除法符号问题**

+ 除法：符号相同为正，相反为负，一律向0取整，直接抹掉小数部分
+ 取余：m%(-n)  = m % n    (-m) % n = -(m%n);   -m % -n = -(m%n)
  + 21 % (-5) = 1; (-21) % 5 = -1;(-21) % (-5) = -1;

**记得 && 和 || 存在短路求值的问题**

**赋值运算符满足右结合律**

+ a = b = 0，b=0是赋值运算，返回左侧运算对象，将b=0的运算结果作为右值
+ 对于多重赋值语句的每一个对象，他的类型与右边对象类型相同，或可以右边对象转换来

**复合赋值运算符**

+ a += b 和 a = a+b；+=左值只计算一次，而后者左值要计算两次

**递增递减**

+ ++和-- 的优先级高于解引用 所以：*vect++ = *(vect++)；
+ 不能同时左边出现某个值，右边还对这个值进行了修改：如，\*beg = toupper(*beg++)，能运行，但结果会出错。

**冒号表达式**

+ `contion?a:b `：满足条件返回a，否则返回b。
+ 可以嵌套：`conditon1?a:contion2?b:c`
+ ? 的优先级很低，需要加括号

**sizeof运算符**

+ sizeof 返回一条表达式或一个类型名所占的字节数，满足右结合律，所得值是size_t类型的常量表达式
+ size (*tpye*)
+ size *expr*     返回表达式结果类型的大小
+ 对char类型返回1
+ 对引用类型，返回被引用对象所占空间大小
+ 对指针，返回指针本身所占空间
+ 对解引用指针，返回指针指向对象所占空间大小
+ 对数组，返回整个数组所占空间大小
+ 对string或vector，只返回该类型固定部分的大小

**逗号运算符**

+ 按从左向右的顺序依次求值，以右侧值为准。
+ 常用于for循环：`for(vecotr<int>::size_type ix = 0;ix !=ivec.size();++ix,cnt--)`

**类型转换**

+ 隐式类型转换
  + 表达式中，比int小的类型，优先转换为int类型
  + 条件中，非布尔值转布尔值
  + 初始化中，初始值转换为变量的类型
  + 赋值中，右侧运算对象转换为左侧对象类型
  + 算术或关系运算，需要转成同一种类型
  + 函数调用时
  
+ 算术转换
  + **整型提升：**小整数类型，如bool,char,signed char,unsigned char,short,unsigned short等，只要其所有可能的值能放在int里，就会被提升为int类型；较大的char类型，提升成int，unsigned int,long,long long等中最小的类型
  + **无符号类型：**
    + 如果两个都是有符号或者无符号，小类型转换为大类型
    + 如果一个有符号一个无符号
      + 无符号类型大小**不小于**等于带符号类型，带符号的转成无符号的：
        + unsigned int 和int ，int转为unsigned int	
      + 无符号类型大小**小于**带符号类型：
        + 如果无符号类型所有的值都能存在带符号类型中，转为带符号：
          + 如果long比unsingedint占用空间多，转为long
        + 如果不能，带符号类型转成无符号类型
          + 对于long和unsigned 如果long占用空间和int相同，则转为unsigned int
  
+ 数组转指针
  + 大多数数组表达式中，数组会自动的转换为指向首元素的指针
  + 但在取地址符，sizeof，typeid，decltype等运算对象时，不会转换为指针
  
+ 隐式指针转换

  + 常量值0和nullptr能转成任意指针类型

  + 指针可以自动转为布尔类型

  + 可以将指向非常量类型的指针转换为指向相应常量类型的指针，引用也一样;反之不行

  + ```
    int i;
    const int &j = i;
    const int *k = &i;
    ```

+ **强制转换**

  + 尽量避免**转型** 

  + 旧式转型：

    + c风格：(type) *expression*
    + 韩束风格 type(*expreesion*)
  
  + c++类型**新式转型**：`cast_name<type>expreesion`
  
  + type具有四种类型：**static_cast**，**dynamic_cast**，**const_cast**，**reinterpret_cast**
  
  + **static_cast**

    + static_cast用来**强迫隐式转换**
  
    + 任何具有明确定义的类型转换，只要不包含底层const，都可以用static_cost完成
  
    + 例如将：non-const转为const、int转double、void*指针转为typed指针、指向基类的指针转为指向子类的指针
  
    + ```
      int j = 2；
      double res = static_cast<double>(J) / i
      ```
  
  + **const_cast**
  
    + 改变运算对象的底层const性质：去const
  
    + 唯一能去const的运算符
  
    + ```
      const char *pc; //指向常量的指针
      char *p = const_cost<char*> pc;
      ```
  
    + 去掉const性质后，编译器会允许我们对对象写，但如果对象本身是常量，就不行
  
  + **reinterpret_cost**
  
    + 用于执行低级转型，例如将*pointer to int*转为*int*
  
    + 对运算对象的位模式提供较低层次上的重新解释
  
    + ```
      int *ip;
      char *pc = reinterpret<char*>(ip);
      ```
  
    + 此时的pc指的真实对象是一个int而不是char
  
    + 危险！
  
  + **dynamic_cast**
  
    + 执行”安全向下转型“，用来决定某对象是否归属继承体系中的某个类型、执行过程很慢
    + 可替代：用智能指针指向子类、**用虚函数解决**
    + 例如：在一个你认为是子类对象上执行子类的成员函数，但手上只有一个基类的指针和引用
  
+ 旧的强制类型转换

  + type(expr)；//函数风格
  + (type) expr；早期c语言
  + //如果替换后合法，换成const_cost和static_cost也合法，否则执行reinterpret_cost类似功能



<h3>函数</h3>

___

+ 局部变量：形参和函数体内部定义的变量统称为局部变量

+ 自动对象：只存在于函数体块执行期间的对象，在定义块结束时被销毁，如形参

+ 局部静态对象：让局部变量的生命周期贯穿函数调用及之后的时间

  + `static int a = 1; //第一次经过时才初始化，后面再次经过不会重复初始化`

+ **参数传递**

  + 如果形参是引用类型，它将绑定到对应的实参上，否则将实参的值拷贝给形参
  + `void function(int &i);`
  + 传引用调用（引用类型） 传值调用（非引用类型）
  + 指针的行为和其他非引用类型一样，拷贝的是指针的值
    + 形参拷贝的对象是指针的值（指向的地址）
    + 形参和实参两个指针，指向的对象一样，但是是两个不同的指针。
    + 因此，传递指针时，指针指向对象的对象可以改变，但形参指针的指向不会影响实参
  + 传引用调用只用传入对象，不用传入对象的地址
  + 可以使用传引用调用，使得函数返回额外的信息（函数会直接修改传进去的变量）

+ const形参和实参

  + 当用实参初始化形参时，会忽略顶层const；当形参有顶层const时，传常量与否都可以

  + ```
    void fun(const int i);
    void fun(int i);
    这两个因为忽略顶层const，所以实际上是一个声明，发生
    void fun(const int &a);
    void fun(int &r);
    底层const，可以同时存在
    ```
  
+ 指针和引用形参

  + 可以使用一个非常量初始化底层const对象，反之不行

  + 一个普通引用(&a)必须用同类型对象初始化

  + ```
    int i = 0;
    const int ci = i;
    string:size_type ctr = 0;
    //假设reset有两个重载
    reset(int *p);
    reset(int &r);
    //和变量一样，底层const不能忽略！
    reset(&ci); //错误，int*的函数，需要一个普通整形指针，提供了一个指向常量的整形指针，类型不匹配
    reset(&i); //正确
    reset(i); //使用int&的函数，需要一个普通引用，提供了一个普通对象
    reset(ci); //错误，int&的函数，需要一个普通引用，提供一个常量对象，不能把普通引用绑定到常量对象上
    reset(42); //错误，不能把普通引用绑定到常量引用上
    //如果有reset(const int &a); c++允许使用字面值初始化常量引用，则reset(ci),reset(42)都合法
    reset(ctr); //错误，类型不匹配,ctr是无符号类型
    ```

  + 尽量使用常量引用,非常量引用会限制接收值类型

  + 在使用常量引用时，传入的即可以是常量对象也可以是普通对象，还可以是字面值

  + ```
    int a = 1;
    const int b = 2;
    假设有
    void reset(const int &c){
    	func;
    }
    则reset(a)和reset(b)都合法
    ```

+ 数组形参

  + 数组具有特殊性质

    + **不允许拷贝数组**（无法用值传递的方式使用数组参数）
    + **使用数组时会转成指针**（函数传递时，实际上传递的是指向首元素的指针）

  + 虽然不允许值传递，但可以把形参写成数组的形式

  + ```
    void print(const int*);
    void print(const int[]);
    void print(const int[10]); //只是期望传入的长度，实际传入多少都可以
    上面三个定义方式等价，形参都会被编译器解释为const int*类型
    ```

+ 数组引用形参

  + c++允许将变量定义成数组的引用，因此形参也可以是数组的引用；

  + ```
    void print(int (*arr)[10]){ //arr是一个具有10个整数类型的数组的引用
    	for(auto elem:arr)
    		balabala
    }
    //这样的话只能传递10个元素的数组，不一样不行
    ```

+ 传递多维数组

  + 多维数组实际上是数组的数组，所以首元素本身就是一个数组，首指针是一个指向数组的指针

  + 数组第二维(及其后面的所有维度)的大小都是数组类型的一部分，不可以省略

  + ```
    void print(int (*matrix)[10]) //注意括号，matrix是个指针，指向含有10个元素的数组
    或者写成
    void print(int matrix[][10]);
    ```

+ 可变形参的函数

  + 如果所有实参类型相同，可以使用**initializer list**的标准库类型
  + 如果实参类型不同，可以编写可变参数模板

+ **initializer list**

  + initializer list是一种标准库类型，用于表示某种特定类型的值的数组，定义在同名头文件中

  + 提供如下操作

  + ```
    initializer_list<T> lst; //初始化；T类型元素的空列表
    initializer_list<T> lst{a,b,c..};//带元素的初始化
    lst2(lst); //浅拷贝
    lst.size() //元素数量
    lst.begin() //首元素指针
    lst.end() //尾元素指针
    ```

  + initializer list中的元素都是常量，无法修改

  + 想向initializer list形参中传递一个值序列，需要使用{}

  + `some_func({"a","b"})`

  + 含有initializelist的函数也可以拥有其他形参

+ 省略符形参

  + 便于c++程序访问c代码设置的，省略符形参通常只出现的形参的最后位置
    + void func()

+ **返回类型**

+ 不要返回局部对象的引用或指针

+ 函数的返回类型决定了函数调用是否是左值，调用一个**返回引用**的函数得到左值，其他类型得到右值，可以像其他左值一样来使用返回引用的函数的调用：如`char& get_val(string &str,string;:size_type ix){return str[ix];}`，我就可以使用`get_val(s,0) = 'A';`这样的写法

+ 函数不能返回数组，但可以返回数组的指针

  + 使用类型别名

  + ```
    typedef int arrT[10]; //arrT等价于一个含有10个整数类型的数组
    arrT* func(int i);
    ```

  + 不使用别名(很麻烦)

  + ```
    例如：
    int (*func(int i))[10]; //返回一个10个整数类型的数组的指针
    func(int i) //表示func函数需要int形参
    *func(int i) //表示我们对函数返回结果进行解引用操作
    (*func(int i))[10] //表示解引用func的调用将得到一个大小是10的数组	
    ```

  + 使用尾置返回类型，真正的返回类型跟在->后

  + ```
    auto func(int i) -> int(*)[10];
    ```

+ **函数重载**

  + 名字相同但形参列表不同的函数

  + 注意：顶层const被忽略、声明、参数名省略可能导致函数看起来不一样，但实际是一样的

  + ```
    如
    int fun1(const int a);
    int fun2(int a);
    ```

  + 如果形参是某种类型的指针或引用，可以区分其指向的对象是常量对象还是非常量对象，底层const不会被忽略

  + 因为非常量可以转换为const，当传入的是非常量对象或一个指向非常量对象的指针时，会优先选择非常量版本

  + 在局部作用域内，内层声明了名为xxx的函数，该函数会覆盖外层同名函数而不会作为xxx的一个重载。

+ 默认实参

  + 设定初始值，设定初始值的形参必须放在最后

  + `int func(int a,int b,int c=3,int d=4);`

  + 使用：用默认值，不填即可

  + 只要表达式类型能转换成形参所需的类型，该表达式就能作为默认实参

  + ```c++
    //变量声明在函数外
    sz wd = 80;
    char def = ' ';
    sz ht();
    string screen(sz = ht();sz = wd;char = def);
    string window = screen() //调用screen(ht(),80,' ');
    //默认实参的名字在函数声明所在作用域内解析，其求值发生在函数调用时，如：
    void f2()
    {
    	def = '*';   //直接改变了默认实参
    	sz wd = 100; //隐藏了外层的wd，但没改变默认值
    	window = screen(); //screen(ht(),80,'*')
    }
    ```

+ 内联函数

  + 避免函数调用的开销
  + 特点：函数规模小，流程简单，多次调用
  + 声明方法：声明前加inline
  + 会在调用函数时展开

+ constexpr函数

  + constexpr函数是指能用于常量表达式的函数

  + 函数的所有返回值和形参都必须是字面值类型，有且只有一条return

  + 允许constexpr返回值不是常量表达式

  + `constexpr size_t scale(size_t cnt){ return new_sz()*cnt ;}`

  + 当实参是常量表达式时，返回值也是常量表达式，

  + 当实参是非常量表达式，返回值也是一个非常量表达式，当上下文环境中需要常量表达式时，会报错

  + ```c++
    int arr[scale(2)]; //返回常量表达式
    int i = 2;
    int arr[scale(i)]; //返回非常量表达式
    ```

+ 函数重载中的实参匹配

  + 匹配排序：
  + **1.精确匹配**
    + 实参和形参类型相同
    + 实参从数组类型或函数类型转换成对应指针类型
    + 添加或删除顶层const
  + **2.const转换实现匹配**
  + **3.类型提升实现匹配**
    + 小整型转换成int或更高级，只有提供小整型类才会调用对应函数，否则即使值很小，也会直接调用int
  + **4.算数类型转换或指针转换实现匹配**
    + 所有算数类型转换级别一样，均能转换则会发生二义性问题
  + **5.类类型转换**

+ **函数指针**

  + 函数指针指向函数，而非对象

  + ```c++
    //存在函数
    bool lengthCompare(const string &,const string &);
    //函数类型是bool，要声明一个函数指针，把函数名换成指针即可
    bool (*pf)(const string &,const string &);//未初始化
    // *pf 是指针，右边有形参列表，是函数，左边是bool，返回类型
    //如果不带括号，pf将是一个返回bool指针的函数
    ```

  + 当把函数名作为值使用时，将自动转换为指针

  + ```c++
    pf = lengthCompare;
    pf = &lengthCompare; //两个都行
    ```

  + 可以直接用指针调用函数

  + ```
    bool b1 = pf("hello","bye");
    bool b2 = (*pf)("hello","bye");
    bool b3 = lengthCompare("hello","bye");
    //三者等价
    ```

  + 当指向重载函数时，指针类型必须与重载函数中的某一个**精确匹配**

  + 函数指针形参

  + ```c++
    void useBigger(const string &s1,const string &s2,bool pf(const string &,const string &));//第三个参数看起来是函数，实际上是指针
    useBigger(s1,s2,lengthCompare); //可以直接传入函数名
    
    //用类型别名简化
    //函数类型别名
    typedef bool Func(const string &,const string &);
    typedef decltype(lengthCompare) Func2; //等价
    
    //指向函数指针
    typedef bool (*Funcp)(const string &,const string &);
    typedef decltype(lengthCompare) *Func2p; //等价
    
    //重新声明
    void useBigger(const string&,const string&,Func);//自动转指针
    void useBigger(const string&,const string&,Func2p);//传入指针
    
    ```

  + 返回函数指针

  + ```c++
    //类型别名
    using F = int(int*,int); //函数别名
    using PF = int(*)(int*,int); //函数指针别名
    
    //必须显示地将返回类型定义为指针
    PF f1(int); //PF是指向函数的指针，f1返回该类型
    F *f1(int); //F是函数，*代表这是个指针
    
    //直接声明
    int (*f1(int))(int*,int);
    //尾置
    int f1(int) -> int(*)(int *,int );	
    
    也可以用decltype
    ```

<h3>类</h3>

___

+ 定义抽象数据类型

  + ```cpp
    //定义一个类用于说明
    #include<iostream>
    #include<string>
    using namespace std;
    struct Sales_data{
        //数据成员	
    	string bookNo;
    	unsigned units_sold = 0;
    	double revenue = 0;
    
    	//成员函数
    	string isbn() const
    	{
    		return bookNo;
    	}//体内定义
    	Sales_data& combine(const Sales_data&); //体外定义
    	double avg_price const();
    };
    //非成员函数
    Sales_data add(const Sales_data&,const Sales_data&);
    ostream &print(ostream&,const Sales_data&);
    istream &read(istream&,Sales_data&);
    ```

  + 所有成员函数都必须在类的内部声明，但定义既可以在体内，也可以在体外。

  + **引入this**：成员函数通过一个名为this的额外隐式参数来访问调用它的那个对象，当调用一个成员函数时，用请求该函数的对象来初始化this。因此，对于isbn函数，实际上编译器调用的形式如下：

  + ```c++
    someObj.isbn()   //等价于Sales_data::isbn(&someObj); //传给this
    string isbn() const {return this->bookNo};//this可以省略掉
    ```

  + **成员函数后面的const：**这里的const关键字作用是修改隐式this指针的类型

    + 默认下，this的类型是**指向非常量类类型**的**常量指针**，例如在Sales_data中，this的默认类型是Sales_data *const。这意味着，在默认情况下，不能把this**绑定到一个常量对象上**，因此不能在一个常量对象上调用普通的成员函数。
    + c++允许把const关键字放在成员函数的参数列表之后，此时，紧跟在参数列表后面的**const**表示**this**是一个**指向常量类类型的常量指针**，即**const Sales_data \*const**，这样的函数称为**常量成员函数**，这样可以使函数更加灵活。

  + 编译器首先编译类中的成员声明，再编译类中的成员函数体，所以不必在意次序。

  + **类外部定义成员函数：**

    + 必须与内部的函数声明一致，如在体外定义avg_price()

    + ```c++
      double Sales_data::avg_price() const{
      	if(units_sold)
      		return revenue/units_sold;
      	else
      		return 0;
      }
      ```

  + 返回**this对象的函数**

    + ```cpp
      Sales_data& Sales_data::combine(const Sales_data &rhs)
      {
      	units_sold += rhs.units_sold;
      	revenue += rhs.revenue;
      	return *this;  //返回调用该函数的对象
      }
      //如果不是引用类型，将返回一个副本
      /*
      当执行total.combine(trans)时
      total的地址被绑定到this上
      units_sold += rhs.units_sold; 表示求两个对象的units_sold之和，并把保存到this->units_sold中。
      因为复合赋值运算符+=返回左值，为了保持一致，我们也应该返回左值，所以返回的对象是引用类型
      函数返回的是total的引用
      */
      ```

  + **类相关的非成员函数**

    + 需要定义一些辅助函数，比如add，read，print等，这些函数从操作概念上属于类的接口（啥意思）的组成部分，但实际上并不属于类本身

    + ```cpp
      //这里定义read和print函数
      istream &read(istream &is,Sales_data &item)
      {
      	double price = 0;
      	is >> item.bookNo >> item.units_sold >> price;
      	item.revenue = price * item.units_sold;
      	return is;
      }
      ostream &print(ostream &os,const Sales_data &item)
      {
      	os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
      	return os;
      }
      ```

    + 定义add函数

    + ```c++
      Sales_data add(const Sales_data &lhs,const Sales_data &rhs)
      {
      	Sales_data sum = lhs;
      	sum.combine(rhs);
      	return sum;
      }
      //定义了一个新的Sales_data对象，并将其命名为sum，用lhs的副本来初始化sum。默认情况下，拷贝类的对象其实拷贝的是对象的数据成员
      ```

+ 构造函数

  + 当没有定义构造函数时，类通过一个默认构造函数来进行默认的初始化流程，空参

  + 只要当类没有声明任何构造函数时，才会生成一个默认构造函数

  + ```c++
    //声明几个构造函数
    Sales_data() = default; //需要默认的行为
    Sales_data(const string &s): bookNo(s) { } //构造函数初始值列表
    //负责为新创建对象的一个或几个数据成员赋值。
    Sales_data(const string &s,unsigned n,double p):bookNo(s),units_sold(n),revenue(p*n) { } //构造函数初始值
    //另一种赋值的方式
    Sales_data(const string &s,unsigned n,double p){
    		booksNo = s;
        	units_sold = n;
        	revenue = n * p;
    }
    Sales_data(istream &);
    
    //体外定义构造函数
    Sales_data::Sales_data(istream &is)
    {
    	read(is,*this);//从is中读取一条交易信息后传入this中
    	//对象先执行了默认初始化
    }
    ```

  + 数据成员初始化和数据成员赋值不完全等价，当有成员是**const或者引用类型或者无默认构造函数的其他类类型时**，就必须使用数据成员初始化

  + ```c++
    class ConstRef{
    public:
        ConstRef(int i1);
    private:
        int i;
        const int c1;
        int &ri;
    }
    //赋值法 实际上是先默认初始化再赋值
    ConstRef::ConstRef(int i1)
    {
        i = i1;   //正确
        c1 = i1;	//错误
        ri = i;	//错误
    }
    //构造函数初始化
    ConstRef::ConstRef(int i1):i(i1),ci(i1),ri(i) {} //正确
    ```

  + 成员的初始化顺序与其在类定义中出现的顺序一致，最好不要用成员初始化另一个成员

  + 委托构造函数：使用所属类的其他构造函数来执行自己的初始化过程

  + ```c++
    class Sales_data {
    public:
        //非委托构造函数
        Sales_data(std::string s,unsigned cnt,double price):bookNo(s),units_sold(cnt),revenue(cnt * price){ }
        //委托构造函数
        Sales_data():Sales_data("",0,0);
        Sales_data(std::string s ):Sales_data(s,0,0);
        Sales_data(std::istream &is):Sales_data() {read(is,*this);}
        
    }
    ```

  + 使用默认构造函数时，不要加()

  + ```
    Sales_data obj();这是个函数
    Sales_data obj2;这才是对象
    ```

  + **转换构造函数：**如果构造函数只接受一个实参，则实际上定义了此类类型的隐式转换机制

  + 例如上述的Sales_data：有两个只有一个参数构造函数：string和istream；当需要使用sales_data时，能够用string和istream替代

  + ```c++
    string null_book = "9-99-9-9";
    //这相当于构建了一个临时的Sales对象：ID为9-99-9-9，数量，价格0
    //item是一个Sales_data对象，则
    item.combine(null_book); //合法
    ```

  + 但是只允许**一步类型转换**：item.combine("9-99-9-9")因为多转换了一步string,就不合法了

  + 阻止隐式转换，在相应的成员函数前增加**explicit**关键字,在类内声明即可，此时该构造函数不再支持拷贝初始化，即用(=)；

  + 显示转换

  + ```
    item.combine(Sales_data(null_book));
    item.combine(static_cast<Saels_data>(cin));
    ```

  + 

+ **访问控制与封装**

  + 访问说明符：public、private

    + public：成员在整个程序内可被访问，public成员定义类的接口
    + private：成员可以被类的成员函数访问，但不能被使用该类的代码访问

  + 重写后的类如下所示：

  + ```c++
    class Sales_data{
    public:
    	//构造函数
    	Sales_data() = default; 
    	Sales_data(const string &s): bookNo(s) { } 
    	Sales_data(const string &s,unsigned n,double p):bookNo(s),units_sold(n),revenue(p*n) { }
    	Sales_data(istream &);
    	string isbn() const{return bookNo;}
    	Sales_data& combine(const Sales_data&);
    private:
        //数据成员	
    	string bookNo;
    	unsigned units_sold = 0;
    	double revenue = 0;
    	double avg_price () const {return units_sold?revenue/units_sold:0;}	
    };
    ```

  + **struct和class**的唯一区别是：默认访问权限不同

    + 类可以在它的第一个访问说明符之前定义成员，如果是struct关键字，则这些成员都是public的，如果是class关键字，则这些成员都是private的

  + **友元**

    + 当类重新改写后，之前的read，print等函数都无法使用了，因为数据成员的权限变为了私有，而这些函数虽然是类的接口，但不是类的成员。类可以**允许其他类或函数访问它的非公有成员**，只要令其他类或函数成为它的**友元**，增加一条friend关键字开始的函数声明即可。

    + ```c++
      class Sales_data{
          //友元声明
          friend Sales_data add(const Sales_data&,const Sales_data&);
          friend ostream &print(ostream&,const Sales_data&);
          friend istream &read(istream&,Sales_data&);
          //其他内容
      }
      ```

    + 友元只出现在类定义的内部，友元声明仅仅指定访问权限，而非一个函数声明，因此还需要进行再次声明

    + **友元类：**

    + 类也可以声明为友元，友元类的成员函数可以访问此类包括非公有成员在内的所有成员

    + `friend class other;`

    + 还可以只让其他类的某个成员函数称为友元

    + `friend void otherclass::otherfunc(){};`

    + 对于每个重载函数需要单独声明友元。

+ 其他类的特性

  + 令成员作为内联函数，类内定义的成员函数自动inline，类外的可以加inline设置为内联

  + 成员函数也能够重载

  + 可变数据成员：既然在const对象中，也能够修改某个数据成员，通过mutable关键字实现，可在const成员函数中修改数据成员

  + ```
    mutable size_t ctr;
    ```

  + 基于const的重载

    + 可以区分成员函数是否是const的，来进行重载，非常量对象虽然可以用常量函数进行匹配，但使用非常量版本是最佳选项。

  + 类的作用域和编译顺序

    + 一般情况：

      + 在名字所在的块中寻找声明语句，只考虑在名字的使用之前出现的声明
      + 如果没有，找外层作用域
      + 未找到任何声明，程序报错

    + **成员函数**处理：

      + 仅在成员函数中这样处理
      + 首先编译全体成员的声明
      + 类全部可见后，才编译函数体，即处理完声明后才处理成员函数的定义

    + **声明中**使用的名字，返回或参数列表中使用的名字，使用要声明，如果类中的成员声明出现了尚未出现的名字，编译器会在定义该类的作用域中继续查找。**先找类内部，再找定义类的作用域**

    + 成员定义中的普通块作用域

      + 先在**成员函数内**查找，只能找函数使用之前的定义

      + 在**类内**查找，可以找类中所有成员

      + 类内也找不到，在**类定义之前的作用域**继续找

      + 当**成员函数定义在外部**，也会在**成员函数定义之前**找

      + ```c++
        int height;  //外层作用域中的height
        class Screen{
        public:
            typedef std::string::size_type pos;
            void setHeightt(pos);
            pos height = 0; //会隐藏外层的height
           	//想调用外部作用域的height 用::height
        };
        Screen::pos verify(Screen::pos); //类定义作用域之后，但在外部成员函数定义作用域之前
        void Screen:setHeight(pos var){
            //var参数
        	//height：类成员
            //verify：全局函数
            height = verify(var); //合法
        }
        ```

      + 成员对象与类外作用域对象同名时，会将类外作用域的对象隐藏。

      + 不建议命名冲突

+ **聚合类**

  + 特点：用户可以直接访问成员，具有特殊的初始化语法形式

    + 所有成员都是public
    + 没有定义任何构造函数
    + 没有类内初始值
    + 没有基类或者virtual函数

  + ```c++
    struct Data{
    	int val;
    	string s;
    };
    //初始化
    Data vall = {0,"abc"}; //值的顺序要与声明顺序一致
    ```

+ 字面值常量类

  + **定义：**数据成员都是字面值类型的聚合类，是字面值常量类

  + **定义2：**当该类不是聚合类，满足以下条件时，也为字面值常量类

    + 数据成员均为字面值类型
    + 类必须至少含有一个constexpr构造函数
    + 如果一个数据成员有初始值，则内置类型成员的初始值必须是常量表达式；或者如果成员属于某种类，则初始值必须使用成员自己的constexpr函数
    + 类必须使用析构函数的默认定义

  + constexpr构造函数

    + 在函数前加constexpr关键字，函数体一般为空,必须初始化所有数据成员

    + ```c++
      class Debug{
      public:
      	constexpr Debug(bool b=true):hw(b),io(b);other(b) {}
      	constexpr Debug(bool h,bool i,bool o):hw(h),io(i),other(o) {}
      private:
          bool hw;
          bool io;
          bool other;
      };
      ```

+ **类的静态成员**

  + 跟java的静态一样

  + static关键字声明一个静态成员，仅出现于类的内部

  + 类的静态成员存在于任何对象之外，静态成员函数不与任何对象绑定，不能声明为const，无法使用this指针

  + 一般来说，必须在类内部声明静态成员，在类外部定义和初始化**静态数据成员**，可以在类内部定义静态函数成员

  + 特例：我们可以为静态成员提供const整数类型的初始值，并且静态成员是字面值常量类型的constexpr。

    + 并且：仅当编译器可以替换他的值的时候，才不用在类外定义，否则依然需要类外定义

  + ```
    static constexpr int period = 30;  //此时的period作为一个常量表达式，可以定义在类内部
    double daily_tbl[period]; //不需要
    ____类外
    当需要将其传递给一个接受const int&的函数时，就必须在类外重新定义
    如果内部提供了初始值，则外部就不能指定初始值了
    ```

  + 使用静态成员：

  + ```
    //假定有一个类Account
    double r = Account::rate(); //直接调用类的静态成员函数
    //同样可以用指针形式
    Account *ac1 = &ac;
    double r = ac.rate();
    double r = ac1->rate();
    ```
  
  + 静态成员可以作为成员函数的默认实参，非静态成员不行

  + 当类是不完全类型，静态数据成员的类型可以就是其所属的类的类型，而非静态数据成员必须声明成他的引用或指针
  
  + ```c++
    class Bar{
        public:
        	//...
        private:
        	static Bar mem1; //正确，静态成员可以是不完全类型
        	Bar *mem1; //指针成员可以是不完全类型
        	Bar mem1; //错误，数据成员必须是完全类型
    }
    ```



___

<h2>C++标准库</h2>

<h3>IO库</h3>

___

之前已经见到IO库内容

+ istream输入流类型，提供输入操作
+ ostream输出流类型，提供输出操作
+ cin，一个istream对象，从标准输入读取数据
+ cout，一个ostream对象，向标准输出写入数据
+ cerr，一个ostream对象，写入到标准错误
+ \>\>运算符，用来从一个istream对象读取输入数据

**IO类**

+ 除istream和ostream外，标准库中还有其他IO类型，分别储存在不同的头文件中

  + iostream头文件：istream、ostream、iostream   从流中读取写出数据
  + fstream头文件：ifstream、ofstream、fstream 从文件中读写
  + sstream头文件（读写内存string对象）：istringstream、ostringstream、stringstream 读写string
  + 上述9个类型后，加前置w；例如wistream，用来操作wchar_t类型的宽字符语言数据。
  + 类型ifstream和istringstream都继承自istream，因此具有相同的操作

+ 流对象不能进行拷贝或赋值

+ 当读写一个IO对象时，会改变其状态，因此传递和返回的引用不能是const

  + IO类定义了一些函数和标志，用来帮我们访问条件状态

+ 通常在使用一个流前需要检查是否有效

+ ```
  while(cin >> word)
  	ok;//读取成功
  ```

+ 查询流的状态

  + badbit表示系统级错误，一旦被置位，流就无法使用了
  + failbit表示可恢复错误，如期望读取数值却读到一个字符串，修正后流依然能继续使用
  + 到达文件结束位置，eofbit和failbit均被置位
  + goodbit值为0，表示流未发生错误
  + badbit、eofbit、failbit任何一个被置位，则检测流的条件都会失败
  + 标准库定义了函数来查询状态：常用的有good()：所有错误均未置位返回true和fail()：有一个置位就返回ture

+ 设置流的状态

  + 流对象的rdstate成员返回一个iostate值，对应流当前状态
  + setstate操作将给定条件置位
  + clear操作具有两个重载版本，一个是不带参数用于复位所有的标志位，一个接受一个iostate参数，表示流的新状态

+ 输出缓冲

  + 每个输出流都有一个缓冲区，用于保存程序读写的数据，操作系统可以将程序的多个输入输出操作，组合成单一的系统级操作
  + 缓冲区刷新条件:
    + 程序正常结束，作为main函数的return操作的一部分
    + 缓冲区满时，需要立刻刷新缓冲
    + 使用操作符endl，来显式刷新缓冲区
    + 在输出操作后，用操作符unitbuf设置流的内部状态，来清空缓冲区
    + 一个输出流被关联到另一个流时，读写被关联的流，会导致关联的流的缓冲区刷新
  + endl：刷新缓冲区并换行；flush：刷新缓冲区但不附加额外字符；ends刷新缓冲区输出一个空字符
  + unitbuf操作符：`cout<<unitbuf;`代表接下来的输出操作都会立刻刷新缓冲；`cout<<nounitbuf;`恢复正常

+ 关联输入输出流：

  + 标准库默认关联了cin和cout，cin的读操作导致cout立刻刷新

<h3>文件输入输出</h3>

___

```
//fstream中定义了一些特有操作
fstream fstrm;  //创建一个未绑定的文件流
fstream fstrm(s); //创建一个fstream，并打开名为s的文件，s可以是字符串或者char数组
fstream fstrm(s,mode);//按指定mode打开文件
fstrm.open(s); //打开文件
fstrm.close(); //关闭文件
fstrm.is_open(); //是否打开
```

用法：

```c++
#include<iostream>
#include<fstream>
#include<string>
using namespace std;
int main(){
    string infile = "test.txt";
    string outfile = "testout.txt";
    ifstream in(infile);
    ofstream out;
    out.open(outfile);
    string s;
    string t;
    if(in>>s>>t)
        out<<s<<" "<<t<<endl;

    system("pause");
    return 0;
}
```

+ 当一个fstream对象离开其作用域时，与之关联的文件会自动关闭

+ **文件模式**
  + 在初始化或open中指定
  + in：只读，只用于读对象
  + out：只写，只用于写对象
  + app：每次写操作追加到末尾，app自带out
  + ate：打开文件后立刻定位到文件末尾
  + trunc：截断文件，设定out才能trunc，默认情况下，以out模式打开也会被截断，必须同时指定app或in模式才能同时读写，即当out模式打开一个文件时，文件中的内容会被丢弃，以前的内容直接没了
  + `ofstream app("file",ofstream::out | ofstream::app)`;这样才不会
  + binary：以二进制方式进行IO
  + istream默认in模式，ostream默认out模式

<h3>String流</h3>

___

​	sstream头文件定义了三个类型来支持内存IO，这些类型可以向string写入数据，从string读取数据

sstream特有的操作

```
sstream strm;  //未绑定的sstream对象
sstream strm(s); //一个sstream对象，保存string s的一个拷贝，构造函数是explicit的

strm.str() 返回strm所保存的string的拷贝
strm.str(s) 将string s拷贝到strm中
```

+ istringstream流

  + 当工作需要对整行文本进行处理，需要处理行内的单个单词时，通常可以用，就是把string当成一个输入流

  + ```c++
    //例如我们有一个类PersonInfo，用于保存一个string人名和一个vector的电话号码集合
    string line,word;
    vector<PersonInfo> people;
    while(getline(cin,line))//循环读取每行记录
    {
        PersonInfo info;
        istringstream record(line); //将记录绑定到刚读入的行
        record >> info.name; //读取名字
        while(record >> word) //读取电话
            info.phones.push_back(word);
        people.push_back(info);
    }
    ```

+ ostringsteam流

  + 当逐步构造输出，最后一起打印可用，例如当想逐个验证电话号码是否有效并改变格式时，可以使用ostringstream，最后保证验证完所有电话号码后，不输出错误电话到新文件中

  + ```c++
    for(const auto &entry:people)
    {
        ostringstream formatted,badNums;
        for(const auto &nums:entry.phones)
        {
            if(!valid(nums)) //号码错误
                badNums << " "<<nums;
            else //号码正确
                formatted << " " << format(nums); //把正确号码存到一个字符串流里
        }
        if (badNums.str().empty())
            os << entry.name << " " << formatted.str() endl; //最终输出
    }
    ```




<h3>顺序容器</h3>

___

顺序容器类型：

+ vector：可变大小数组，内存连续存储，读取快，在中间添加或删除元素较慢，增加元素可能需要额外分配新的存储空间
+ deque：双端队列，支持随机访问，两端添加删除元素很快
+ list：双向链表，只支持双向顺序访问，不支持随机访问
+ forward_list：单向链表，以达到与手写链表相当性能而设计，不支持size
+ array：固定数组，支持随机访问，不能添加删除元素
+ string：与vector相似，专门用于保存字符，内存连续存储
+ 标准库容器的优化很好，可以尽量选择标准库容器而非内置数组

一般来说，每个容器定义在一个头文件中，文件名与类型名相同，均被定义为模板类，几乎没有类型限制，对于没有默认构造函数的类类型，不能直接用元素个数初始化。

容器操作中的一些类型别名：

+ iterator：容器类型的迭代器类型
+ const_iterator：可以读取元素，但不能修改的迭代器类型
+ size_type：无符号整数，容器大小
+ difference_type：带符号整数，保存两个迭代器之间的距离
+ reference
+ const_reference
+ value_type

容器操作一些方法：

+ swap(a,b)交换 其他容器为常数时间，不会交换元素，array会真正的交换元素
+ c.size() c中元素数目
+ c.empty() 判空
+ c.clear() 清空
+ c.erase(args) 删除args指定元素
+ 关系运算符也支持
+ c.begin()、c.end()、c.rbegin()、c.rend()、c.cbegin()、c.cend()、c.crbegin()、c.crend()

**迭代器**

迭代器有公共的接口，但部分容器存在例外，例如forward_list不支持递减运算符

+ 迭代器范围
  + 迭代器范围由一对迭代器表示，分别指向开头和结尾之后的位置，相当于左闭合区间，这样保证了，当begin==end时，范围是空的
  + 要求两个迭代器指向同一个容器，并且begin在end之前
  + 用迭代器初始化时，元素的类型可以不一样，只要能转换成一样的即可

**容器初始化**

```
C c;   空的容器
C c1(c2);  c1的初始化为c2的拷贝
C c{a,b,c}; 列表初始化
C c = {a,b,c} 
C c (b,e); 迭代器初始化
//顺序容器，不包括aray特有的
C seq(n); seq包含n个元素，值默认初始化
C seq(n,x); seq包含n个初始值为x的元素
```

**标准库array**

array的大小也是类型的一部分，定义一个array除了要指定元素类型，还要指定容器大小

例如：`array<string,42>`

默认会进行元素的初始化，列表初始化时提供的值必须小于等于其大小，元素类型是类类型时，必须要有一个默认构造函数

内置数组不支持拷贝赋值，而array可以	

**assign**

仅顺序容器支持assign，允许从一个不同但相容的类型中赋值，例如把const char*变成string

```
seq.assign(b,e); //将seq中的元素替换为迭代器b和e所表示范围内的元素
seq.assign(i1); //将seq中元素替换为初始化列表i1中的元素
seq.assign(n,t); //将seq中的元素替换为n个值为t的元素
```

**顺序容器操作**

```c++
----------------------------添加元素-----------------------------
//array不能添加元素，所以都不支持
//forward_list有自己的insert和emplace
//forward_list不支持push_back和emplace_back
//vector和string不支持push_front和emplace_front
c.push_back(t); //尾插
c.push_front(t); //头插
c.emplace_back(args);
c.emplace_front(args);

//insert有返回值，返回一个指向新元素的迭代器
c.insert(p,t); //在迭代器p指向的元素之前，创建一个值为t的元素
c.insert(p,n,t); //...,创建n个值为t的元素
c.insert(p,b,e); //插入从迭代器b到迭代器e范围内的元素
c.insert(p,i1); //插入一个花括号包围的元素值列表
//对于不支持push_front的元素，我们可以用insert到begin之前来实现，

//对于emplace相关操作，其主要功能是，使用给定参数，在容器中直接构造元素
//例如对应一个保存Sales_data类的容器c
c.emplace_back("978-0500",25,15.99); //使用了三个参数的构造函数构造了一个salesdata放入容器
c.push_back("978-0500",25,15.99); //错误，而对于pushback这样就不合法了
c.push_back(Sales_data("978-0500",25,15.99)); //这样才合法

---------------------------------删除元素-------------------------
//不适用于array
//vector和string不支持pop_front;
//foward_lsit 不支持pop_back?
c.pop_back();
c.pop_front();

c.erase(p); //删除迭代器p所指定的元素，返回一个指向被删除元素之后的迭代器，如果p指向尾元素则返回尾后迭代器
c.erase(b,e); //删除迭代器b和e范围内的元素
c.clear(); //清空
```

**顺序容器访问**

```c++
//at 和 下标访问只适用于string,vector,deque和array
//back不适用于forward_list
//这些返回的都是引用
c.back();  //返回c中尾元素的引用
c.front(); //返回c中首元素的引用
c[n] 下标	
c.at(n);
```

**特殊的forward_list**

forward_list是一个单向链表，具有自己特殊版本的添加和删除操作，是通过改变给定元素之后的元素来完成的。

```c++
//lst是一个forward_list
//forward_list定义了一个before_begin的首前迭代器，允许我们在首元素之前的并不存在的元素之后添加或删除
lst.before_begin(); //返回一个首前迭代器，不可解引用
lst.cbefore_begin(); //常量版本

//这些操作都是在p元素之后进行操作
//insert
lst.insert_after(p,t);
lst.insert_after(p,n,t);
lst.insert_after(p,b,e);
lst.insert_after(p,i1);

emplace_after(p,args); 

lst.erase_after(p); //删除p指向的位置之后的元素
lst.erase_after(b,e) //删除b之后但不到e的元素	返回一个指向被删除元素之后元素的迭代器
```

**vector增长原理**

+ capacity()函数，显示最多可以保存多少元素；reverse()函数，分配至少能容纳n个元素的内存空间

+ 当需求空间大小小于等于目前的内存空间大小capacity时，reverse函数什么也不做
+ 当需求空间大小大于目前的内存空间大小capacity时，reverse至少分配与需求一样大的空间（更大）
+ 分配策略根据具体实现不同

**string额外操作**

+ 其他的构造方法：

+ ````c++
  string s(cp,n);  //s是cp指向的数组中前n个字符的拷贝，数组应该至少包含n个字符
  string s(s2,pos); //s是string s2从下标pos开始的字符的拷贝
  string s(s2,pos,len); //s是string s2从下标pos开始len个字符的拷贝
  接受一个string或者const char*作为参数
  substr操作，返回子串
  string s2 = s1.substr(pos,n) 返回从pos开始的n个字符的拷贝
  ````

+ string类提供了额外接受下标的insert和erase以及接受C风格字符数组的insert和assgin版本

+ ```c++
  s.insert(s.size(),5,'a'); //在末尾插入5个a
  s.erase(s.size()-5,5); //从s删除最后5个字符
  const char *cp = "Stately,plump Buck";
  s.assign(cp,7); //将s替换为从cp指向地址开始的7个字符"Stately"
  s.insert(s.size,cp+7); //插入从cp+7后的所有字符"Stately,plump Buck"
  ```

+ 可以直接将其他字符串插入到当前string中

+ ```c++
  string s = "some string";
  string s2 = "some other string";
  --
  s.insert(0,s2); 在s中的位置0之前插入s2
  s.insert(0,s2,0,s2.size()); //在s[0]之前插入s2[0]开始的s2.size()个字符
  ```

+ String额外定义了append和replace操作

+ ```c++
  string s = "abc";
  //append是在string末尾插入操作的简写
  s.append("efg");//等价于s.insert(s.size(),"abc")
  //replace是调用erase和insert的一种简写
  s.replace(3,2,"dd"); //从位置3开始，删除两个字符，并插入dd
  ```

+ string搜索操作

+ find、rfind、find_first_of、find_last_of,find_first_not_of、find_last_not_of

+ 数值转换：

+ ```
  stoi(s,p,b); //s表示字符串，p表示起始位置，b表示基数,stoi表示转换为int类型
  同理还有
  stol:long
  stoul:unsigned long
  stoll:longlong
  stoull:unsigned longlong
  stof:float
  stod:double
  stold:long double
  ```

<h3>容器适配器</h3>

标准库定义了三个容器适配器：stack、queue、priority_queue

一个容器适配器接受一种已有的容器类型，但使其行为看起来像另一种不同的类型

例如stack接受一个顺序容器，并使该容器操作起来像stack一样

在某个顺序容器上实现的某个适配器，此时只能使用适配器操作，而不能使用容器操作

**定义一个适配器**

每个适配器包含两个构造函数：一个默认构造函数创建一个空对象，一个接受一个容器的构造函数，通过拷贝该容器来初始化适配器。

例如：deq是一个deque\<int\>，我们可以用dep来初始化一个新的stack:

`stack<int> stk(deq);`

默认情况stack和queue用**deque**实现，priority_queue用vector实现，可以在创建适配器时，将一个带类型的顺序容器作为第二个类型参数，来重载默认的类型，例如在vector上实现空栈。

对于一个适配器，可以使用的容器是有限制的

`stack<string,vector<string>> str_stk;空栈`

`stack<string,vector<string>> str_stk(svec);初始化svec的拷贝`

+ stack要求：push_back,pop_back和back：可以使用**除array和forward_list外**的**所有顺序容器**
+ queue要求：back、push_back、front、push_front：可以**使用list和deque**
  + priority_queue要求：front、pop_back、push_back，能随机访问：可以**使用vector或者deque**


**栈适配器**

stack类型定义在stack头文件中

支持的操作：

```
s.pop();//删除但不返回
s.push(item);
s.emplace(args);
s.top();//返回栈顶
s.size();
s.empty();
```

**队列适配器**

queue和priority_queue都在queue头文件中

支持的操作：

```
q.pop();//删除元素
q.front();//返回队首 只用于queue
q.back();//返回队尾，只用于queue
q.top();//返回最高优先级，只用于priority_queue;
q.push(item);
q.emplace(args);
```

**priority_queue优先队列**

```c++
 //使用lambda表达式
auto cmp = [](int a, int b) { return a > b; };
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
//a是原来的，b是新加入的
// a > b 时返回true 意思是如果新来的比原来的小，那么他的优先级就高，实现小根堆
```



<h3>泛型算法</h3>

___

大多数算法定义在头文件"algorithm"中，标准库还在"numeric"中定义了一组泛型算法，一般情况下，这些算法不会直接操作容器，而是遍历由两个迭代器指定的一个元素范围。

```c++
//标准库算法：find
auto result = find(vec.begin(),vec.end(),value); //在be之间找vaule，返回一个迭代器
//accumulate：求和，定义在numeric中
int sum = accumulate(vec.begin(),vec.end(),value); //求和 初始值是value
//equal：确定两个序列中的值是否相同
equla(vec1.begin(),vec1.end(),vec2.begin());
//fill:范围赋值
fill(vec.begin(),vec.end(),0); //将0赋给begin到end
//fill_n:参数不同,必须要保证有元素
fill_n(vec.begin(),vec.size(),0);
//copy：拷贝，传递给copy的目的序列至少与拷贝序列元素长度相同
int a1[] = {1,2,3,4,5,6,7,8,9};
int a2[sizeof(a1)/sizeof(*a1)];
auto ret = copy(begin(a1),end(a1),a2);//将a1拷贝给a2 返回目的迭代器递增后的值
//replace：对一个序列，将某个值的所有元素改为另一个值
replace(vec.begin(),vec.end(),0,42);//把b到e所有的0换成42
//replace_copy:保留原序列，多留出一个保存位置
replace_copy(vec.begin(),vec.end(),back_inserter(ivec),0,42);
//sort:普通排序
sort(vec.begin(),vec.end());
//unique:重排，使不重复元素放在容器前部，重复元素放在后部，返回指向不重复区域之后一个位置的迭代器
auto end_unique = unique(vec.begin(),vec.end());
vec.erase(end_unique.vec.end()); //真正的删除
//find_if：接受一个范围和一个谓词,对每个元素调用谓词，返回第一个使谓词返回非0值的元素
//for_each:对输入序列的每个元素调用此对象
```

**插入迭代器**：是一种能够向容器中添加元素的迭代器

```
back_inserter定义在iterator头文件中
vector<int> vec;
auto it = back_inserter(vec); //返回一个插入迭代器
*it = 42; //对迭代器赋值会调用push_back添加元素
fill_n(it,10,0); //添加10个元素到vec
```

**定制操作**

**谓词：**是一个可调用的表达式，返回一个能用作条件的值

+ 一元谓词：只接受一个参数
+ 二元谓词：接受两个参数

```
bool isShorter(const string &s1,const string &s2)
{
	return s1.size()<s2.size();
}
sort(vec.begin(),vec.end(),isShorter); //isShorter是一个二元谓词，这样实现了按长度排列
//有点像传递一个比较器
//stable_sort：稳定排序，保持相等元素之间的相对顺序
```

**lambda表达式**

___

+ 基本定义

lambda表达式表示一个可调用的代码单元，可以将其理解为一个未命名的内联函数，一个lambda表达式具有一个返回类型，一个参数列表和一个函数体，格式如下：

`[capture list] (parameter list) -> return_type {function body}`

其中caputurelist捕获列表是一个lambda所在函数中定义的局部变量列表，通常为空

**可以忽略参数列表和函数体，但必须保留捕获列表和函数体**

例如：`auto f = [] {return 42;};`

**真实类型：**

定义了一个可调用对象f，不接受参数，返回42，调用与普通函数一样：f()

如果忽略返回类型，lambda会根据代码推测返回类型：

​	如果函数体**只有一个return** 则为return后面的类型；反之 只要包含**任何除return外**的内容，类型都为void

lambda表达式中**不能包含默认参数**，因此实参永远和形参数目相等

+ 带参数的lambda表达式

例如：实现一个lambda表达式版本的isShorter

```c++
[](const string &s1,const string &s2) {return s1.size()<s2.size();}
//空捕获列表表明，该表达式不使用其所在函数的任意局部变量
//此时，stable_sort可以写成
stable_sort(vec.begin().vec.end(),[](const string &s1,const string &s2){return s1.size()<s2.size();});
```

+ 带捕获列表的lambda表达式

```
//查找第一个长度大于等于函数中定义值sz的元素
auto wc = find_if(vec.begin(),vec.end(),[sz](const string &s){return s.size() >= sz})
```

+ 值捕获

  + 前提是变量可以拷贝，被捕获的变量是在lambda创建时拷贝，而不是调用时拷贝

+ 引用捕获

  + [&v1] {return v1;};
  + 使用引用所绑定的对象

+ 隐式捕获

  + 让编译器推断我们使用哪些变量

  + &表示引用捕获 =表示值捕获

  + 例如：

  + ```
    find_if(words.begin(),words.end(),[=](const string &s){return s.size()>=sz;};
    ```

  + 可以混合使用：第一个指定默认的隐式捕获方式，必须是=或者&，后面的另一种捕获方式的变量

**可变lambda**

默认情况下，对于被值拷贝的变量，lambda不改变其值，想要改变需要加mutable关键字

对于引用捕获，取决于引用是否为底层const

```
int v1 = 42;
auto j = [v1] {return ++v1;};  //这是不合法的
auto i = [v1]() mutable {return ++v1;}; //ok
```

**指定返回值类型**

```
//transform:替换序列中的每一个数 前两个迭代器表示范围，第三个表示替换起点，后面是lambda表达式，因为不仅仅含有return 所以要指定返回类型
transform(vec.begin(),vec.end(),vec.begin(),[](int i) -> int {if(i<0) return -i;else return i;};
```

**参数绑定**

当不使用lambda表达式，而使用一个函数作为find_if的谓词时，由于find_if只允许一元谓词，所以，其中的局部变量sz无法传递给函数，因此引用一个bind方法，用于将sz与check_size函数绑定,_n表示占位符，为函数原有的参数，bind函数定义在functional中

`auto wc = find_if(vec.begin(),vec.end(),bind(check_size,_1,sz));`

+ _n参数定义在std::placeholders中
+ bind可以重排参数顺序
+ 默认情况下，对于非占位符的参数进行拷贝，如果想要绑定一个引用，需要把原来的对象调用标准库ref函数



**其他迭代器**

___

标准库在头文件iterator中定义了多种迭代器

+ 插入迭代器：绑定在容器上，可向容器中插入元素
+ 流迭代器：绑定在输入输出流上，遍历关联的IO流
+ 反向迭代器：反向移动
+ 移动迭代器：不拷贝，而是移动元素

**插入迭代器**

插入迭代器是一种迭代适配器，接受一个容器，生成一个迭代器，当向插入迭代器赋值时，该迭代器调用容器操作来给指定位置加入一个元素

插入迭代器有三种类型：

+ back_inserter  使用push_back插入，永远指向最后
+ front_inserter 使用push_front插入，永远指向首
+ inserter 使用insert插入，第二个参数必须传入一个指向指定容器的迭代器，插入到指定元素之前

**iostream迭代器**

istream_iterator读取输入流

ostream_iterator读取输出流

创建迭代器时，需要指定读写的对象类型，可以绑定一个流或为空

```
//用输入迭代器从标准输入流中读取数据并存到一个vector中
istream_iterator<int> in_iter(cin);
istream_iterator<int> int_eof; //空的istream_iterator，作为istream尾后迭代器
while(in_iter != int_eof)
	vec.push_back(*in_iter++);
	
//更为简洁的重写
vector<int> vec(in_iter,int_eof);
```

对于ostream_iterator

out = val //用<<运算符将val写入到out所绑定的ostream中

例如对于：

```
ostream_iterator<int> out_iter(cout," ");
for(auto e:vec)
	*out_iter++ = e;
cout<<endl;
//将vec中的每个元素写到cout，并且加空格
//调用copy来实现循环
copy(vec.begin(),vec.end(),out_iter);
```

 //流迭代器也可以作用于类类型

**反向迭代器**

反向移动：以正向坐标轴为参考，++往前 --往后

成员函数：base，能将反向迭代器转为普通迭代器，指向的并不是相同元素

![image-20230204205442706](C:\Users\admin\Desktop\code\image-20230204205442706.png)



**按算法需求分类迭代器**

+ 输入迭代器：只读不写，单遍扫描，只递增
  + ==、！=
  + ++
  + *
  + ->
+ 输出迭代器：只写不读，单遍扫描，只递增
  + ++
  + *
+ 前向迭代器：可读可写，多遍扫描，只递增、
  + 支持所有输入输出迭代器操作
  + 多次读写同一个元素
+ 双向迭代器：可读可写，多遍扫描，递增递减
  + 额外支持--
+ 随机访问迭代器：可读可写，多遍扫描，支持全部迭代器运算
  + 全部双向迭代器功能
  + <、<=、>、>=
  + +、+=、-、-=
  + 迭代器减（-）
  + 下标运算

array、deque、string和vector都是随机访问迭代器





<h2>关联容器</h2>

___

关联容器中的元素是按关键字来保存和访问的，两个主要的类型是**map**和**set**

关联容器都支持普通容器操作，不支持顺序容器的位置相关操作：pushback、pushfront

关联容器的迭代器都是双向的

标准库提供8个关联容器：

+ map
+ set
+ multimap
+ multiset
+ unordered_map
+ unordered_set
+ unordered_multimap
+ unordered_multiset

主要**划分维度**为：

+ 一个set或者是一个map
+ 是否允许重复关键字
  + 允许重复：multi
+ 按顺序保存或者无序存储
  + 无序存储的容器：unordered
  + 无序容器使用哈希函数组织元素

+ map和multimap定义在头文件map中
+ set和multiset定义在头文件set中
+ 无序容器定义在头文件unordered_map和unordered_set中



**使用一个map和set**

```c++
//下面是一个统计单词出现次数的map
map<string,size_t> word_count;
//初始化一个map必须提供key和value
//如：map<string,string> cmap = {{"Jr","diana"}};
string word;
while(cin>>word)
{
	++word_count[word];
}
for(const auto &w : word_count) //对map中的每个元素  得到一个pair类型对象
    //pair是一个模板类型
{
    cout<<w.first<<" occurs "<<w.second<<((w.second > 1)?"times":"time") <<endl;
}
//下面定义一个set
set<string> exclude = {"The","But","And"};
string word;
while(cin>>word)
{
    if(exclude.find(word) == exclude.end()) //过滤常见单词
        ++word_count[word];
}
```

**关键字类型**

对于有序容器：关键字必须定义元素比较的方法，默认情况下，标准库使用<来比较关键字，可以自己定义操作来代替关键字的<运算符，但所提供的操作必须**严格弱序**：小于等于，一般只要是正常的<都可以用作关键字。

**pair类型**

pair类型定义在头文件utility中

一个pair保存两个数据成员

`pair<string,int> p = {"a",1};	`

第一个成员：p.first

第二个成员：p.second

**类型别名**

+ **key_type**:容器的关键字类型
+ **mapped_type**：每个关键字关联的类型，只用于map，值的类型
+ **value_type**：对于set，和key_type相同；对于map，是pair<const key_type,mapped_type>

**关联容器迭代器**

```c++
//获得指向word_count中一个元素的迭代器
//对于map，其first成员保存的是const关键字，不能修改，second可以修改
auto map_it = word.count.begin();
cout<<map_it->first;
//set的迭代器也是const的
```

当用迭代器遍历有序容器时，迭代器按关键字升序遍历

**insert添加元素**

```c++
vector<int> ivec = {1,2,3,4,5};
set<int> set2;
//两种方法
set2.insert(ivec.begin(),ivec.end());
set2.insert({1,2,3,4,5});
set2.insert(1);

//map添加元素四种方法
//map的元素类型是一个pair
word_count.insert({word,1});
word_count.insert(make_pair(word,1));
word_count.insert(pair<string,size_t>(word,1));
word_count.insert(map<string,int>::value_type(word,1));
//对于单个元素，插入返回一个pair：first是一个指向给定关键字的元素（插入的那个pair/元素），second是bool值，是否插入成功

//对于multimap和multiset：一样，但不用返回bool值了，因为无论如何插入都会成功
```

**删除元素**

```c++
//除与顺序容器同样的提供一个迭代器或者一个迭代器范围来删除元素外
//关联容器提供一个额外erase操作，接受一个key_type参数，删除所有给定关键词的元素，返回实际删除元素的数量
cmap.erase(removal_word);
```

**map的下标操作**

**map和unordered_map**提供了下标运算符和一个对应的at函数，当询问的关键字不在容器中时，会为他创建一个元素并插入，并进行值初始化

set、无序map都不支持下标

```
map<string,int> cmap;
cmap["anna"] = 1;
//先找anna，找不到
//创建anna，并初始化值为0
//令anna关联的值为1
```

**访问元素**

+ 对于不允许重复的容器，最好用find
+ 对于允许重复的容器，计数用count，否则用find

+ 对于map，当使用下标访问时，如果元素不存在，会自动创建一个元素，当不想改变map中的元素值时，应该使用find，

+ 对于multiset和multimap：如果其中含有多个相同关键字元素，会相邻存储。
  + count返回关键字的数量
  + find返回一个迭代器，指向第一个关键字符合的元素
  + lower_bound(k)：指向第一个关键字不小于k的元素
  + upper_bound(k)：指向第一个关键字大于k的元素
  + equal_range(K)：返回一个迭代器pair，表示等于k的元素所在的范围

**无序容器**

___

当关键字之间没有明显的序的情况下，无序容器性能更好

无序容器提供了跟有序容器相同的操作：如find、insert

```
//unordered_set
insert(): 将一个元素插入unordered_set中。

emplace(): 通过直接在unordered_set中构造元素来插入元素。

erase(): 删除unordered_set中指定的元素。

clear(): 删除unordered_set中的所有元素。

find(): 查找给定的元素是否在unordered_set中。

count(): 返回unordered_set中与给定元素相等的元素数量。

empty(): 判断unordered_set是否为空。

size(): 返回unordered_set中元素的数量。

max_size(): 返回unordered_set可以容纳的最大元素数量。

bucket_count(): 返回unordered_set中桶的数量。

load_factor(): 返回unordered_set中平均每个桶的元素数量。

rehash(): 更改unordered_set的桶的数量。

reserve(): 预留足够的内存以容纳给定数量的元素。
```

```
unordered_map
insert(): 将一个键值对插入unordered_map中。

emplace(): 通过直接在unordered_map中构造键值对来插入键值对。

erase(): 删除unordered_map中指定的键值对。

clear(): 删除unordered_map中的所有键值对。

find(): 查找给定的键是否在unordered_map中。不存在返回迭代器

count(): 返回unordered_map中与给定键相等的键值对数量。

empty(): 判断unordered_map是否为空。

size(): 返回unordered_map中键值对的数量。

max_size(): 返回unordered_map可以容纳的最大键值对数量。

bucket_count(): 返回unordered_map中桶的数量。

load_factor(): 返回unordered_map中平均每个桶的键值对数量。

rehash(): 更改unordered_map的桶的数量。

reserve(): 预留足够的内存以容纳给定数量的键值对。

operator: 访问unordered_map中给定键的值，如果该键不存在，则插入一个新的键值对并返回默认值。
```



**管理桶**

无序容器在存储上组织为一组桶，每个桶保存零个或多个元素，无序容器使用哈希函数将元素映射到桶，具有一个特定哈希值(hash<key_type>类型)的所有元素，都被放在一个桶中，当一个桶保存多个元素时，按顺序查找

+ 桶接口

+ ```
  c.bucket_count(); //正在使用的桶的数目
  c.bucket_size(n);//第n个桶中有多少个元素
  c.bucket(k); //关键字为k的元素在哪个桶中
  
  local_iterator //桶迭代器
  ```

对于内置类型、string和智能指针，标准库提供了hash模板，对于自定义类类型，需要我们自己提供hash模板，可以自己重载==运算符和hash计算函数。	





<h2>动态内存</h2>

___

c++支持动态分配对象，动态分配对象的生命周期与在哪里创建无关，只有手动释放，才会被销毁。

**静态内存：**保存局部static对象、类static数据成员、以及定义在函数之外的变量

**栈内存：**保存定义在函数内的非static对象

静态内存和栈内存的对象由编译器自动创建和销毁

除了这两部分，每个程序还有一个内存池用于存储动态分配对象，称为自由空间或者**堆**

动态内存管理：**new和delete**

但动态内存使用极易出现问题，所以c++引入了两种智能指针用于自动释放所指向的对象：

+ **shared_ptr:**允许多个指针指向同一个对象
+ **unique_ptr:**独占所指向的对象
+ **weak_ptr:**伴随类，是一种弱引用，指向shared_ptr管理的对象
+ 三种类都定义在memory中

**shared_ptr类：**

___

类似于vector，智能指针也是模板，需要给出类型信息：`shared_ptr<list<int>> p1;`

智能指针的使用方式与普通指针类似

shared_ptr独有操作有：

```c++
make_shared<T>(args); //返回一个shared_ptr,指向一个动态分配的类型为T的对象，用args初始化此对象
shared_ptr<T>p(q); //p是shared_ptr q的拷贝
p = q;
p.unique(); //若p.use_count()为1，返回true
p.use_count(); //返回与p共享对象的智能指针数量
```

**make_shared**

最安全的分配和使用动态内存的方法是调用该标准库函数，函数在**动态内存中分配一个对象并初始化他**，返回指向此对象的**shared_ptr**。函数用参数来构造给定类型的对象，类似于emplace

`shared_ptr<int>(auto) p = make_shared<int>(42);//指向一个值为42的int的shared_ptr`

**shared_ptr的拷贝与赋值**

当进行拷贝和赋值操作时，每个shared_ptr会记录有多少个shared_ptr与他指向相同的对象。

每个shared_ptr都有一个引用计数：当拷贝一个shared_ptr时，计数器递增，如**用一个ptr去初始化另一个ptr**、**将其作为参数传递给函数**、**作为函数的返回值**时；当给一个shared_ptr赋一个新值或是其被销毁(局部的ptr离开作用域)，计数器递减。

当计数器归0时，指针通过特殊的析构函数，释放自己管理的对象，例如：

```
auto r = make_shared<int>(42);
r = q; //此时r的引用计数为0，int42被释放
```

shared_ptr的析构函数会递减其引用计数；并在计数归0时销毁对象，释放内存。	



使用动态内存的原因：

+ 程序不知道自己要用多少对象：容器类
+ 程序不知道所需对象的准确类型
+ 程序需要在多个对象间共享数据：使用智能指针作为数据成员，保证在对象拷贝时，智能指针发生拷贝，但底层元素相同，当拷贝、赋值、销毁一个该类对象时，其shared_ptr成员也会随之被拷贝、赋值销毁。

**直接管理内存**

在堆中分配的内存是无名的，所以**new**无法为其分配对象名，只能返回一个该对象的指针

支持默认初始化、直接初始化、圆括号初始化、列表初始化、值初始化(后加括号)

对于定义了自己构造函数的类型(如string)来说，值初始化没有意义；但对于内置类型来说，值初始化的对象相较于未定义值要更好

`int *pi = new int(1025);`

`int *pi = new int();`值初始化

动态分配const对象：

`const int *pc1 = new const int(114);`

**内存耗尽**

当内存耗尽时，默认情况下，new会抛出一个**bad_alloc**异常(定义在头文件new中)，

使用定位new的方式，来阻止异常抛出，用一个空指针代替

`int *p1 = new (nothrow) int(42);`

**释放内存**

`delete p;`

+ 销毁给定指针指向的对象
+ 释放对应内存

提供的指针必须指向动态分配的内存或是一个空指针。

释放一块非new分配的内存并不合法

由内置指针管理的动态内存必须显式释放

**shared_ptr和new结合**

我们可以用new返回的指针来初始化智能指针

`shared_ptr<int> p2(new int(12));`

接受指针参数的智能指针构造函数是explicit的，所以**不能将一个内置的普通指针转换为智能指针**，即

```
shared_ptr<int> p1 = new int(42);是错误的
shared_ptr<int> p1(new int(42));才是正确的
```

**自定义释放操作**

当使用了分配资源但没有定义析构函数释放资源的类时，可以使用智能指针，并额外传递一个删除器函数，确保在调用结束后，资源能被正常释放，删除器必须接受单个类型的指针作为参数

`shared_ptr<connection> p (&c,end，_connection);//end_connection是删除器`

当p被销毁时，智能指针不调用delete，而他会调用end_connection进行销毁

**基本规范**

+ 不使用相同的内置指针初始化多个智能指针
+ 不delete get()返回的指针
+ 不使用get()初始化或reset另一个智能指针
+ 使用get（）时，切记当最后一个智能指针销毁后，指针变为无效
+ 如果使用的智能指针管理的资源不是new分配的内存，需要传入一个删除器确保资源正常释放



**unique_ptr类：**

___

一个时刻，只能有一个unique_ptr指向一个特定对象

当定义一个unique_ptr类时，需要将其绑定到一个new返回的指针上，采用直接初始化形式

unique_ptr不支持普通的拷贝、赋值，只能拷贝或赋值一个**将要被销毁的**unique_ptr

+ 可以置位nullptr，

+ 可以reset和release转移所有权

+ ```
  unique_ptr<string> p2(p1.release()); p1转给p2 p1置空
  
  unique_ptr<string> p3(new string("abc"));
  
  p2.reset(p3.release()); p3转给p2，释放p2原来指向的对象，p3置空
  ```

+ unique_str能作为函数返回值

unique_str默认情况下用delete释放它指向对象

传递删除器：

```
unique_ptr<objT,delT> p (new objT,fcn);
//p指向一个objT的对象，使用一个类型为delT的对象释放objT
//调用一个名为fcn的delT对象
例如：
unique_ptr<connection,decltype(end_connection)*> p (&c,end_connection);
```

**weak_ptr**

___

weak_prt是一种不控制所指向对象的智能指针，指向一个shared_ptr对象，但不会增加该指针的引用计数，当最后一个shared_ptr被销毁时，对象就会被释放，无论有无weak_ptr指向。

```c++
weak_ptr<T> w;
weak_ptr<T> w(sp);  //与shared_ptr sp指向相同对象的weak_ptr,T必须能转换成sp指向类型
w = p; p是一个shared_ptr或者weak_ptr
w.reset(); //将w置空
w.use_count(); //与w共享对象的shared_ptr的数量
w.expired(); //use_count返回0为true 否则为false
w.lock(); //如果expired为true，返回一个空的shared_ptr,否则返回一个指向w的对象的shared_ptr
```

**动态数组**

___

完成一次为很多对象分配内存的功能

动态数组并不是数组类型，不能使用begin或者end或者范围for

+ new分配数组

`int *pia =  new int[get_size()];` //得到一个数组元素类型的指针

和数组一样，能够使用括号，列表初始化

+ delete

使用一种特殊形式的delete，在指针前加上一个[]

`delete [] pa;`

按逆序销毁pa指向数组中的元素，并释放内存

**智能指针与动态数组**

标准库提供可以管理动态数组的unique_ptr，需要在类型后，跟一对[]

`unique_ptr<int[]> up(new int[10]);`

+ 不能使用点和箭头运算符
+ 可以使用下标运算符访问

**allocator类**

定义在头文件memory中，用于分离构造对象和内存分配（一般情况下，在构造一个数组时，会自动初始化里面的元素，但allocator能将其分离），因此其分配的内存都是未构造的，使用前必须用construct构造

allocator也是模板

```
allocator<string> alloc; //可以分配string的allocator对象
auto const p = alloc.allocate(n); //分配n个未初始化的string

--其他方法
a.deallocate(p,n); //释放从T*指针p中地址开始的内存，这块内存保存了n个T类型对象，用来释放全部内存，n必须和定义的时候的n一致
a.construct(p,args); //p是一个T*指针，arg被传递给类型为T的构造函数
a.destory(p); //p为T*类型的指针

---拷贝和填充算法
uninitialized_copy(b,e,b2); //b和e是容器类型迭代器，b2是指向动态内存元素指针
uninitialized_copy(b,n,b2); //b开始拷贝n个

uninitialized_fill(b,e,t); //在动态数组迭代器be之间创建对象，值均为t的拷贝
uninitialized_fill(b,n,t); //b开始拷贝n个
```

使用完毕后，需要对每个元素进行destroy来销毁他们



<h2>拷贝控制</h2>

___

c++允许控制类在发生拷贝、赋值、移动和销毁时该做什么：通过5个特殊的成员函数：**拷贝构造函数、拷贝赋值函数、移动构造函数、移动拷贝函数和析构函数实现**

<h4>拷贝、赋值、销毁</h4>

**拷贝构造函数**	

___

定义：如果一个构造函数的**第一个参数是自身类型的引用**，且**任何额外参数都具有默认值**，则该构造函数是拷贝构造函数：拷贝构造函数不能是explicit的

```c++
class Foo{
public:
	Foo();
	Foo(const Foo &);
}
```

**合成拷贝构造函数**

如果没有人为定义拷贝构造函数，编译器会自动合成一个拷贝构造函数，编译器将给定对象中的每个非static成员拷贝到正在创建的对象中。即：逐个成员拷贝

+ 直接初始化：`string a(10,'9');`编译器选择与我们提供的参数最匹配的构造函数
+ 拷贝初始化：`string b = a;`将右侧的对象，拷贝到左侧对象中，使用的是拷贝构造函数
+ 编译器可以（不是必须）绕过拷贝构造函数：将拷贝初始化转换为直接初始化

**拷贝初始化发生时机：**

+ 使用=定义变量
+ 将一个对象作为实参传递给一个非引用类型的形参
  + 非引用类型的参数需要进行拷贝初始化，因此拷贝构造函数自身必须是引用类型参数
+ 从一个返回类型为非引用类型的函数返回一个对象
+ 用花括号列表初始化一个数组中的元素或一个聚合类中的成员



**拷贝赋值运算符**

___

**重载运算符：**重载运算符本质上是函数，名字由**operator关键字后接表示要定义的运算符符号**组成。左侧运算对象绑定到隐式的this参数

```c++
class Foo{
public:
	Foo& operator={const Foo&}; //赋值运算符
	//返回一个指向其左侧对象的引用
}
```

**合成拷贝赋值运算符**：未定义拷贝赋值运算符时，编译器定义一个合成拷贝赋值运算符，将右侧的对象中的非static成员，以此拷贝到左侧对象中

```c++
//合成拷贝赋值运算符相当于下列拷贝赋值运算符
Sales_data& Sales_data::operator=(const Sales_data &rhs)
{
	bookNo = rhs.bookNo;
    units_sold = rhs.units_sold;
    revenue = rhs.revenue;
    return *this;
}
```



**析构函数**

___

析构函数用于释放对象使用的资源，销毁对象的非static数据成员

析构函数是类的一个**成员函数**，名字由**波浪号接类名**构成，没有返回值，不接受参数

```c++
class Foo{
public:
	~Foo(); //析构函数
}
```

构造函数首先按类定义顺序初始化成员，然后执行函数体

析构函数首先执行函数体，然后按初始化顺序逆序销毁成员

**调用析构函数的时机：**

无论何时，对象被销毁就会自动调用其析构函数

+ 变量在离开其作用域时
+ 当一个对象被销毁时，其成员也被销毁
+ 容器被销毁时，容器中的元素被销毁
+ 对于动态分配，当对指向他的指针delete时
+ 对于临时对象，创建它的完整表达式结束时

**合成析构函数**

当类未定义自己的析构函数时，编译器会定义一个合成析构函数；内容用于阻止该类型对象被销毁或者直接为空

因此，析构函数本身不直接销毁成员，而是析构函数体之后的隐含的析构阶段才进行的。

**基本原则**

+ 如果一个类需要自定义析构函数，它几乎一定要自定义拷贝赋值运算符和拷贝构造函数
+ 如果一个类需要一个拷贝构造函数，它几乎一定要定义拷贝赋值运算符，反之亦然



**阻止对象拷贝**

___

**default**

和构造函数一样，我们可以将拷贝控制成员定义为=default，来让他显式地要求编译器生成合成的版本

**阻止拷贝**

将拷贝构造函数和拷贝赋值运算符定义为删除的函数(deleted function)来阻止拷贝：

删除的函数：虽然声明了他们，但不能以任何方式使用，需要在参数列表后面加上=delete

```
struct Nocopy{
	Nocopy() = default;
	Nocopy(const Nocopy&) = delete; //阻止拷贝构造函数
	Nocopy& operator=(const Nocopy&) = delete;//阻止拷贝赋值构造函数
}
```

**析构函数不能是删除的**

当某个类中或者某个类中的成员类中定义了删除的析构函数，编译器将不允许创建该类的变量或临时对象，只能通过动态分配创建对象。

**合成的拷贝控制成员有可能是删除的**

+ 类的某个成员的析构函数为删除的时，该类的合成析构函数为删除的
+ 类的某个成员的拷贝构造函数是删除的或不可访问的时，该类的合成拷贝构造函数定义为删除的
+ 类的某个成员的析构函数是删除的或不可访问的，该类的合成拷贝构造函数也为删除的
+ 类的某个成员的拷贝赋值运算符为删除的或不可访问的，或类中有一个const或引用成员，该类的合成拷贝赋值运算符定义为删除的
+ 类的某个成员的析构函数为删除的或不可访问的，或者类有一个引用成员，它没有类内初始化器，或者类内有一个const成员，他没有类内初始化器且其类型未显式定义默认构造函数，该类的默认构造函数定义为删除的。

总结：如果一个类的数据成员有不能默认构造、拷贝、赋值、销毁的，则其对应成员函数为删除的



**拷贝控制和资源管理**

___

**行为像值的类**：拷贝一个对象时，新对象与原对象是完全独立的，改变副本不会对原对象有任何影响

+ 对于类管理的资源，每个对象都应该有一份自己的拷贝，例如下面的类就是一个类值

```c++
class HasPtr{
    public:
    HasPtr(const string &s = string()):ps(new string(s)),i(0); //构造函数
    HasPtr(const Hasptr &p):ps(new string(*p.ps)),i(p.i); //拷贝构造函数
    //保证了底层数据也进行了拷贝
    HasPtr& operator=(const HasPtr &);
    ~HasPtr() {delete ps;} //保证释放资源
   	private:
    string *ps;
    int i;		
}
//赋值运算符组合了析构函数和构造函数
HasPtr& HasPtr::operator=(const Hasptr &rhs)
{
    auto newp = new string(*rhs.ps);//拷贝右侧资源对象
    delete ps;//释放左侧资源对象
    ps = newp;//更新指针指向
    i = rhs.i;
}
```





**行为像指针的类：**拷贝一个对象时，新对象和原对象使用相同的底层数据，改变副本也会改变原对象

直接拷贝指针本身，不新建对象

**引用计数：**

+ 构造函数，创建一个计数器：置1
+ 拷贝构造函数：递增共享的计数器
+ 析构函数：递减计数器
+ 拷贝赋值运算符：递减左边的计数器，递增右边的计数器
+ 计数器为0时销毁

```c++
class HasPtr{
public:
    HasPtr(const string &s = string()):ps(new string()),i(0),use(new size_t(1)){} //构造函数，use为动态内存，表示引用计数
    //拷贝全部三个数据成员，并递增计数器
    HasPtr(const HasPtr &p):ps(p.ps),i(p.i),use(p.use){++*use;};
    //拷贝赋值运算符，完成构造和析构
    HasPtr& operator=(const Hasptr *rhs)
    {
        ++*rhs.use;
        if(--*use == 0)
        {
            delete ps;
            delete use;
        }
        ps = rhs.ps;
        i = rhs.i;
        use = rhs.use;
        return *this;
    }
    //析构 计数器为0的时候，说明要释放资源
    ~HasPtr()
    {
        if(--*use == 0)
        {
            delete ps;
            delete use;
        }
    }
    
}
```



**交换操作**

___

标准库的swap主要实现为拷贝对象并赋值

```
HasPtr temp = v1;
v1 = v2;
v2 = temp;
```

实际上，交换指针的方式要更好

```
string *temp = v1.ps;
v1.ps = v2.ps;
v2.ps = temp;
```

因此，我们自定义一个swap函数实现重载

```c++
inline void swap(HasPtr &lhs,HasPtr &rhs)
{
	using std::swap; //保证swap如果有更好的匹配时，会优先于std::swap
	swap(lhs.ps,rhs.ps);
	swap(lhs.i,rhs.i);
}
//我们也因此能够在赋值运算符中使用swap
HasPtr& operator=(HasPtr rhs) //这里传的不是引用，而是拷贝
    //这样就保证提前先拷贝右侧，再释放左侧
{
    swap(*this,rhs);
    return *this;
    //最后res会被释放掉，相当于释放左侧资源
}
```



**动态内存管理类**

某些类需要在运行时分配可变大小的内存空间，通常可以用标准库容器来管理其元素的底层内存，但当类需要自己进行内存分配时，需要定义自己的拷贝控制成员来管理所分配的内存。

实现一个标准库vector类的简化版本,制作一个用于string的vec类

```c++
class StrVec{
public:
    //默认构造函数
    StrVec():elements(nullptr),first_free(nullptr),cap(nullptr){}
    //拷贝构造函数
    StrVec(const StrVec &s)
    {
        auto newdata = alloc_n_copy(s.begin(),s.end());//申请新空间，拷贝元素，返回指针
        //分配的空间恰好容纳给定的元素
        elements = newdata.first;
        first_free = cap = newdata.second;
    }
    //拷贝赋值运算符
    StrVec& operator=(const StrVec &rhs)
    {
        auto data = alloc_n_copy(rhs.begin(),rhs.end());
        free();
        elements = data.first;
        first_free = cap = data.second;
        return *this;
    }
    //析构
    ~StrVec() { free();};
    //容器该具备的基本操作
    void push_back(const string &s)
    {
        chk_n_alloc();//先确保空间内容纳元素
        //allocator分配的内存空间是未构造的
        alloc.construct(first_free++,s);//用allocator成员来construct新的尾元素
    }
    size_t size() const {return first_free - elements;}
    size_t capacity() const {return cap - elements;}
    string *begin() const {return elements;}
    string *end() const {return first_free;}
private:
    static std::allocator<string> alloc; //分配元素
    void chk_n_alloc()  //是否还有剩余空间，没有就重新分配
    {
        if(size() == capacity())
            reallocate();
    }
    //当拷贝StrVec时，需要分配独立内存，并从原StrVec中拷贝元素至西对象，alloc_n_copy分配足够的内存来保存给定范围内的元素。并将这些元素拷贝到新分配的内存中，两个指针分别指向新空间开始的位置和拷贝的尾后位置
    pair<string*,string*> alloc_n_copy(const string* b,const string* e)
    {
       //分配空间以保存给定范围内的元素
        auto data = alloc.allocate(e - b);
        //初始化返回一个pair，该pair由data和uninitialized_copy的返回值构成
        //uninitialized_copy用于填充元素，b和e是容器类型迭代器，data是指向动态内存元素指针
        //返回最后一个构造元素之后的位置指针
        //因此这个pair，相当于返回了首尾指针
        return {data,uninitialized_copy(b,e,data)};
    }
    void free()//销毁元素，释放内存
    {
        if(elements)
        {
            for(auto p = first_free;p != elements;)
                alloc.destroy(--p);//逐个逆序销毁元素，运行string的析构函数
            alloc.deallocate(elements,cap-elements);//释放空间	
        }
    }
    void reallocate(); //获得更多内存并拷贝已有元素
    string *elements; //指向首元素的指针
    string *first_free; //指向数组第一个空闲元素的指针
    string *cap; //指向数组尾后位置的指针
}
```

**移动构造函数**

将资源移动，而非拷贝到正在创建的的对象中，标准库函数：**move**，定义在头文件utility中

对于上文的reallocate成员

```c++
void StrVec::reallocate()
{
	auto newcapacity = size()?2*size():1; //每次加倍
    auto newdata = alloc.allocate(newcapacity);
   	auto dest = newdata; //指向新数组中下一个空闲位置
    auto elem = elements; //指向旧数组中下一个元素
    for(size_t i = 0; i != size();i++)
        //调用move表示希望使用string的移动构造函数，没有move将使用string的拷贝构造函数
        alloc.construct(dest++,std::move(*elem++));
    	//string管理的内存不会被拷贝，我们构造的新string会从elem指向的string接管内存所有权
    free(); //释放旧内存空间
    //此时原来的string成员，已经不再管理它们曾经指向的内存
    elements = newdata;
    first_free = dest;
    cap = elements + first_free;
}
```

**对象移动**

___

​	**移动对象的原因：**

当对象被拷贝后立刻被销毁时，移动而非拷贝对象会大幅提升性能；

同时像IO类或unique_ptr这样的类，这些类中包含不能被共享的资源，只能被移动而不能被拷贝

**右值引用**

右值引用是必须绑定到右值的引用，使用&&

右值引用只能绑定到一个**将要销毁的对象**，绑定到要求转换的表达式，字面常量，或者返回右值的表达式上

```
int i = 42;
int &r = i;
int &&rr = i; //错误，不能将右值引用绑定到左值上
int &r2 = i*42; //错误，i*42是个右值
const int &r3 = i*42; //正确，可以将一个const引用绑定到一个右值上
int &&rr2 = i*42; //正确

int &&rr1 = 42;  //正确 42是字面常量，是右值
int &&rr2 = rr1; //错误 rr1是变量，变量是左值
```

**标准库move函数**

获得绑定到左值上的右值引用：相当于告诉编译器，想把左值作为右值处理

最好直接用std::move，避免潜在的名字冲突

```
int &&rr3 = std::move(rr1); //正确的
```

**移动构造函数和移动赋值运算符**

目的：让自定类型支持移动操作

**移动构造函数**

第一个参数类型是该类类型的右值引用，任何额外参数都必须有默认实参

必须确保移后源对象处于**销毁无害**状态，一旦资源完成移动，源对象必须不再指向被移动的资源

要加std，不知道书里为什么没加

```c++
//为StrVec类定义一个移动构造函数
StrVec::StrVec(StrVec &&s) noexpect:elements(s.elements),first_free(s.first_free),cap(s.cap)
{
    s.elements = s.first_free = s.cap = nullptr;
    //为什么这里用的不是std::move(s.elements)???
    //不用std的话，根本不是移动构造，依然是拷贝构造
}
//移动操作不应该抛出任何异常
//noexpect 通知标注库我们的构造函数不抛出任何异常 出现在参数列表之后和初始化列表开始之前
//需要在定义和声明中，都指明noexcept
//为什么需要noexcpet？标准库容器为了保证发生异常时保证自身不变，对元素进行拷贝而不是移动，这样在异常发生时，可以直接释放新的内存空间；如果想让vector在处理自定义类时移动而不是拷贝，必须显式声明noexpect
```

**移动赋值运算符**

移动赋值运算符执行与析构函数和移动构造函数相同的工作

```c++
StrVec& operator=(StrVec &&rhs) noexpect
{
    if(this != rhs) //自赋值
    {
        free();
        elements = rhs.elements;
        first_free = rhs.first_free;
        cap = rhs.cap;
        rhs.elements = rhs.first_free = rhs.cap = nullptr; //源对象可析构
    }
    return *this;
}
```

**合成移动操作**

当一个类自定义了拷贝构造函数、拷贝赋值运算符或析构函数，编译器不会合成其的移动构造函数或移动赋值运算符。

只有当一个类：未定义任何拷贝控制成员、类的每个非static数据成员均可移动时，编译器才会合成移动操作

合成移动操作定义为**删除的**的条件与合成拷贝操作类似。

+ 类成员的合成移动为删除的或不可访问的，类也是
+ 类的析构为删除的或不可访问的
+ 有类成员是const或是引用
+ 不同点：移动构造函数定义为删除的条件为：1.有类成员定义了自己的拷贝构造函数，但未定义移动构造函数；2.有类成员未定义自己的拷贝构造函数，且编译器不能为其合成移动构造函数

如果一个类定义了移动操作，那么其合成拷贝操作将被定义为删除的。

**移动右值、拷贝左值**

当一个类既有拷贝构造操作又有移动操作，编译器使用普通的函数匹配规则确定使用哪个函数

```
StrVec v1,v2;
v1 = v2;  //使用拷贝赋值
StrVec getVec(istream &); //getVec返回一个右值
v2 = getVec(cin); //使用移动赋值
```

当没有移动操作函数时，右值也会被拷贝

**正确的移动构造函数需要加std::move**,只有加了这个才会移动，而非拷贝，虽然不加调用了移动构造函数，但实际功能还是拷贝

```c++
Message::Message(Message &&m):contents(std::move(m.contents))
{
    move_Folders(&m);
}
```

**移动迭代器**

移动迭代器的解引用运算符生成一个右值引用

通过调用标准库的**make_move_iterator**函数，将一个普通迭代器变为移动迭代器

**右值引用与成员函数**

可以将一个成员函数，重载两个不同版本，一个用cosnt的左值引用，一个用右值引用：

例如push_back：第一个版本用于可转换为该类型的任何对象，第二版本用于非const的右值来窃取数据

```c++
void push_back(const string &);
void push_back(string &&);
```

**引用限定符**

阻止向右值赋值，在参数列表后，添加一个引用限定符(&或者&&)，只用于非静态函数，声明和定义中同时出现

对于&限定的函数，我们只能将它用于左值

对于&&限定的函数，我们只能将它用于右值

函数可以同时被const和引用限定符修饰，引用限定符需要跟在const之后	

```c++
class Foo{
public:
    Foo& operator=(const Foo&) &;//只能向可修改的左值赋值
};
Foo &Foo::operator=(const Foo &rhs) &
{
    return *this;
}
```





<h2>重载运算和类型转换</h2>

**基本概念**

重载运算符是具有特殊名字的函数，名字由关键字operator和其要定义的运算符号共同组成

调用方式：

+ 通过运算符号
+ 直接调用函数名称

不建议重载的运算符：逗号、取地址、逻辑与、逻辑或

**是否是成员函数：**、

+ 赋值、下标、调用和成员访问箭头，必须是成员函数
+ 改变对象状态的运算符或者与给定类型密切相关的运算符：如递增、递减或者解引用，通常是成员
+ 具有对称性的运算符：如算术、相等性、关系和位运算符，通常是非成员函数



<h4>输入输出运算符</h4>

**输出运算符**

输出运算符第一个形参是一个非常量的ostream对象引用；第二个形参是一个常量的引用。

operator<< 一般**返回它的ostream形参**

与iostream标准库兼容的输入输出运算符必须是**普通的非成员函数**，IO运算符通常被声明为友元

例如编写一个Sales_data类的输出运算符

```c++
ostream& operator<<(ostream &os,const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}
```

**输入运算符**

第一个形参是运算符将读取流的引用；

第二个形参是将要入的非常量对象的引用

返回某个给定流的引用

例如，编写一个Sales_data类的输出运算符

```c++
istream& operator>>(istream &is,Sales_data &item)
{
    double price;
    is >> item.booNo >> item.units_sold >> price;
    if (is) //是否输入成功
        item.revenue = item.units_sold * price;
    else
        item = Sales_data(); //输入失败：输入类型错误、到达文件末尾等等
  	return is;
}
```



<h4>算术和关系运算符</h4>

通常定义成**非成员函数**以允许对左侧或右侧的运算对象进行转换

**形参均为常量的引用**

最好**使用复合赋值来定义算术运算符**

例如：

```c++
Sales_data operator(const Sales_data &lhs,const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum += rhs; //使用了sum的复合赋值运算符
    return sum;
}
```

**相等运算符**

```c++
bool operator==(const Sales_data &lhs,const Sales_data &rhs)
{
    return lhs.isbn() == rhs.isbn() &&
        	lhs.units_sold == rhs.units_sold &&
        	lhs.revenue = rhs.revenue;
}
//一般来说，定义了相等，也需要定义不等
bool operator!=(const Sales_data &lhs,const Sales_data &rhs)
{
    return !(lhs == rhs);
}
```

**小于运算符**

通常是非成员函数

确保<和==不冲突 例子：

```c++
class myobj{
    friend bool operator< (const myobj &lhs,const myobj &rhs);
public:
    myobj(){};
    myobj(int value){val = value;};
    int val = 0;
};
bool operator<(const myobj &lhs,const myobj &rhs)
{
    return lhs.val - rhs.val;
}
int main(){
    myobj a = myobj(5);
    myobj b = myobj(6);
    cout<< a<b <<endl;
    system("pause");
    return 0;
}
```

**赋值运算符：**

必须是成员函数

例如为之前的StrVec类实现花括号的列表初始化的效果

返回左侧对象引用

a = {"a","b","c"}；

```c++
StrVec& StrVec::operator=(initializer_list<string> i1)
{
    auto data = alloc_n_copy(i1.begin(),i1.end()); //申请新的内存空间
    free(); //释放源空间
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}
```

**复合赋值运算符**

建议是成员函数

返回左侧对象引用

```c++
Sales_data& Sales_data::operator+=(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenus;
    return *this;
}
```

**下标运算符**

通常以访问元素的引用作为返回值、

最好定义两个版本，一个返回常量引用，一个返回非常量引用，作用于哪个对象返回哪种

```c++
class StrVec{
public:
	std::string& operator[](std::size_t n) {return elements[n];}
    const std::string& operator[](std::size_t n) const {return elements[n];}
}
```

**递增递减运算符**、

建议是成员函数

前置版本：

```c++
class StrBlobPtr {
public:
    StrBlobPtr& operator++();
    StrBlobPtr& operator--();
};
StrBlobPtr& StrBlobPtr::operator++()
{
    check(curr,"error"); //检查当前的指针是否有效,无效则抛出异常
    ++curr;
    return *this;
}
```

后置版本：

普通方法无法区分前置和后置的重载，因此后置引入一个：额外的int类型实参(不使用)

```c++
class StrBlobPtr {
public:
    StrBlobPtr& operator++(int);
    StrBlobPtr& operator--(int);
};
StrBlobPtr& StrBlobPtr::operator++(int)
{
    StrBlobPtr ret = *this; //记录当前值
    ++*this; //向前移动：调用前置版本
    return ret;//返回之前记录的状态
}
```

**成员访问运算符**

```c++
class StrBlobPtr
{
public:
	std::string& operator* const
    {
        auto p = check(curr,"dpe");
        return (*p)[curr]; //*p是对象所指的vector
    }
    std::string* operator->() const
    {
        return & this->operator*(); //返回解引用结果元素的地址
    }
}
```

因此对于point->mem表达式，根据point的类型不同，分别等价于：

+ 如果point是内置类型的指针：等价于(*point).mem;
+ 如果point是重载了->的对象：等价于point.operator->().mem;
  + 使用operator->()的结果来获取mem，如果**该结果本身就含有重载operator->()**（本身也是个重载了->的类？，重载运算符必须返回类的指针，或者自定义->的某个类的对象)，就重复调用当前步骤

**函数调用运算符**

___

必须是成员函数

允许我们像使用函数一样使用该类的对象

例如,定义一个返回绝对值功能

```c++
struct absInt{
    int operator()(int val) const{
        return val<0?-val:val;
    }
};
int main(){
    int i = 42;
    absInt absObj;
    int ui = absObj(i);
}
```

+ **lambda表达式其实也是一种函数对象**

对于lambda表达式，编译器将其翻译成一个未命名类的未命名对象，在lambda表达式产生的类的中，**重载函数调用运算符**，例如对于如下：

```c++
//lambda
stable_sort(words.begin(),words.end(),[](const string &a,const string &b){return a.size()<b.size();});
//上述的lambda表达式等价于形如下列出的类
class ShorterString{
public:
    bool operator()(const string &s1,const string &s2) const{
        return si.size()<s2.size();
    }
};
//当lambda使用了捕获时，如果是引用捕获，编译器可以直接引用而无需在生成的类中将其存储为数据成员
//如果是值捕获，值捕获的变量被拷贝到lambda中，因此需要在产生的类中为每个值捕获的变量建立对象数据成员、并创建构造函数
auto wc = find_if(words.begin(),words.end(),[sz](const string &s){return s.size()>=sz;});
//找到第一个长度不小于给定值的string
//生成的类形式如下
class SizeComp{
    public:
    SizeComp(size_t n):sz(n) {}//构造函数
    bool operator()(const string &s){
        return s.size()>=sz;
    }
    private:
    	size_t sz; //生成对应的数据成员
}

```

+ **标准库定义的函数对象**

标准库定义一组：表示算术运算符、关系运算符和逻辑运算符的类，重载调用函数并定义为模板形式，包含以下几种，定义在functional中

+ 算术运算符
  + plus<Type>
  + minus
  + multiplies
  + divides
  + modulus
  + negate
+ 关系运算符
  + equal_to
  + not_equal_to
  + greater
  + greate_equal
  + less
  + less_euqal
+ 逻辑运算符
  + logical_and
  + logical_or
  + logical_not

用法：

```c++
plus<int> intAdd; //可以用来实现int的加法
int sun = intAdd(10,20); //sum = 30;
//可以在算法中使用，用来改变默认的排序方式，实现传入一个lambda表达式的功能
vector<string> svec;
sort(svec.begin(),svec.end(),greater<string>()); //默认的是按升序排序，通过传入一个标准库函数对象，实现按降序排序
//同时，标准库函数对象定义良好，可以用来排序指针的内存地址，这在lambda表达式中是无法实现的
```

**可调用对象与function**

C++中的可调用对象包含：函数、函数指针、lambda表达式、bind创建的对象、重载了函数调用运算符的类

可调用对象的类类型不同、但可能具有相同的调用形式，如对于不同的函数、lambda表达式、类等，可能共用一种形式：int(int,int)：输入两个int、返回一个int

```c++
int add(int i,int j){return i+j};
auto mod = [](int i,int j){return i%j;}
struct divide{
    int operator()(int i,int j) {return i/j;}
};
```

因为类型不一致，无法同一存储在map中，引入**function**类型

**function**是一个模板，需要我们提供能够表示对象的调用形式，如

```c++
funciton<int(int,int)> f1 = add;
funciton<int(int,int)> f2 = divide();
funciton<int(int,int)> f3 = [](int i,int j){return i*J;};

cout<<f1(4,2)<<endl; //6
cout<<f2(4,2)<<endl; //2
cout<<f3(4,2)<<endl; //8
```

**二义性问题**

当出现函数重载时，将函数名字传入，会引起二义性问题

```c++
int add(int i,int j) {return i+j;}
Sales_data add(const Sales_data &,const Sales_data &);
funtion<int(int,int)> f1 = add; //不知道是哪个add
```

解决1：使用函数指针而非函数名

```c++
int (*fp)(int,int) = add;
function<int(int,int)> f1 = fp;
```

解决2：使用lambda表达式

```c++
function<int(int,int)> f2 = [](int a,int b){return add(a,b);}; //因为传入了2个int，所以只匹配接受两个int的add版本
```



<h4>类的类型转换</h4>

___

一个实参能够通过**非显式**的构造函数，隐式地转换为类类型；

反之，类类型也能通过**类型转换运算符**转换为其他类类型

```
operator /type/() const; type表示某种类型，面向任意类型除了void
```

+ 没有显示的返回值类型
+ 没有形参
+ 必须定义为类的成员函数
+ 不应改变对象的内容，因此应该为const的

**例子：**

```c++
class SmallInt{
public:
    SmallInt(int i = 0):val(i)
    {
        if(i<0 || i> 255)
            throw std::out_of_range("Bad SmallInt value");
    }
    operator int() const {return val;} //转换为int
private:
    std::size_t val;
}
int main(){
    SmallInt si;
    si = 4; //将4隐式转换为SmallInt，然后调用SmallInt的operator=
    si + 3; //将si隐式的转换为int，执行整数加法
}
```

编译器一次**只能执行一个用户定义的类型转换**，但是隐式的用户定义类型转换可以置于一个**标准类型转换之后或之前**

```
SmallInt si = 3.14; //double->int->si;
si + 3.14; //si->int->double
```

一般很少定义类类型转换运算符，容易发生意外效果，但可以定义**转向bool类型**的运算符（定义成显式的）

**显式类型转换运算符**

```
class SmallInt{
public:
	explicit operator int() const{return val;}
};
```

这样就不会进行隐式调用，但当出现以下情况时，**仍然会进行隐式调用**：

+ if、while及do语句开头部分
+ for语句头的条件表达式
+ ！、||、&&的运算对象
+ ?:的表达式

二义性：

+ 两个类提供了相同的类型转换：A提供了接受B类的转换构造函数、B提供了转换为A类的转换运算符
+ 类定义了多个转换规则，它们的转换目标类型本身可以通过其他类型联系在一起（当定义多个算术类型）



<h2>面向对象程序设计</h2>

**基类**

设计一个基类用于演示

```c++
class Quote{
public:
    Quote() = default;
    Quote(const string &book,double sales_price):bookNo(book),price(sales_price){}
    string isbn() const { return bookNo;}
    //返回给定数量数据销售总额，由派生类负责改写
    virtual double net_price(size_t n) const{
        return n*price;
    }
    virtual ~Quote() = default; //对析构函数动态绑定
    //作为继承关系的中的根节点通常都会定义一个虚析构函数
private:
    string bookNo;
public:
    double price = 0.0;
};
```

**虚函数：**基类希望派生类进行覆盖的函数，在成员函数声明语句之前加上关键字virtual，**任何构造函数之外的非静态函数都可以是虚函数**

**动态绑定：**通过使用动态绑定，我们能使用同一段代码分别处理Quote和Bulk_quote对象

<h4>定义派生类</h4>

___

+ 派生类必须通过使用**类派生列表**指出他是从哪个类继承而来的，其形式为：一个冒号，后面紧跟以逗号分割的基类列表，其中每个基类前可以用三种访问符中的一个

+ 派生类声明**不包含它的派生列表**：与基类一样

+ 作为基类的类，必须**已经定义**，而非仅声明
+ 如果不想类被继承，可以在类名后加一个**final**
+ 一个类可以同时作为基类和派生类：Base->D1->D2

+ 派生类必须将其继承而来的成员函数中需要覆盖的部分重新声明。

```c++
class Bulk_quote:public Quote{
public:
	Bulk_quote() = default;
    Bulk_quote(const string &,double,size_t,double);
    //覆盖基类的函数
    double new_price(size_t) const override;
private:
    size_t min_qty = 0; //最低购买量
    double discount = 0.0 //折扣额
};
```

允许将：派生类的对象当成基类对象、将基类指针或引用绑定到派生类对象的基类部分上

**派生类构造函数**

派生类必须**使用基类的构造函数**来初始化它的基类部分，通过**构造函数初始化列表**来将实参传递给基类构造函数，上述中的4参数构造函数如下：

```c++
Bulk_quotr(const string &book,double p,size_t qty,double disc):Quote(book,p),min_qty(qty),discount(disc){};
```

将其前两个参数传递给了基类Quote的构造函数

派生类能够访问基类的**public成员**及**protected成员**

重写net_price

```c++
double Bulk_quote::new_price(size_t cnt) const
{
    if(cnt >= min_qty)
        return cnt*(1-discount) * price;
    else
        return cnt*price; //price是基类成员，但派生类也可以使用
}
```

**静态成员**

如果基类定义了一个静态成员，则在整个继承体系中只存在该成员的**唯一定义**，如果某静态成员是可访问的，我们既能通过基类也能通过派生类来使用它

<h4>类型转换</h4>

注意：类型转换只发生于引用和指针类型

允许将基类的指针或引用绑定到派生类对象上，因此，在**使用基类的引用或指针**时，不清楚该引用或指针绑定对象的真实类型

不存在基类向派生类的隐式类型转换，即使一个基类指针或引用绑定在一个派生类对象上了也不行：

如果基类含有一个或多个虚函数，可以使用dynamic_cast来请求一个类型转换，检查将在运行时执行

当确认安全时，可以使用static_cast来强制转换

```
Quote &a = bulk1;
Quote &b = *bulk2;
```

**静态类型：**编译时已知，为变量声明时的类型或表达式生成的类型

**动态类型：**运行时可知，为变量或表达式表示的内存中的对象类型

上面的例子中：指针和引用的静态类型是Quote，但动态类型是Bulk_Quote

当表达**式既不是引用，也不是指针**时，其动态类型与静态类型**永远一致**

当我们使用一个派生类对象**为一个基类对象初始化或赋值**时，只有该派生类对象中的**基类部分**会被**拷贝、移动、赋值**，派生类部分将被**忽略**

<h4>虚函数</h4>

___

由于**动态绑定**，直到运行时才能知道到底调用了哪个版本的虚函数，直到运行时必须为**每个虚函数提供定义**

当某个**虚函数通过指针或引用调用时**，**被调用的函数**是**与绑定到指针或引用上的对象的动态类型**匹配的那个

当通过普通类型表达式调用虚函数时，在编译时就会将调用的版本确定下来

**回避机制：**使用作用域运算符，强制规定执行**虚函数的某个函数版本**

+ `double undiscounted = baseP->Quote::net_price(42);//强制执行Quote中的net_price而不是Bulk_quote`

```c++
//定义一个打印函数
double print_total(ostream &os,const Quote &item,size_t n)
{
    //根据传入item形参的对象类型调用对应的net_price
    double ret = item.net_price(n);
    os << "ISBN: " << item.isbn() << " # sold: " << n << " total due: " << ret << endl;
    return ret;
}
Quote base("0-201-82470-1",50);
Bult_quote derived("0-201-82470-1",50,5,.19);
//因为item是引用，而net_price是虚函数，所以运行哪个版本依赖于运行时绑定到item的实参的动态类型
print_total(cout,base,10); //调用Quote的net_price
print_total(cout,derived,10); //调用Bulk_quote的net_price
```

**override和final**

final和override说明符出现在形参列表（包括任何const或引用修饰符）以及尾置返回类型之后

当派生类中函数**被override的修饰**时，基类函数中必须有相对应的**虚函数**

当基类中的函数**被final修饰**时，派生类中的函数不能**覆盖**该函数



<h4>抽象基类</h4>

___

**纯虚函数：**没有实际意义，无需定义，在函数体的位置声明=0。

**抽象基类：**包含纯虚函数的基类称为抽象基类

抽象基类负责定义接口，其他类覆盖该接口，因此不能直接创建一个抽象基类对象

下面定义一个抽象基类：表示所有书籍打折策略的的公共接口

```c++
class Disc_quote:public Quote{
public:
    Disc_quote() = default;
    Disc_quote(const string &book,double price,sizt_t qty,double disc):Quote(book,price),quantity(qty),discount(disc){}
    //纯虚函数
    double net_price(size_t) const = 0;
protected:
    size_t quantity = 0; //购买量
    double discount = 0.0;
};
//重新定义Bulk_quote
class Bulk_quote:public Dis_quote{
public:
    Bulk_quote() = default;
    //Bulk_quote的新直接基类是Dis_quote，调用直接基类的构造函数进行初始化
    Bulk_quote(const string &book,double price,size_t qty,double disc):Disc_quote(book,price,qty,disc){}
    double net_price(size_t) const override;
}
```

<h4>访问控制与继承</h4>

____

**protected**

+ 对用户**不可访问**
+ 对派生类的成员和友元允许访问
+ 派生类的成员或友元只能**通过派生类对象**来访问基类的受保护成员，而不能通过基类对象访问



**派生访问说明符：**

种类：公有继承、私有继承、受保护继承

控制派生类用户对于基类成员访问权限，不影响派生类的成员能否访问直接基类的成员

**下面既包括public 也包括protected**

公有继承：继承自基类的成员是**public**的

+ Public继承意味着is-a

私有继承：继承自基类的成员是**private**的，对于该派生自该类的新类来说，这些基类成员依然是私有的，无论采用何种方式继承

+ Private继承意味着implemented-in-terms-of：根据某物实现出；能使用复合最好用复合而不是private继承
+ 用途：当不是is a关系的两个class，其中一个需要访问另一个的protected成员，或重新定义一个或多个virtual函数

受保护继承：继承自基类的成员是**protected**的

**派生类向基类的转换的可访问性：**

+ 当继承方式为公有继承，用户代码（创建对象，调用函数）才能使用派生类向基类转换，反之不行
+ 无论何种继承方式，派生类的成员函数和友元（定义函数）都能使用派生类向基类的转换
+ 当继承方式为公有或受保护继承，派生类D的派生类成员和友元可以使用D向基类的转换，反之不行
+ 这个派生类向基类的转换：大概是可以理解为使用基类的可访问成员

**友元关系**

**友元关系不可继承**

每个类负责控制自己成员的访问权限，因此，基类的友元依然能够**通过派生类对象访问基类成员**，包括Base对象内嵌在其派生类对象中的情况。

例如：

```c++
class Base {
    friend class Pal; //Pal在访问Base的派生类时不具有特殊性
}
class Pal{
public:
    int f(Base b) { return b.prot_mem; } //Pal是基类友元，能访问基类的受保护对象
    //Sneaky为Base的公有继承派生类
    int f2(Sneaky s) { return s.j; } //错误！因为Pal不是Sneaky的友元，无法访问它的私有对象
    //对基类的访问权限由基类本身控制，即使对于派生类的基类部分也是如此
    int f3(Sneaky s) { return s.prot_mem; } //正确 Pal是Base的友元
}
//但友元关系无法继承
class D2:public Pal{
public:
    int mem(Base b) { return b.prot_mem; } //错误，因为D2不是Base都友元，无法访问Base的受保护或者私有成员
}
```

**改变个别成员的可访问性**

使用**using**声明改变派生类继承的某个名字的**访问级别**：在类的内部使用using语句，可以**将该类的直接或间接基类中的任何可访问成员**标记出来，using声明语句中的访问权限由该using声明语句**之前的访问说明符决定**

例如：

```c++
class Base{
public:
    size_t size() const (return n;)
private:
    size_t n;
}
class Derived:private Base{ //私有继承，因此size和n本来应该都是Derived的private
public:
    using Base::size; //将size的访问权限由private改为public，该类的所有用户都能访问它
protected:
    using Base::n; //将n的访问权限由private改为protected，该类的成员、友元和派生类都能访问它
}
```

默认情况下：

+ 使用class定义的派生类是私有继承
+ 使用struct定义的派生类是公有继承
+ 因此：class和struct唯一的差别，就是**默认成员访问说明符及默认派生访问说明符**

<h4>类作用域</h4>

___

派生类的作用域嵌套在基类作用域之内，因此，当一个名字在派生类中找不到时，会以此往上寻找它的基类、间接基类等

因此：**一个对象、引用或指针的静态类型，决定了该对象哪些成员可见**

当将一个派生类对象赋给一个静态类型**为基类的指针或引用时**，作用域中的搜索将直接从**基类开始**，此时即使实际对象是派生类，也**无法找到派生类中的成员**（因为它在基类的作用域中根本不存在），但**如果通过名字检查**：并且动态绑定虚函数，此时将**根据动态类型**确定调用哪个版本的虚函数。

例如：

```c++
Bulk_quote bulk;
Quote *itemp = &bulk;
//此时的itemp将无法访问Disc_quote和Bulk_quote中特有对象
//假设Base->D1->D2
class Base{
public:
    virtual int fcn();
};
class D1:public Base {
public:
    int fcn(int);
    virtual void f2();
};
class D2:public D1 {
public:
    int fcn(int);
    int fcn();
    void f2();
};
Base bobj;D1 d1obj;D2 d2obj;
Base *bp1 = &bobj,*bp2 = &d1obj,*bp3 = &d2obj;
bp1->fcn(); //调用Base::fcn
bp2->fcn(); //调用Base::fcn D1中没有匹配参数类型
bp3->fcn(); //调用D2::fcn 根据动态类型确定
D1 *d1p = &d1obj;D2 *d2p = &d2obj;
bp2->f2(); //通不过名字检查 Base里没有f2
d1p->f2(); //调用D1::f2
d2p->f2(); //调用D2::f2
////如果fcn是非虚函数
Base *p1 = &d2obj;D1 *p2 = &d2obj; D2 *p3 = &d2obj; //都指向D2对象
//此时不发生动态绑定
p1->fcn(42); //错误 Base中没有接受int的fcn
p2->fcn(42); //D1::fcn(int);
p3->fcn(42); //D2::fcn(int);

```

总体流程为：

+ 先确定静态类型
+ 在静态类型中查找所需的名字，如果找不到，就在其直接基类中查找，以此直到到达继承链顶端
+ 一旦找到mem，就进行类型检查
+ 如果调用合法，将根据调用的是否是虚函数产生不同代码（根据动态类型）

**名字查找优先于类型检查：**当派生类成员和基类成员**同名时**，派生类作用域内会**隐藏基类成员**，即使形参列表不一致，也会隐藏，因此基类和派生类中的虚函数必须具有相同的形参列表，否则将永远无法正确调用。

**覆盖重载的函数**

成员函数无论是否是虚函数均可被重载，默认情况下，如果派生类希望重载其中某个版本，那么其他版本会被隐藏，此时必须覆盖基类中的每一个版本。

**解决方案：**为重载的成员提供一条using声明语句，就无须覆盖基类中的每一个重载版本，访问未重新定义的重载版本实际上是对using声明点的访问。

<h4>构造函数和拷贝控制</h4>

___

**虚析构函数**

情况：删除一个基类对象的指针时，指针实际指向的是一个派生类对象，此时应该执行派生类的析构函数而不是基类的析构函数，否则将产生未定义行为。

因此，我们应将**基类中析构函数定义成虚函数**以确保执行正确的析构函数版本

**拷贝构造函数**

与默认构造函数类似：**合成的派生类拷贝构造函数使用直接基类的合成拷贝构造函数**，之后依次往上

例如：Bulk_quote的合成拷贝构造函数使用Disc_quote的合成拷贝构造函数，后者使用Quote的合成拷贝构造函数。

派生类的拷贝和移动构造函数在**拷贝和移动自有成员**的同时，也要**拷贝和移动基类部分成员**（在初始值列表中，使用基类的拷贝和移动构造函数）

```c++
//使用对应的基类构造函数初始化对象的基类部分
class Base{};
class D:public Base {
public:
    D(const D& d):Base(d){/* ... */}
    D(D&& d):Base(std::move(d)) {/*..*/}
};
```

Base(d)：**将一个D对象传给基类的构造函数**，Base(d)一般会**匹配Base的拷贝构造函数**，d被绑定到该构造函数的Base&形参上，将d的基类部分拷贝给要创建的对象

**移动操作**

基类通常不含有合成的移动操作，因此它的派生类中也没有合成的移动操作

当需要移动操作时，应先在基类中定义，即使是合成的版本，也应该显式地定义这些成员。

**拷贝赋值运算符**

同样，也需要调用基类的赋值运算符为对象赋值

```c++
D &D::operator=(const D &rhs)
{
    Base::operator=(rhs); //调用基类的拷贝赋值运算符为基类部分赋值
    //按自己的方式处理
    return *this;
}
```

**析构函数**

与构造函数和赋值运算符不同，析构函数只负责销毁由派生类自己分配的资源，会自动调用基类的析构函数



初始化时：**基类首先被创建**，此时派生类对象是**未完成**的状态

析构时：**派生类部分首先被销毁**，此时派生类对象也是**未完成**状态

因此：当在构造函数或者析构函数中调用了**虚函数**的时候，需要将**该对象的类和构造函数/虚函数的类当做同一个类**，此时执行的是与构造或析构函数所属类型相对的虚函数版本。否则将出现调用未完成部分的虚函数的状况。



**构造函数继承**

一个类只能“继承”其直接基类的构造函数，类不能继承默认、拷贝和移动构造函数，如果派生类中未定义，编译器将会合成他们。

通过**注明直接基类名的using声明语句**，提供派生类继承基类构造函数的方式，但不会改变访问级别

```c++
class Bulk_quote:public Disc_quote{
public:
    using Disc_quote::Disc_quote; //当using作用于构造函数时，using将令编译器产生代码
    //生成的构造函数形如：derived(parms) : base(args) {} derived是派生类名字、base是基类名字，parms是构造函数形参列表，args将派生类构造函数的形参传递给基类构造函数。
    double net_price(std::size_t) const;
};
```

此时如果派生类有自己的数据成员，这些成员将被初始化。

基类构造函数的默认实参**不会被继承**，**派生类**中将**生成多个继承的构造函数**，其中每个构造函数分别省略一个含有默认实参的形参

如果基类有**多个构造函数**，派生类大多数会继承所有构造函数，但存在例外：

+ 当派生类定义的构造函数与基类的构造函数具有相同参数列表时，该构造函数不会被继承
+ 默认、拷贝和移动构造函数不会被继承



<h4>容器与继承</h4>

向一个存储类型为基类的容器传入一个派生类对象，其**派生类对象将被忽略**

**解决方案：**在容器存放**基类的指针/智能指针**





<h2>模板与泛型编程</h2>

___

<h4>定义模板</h4>

**函数模板**

___

模板定义以关键字**template**开始，后跟一个模板参数列表，一个逗号分隔的一个或多个模板参数的列表，用<>包围起来

模板参数表示：在类或函数定义中用到的类型或值，当使用模板时，指定模板实参，将其绑定到模板参数上

inline和constexpr说明符可以跟在模板参数列表之后

```c++
template <typename T> //类型参数必须加typename或者class
int compare(const T &v1,const T &v2)
{
    if(v1 < v2) return -1;
    if(v2 < v1) return 1;
    return 0;
}
```

**实例化参数模板**

当调用一个函数模板时，编译器用函数实参为我们推断模板实参

`cout<<compare(1,0)<<endl; //此时T为int`

编译用推断出的模板参数，为我们实例化一个特定版本的函数

**非类型参数：**一个非类型参数表示一个值而非一个类型，通过**特定的类型名**指定，在模板被实例化时，非类型参数被用户提供或编译器推断的值代替

非类型参数的模板实参必须是**常量表达式**

```c++
template<unsigned N,unsigned M>
int compare(const char (&p1)[N],const char (&p2)[M]) //用于指定数组大小
{
    return strcmp(p1,p2);
}
//调用
compare("hi","mom"); //此时N=3，M=4 空字符+1

```

模板直到实例化时才会生成代码，关于类型的错误将在实例化时报告



**类模板**

编译器不能为类模板推断模板参数类型

```c++
template <typename T> class Blob{
    Blob();
    Blob(std:initializer_list<T> il);//在模板中使用其他模板类型
    ...
}
```

当用户实例化Blob，T就会被替换为特定的模板实参类型

在类模板自己的作用域中，我们可以直接使用模板名T，并且编译器在处理自身引用时：就好比**我们提供了与模板参数匹配的实参**一样。

```c++
//在类模板作用域内
BlobPtr& operator++();
//就好比我们提供了模板参数
BlobPtr<T>& operator++();
```

**体外成员函数的函数体内**也算类的作用域

**类模板的成员函数**

既可以定义在类模板内部（将被声明为内联函数），也可以定义在类模板外部（必须以关键字template开始，说明成员属于哪个类，名字中必须也包含模板实参）

```c++
template <typename T> 
return-type Blob<T>::member_name(parms);
```

构造函数也一样

对于一个实例化了的类模板，成员函数只有在**被使用到的时候才会实例化**，不用就不实例化。包括重载运算符的成员函数

->因此，即使某种类型**不完全符合模板要求**，我们也能实例化此类

**友元**

___

类与友元各自是否是模板**无关联**：

+ 如果一个类模板包含一个非模板友元，该友元可以访问所有模板实例
+ 如果友元自身是模板，类可以授权给所有模板实例，也可以授权给特定实例

**一对一友好关系**

对应实例及其友元间的友好关系

为引用模板的一个特定实例，我们必须先声明模板自身。

```c++
//为Blob类将BlobPtr类和一个模板版本的Blob相等运算符定义为友元
template <typename> class BlobPtr;
template <typename> class Blob; //运算符==中的参数所需要
template <typename T> bool operator==(const Blob<T> &,const Blob<T> &);

template <typename T> class Blob {
    friend class BlobPtr<T>;
    friend bool operator==<T>(const Blob<T> &,const Blob<T> &);
    //每个Blob的实例，将相同类型(T)实例化的BlobPtr和相等运算符作为友元
    //将友好关系限定在同类型实例化的BlobPtr和相等运算符之间
};
```

**通用和特定的模板友好关系**

一个类能够将另一个模板的**每个或特定实例**都声明为自己的友元

```c++
//前置声明,在将模板的一个特定实例声明为友元时使用
template <typename T> class Pal;
class C{//一个普通的非模板类
    //用类C实例化的Pal类是C的一个友元
    friend class Pal<C>;
    //Pal2的所有实例都是C的友元
    template <typename T> friend class Pal2;
}

template <typename T> class C2 {//一个模板类
    //C2的每个实例都将相同类型实例化的Pal声明为友元
    friend class Pal<T>;
    //Pal2的每个实例都是C2的每个实例的友元，无需前置声明
    template <typename X> friend class Pal2;//必须使用与类模板本身不同的模板参数
    //Pal3是一个非模板类，是C2所有实例的友元
};

```

**令模板自己的类型参数成为友元**

将模板类型参数声明为友元

```c++
templete <typename Type> class Bar {
    friend Type; //将访问权限授予用来实例化Bar的类型
};
//例如：声明Bar<Foo>后，Foo将成为Bar<Foo>的友元
```

**模板类型别名**

对于特定的实例化类型，可用typedef直接起别名

`typedef Blob<string> StrBlob;`

无法定义一个typedef引用Blob<T>

允许为类模板定义一个类型别名：

```c++
template<typename T> using twin = pair<T,T>;
twin<string> authors; //此时的authors是一个pair<string,string>
//将twin定义为成员类型相同的pair的别名
//同时也可以固定一个或多个模板参数
template <typename T> using partNo = pair<T,unsigned>;
partNo<string> books; //books是一个pari<string,unsigned>
```

**静态成员**

所有的模板实例对象都有一个**自己独有的静态成员**

定义、初始化及使用时，必须引用一个特定的实例

一个static成员函数只有在使用时才会实例化

```c++
template <typename T> size_t Foo<T>::ctr = 0; //定义并初始化ctr
auto ct = Foo::ctr; //错误 没加实例
```



**模板参数**

___

模板参数遵循普通的作用域规则，内层的模板参数会隐藏外层作用域中声明的相同名字。

在模板内，**不能重用模板参数名**

一个模板参数名，在一个特定模板参数列表中只能出现一次

```c++
typedef double A;
template <typename A,typename B> void f(A a,B b)
{
    A tmp = a; //隐藏外层的double类型
    double B; //错误，重声明了模板参数B
}
```

一个给定模板的每个声明都必须有相同数量和种类的的参数

```c++
//声明中的模板参数的名字不必与定义相同
//下面三个声明和定义，其实是一个函数模板
template <typename T> T calc (const T&,const T&);
template <typename U> U calc (const U&,const U&);
template <typename Type>
Type calc(const Type &a,const Type &b) { /***/}
```



**使用类的类型成员**

问题：对于模板类型参数T`T::size_type * p; //编译器不知道size_type是一个类型还是一个变量`

C++假定通过作用域运算符访问的名字不是类型，如果想指定类型，必须显式地告诉编译器这是一个类型：**使用关键字typename**

```c++
template <typename T>
typename T::value_type top(const T &c)
{
    if (!c.empty())
        return c.back();
    else 
        return typename T::value_type();
}
```



**默认模板实参**

新标准中，可以为函数和类模板提供默认实参

```c++
template <typename T,typename F = less<T>> //为模板参数提供默认实参
int compare(const T &v1,const T &v2,F f = F()) //为对应的函数参数提供默认实参 f是类型F的一个默认初始化对象
{
    if (f(v1,v2)) return -1;
    if (f(v2,v1)) return 1;
    return 0;
}
```

为模板添加了第二个类型参数，名为F，表示可调用对象的类型；定义了一个函数参数f，绑定到了一个可调用对象上

无论何时使用一个类模板，都需要在模板名后接尖括号，但当一个类模板为其**所有模板参数**都提供了**默认实参**，当希望使用这些默认实参时，可以在模板名之后跟一个空的尖括号对

`Numbers<> aaa;`



**成员模板**

___

一个类可以包含本身是模板的成员函数，称为**成员模板**

**普通类的成员模板**

```c++
class DebugDelete{
public:
    DebugDelete(ostream &s = cree): os(s) {}
    template <typename T> void operator()(T *p) const
    {
        os << "deleting unique_ptr" << endl;
        delete p;
    }
private:
    ostream &os;
};
double *p = new double;
DebugDelete d;
d(p);
int *ip = new int;
DebugDelete()(ip);//在一个临时对象上调用重载调用运算符
```

**类模板的成员模板**

类成员可以各自有自己的独立模板参数

```c++
template <typename T> class Blob {
    template <typename It> Blob(It b,It e);
};
//成员模板是函数模板
//当在类模板外定义一个成员模板时，必须同时为类模板和成员模板提供模板参数列表
template <typename T>  //类的类型参数
template <typename It>  //成员模板的类型参数
Blob<T>::Blob(It b,It e):data(make_shared<vector<T>(b,e)) {}
```

编译器根据传递给成员模板的函数实参来推断它的模板实参



**显式实例化**

避免多个源文件使用相同的模板，并提供相同模板参数，导致每个文件中都有一个该模板实例所带来的开销

显式实例化形式：

```
extern template declaration; //实例化声明
template declaration; //实例化定义
//declaration是一个类或函数声明，其中所有模板参数都被替换为模板实参
//eg
extern template class Blob<string>; //声明
template int compare(const int &,const int &); //定义
```

当编译器遇到extern模板声明时，不会在本文件中生成实例化代码；表示承诺在程序其他位置有该实例化的一个非extern声明

extern声明必须出现在任何使用此实例化版本的代码之前

```c++
extern template class Blob<string>;
Blob<string> sa1,sa2; //不会实例化Blob<string>，定义将在程序中的其他位置出现
Blob<int> a1 = {0,1,2,3,4,5}; //Blob<int>和对应的初始化列表构造函数在本文件中实例化

//实例化文件必须为每个在其他文件中声明为extern的类型和函数提供一个定义
template class Blob<string>; //实例化模板的所有成员
```

**实例化定义会实例化所有成员**



<h4>模板实参推断</h4>

___

**模板实参推断：**从函数实参来确定模板实参

**隐式类型转换：**

+ 仅支持下列两个转换类型

+ 支持非const对象的引用或指针传递给一个const的引用或指针形参

+ 如果函数形参不是引用类型，数组实参->指向首元素的指针、函数实参->该函数类型的指针

+ ```c++
  template <typename T> T fobj(T,T);
  template <typename T> T fref(const T&,const T&);
  string s1("abc");
  const string s2("and");
  fobj(s1,s2); //f(string,string) 忽略顶层const
  fref(s1,s2); //f(const string&,const string&)
  int a[10],b[42];
  fobj(a,b); //f(int*,int*)
  fref(a,b); //不合法
  ```

+ 算术转换、派生类向基类转换、用户定义转换均**不能用于函数模板**

如果函数模板的**函数参数类型**不是模板参数，则对实参进行**正常的类型转换**



**函数模板显式实参**

___

**指定显式模板实参**

我们希望允许用户指定结果的类型

```c++
template <typename T1,typename T2,typename T3>
T1 sum(T2,T3); //此时无法推断T1的类型，因为T1不在函数的实参列表种
//因此，要为T1提供一个显式模板实参
auto val3 = sum<long long>(int a,long b); //long long sum(int,long)
```

对于用模板类型参数已经显式指定的函数实参，允许进行**正常的类型转换**

```c++
long lng;
compare(lng,1024); //非法，类型不一致
compare<long>(lng,1024); //实例化compare(long,long)
compare<int>(lng,1024); //实例化compare(int,int) lng被转换为int
```

**尾置返回类型**

无法得知返回结果的准确类型，但可以由函数实参中表示出

```c++
template <typename It>
auto fcn(It beg,It end) -> decltype(*beg)  //返回一个*beg类型
```

**标准库类型转换模板**

定义在type_traits头文件中

+ remove_reference `remove_reference<int&>::type 得到一个 int 获得被引用类型`
+ 其他的很多



**左值、右值引用推断**

```c++
template <typename T>void f1(T&); //实参必须是左值
f1(i); //i是一个int，T是int
f1(ci); //ci是一个const int，T是const int
f1(42); //错误 不接受右值
template <typename T> void f2(const T&); //左右值都可以
f2(i); //T是int
f2(ci); //T依然是int，因为const是函数参数类型的一部分，所以不会是模板参数类型的一部分
f2(5); //T是int
teplate <typename T> void f3(T&&);
f3(42);//实参是一个int右值，T是int
f3(i); //T是int& 引用折叠
//如果函数参数是一个指向模板类型参数的右值引用，则他可以被绑定到一个左值
//此时实参类型将是一个左值引用
```

**引用折叠：**间接创建一个**引用的引用**，在所有情况（除了一种）下，引用会折叠成一个普通的左值引用类型，当引用为右值引用的右值引用时，才会折叠成右值引用

+ `X& &、X& &&、X&& &都折叠成X&`
+ `X&& &&折叠成X&`

**转发：**某些函数将其一个或多个实参连同类型不变地转发给其他函数（保留顶层const和引用）

```c++
//错误的
template <typename F,typename T1,typename T2>
void flip1(F f,T1 t1,T2 t2) //无法保留顶层const和引用
{
    f(t2,t1);
}
```

**将一个函数参数定义为一个指向模板类型参数的右值引用，我们可以保持其对应实参的所有类型信息，包括左右值属性；使用引用参数，使我们可以保持const属性**

```c++
//一半正确的
template <typename F,typename T1,typename T2>
void flip2(F,f,T1 &&t1,T2 &&t2) //右值引用   无法接受右值引用参数的函数
{
    f(t2,t1)
}
```

**std::forward：**通过显式模板实参调用，forward返回该显式实参类型的右值引用：`forward<T> 返回T&&`

```c++
//正确的
template <typename F,typename T1,typename T2>
void flip2(F,f,T1 &&t1,T2 &&t2)
{
    f(std::forward<T2> t2,std::forward<T1> t1);
}
```



<h4>模板重载</h4>

___

函数模板可以被另一个模板或一个普通非模板函数重载：名字相同的函数必须具有不同数量或类型的参数

函数匹配机制有所变化：

+ 对于一个调用，候选函数包括所有模板实参推断成功的函数模板实例
+ 候选的函数模板总是可行的，因为模板实参推断会排除任何不可行的模板
+ 可行函数按类型转换来排序
+ 如果恰有一个函数提供最好的匹配，选择此函数
  + 同样好的函数中非模板函数优先
  + 同样好的函数中，更特例化的函数优先

<h4>可变参数模板</h4>

**可变参数模板：**接受可变数目参数的模板函数或模板类，可变数目的参数称为参数包（模板参数包、函数参数包）

**表示方法：**	用一个省略号来指出模板参数或函数参数表示一个包：

在模板参数列表中，class...和typename...指出接下来的参数表示0或多个类型列表；

一个类型名后面跟一个省略号表示0个或多个给定类型的非类型参数列表

```c++
template <typename T,typename... Args> //Args模板参数包
void foo(const T &t,const Args&... rest) //rest函数参数包
```

<h4>模板特例化</h4>

函数模板可特例化：不可部分特例化

类模板可特例化：可特例化部分模板参数，也可只特例化成员

定义模板特例化

```c++
template <typename T> int compare(const T&,const T&); //原版
//为了处理字符指针 为其定义一个模板特例化
template<> //关键字后跟一个空的尖括号对，指出我们将为原模板的所有模板参数提供实参
int compare(const char* const &p1,const char* const &p2) //函数参数需要与一个先前声明的模板中对应的类型匹配
//此时的T为const char*
{
    return strcmp(p1,p2);
}
```

特例化的本质是实例化一个模板，而非重载。



<h2>标准库拓展</h2>

___

<h4>tuple类型</h4>

queue
