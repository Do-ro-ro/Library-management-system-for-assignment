# disign

## implementation

### class Library

```cpp
class Library
private:
//数据成员 //用链表+动态节点存书、用户、管理员
Book* book_head;//藏书链表首 
Book* book_end; //藏书链表尾 book_end->next_book == 0
Reader* reader_head;//学校用户链表首
Reader* reader_end; //学校用户链表尾 reader_end->next_reader == 0
Admin* admin_head;//管理员链表首
Admin* admin_end;//管理员链表尾 admin_end->next_admin == 0

string dfut_reader_pswd = “123456”;//默认用户密码
string dfut_admin_pswd = “123456”;//默认管理员密码

int borrow_len = 30;//书籍单次外借最长时长 天数

//查找学校用户 
Reader* search_reader(string accnum);//账号精确匹配查找
//返回匹配账号的用户的指针，未找到返回空指针
//查找管理员 
Admin* search_admin(string accnum);//账号精确匹配查找

public:
Library();//构造函数 //链表首尾节点都是0，初始化列表
	//不用考虑读文件的构造函数，读文件直接调用下面的成员函数
~Library();//析构函数 //释放链表节点

//链表操作
	//插入
//尾部插入书籍（把节点new_node插入尾部）
void append(Book* new_node);//注意更新book_end
//指针节点pre_book后面插入书籍
void insert(Book* pre_book, Book* new_node);
//尾部插入用户
void append(Reader* new_node);//注意更新reader_end
//指针节点pre_reader后面插入用户
void insert(Reader* pre_reader, Reader* new_node);
	//删除
//删去node节点
void erase(Book* node);
void erase(Reader* node);


//读文件 藏书列表，从start节点开始链（意即从文件中读出的第一本书链到start节点的后面）
void readf_book(string filename,Book* start = book_end);
//读文件 用户列表
void readf_reader(string filename,Reader* start = reader_end);
//读文件 管理员列表
void readf_admin(string filename,Admin* start = admin_end);

//藏书列表写入文件，从start节点开始写（含start节点）
void writef_book(string filename,Book* start = book_head);
//用户列表写入文件
void writef_reader(string filename,Reader* start = reader_head);
//管理员列表写入文件
void writef_admin(string filename,Admin* start = admin_head);


//查找书
Book* isbn_search(string isbn);//isbn精确查找 
//返回匹配isbn的书的指针，未找到返回空指针
//遍历藏书链表，找到即break，循环外返回指向该节点的指针（该指针初始值为0）
Book* name_search(string book_name);//书名精确查找 
//同上
vector<Book*> author_search(string author);//作者范围查找 
//返回所有匹配作者的书的指针组成的向量，未找到返回空向量
//遍历藏书链表，找到即push_back指针，循环外返回向量（该向量初始为空向量）
vector<Book*> clasf_search(string classifer);//分类号范围查找 
//返回所有匹配分类号的书的指针组成的向量，未找到返回空向量
//遍历藏书链表，找到即push_back指针，循环外返回向量（该向量初始为空向量）
	//中图分类法https://lib.cqmu.edu.cn/fenlei.htm 分类号两种类型
//①二级分类 Axxx (A-Z)②T开头的还有三级分类 TPxxx  （xxx表示其他字符（数字短线等））

//友元
friend Reader* reader_log();
//用户登陆函数，该函数用到Reader* search_reader(string accnum)
friend Admin* admin_log();
//管理员登陆函数，该函数用到Admin* search_admin(string accnum)
friend Admin;

```

### class Admin

```cpp
class Admin
private:
Admin* next_admin;//下一个管理员//用链表+动态节点存管理员
string accnum;//账号
string password;//密码

public:
//构造函数
//拷贝构造函数
//重载赋值运算符
//析构函数

//设置用户默认密码
//设置用户的密码 //询问；查找用户（调用Reader* search_reader(string accnum)）
//添加用户//询问；新建节点尾接（调用void Library::append(Reader* new_node);）
//删除用户//询问；查找用户（调用Reader* Library::search_reader(string accnum)）；删除该节点用户（调用void Library::erase(Reader* node);）

//修改书信息 //询问；查找书（调用library搜索函数）
//添加书籍//询问；新建节点尾接（调用void Library::append(Book* new_node);）
//删除书籍//询问；查找书（调用library搜索函数）；删除该节点书籍（调用void Library::erase(Book* node);）

friend Library;

```

### class Reader

```cpp
class Reader
private:
Reader* next_reader;//下一个用户//用链表+动态节点存用户
string accnum;//账号
string password;//密码
vector<Book*> book_curr;//当前在借的书籍指针向量//指向书籍链表中的部分节点
vector<Book*> book_history;//历史借阅书籍

public:
//构造函数
//拷贝构造函数
//重载赋值运算符
//析构函数

//查找书，用于还书
//flag==1：查找范围为当前借阅书籍 
//flag==0：查找范围为历史借阅书籍
//四个函数，类似library的搜索函数

//修改密码

//还书
void return_book();
	//引导选择何种索引方式确定书籍来归还，分别调用不同的reader搜索函数，并显示搜索结果
	//显示搜索结果：
//对于每本书（一行），显示index（精确查找index=0；范围查找index即搜索结果向量中当前书对应的下标）+调用Book的display
//一面显示多少条，以及翻页操作
	//搜索结果显示，并询问删除书籍的index，然后归还这本书（向量[index]获取要归还书籍的指针，修改该书的借阅者、时间、借阅标记）
	//更新book_curr向量

//借书
void borrow_book();
//同上，不过 调用library搜索函数（没找到有提示），更新book_curr、book_history向量

//查看借阅记录 //book_history显示已借阅书籍信息（不含借阅时间）

friend Library;
```

 ### log function

```cpp
Reader* reader_log();//用户登陆
//登陆成功返回该用户指针，之后对该指针进行操作；登陆失败且取消登陆则返回空指针
//引导输入账号密码，调用library成员函数Reader* search_reader(string accnum)查找输入账号对应用户，若密码匹配则登陆成功

Admin* admin_log();//管理员登陆
//同上

```

### class book

```cpp
class Book
private:
Book* next_book;//下一本书//用链表+动态节点存书
string isbn;
string book_name;//书名
string author;//作者名
string classifier;//分类号

//借阅者相关信息
string reader_accnum;//当前借阅本书的用户账号
string date;// 当前借阅本书的用户的借出日期
bool flag;//1 已借出

public:
//构造函数
//拷贝构造函数
//重载赋值运算符
//析构函数 

//用于还书借书：
//修改借阅者账号
void setReader_accnum(string acc);
//修改借出日期
void setDate(string ndate);

//显示书籍信息 //一行
void display();
//显示书籍信息（不显示借阅者相关信息） //一行
void display_raw();

//友元
friend Admin;

```

## main

```cpp
//注意交互，询问获取输入
主界面（功能）
    用户登录
        退出登陆
        ……
    管理员登陆
        退出登陆
        ……
    书目检索
    关闭系统//写文件，界面显示
```

## further modify

```cpp
//提升搜索性能：
仍是一个书籍链表
图书馆类增加数据成员vector<Book*> top_clasf_heads;//字母分类号分类，各类的“链首”
vector<Book*> top_clasf_ends; //各类的“链尾”
//增加书籍时链到各分类的“链尾”//提升按分类号范围搜索的性能，从对应分类号“链首”开始
book0,book1,…,booki,booki+1,…,bookj,bookj+1,…… ,bookk,bookk+1,…
↑                 ↑           … ↑                    ↑
Ahead            Bhead            Thead               Tphead

```

