# system和popen

##  1、system、[popen](https://so.csdn.net/so/search?q=popen&spm=1001.2101.3001.7020)函数简介

system 和 popen 都是通过调用 fork->exec->[waitpid](https://so.csdn.net/so/search?q=waitpid&spm=1001.2101.3001.7020)的处理流程，完成在另外一个进程中执行shell命令，使用者只需关心最后的返回值即可

## 2、源码

system源码

```c++
int system(const char *command)
{
    struct sigaction sa_ignore, sa_intr, sa_quit;
    sigset_t block_mask, orig_mask;
    pid_t pid;

    sigemptyset(&block_mask);
    sigaddset(&block_mask, SIGCHLD);
    sigprocmask(SIG_BLOCK, &block_mask, &orig_mask);        //1. block SIGCHLD

    sa_ignore.sa_handler = SIG_IGN;
    sa_ignore.sa_flags = 0;
    sigemptyset(&sa_ignore.sa_mask);
    sigaction(SIGINT, &sa_ignore, &sa_intr);                //2. ignore SIGINT signal
    sigaction(SIGQUIT, &sa_ignore, &sa_quit);                //3. ignore SIGQUIT signal

    switch((pid = fork()))
    {
        case -1:
            return -1;
        case 0:
            sigaction(SIGINT, &sa_intr, NULL); 
            sigaction(SIGQUIT, &sa_quit, NULL); 
            sigprocmask(SIG_SETMASK, &orig_mask, NULL);
            execl("/bin/sh", "sh", "-c", command, (char *) 0);
            exit(127);
        default:
            while(waitpid(pid, NULL, 0) == -1)    //4. wait child process exit
            {
                if(errno != EINTR)
                {
                    break;
                }
            }
    }
}
return 0;

```

popen源码

```c++
static pid_t    *childpid = NULL;  
                        /* ptr to array allocated at run-time */  
static int      maxfd;  /* from our open_max(), {Prog openmax} */  

#define SHELL   "/bin/sh"  

FILE *  
popen(const char *cmdstring, const char *type)  
{  
    int     i, pfd[2];  
    pid_t   pid;  
    FILE    *fp;  

            /* only allow "r" or "w" */  
    if ((type[0] != 'r' && type[0] != 'w') || type[1] != 0) {  
        errno = EINVAL;     /* required by POSIX.2 */  
        return(NULL);  
    }  

    if (childpid == NULL) {     /* first time through */  
                /* allocate zeroed out array for child pids */  
        maxfd = open_max();  
        if ( (childpid = calloc(maxfd, sizeof(pid_t))) == NULL)  
            return(NULL);  
    }  

    if (pipe(pfd) < 0)  
        return(NULL);   /* errno set by pipe() */  

    if ( (pid = fork()) < 0)  
        return(NULL);   /* errno set by fork() */  
    else if (pid == 0) {                            /* child */  
        if (*type == 'r') {  
            close(pfd[0]);  
            if (pfd[1] != STDOUT_FILENO) {  
                dup2(pfd[1], STDOUT_FILENO);  
                close(pfd[1]);  
            }  
        } else {  
            close(pfd[1]);  
            if (pfd[0] != STDIN_FILENO) {  
                dup2(pfd[0], STDIN_FILENO);  
                close(pfd[0]);  
            }  
        }  
            /* close all descriptors in childpid[] */  
        for (i = 0; i < maxfd; i++)  
            if (childpid[ i ] > 0)  
                close(i);  

        execl(SHELL, "sh", "-c", cmdstring, (char *) 0);  
        _exit(127);  
    }  
                                /* parent */  
    if (*type == 'r') {  
        close(pfd[1]);  
        if ( (fp = fdopen(pfd[0], type)) == NULL)  
            return(NULL);  
    } else {  
        close(pfd[0]);  
        if ( (fp = fdopen(pfd[1], type)) == NULL)  
            return(NULL);  
    }  
    childpid[fileno(fp)] = pid; /* remember child pid for this fd */  
    return(fp);  
}  

```



## 3、差异

可以看到，system命令在执行时，首先进程本身屏蔽了SIGCHLD（防止其他进程退出，waitpid获取到错误状态），随后在子进程中忽略了SIGINT和SIGQUIT信号，父进程则调用waitpid，阻塞式等待回收子进程，子进程执行[shell命令](https://so.csdn.net/so/search?q=shell命令&spm=1001.2101.3001.7020)后，调用exit函数退出进程。

而popen函数，则是创建一个管道后，进行fork，子进程关闭管道的读端，随后执行命令并退出，父进程关闭管道的写端，随后打开管道的读端，并保存子进程pid，最后返回管道读端的文件描述符。
最后的子进程回收通过pclose函数完成



## 4、总结

1）system 函数需要阻塞式等待命令执行完毕，而popen函数不阻塞等待，在pclose执行才回收子进程。
2）sytem函数会在执行中对信号进行屏蔽操作，而popen不会。当屏蔽SIGCHILD时，主进程无法使用捕捉函数回收子进程，在多个子进程未回收的情况下，下次fork进程时，可能提示进程数量不足的问题。
3）system函数返回值不好判断，需要判断errno，popen函数返回值判NULL即可。

尽量使用popen去代替system函数
