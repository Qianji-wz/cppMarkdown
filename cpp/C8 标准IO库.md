# C8 标准IO库
## 8.1 文件的输入和输出
fstream头文件定义了三种支持文件IO的类型:
1. ifstream读文件
2. ofstream写文件
3. fstream可读可写同一个文件

文件的打开方式：  
ios::in     读文件  
ios::out    写文件  
ios::ate    初始位置：文件尾  
ios::app    追加方式写文件  
ios::binary 二进制方式打开文件  




读写文本文件：
```c++
ofstream ofs;//创建流对象，只写操作
ofs.open("text.txt", ios::out);//打开方式只写
ofs << "ni hao" << endl;
ofs << "zai jian" << endl;
ofs.close();
```

```c++
ifstream ifs;//创建流对象，只读操作
ifs.open("./text.txt", ios::in);//打开方式只读
if(!ifs.is_open())//检查文件是否打开成功
{
    cout << "file open failed" << endl;
    return -1;
}
//读方式1
string buf;
while(getline(ifs,buf))
{
cout << buf << endl;
}
//读方式2
char buf[1024] = {0};
while(ifs >> buf){cout << buf << endl;}
ifs.close();
```
如果想读写二进制文件：
```c++
ofs.open("textt.txt", ios::out|ios::binary);//二进制文件只写
```
## 8.2 输出缓冲区管理
缓冲区的内容被刷新，即将缓冲区的数据写入到真实的输出设备或文件：
1. 程序正常结束，清空所有输出缓冲区；
2. 缓冲区已满，缓冲区在写下一个值之前刷新；
3. 用操作符刷新缓冲区，如**endl**；
4. 每次输出操作后，用**unitbuf**设置流的内部状态；
5. 将输入流和输出流关联起来(tie)；

endl：用于输出一个换行符并刷新缓冲区  
flush：用于刷新流，但不在输出中添加任何字符  
ends：在缓冲区中插入空字符null，然后刷新
