---
title: c++
tags: code
categories: languages
mathjax: true
abbrlink: 6f1ca842
---
关于c++相关内容的总结
<!--more-->
## 杂七杂八 
一个中文占两个字符变量的长度

cin.peek()可以从缓冲区读一个字符，且让该字符留在缓冲区，故可用如下代码实现空行结束 输入

```c++
while(std::cin.peek() != '\n'){
        ...
        getchar();
    }
```



### 文件作用域
extern关键字引入外部变量
static修饰局部变量时延迟了变量的生命周期到程序运行结束
修饰全局变量时使得该变量只能在本文件使用
修饰函数时使得该变量只能在本文件调用

### 二维数组
二维数组作为传参：
```c++
void arrayFunction(int (*a)[2]);
void arrayFunction(int a[][2]);
```

后面的长度不知道时可传一位第一个元素的地址,然后按如下形式调用:
```c++
void arrayFunction(int *a,int m,int n){

 *(a+1*n+1)=7;//等级于a[1][1]=7

}
```

```c++
int (*a)[4];
int b[4] = {1,2,3,4};
a = &b;
//a可视为a[][4],上面的操作使a的第一行为b数组
```



### enum
利用这个变量类型可控制赋值时只能赋给一定范围的值
定义:
```c++
enum name{Monday,Tuesday};
```
默认里面类型的值位0,1
可以改变里面的类型代表的值
```c++
enum name{Monday=1,Tuesday};
```
注意枚举值可以相同
少见的用法:
``` c++
enum{month=12}
```
相当于
static const int

c++提供了另一种使用enum的方法:
```c++
enum class name1{big,small};

 enum class name2{big,small};

 name1 examp1=name1::big;

 name2 examp2=name2::big;
```

这样可避免两种enum里面的变量相同时出错,但使用这种类型时想
使用examp的值需进行类型转换

### 获取系统时间

```c++
#include<ctime>
#include<cstdio>

struct tm get_current_time(){
    time_t nowtime;
    //首先创建一个time_t 类型的变量nowtime
    struct tm* p;
    //然后创建一个新时间结构体指针 p
    time(&nowtime);
    //使用该函数就可得到当前系统时间，使用该函数需要将传入time_t类型变量nowtime的地址值。
    p = localtime(&nowtime);

    return *p;
}
//注意年得家1900,月得加1
```



## 错误

### link错误

模板类不支持分离编译,需要将模板类的声明定义放在一起

### 读取访问权限冲突。_Pnext 是 0x1233F1C

相应的变量未进行初始化导致，检查是否有变量未初始化且使用了地址。

### multiple definition

可以把所有的全局变量放入一个头文件 global.h (名字随意起，但要加条件编译)中，每一个变量前面加extern，声明一下这些变量将在其它文件中定义。 然后建立一个和头文件名字对应的.c or .cpp文件 如global.c。在里面声明所有的全局变量

## conversions
c++会自动转换数字相关的数字类型
不相容的类型无法自动转换,如pointer和int
## 常用函数:
### sort():
在algorithm头文件里面
```c++
sort (first, last)
```
传入数组的首地址,某地址即可进行排序,排序原理是快速排序
也可加上一个参数,使用自己的比较规则函数或者库里的greater比较规则函数:
```c++
#include<algorithm>

bool cmp(int a,int b){
    return (a<b);
}

main:
    int array[5]={1,2,7,4,5};
    std::sort(array,array+5);
    std::sort(array,array+5,cmp);
    std::sort(array,array+5,std::greater<int>());
    std::stable_sort(array,array+5);
```
其中stable_sort与sort的用法相同,只是它在元素相等时不会改变元素的相对位置
注意cmp函数里面若返回a>b则会是降序排序,greater的结果也是降序排序

这个函数对可使用快速迭代器的容器都适用,如vector或使用new分配的动态数组


## input output

### 输出
左对齐制表：
```c++
    cout.setf(ios::left);
    
    for(int i=1;i<=n;++i){
        cout.width(4);
        cout<<i;
        for(int j=1;j<=i;++j){
            cout.width(4);            
            cout<<i*j;
        }
        cout<<endl;
    }
```

一次输入多行字符串：
```c++
string text[100];
do{
    cin>>text[i];
    i++;
}while(cin.get()!=EOF);
```
多行字符串带空格：
```c++
string text[100];
while(getline(cin,text[i])){
    i++;
}
```

控制小数位输出：
```c++
#include<iomanip>

double test=3.02;
cout.setf(ios::fixed);
cout<<setprecision(3)<<test<<endl;
```

输出八进制 十六进制
```c++
cout<<hex<<num<<endl;
cout<<oct<<num<<endl;
```
注意使用后会使后面的数字都暗相应的进制输出

### 输入异常
若cin>>输入了不同的数据类型,它的返回值会是0,利用这个可以设计退出
## fuction
### inline

在函数定义或声明前加keyword inline即可使用
调用时相当于直接往调用的地方copy了函数中的代码，它可节省运行实现，但会使用更多的空间

### reference 
有点像取别名，引用后使用两个名字是等价的
简单的使用：
```c++
int var;
int &ref=var;
```
注意reference必须初始化，且初始化后不可改变

作为函数参数使用：
```c++
void swap(int &num1,int &num2){
    int temp=num1;
    num1=num2;
    num2=temp;
}

int main（）{
	...
    swap(num1,num2);
}
```
这样可以直接修改调用函数区域的变量的值

若要引用且不能修改，可这样使用：
```c++
const int &ref=var;
```




### overloading
c++允许多个函数使用同个名字，利用signature，即利用形参的不同的变量类型、数量进行区别，主要parameter的名字并不能作为区分
reference和它对应的变量类型无法作为signature的区别，但const和对应的变量类型则可以
注意函数返回值类型的不同不能作为overload的区分
引用在overload 的各自区别。
### template
利用template可形成一个函数模板，然后编译器会根据调用时的parameter的类型，创建相应的函数。
使用实例：
```c++
template<typename T>//or class T,older usage
void swap(T &a,T &b){
    T t;
    t=a;
    a=b;
    b=t;
}

int main(){
    double a=1.2,b=2.0;
    swap(a,b);
    std::cout<<a<<" "<<b;

    return 0;
}

```

template里面可指定多个标识符
可以像overload正常的函数一样重载函数的template
主要每个函数想使用这个前必须有template
利用template时可能出现某种类型的变量跟模板里的algorithm不匹配的情况，这种时候可利用explicit specialization，指定特殊的类型利用特殊的模板
在编译时，编译器遇到有多个匹配模板的情况时，会先匹配不是template定义的函数，然后是explicit specialization，然后才是template
示例：
```c++
void swap(double &a,double &b){
    a=4;
    b=5;
}

template<typename T>//or class T
void swap(T &a,T &b){
    T t;
    t=a;
    a=b;
    b=t;
}
template<> void swap<double>(double &a,double &b){//可不加<double>
    a=3.0;
    b=4.0;
}
```

在函数内调用时利用template作为模板创建函数称为implicit instantiation,也可利用
explicit instantiation创建函数:
```c++
template<typename T>//or class T
void swap(T &a,T &b){
    T t;
    t=a;
    a=b;
    b=t;
}
template void swap(double &a,double &b);
template <> void swap(int &a,int &b);
```
最后一个语句的意思是不可以利用模板创建相应的函数,该函数可继续进行单独的定义.
在函数内调用时加上<typename>也是explicit instantiation
注意同时使用explicit instantiation 和explicit specialization会导致错误
implicit instantiation explict instantiation explicit instantiation统称为specialization,因为它们都创建了一个有具体数据类型的函数
overload resolution process(有多个函数时匹配时找出匹配得最好的那一个)
可以使用fun<>()来指定调用template
```c++
decltype(expression) var;
decltype(expression) var=expression2
```
可将后面的var转化为和前面的expression一样的类型,注意expression也可以是变量,如果expression里面是一个变量加上括号,则var会转化为reference
c++11满足一种新的函数定义声明方法,可以在声明变量后再决定return type:
```c++
auto h(int a,int b)->decltype(a+b){

    return a+b;
}
```
## namespace
declerative region
变量可以进行声明的区域
potential scope
从变量的定义到它decelerative region截至，就是变量可能可以使用的地方
scope
就是变量可以使用的地方，区分potential scope和scope是因为可能出现全局变量被局部变量屏蔽的情况
global namespace:全局变量属于的namespace

namespace 定义：
```c++
namespace test{
    int a;
    int b;
    struct c{
        int d;
    }
}
```

使用：
using directive
```c++
using namesapce test;
```
或者加上前缀：
```c++
test::a=1;
::a(global namespace)
```
当然两个可以一起用（using declarative）：
```c++
using test::a;
```
注意使用命令的区间，在函数内部使用则可以在函数内部使用相应的变量，在函数外则可以全局使用
在函数内部使用时相当与在外部声明，不过scope小了
可以using多个namespace，不过可能导致变量指向不明
可以在namespace内部使用using
```c++
namespace myth{
    using std::cout;
    using std::cin;
    using std::endl;
}
using namespace myth;

int main(){
	cout<<"111"<<endl;
}
```
若namespace内的函数进行了重载，则using后可使用所有重载
注意再好不要在头文件使用using namespace，这样可能导致使用不明，而且头文件的顺序也有可能产生影响
优先在局部使用using

namespace可嵌套：
```c++
namesapce a{
    namespace b{
        int a;
    }
}
```

取别名：
```c++
namespace myth{
    int a;
}
namespace youth=myth;
```
unnamed namesapce:
```c++
namespace{
    int a;
}
```
它的作用域只在所在的文件

namespace是开放的，可以在多个文件为其添加成员，不过如果要使用在其他文件添加的成员，需要先声明
主要声明变量加extern关键字

## 内存分配
使用new，delete分配内存：
分配一个类型的单位内存：
```c++
p=new int;
*p=1;
delete p;
```

分配一个类型的多个内存：
```c++
delete p;
p=new int[10];
delete []p;
```

## 类与对象

### 类的定义
类是一个模板，对象是由类这个模板创建的，因为类是模板，故定义类里面的变量时不可初始化；
类一般在函数外定义，也可在函数内定义，不过一般不这么干；
类的成员可以是变量，也可以是函数；

一个例子：
```c++
class people{
    public:
        int age;
        int height;
        int weight;
    void born(people *someone){
        someone->age=0;
        someone->height=40;
        someone->weight=5;
    }
};
```

也可在外面定义成员函数，不过需在函数名前加上people::，而且需先在类内声明，这样上面那个就会变成：
```c++
void people::born(people *someone){
    someone->age=0;
    someone->height=40;
    someone->weight=5;
}
```
在类内定义的函数会自动变成内联函数[[#inline]],故一般在类外定义函数

类内定义时，private可定义受保护的变量，类外不能直接访问，类内则没有限制，一般通过public的函数来访问，定义类时若未声明public或private，则默认为private
```c++
class people{
    public:
        int age;
        int height;
        int weight;
    void born(people *someone);
    private:
        int IQ;
        int EQ;
};

int main(){
	some.IQ...\\error！
}
```

```c++
class people{
    private:
        int age;
        int height;
    public:
        void show();
        void input(int inage,int inheight);
};

void people::show(){
    cout<<"age:"<<age<<endl;
    cout<<"height:"<<height<<endl;
}
void people::input(int inage,int inheight){
    age=inage;
    height=inheight;
}
```

类的成员访问限定符可出现多次，不过应该尽量避免

### 构造函数
类的成员函数有一类特殊的函数称为构造函数，在生成对象时自动执行,构造函数的属性是public，因为需在创建对象时调用
注意如果未定义构造函数,编译器会自动提供一个,如果定义了,还需要自己定义一个default constructor
示例：
```c++
class people{
    private:
        int age;
        int height;
    public:
        void show();
        people(int inage,int inheight);
        people();
};

void people::show(){
    cout<<"age:"<<age<<endl;
    cout<<"height:"<<height<<endl;
}

people::people(int inage,int inheight){
    age=inage;
    height=inheight;
}

people::people(){
    age=1;
    height=30;
}
```

调用时可explicit
```c++
	people someone=people(3,50);
```
也可implicit
```c++
	people someone(3,50);
```
使用new时也可使用:
```c++
	people *someone=new people(3,50);
```

### destructor
类似constructor,destructor在类被清除时被编译器调用
定义:
```c++
	class people{
   
        ~people(){
            cout<<"bye"<<endl;
        }
};
```
### this
this指针代表对象的地址,故上述destructor可利用this指针自动释放内存:
```c++
class stock{
	private:
		int data;
	public:
		const stock & topval(const stock & s)const;
		~stock(){
			delete this;
		}
};
```

### array of objects
利用constructor初始化数组:
```c++
stock object[2]={

	 stock(3),

	 stock(4),

 };
```
使用时可使用不同的constructor,也可只初始化几个元素
	
### const
class里面想要使用const定义常量(c++11后无限制）有两种方式:
```c++
	enum{month=12};

 	static const int num=11;
	
	const int item+member initializer list
```
前两种定义的变量是整个类公用的，只分配一个存储空间

```c++
const stock & topval(const stock & s)const
```

第三个const保证调用它的类不改变

若对象加上了const，则它可调用的只有加了const的成员函数

在数据前面加上mutable即可在const函数内修改

### 友元函数
类可声明友元，友元可以是独立的函数，可以是类中的函数，也可以是类，友元可以访问类中的所有元素
```c++
class people;
class person{
    public:
        void one(people *someone);
};
class people{
    public:
        int age;
    friend void creat(people *someone);
    friend void person::one(people *someone);
    private:
        int IQ;
};//也可将people和person的定义调换位置，这样就不用先声明类了

void creat(people *someone){
    someone->age=1;
    someone->IQ=100;
}

void person::one(people *someone){
    someone->IQ=1;
    cout<<someone->IQ<<endl;
}
```

注意要使用类前先声明
使用友元时可以在类内直接定义
可以将一个函数在两个类中声明friend，这样它就会成为两个类的shared friend
	
### operator overload
类中可将运算符重载，实现利用符号对类进行操作
	
例子：
```c++
class data{

 private:

	 int num1;

	 int num2;

 public:

	 data operator+(data &t);

};
	
data data::operator+(data &t){

	 data sum;

	 sum.num1=num1+t.num1;

	 sum.num2=num2+t.num2;


	 return sum;

}
```

调用时可直接利用符号,当然也可利用函数形式
例子:
```c++
	data sum=test1+test2;
	data sum=test1.operator+(test2);
```

可以同时加多个,结果和下面的调用一致:
```c++
	t4 = t1.operator+(t2.operator+(t3))
```

注意operator overload不需要一定是member function,但它的操作数必须拥有user-defined type
nonmember function用法:
```c++
	Time operator*(double m, const Time & t);
```
调用时下列式子等价:
```c++
	A = 2.75 * B;
	A = operator*(2.75, B);
```
operator overload不可违法语法规则,如%3
不可overload新的运算符
	
下列运算符不可被overload:
	

|operator|
|---|
|sizeof|
| .|
|.*|
|::|
|?:|
|typeid|
|const_cast|
|dynamic_cast|
|reinterpret_cast|
|static_cast|

下列运算符只能作为menber function被overload

|operator|
|---|
|=|
|[]|
|()|
|- >|

### cout<<object
利用友元函数和运算符重载实现:
```c++
class data{

 private:

	 int num1;

	 int num2;

 public:

	 friend std::ostream &operator<<(std::ostream &os,data &t);

};

std::ostream &operator<<(std::ostream &os,data &t){

	 os<<"num1:"<<t.num1<<",num2:"<<t.num2;

	 return os;

}
	
```

需要返回os是因为
c++的cout是从左到右读取的
这样做cout<<object整个相当于一个cout,使得该式可以和其他的输出放在一起
	
### conversion

#### other type to class
类的conversion通过constructor实现
若类中声明了一个如下的constructor:
```c++
	test(double data);
```
则下列式子是合法的:
```c++
	test exam1=1;
```
内在机理是编译器调用了constructor创造了一个临时的对象然后赋值给另一个对象
	
若constructor有多个参数,则无法作为conversion的模板,不过若其他参数有默认值则仍可以作为模板
```c++
	test(double data,int a);// no
	test(double data,int a=3);//yes
```

以上是隐式的conversion,若想只能使用显式的conversion,避免错误,可按如下声明constructor:
```c++
	explicit test(double data);
```

这样就只能使用显式的conversion了,即:
```c++
	exam1=(test)1;
```

#### class to other type

类的conversion需要使用conversion function

```c++
	class
	...
	operator double();
	...
	test::operator double(){

		 return (double)data;

	}
	...
	double x=double(exam1);//or (double)exam1
	
```

这样即可实现类转换为其他类型
注意注意的conversion需要满足:

>在类中
>没有参数
>没有返回值类型

在c++11后conversion function也可以使用explicit关键词
	
#### Conversion in implementing addition
想实现对象与double类型的加法，有两种方法实现：
一是声明如下的友元函数：
```c++
	friend operator+(test &obj1,test &obj2);//obj1y
```

第二是overload+两次
	
第一种方法代码简便,但需要调用constructor进行类型转换,所以速度会慢
	
第二种的优缺点与第一种相反
	
### Classes and Dynamic memory allocation

#### class default member function
class会自动定义四种函数,constructor destructor不会造成任何影响,剩下的两种则有影响
#####  default copy constructor
形式：
```c++
	test(const test &)
```

当程序利用另一个对象初始化一个对象时,它会被调用
下列的语句它也会被调用
```c++
	StringBad * pStringBad = new StringBad(motto);
```
它会把另一个对象的每一个内容复制到新对象,注意它会建立一个匿名的副本,然后再进行复制,它也可以直接利用该constructor建立新对象
即会变成如下形式:
```c++
	StringBad sailor = StringBad(sports)
```

主要重新构造copy constructor时参数一定要加const
不然下列形式就没法使用了：
```c++
	Strings y="345";
```
原因是const变量不能转换为非const应用
	
##### default assignment constructor
形式:
```c++
	Class_name & Class_name::operator=(const Class_name &);
```
会把另一个对象的每一个内容复制给另一个对象
	
	
	
##### programing
因为上述default constructor会导致问题，故如果类中有使用new delete，应该也overload这些member function，还有注意类中那个的new delete要配套使用，new有加[],delete也得加
	
若想直接使得会使用这些constructor的操作非法，可利用dummy private methods，即在private种声明这两个函数
	
### placement new
### initializer-list syntax
c++运行在定义constructor时使用该语法，用于修改const变量，当然也可以用于非const

例子：
```c++
class test{
...
	const int size;
}
	
test::test(int inSize) :size(inSize),...{

 	data=0;

}
```

注意实际初始化时会按class声明时的顺序初始化而不是member initializer list的顺序
	
	
### Inheritance

#### 基础
语法：
```c++
class inher :public test{
...
 }
```

base class的private属性不能被derive class直接访问;
因为base class会先于derive class被创造,所以需利用member initializer list初始化嵌入的base class,语法为:
```c++
deriveClass(type items,baseClass & item) :baseClass(item){
	...}
```

对于引用和指针,可用base class指向derive class,不可反向
	
public inheritance:is-a relationship
即派生出的类也是原来的类,如从水果类派生出的香蕉也是水果
具体而言就是base class有的derive class都有
	
#### virtual function
##### 基础
语法:
```c++
	virtual void functionname();
```
若使用此关键词,则当一个base class 的指针或引用指向一个derive class时,若调用两个class都有的同名函数,则会根据指向的object的类型调用函数,而不是根据指针的类型
	
当在类外写具体函数定义时,不用加virtual
利用这个可利用指针数组同时指向derive class和base class
	
destructor需要加virtual,防止delete 指针时出错,调用了错误的destructor
	
注意若使用virtual,则在derive class中,所有的同名函数都会被隐藏,如果有overload函数,则需要重写所有的overload函数,不过可以只把返回值类型由指向base class的指针或引用改为指向derive class 的指针或引用,这样不会被隐藏:
```c++
class Dwelling {
public: // a base method
	virtual Dwelling & build(int n); 
	...
};
	
class Hovel : public Dwelling {
public:
	// a derived method with a covariant return type 
	virtual Hovel & build(int n); // same function signature
	...
};
```

##### pure virtual function
用于abstract base class(ABC)中,作为一个成员函数的模板,没有实际作用,有pure virtual function的class不能创造对象
语法:
```c++
class ABC{
		virtual double f()  = 0;
};

```

一个真正的ABC至少要有一个pure virtual function
当然,一个pure virtual function也可以进行函数定义,只是定义可以被省去,它最重要的作用是作为ABC的标志,而ABC可作为base class产生很多有用的derive class
#### protected
若使用这个关键字,则在类中相应的部分可以被该类对应的derive class直接访问
	
#### dynamic memory allocate
##### dynamic to no

 base class 使用dynamic memory allocate,derive class不使用时,不需要额外的操作,derive class的copy constructor,assignment operator,destructor都会调用base class对应的函数,故不会出现问题
	
	
##### something to dynamic
此时需要重写copy constructor,destructor,assignment operator,注意destructor会自动调用base class的destructor,constructor则需要利用member initializer list来调用,assignment operator则可以利用如下语句调用:
```c++
	baseClass::operator=(item);
	*this=item;//false
```
在定义assignment constructor时,下面那个语句会调用deriveClass的assignment operator，造成无限循环
	
##### summary
注意一点，derive class的friend函数不可访问base class的private属性，故要访问可利用相应的base class的friend函数，若名字相同，可利用类型转化，如：
```c++
std::ostream & operator<<(std::ostream & os, const hasDMA & hs) {
// type cast to match operator<<(ostream & , const baseDMA &) 
	os << (const baseDMA &) hs; 
	os << "Style: " << hs.style << endl; 
	return os;
}
```

#### private inheritance and protected inheritance
##### private
private inheritance相当于将base class作为derive class的private的一部分，外部不可访问base class的interface，而内部可以
	
同时从多个类inheritance：
```c++
class Student : private std::string, private std::valarray<double> {
public: ...
};
```

注意derive class调用base class的methods时是利用base class的名字调用，即用类名代替对象名
	
如果要调用base class本身，则利用type cast（类型转换），转换*this 
	
注意调用base class 的friend函数时，要用explict type cast(显示类型转换),若不使用,可能会导致递归调用函数(如调用了derive class的<<而不是base class 的<<),若使用了MI(multiple inheritance),还可能导致编译错误
	
使用:
当需要使用base class的protected属性的member,或需要重定义virtual function时,使用private inheritance,其他情况,可以使用containment(即直接定义相关类的object作为member)j
	
##### protected
与private inheritance基本相同，不同的是若从derived class inherite一个新的derived class，则在这个class中base class的依然可以在class内访问
	
### using
利用using keyword可使derived class定义的object可调用base class的methods，语法如下：
```c++
using std::valarray<double>::operator[];
\\注意仅需要函数名,返回值类型和括号都不需要
```
有一个较古老的对private derived class的方法是直接在类型声明相关的函数名,可达到相同的效果,不推荐使用.
	
### multiple inheritance(MI)
#### virtual base class
若从一个base class生成了两个derived class,然后通过这两个derived class再生成一个derived class,此时derived class会有两个base class,这样的话用base class的指针指向derived class就不明确了,我们可以使用类型转换来解决,但也可以使用virtual base class,即在从base class生成两个derived class时:
```c++
class Singer : virtual public Worker {...}; 
class Waiter : public virtual Worker {...};
```
这样的话derived class的derived class就只会有一个base class
若一个derived class的base class中有的使用了virtual,有的没有,则derived class有使用virtual的共用一个base base class,没有的各有一个
	
注意,若使用这个语法,则constructor需要利用member initializer list调用base base class的constructor,初始化bae class不会传递信息给base base class
	
virtual 的函数优先规则无视private之类的access rules
	

####  methods
使用MI时若多个base class中有同名的methods,需要 使用class::来指定methods,不过最好还是重新定义methods
若多个base class是从一个class inherite,则定义新的method会比较麻烦,解决方法是模块化设计,即设计相应的函数,特定输出某个class中的新数据,它的derived class需要数据时直接调用.
这样的操作最好在protected中进行,这样既可以让外界无法使用,也可以让derived class不受影响
	
### class template
#### 基础
和函数的template类似，需在class的定义前加如下语句：
```c++
template <class Type>;
template <typename Type> // newer choice
```
注意单独定义class的methods函数时也需要加
而且class的定域符需要按如下形式书写:
```c++
bool Stack<Type>::push(const Type & item){...}
```

注意使用template语法的member function必须和class的定义在同一个文件内
	
使用方法如下:
```c++
Stack<int> kernels;
```
注意类型必须注明,不能和template function一样可让编译器根据参数自行决定
	
若在类内的函数需要返回类相关的类型,可只写类名,如:
```c++
Stack & operator=(const Stack & st);
```
在类外则需要加上 \<type>
	
template可和具体类型结合在一起,如:
```c++
template <class T, int n>
```
这个语句为类提供了一个具体的int类型变量
这个语法支持integer type(int), enumeration type(enum), reference(&), pointer(*)
使用后该变量的值不可改变,地址不可获得,且对每一个具体值都会单独创建一个类,即:
```c++
ArrayTP<double, 12> eggweights;
ArrayTP<double, 13> donuts;
```
创建了两个类
	
template可作为base class进行inheritance,,template也可以同时定义多个不具体的数据类型.
template可以给类型一个default parameter(默认值),function template则不可以,若是具体的数据类型给默认值,则两者都可以
	
template形成了类可implicit，也可explicit：
```c++
ArrayTP<int, 100> stuff; // 隐性创造了一个类模板,注意只有创造对象时会进行
template class ArrayTP<string, 100>;//显式创造了一个类模板
```

#### specialization:
对应template class,可能有几个类型需要特定的功能,则可以单独定义相关类型的类定义,定义如下:
```c++
template <> class Classname<specialized-type-name> { ... };
class Classname<specialized-type-name> { ... };//较古老的用法
```
c++允许partial specialization,即对应有多个generic type的class template,只指定其中的几个类型,定义如下:
```c++
// specialization with T2 set to int
template <class T1> class Pair<T1, int> {...};
//注意为指定的类型在前缀中需要指明
```
partial specialization也可以指定指针类型:
```c++
template<class T*> class Feeb { ... };
```
partial specialization也可以做出其他限制:
```c++
// general template
template <class T1, class T2, class T3> class Trio{...};
// specialization with T3 set to T2
template <class T1, class T2> class Trio<T1, T2, T2> {...}; 
// specialization with T3 and T2 set to T1* 
template <class T1> class Trio<T1, T1*, T1*> {...};
```

class template可在类中再嵌入一个class template,如:
```c++
template <typename T> class beta {
private: template <typename V> // declaration
...}

template <typename T> 
template<typename V> 
class beta<T>::hold {
...
};
\\注意老的编译器可能仅支持在类内定义
```
#### 内嵌
class template还支持在声明template内嵌template,例子如下:
```c++
template <template <typename T> class Thing> //template <typename T> class is a type
class Crab {
private: Thing<int> s1;
...}

int main(){
	Crab<Stack> nebula;
}
```

#### friend
c++支持在class template中使用friend function,若未将generic type用于该函数,则该函数是全体class template产生的类的friend function,若使用了,则是具体的某个class的friend function
```c++
template <class T> class HasFriend {
public:
	friend void counts(); //friend to all
...
};
	
template <class T> class HasFriend {
	friend void report(HasFriend<T> &); //friend to one
...
};
//注意该friend function本身不是template function,故需要定义多个重载函数来匹配
```

当然也可以让template funciton成为friend function,不过使用时需先声明template function:
```c++
template <typename T> void counts();
template <typename T> void report(T &);
	
template <typename TT> 
class HasFriendT {
... 
friend void counts<TT>(); 
friend void report<>(HasFriendT<TT> &); // the same as report<HasFriendT<TT> >(HasFriendT<TT> &)
};
```

也可以让template funciton的所有specialization都成为每个class template的specialization的friend function:
```c++
template <typename T> 
class ManyFriend {
...
template <typename C, typename D> 
friend void show2(C &, D &); 
};
```
如**void show2<ManyFriend<int> &, ManyFriend<int> &> (ManyFriend<int> & c, ManyFriend<int> & d)**是所有class template形成的类的friend function
	
#### template Alisas
c++11允许使用template取别名:
```c++
template<typename T>
using arrtype = std::array<T,12>; // arrtype<int>=std::array<int, 12>
```

该using语法可拓展到不是class template的情况:
```c++
using pc2 = const char *;//typedef const char * pc1;
```

### nested class
class中可内嵌class的定义，这使得该class拥有class scope，
外界若想使用，则需要看该class定义的access rules
	
class template可在nested class中正常使用
###  RTTI
RTTI被那些有virtual函数的class使用
用于类和派生类的类型转化
#### dynamic_cast
这个操作符用于检查类指针的类型转化是否安全，安全则返回正常的指针，不安全则返回null指针
语法如下：
```c++
der1 *t=dynamic_cast<der1 *>(t1)
```
一般,类型转化发生当t1是从der1 derive产生的类

若引入如typeinfo头文件,且转化的是reference,则转化失败会throw bad_cast exception
#### typeid
用于检查表达式指向的类的类型,语法如下:
```c++
typeid(t)
```
它是一个type_info类,name method会放回类型名,而且可用== !=等操作符判断类型
如果表达式是空指针,且加了*,则会throw bad_typeid exception
	
## exception
### base
调用abort()函数会直接返回错误信息并结束
利用try，catch,可进行exception处理
首先try里面的代码throw一个exception,然后看try后面的catch有哪里匹配了throw出的内容,然后执行catch里的代码
若没有throw exception,则直接执行try catch后的代码
```c++
try{
	if(c==1){
		throw "badEnd";
	} 
}
catch(int s){
	cout<<s<<endl;
}
catch(const char *s){
	cout<<s<<endl;
}
```
catch的参数也可以是object

若在try里面调用了一个函数，该函数调用另一个函数throw exception，这样依然可以和上面得到同样的效果。得到这样的结果利用的是unwinding the stack，
	即遇到exception就开始释放调用函数的stack直到遇到try并由匹配的catch，如果遇到object还会调用destructor加入stack
	
在catch里面使用throw，可rethrow exception，效果类似将try-catch当成一个有throw的函数
	
base class的reference可匹配所有derive class的reference
	
catch(...)可匹配所有类型的exception,一般放最后
	
### exception speciation
这个操作新标准很不支持,了解一下即可
```c++
double harm(double a) throw(bad_thing); // 可能会throw badthing
marm(double) throw();//不会throw exception
throw(char,int)//指定多个
```
noexcept和throw()同理
	
### exception class
exception头文件
exception的what()method返回string,它是给virtual函数,可在derived class重写
	
stdexcept头文件有多个exception类,类名即exception类型,what()会放回该类型名的string
	
new头文件使得new发生错误时会throw bad_alloc exception
此时也可以不throw而只是返回空指针:
```c++
int * pi = new (std::nothrow) int;
```

### 错误
若exception没有匹配的catch,程序会调用terminate(),terminate()默认调用abort(),若想自己设置terminate(),可先定义一个没有参数且返回值为void 的函数,并调用set_terminate()
	
若exception不符合exception specification,则它会调用unexcepted()函数,该函数默认调用terminate()
若想自己写unexcepted(),和定义和上面类似的函数并调用set_unexcepted()
也可以让unexcepted() throw exception
	
>如果该exception满足specification,则执行该try block
若不满足,且specification 没有bad_exception type,则程序调用terminat()
若不满足,且specification有bad_exception,则throw bad_exception

## type cast
**const_cast**
用于转化同类型的const和非const
语法如下:
```c++
const int*t1;
int *t2=const_cast<int *>(t1)
```
注意若指针执行的内容也是const的,则即使转化了也不可修改
**static_cast**
它可以被使用当且仅当指定的type可以被隐式转化为表达式的type
语法如下:
```c++
double t1;
int t2=static_cast<int >(t1);
```
**reinterpret_cast**
用于一些较危险的转化,如指针转数字,数字转结构体
语法:
```c++
int *t1;
int t2=reinterpret_cast<int>(t1);
```
但它有很多限制,它不能将指针转为较小的整数,也不能转化为float或char
它也不能将函数指针转化为data type,反之亦然
## STL
### base

iterator是指针的generalization(一般化),和指针功能类似,利用它我们就可以在STL的容器的内容中移动了.

所有的STL容器都有如下的成员函数

```c++
begin();//开头的iterator
end();//结尾,和'\0'类似,为空
size();//大小
swap();//交换容器中的值
type::iterator x;//对应的iterator类型
type::value_type x;//存储的内容的类型
operator==();
operator!=();//判断两个容器实例s
```

STL还定义了很多共有函数,可对所有适合的容器适用

```c++
for_each(ite begin,ite end,fun f);//对可以任意访问的容器[begin,end)中的值执行f
//若想遍历，在c++11中可用for(auto x:container_name) operation代替
sort(ite begin,ite end,fun compare=increasing);//和上面类似,对其中的值进行升序排序,第三个可选参数可添加比较函数
find(ite begin,ite end,val v);//找出v在容器中的第一次出现为，返回iterator
copy（ite begin，ite end，ite c_P);//将[begin,end)中的内容copy进c_P及之后的位置,注意该容器需要足够大
```

### iterator

STL定义了五种iterator，分别是input output（只读只写，只支持++），forward（可读可写，只支持++），bidirectional（可读可写，支持++，--），random access（可读可写，支持+n），它们的功能依次递增，使用高级的iterator可使用低级iterator的算法，反过来就不行了

可以说它们属于某concept(concept即有某具体要求的层次),如output iterator属于output iterator concept,concept的实例是model,从某一concept继承功能并发展叫refinement

它还定义了其他的iterator来丰富功能

使用时引入iterator头文件

`front_insert_iterator`,`back_insert_iterator`,`insert_iterator`,用于在容器的前或后或中间的任意位置插入内容,它们属于output iterator concept

```c++
back_insert_iterator<vector<int> > back_iter(dice);
insert_iterator<vector<int> > insert_iter(dice, dice.begin() );
```

若使用copy时给的位置的这个,则会执行插入操作

`reverse iteraotr`用于反向操作容器的内容,支持`--`

### algorithms

STL的算法分为四种

第一种是不修改序列的操作

第二种是修改序列的操作

第三种是排序和关系操作

第四种是广义数值操作

前三种操作都在`algorithm`头文件里，第四种单独在`numeric`头文件里

这些算法一般使用iterator确定作用范围,经常还有一个`_if`的分支可以添加函数进行判断

### container

container同意有concept，和上面的concept定义相同

#### sequence

序列要求容器的元素有一定的顺序

共有的操作

![](c++/sequence1.jpg)

特殊操作

![](c++/sequence2.jpg)

##### vector

vector与数学中的vector不同，可看成数组

定义

```c++
vector<int> t1;
```

特殊ite

```c++
t1.rbegin();
t1.rend();//reverse iterator的开始与结束,可用于反向输出
```

##### deque

双向队列,支持在前在后增删元素(`push_front`,`pop_front`),比vector快,也支持random access(随机存取),但比vector慢

##### list

双向链表，不支持`[]`,支持在常数时间在任意位置插入删除元素,比vector快

一些特殊成员函数

| 函数                    | 功能                                                         |
| ----------------------- | ------------------------------------------------------------ |
| merge(list & t)         | 将两个排序好的list归并                                       |
| remove(T & val)         | 删除list所有值为val的节点                                    |
| sort()                  | 升序排序                                                     |
| splice(ite pos,list &x) | 将x中的内容插入pos前然后清空x,它也有只插入一个或范围内值的重载 |
| unique()                | 去除list中的连续重复内容(如{3,3,4,3}去除前面的一个3)         |

###### forward_list

c++11添加的单向链表

##### queue

队列,仅支持队列的基本操作,,不支持在内部迭代

###### priority_queue

优先队列,与队列不同的是最大的元素总会在头部,可以指定比较函数

##### stack

栈,只支持栈的基本操作,不支持迭代

#### associated container

##### set

类似数学中的集合

交、并、相对补集利用STL的算法实现

```c++
set_union(A.begin(), A.end(), B.begin(), B.end(), insert_iterator<set<string> >(C, C.begin()));//前四个参数指定集合,第五个参数指定结果的存放
//set_intersection(),set_difference()同理
```

有用的类methods

```c++
s.insert(val);//添加元素
s.insert(ite begin,ite end);//y
s.lower_bound(val);//返回集合内第一个不小于val的值的iterator
s.upper_bound(val);//返回集合中第一个大于val的值的iterator
```

##### multimap

一些

### valarray

#### constructor
```c++
double gpa[5] = {3.1, 3.5, 3.8, 2.9, 3.3};
valarray<double> v1; //空数组
valarray<int> v2(8); //大小为8的数组
valarray<int> v3(10,8);//大小为8的数组,且每个数初始化为10
valarray<double> v4(gpa, 4);//大小为4的数组,利用gpa前4个元素初始化
valarray<int> v5 = {20, 32, 17, 9}; // C++11,直接初始化
```

#### methods
```c++
operator[]();//和数组的[]作用相同
size();//返回数组大小
sum();//返回所有元素的和
min();//返回所有元素的最小值
max();//返回所有元素的最大值
```
### string

使用前引入string头文件

几个需要了解的constructor:
```c++
string(size_type n, char c);//创建有n个c字符的string
string(const char * s);
string(const string & str);//用string 或c的string初始化
string(const char * s, size_type n);//用s的前n个字符初始化
template<class Iter> string(Iter begin, Iter end);
//用[begin,end)里的元素初始化,动态分配的也可以
string(const string & str, size_type pos, size_type n = npos);
//用str里从pos开始的n个字符或从pos到末尾的字符创建字符串
string(string && str) noexcept;
//类似copy constructor,但不保证str是const
string(initializer_list<char> il);
//使得string支持string piano_man = {'L', 'i', 's','z','t'};
```

它overload的operator：

```c++
operator+()//拼接字符串,也可拼接c的字符串或字符
operator=()//给字符串赋值,后面同样可以接字符串也可以是c的字符串和字符
operator+=()//拼接字符串并赋给调用的对象
operator<<()//输出字符串
operator[]()//访问字符串中的字符,与字符数组类似
```

input:

```c++
operation>>()
geiline(cin,str,':')//遇:停止,不读入:,注意该行剩下的内容仍在stream中
cin.geiline(str,10)//指定str只能读最多9个字符,只能用于c的char数组
```

string支持所有的比较,它可以和string 对象比较,也可以和c的字符串比较

string的length()和size()都返回string 的长度,npos是string字符串长度的最大值

stirng的find函数运行如下:

```c++
str.find(string)//找到str中string出现的第一次,若找不到输出npos
str.find(string,3)//从str的3(index)位置开始找string
str.find("d",2,5)//从str2开始的5个字符中找d,注意不支持字符
```

`find_first_of()`,`find_last_of()`,`find_first_not_of`,的用法都类似,不过找的对象不同,last of的最后出现,first not of是第一次不出现的位置

string分配空间时总会分配较大的空间避免allocate过于频繁,capacity()返回现在的容量,超过就会重新分配,注意capacity已将`\0`需要的空间去除了,故返回的是实际的空间-1

`reverse()`只指定需要的str的最小长度,然后capacity可能会改变

filename可能需要const char *,这时可以调用`c_str()`;

调用`substr`,`erase`时,注意第二个参数是长度

### smart pointer

使用前先引入memory头文件

用法:

```c++
auto_ptr<double> pd(new double);//定义了double *,并分配了内存
unique_ptr<double> pdu(new double); // 后面两个功能类似
shared_ptr<string> pss(new string); // 
```

然后若该指针被释放,则它指向的内存空间也会释放

它的类型转化都是explict的:

```c++
pd = p_reg;//不支持
pd = shared_ptr<double>(p_reg);//支持
```

它可以被解引用,即`*pd`,`pd->a`,但注意pd指向的对象的内存必须是动态分配的,不然会出错

`auto_ptr`应该减少使用而使用c++11添加的后两个,下面是原因

```c++
auto_ptr<string> ps (new string("I reigned lonely as a cloud.")); 
auto_ptr<string> vocation; 
vocation = ps;
```

上面的代码会出错,因为`vocation`和`ps`指向同一个内存空间,所以结束时delete了两次

而使用`shared_ptr`时,会有一个计数器统计有多少个指针指向同一个对象,释放时只释放一次

使用`unique_ptr`时,指向一个对象的指针只能有一个,故上面的代码会报错

`unique_ptr`可以被赋值给它自己的类型或`shared_ptr`当且仅当它是一个临时的指针

当然我们也可以进行赋值:

```c++
unique_ptr<double>p1(new double);
unique_ptr<double> p2;
p2 = move(p1);
```

此时p1已经没有了指向

`unique_ptr`支持new[],而`auto_ptr`不支持,`shared_ptr`同理

访问数组:

```c++
ptr.get()[index]
```

## 文件操作

注意使用文件读写时最好不要用动态分配内存的类（如string），用了也记得记录大小，不然容易出错。

