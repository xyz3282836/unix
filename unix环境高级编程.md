# 1.Unix基础

POSIX.1

文件名：a-z,A-Z,0-9,.,-,_

ls命令的简要实现：

```c
int

main(int argc,char *argv[])

{

DIR *dp;

struct dirent *dirp;

if(argc != 2)

	err_quit("usage: ls directory_name");

if((dp = opendir(argv[1]) == NULL)

	err_sys("can't open %s",argv[1]);

while((dirp = readdir(dp)) != NULL)

	printf("%s \n",dirp->d_name);

closedir(dp);

exit(0);

}

```

标准输入(1)、标准输出(2)、标准错误(3)

不带缓冲的I/O函数：open、read、write、lseek、close

程序是一个存储在磁盘上的某个目录中的可执行文件

**7个exec函数（7种exec的变体）**

**fork**创建一个新进程，对父进程返回子进程的进程ID，对子进程返回0；所以调用一次（在父进程），但是返回两次（父进程和子进程）

**execlp**执行从标准输入的命令，这就用新的程序文件替换了子进程原先执行的程序文件。

fork和execlp和waitpid组合就是产生spawn一个新进程

子进程调用execlp执行新程序文件，而父进程希望等待子进程终止，这是通过waitpid实现的。

**用户标识**  用户ID、组ID，4个字节（双字节整型存放）

**信号** 进程三种方式处理信号



# 2.Unix标准和实现

3个独立组织制定的3个标准：ISO C、IEEE POSIX、Single UNIX Specification

具体实现：SVR4、BSD、FreeBSD、Linux、Mac OS X、Solaris、其他Unix系统