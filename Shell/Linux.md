# Linux 命令笔记

- lsof -i:[端口号]
- tar -zxvf 解压文件
- tar -zcvf 压缩文件
- pwd -P
- ctrl + c 中断目前程序
- ctrl + d 键盘输入程序(EOF), 也可以取代 exit 的输入
- cd -
- cal 2021 日历
- mv
- bc 计算器 scale=3 小数点位数 quit 退出
- basename
- dirname
- command \ [Enter] 换行
- command --help 查看帮助
- man 查看操作手册
- date
- [sed 流编辑器](hJttp://sed.sourceforge.net/sed1line_zh-CN.html)
- seq 产生某个数到另一个数之间的所有整数
- diff
- wc 统计
  - -l 统计行数
  - -w 统计字数
- head 显示文件开头的内容
  - -n <数字> 指定头部行数
  - -c<字符数> 指定头部内容字符数
- tail 输出文件尾部内容
- locale 显示目前支持的语系
- mkdir -p -m 711
- cat 输出文件全部内容(正序)
- tac 输出文件全部内容(倒序)
- nl 显示文件全部内容(带行号)
- more 浏览文件内容(不一次性输出完)
- less 浏览文件内容(不一次性输出完)(可向前翻页)
- rmdir -p
  - od 读取二进制文件	

* kill -9 [pid]

* chmod -v 755 4:读/2:写/1:执行, user/group/other

  * | chmod | u g o a | + (加入) - (减去) = (设置) | rwx | 文件或目录 |

* chown -R user:group dir

* chgrp

* file 查看文件类型

* shutdown, reboot, halt, poweroff

* nohup [xxx.sh] &

* mdfind -name 文件名 [mac 限定]

* sudo --preserve-env=PATH ./install.sh

* which 查看 PATH 变量下指令的位置

* whereis 查找特定目录下的文件或目录

* locate 从数据库中查询??? updatedb 更新数据库

* find 查找

  * -mtime -4 查找 4 天内的内容
  * -mtime +4 查找 4 天前的内容
  * -mtion 4 查找 4-5 天前的内容

* ln -s 源目录 目标目录 创建软链接

* find . -name "*.go" | xargs rm -f 删除当前目录下所有后缀名为"go"的文件

* cat /etc/shells

* chsh -s /bin/zsh

* echo $SHELL

* passwd 修改密码

* curl [options...] <url>

  * | option           | description                                       | usage                                                        | tips                                                         |
    | ---------------- | ------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | -i               | 查看协议头                                        | curl -i <url>                                                |                                                              |
    | -x               | 使用代理                                          | curl -x <proxy url> <taget url>                              |                                                              |
    | -X               | 指定 HTTP 请求方式                                | curl -X POST <url>                                           |                                                              |
    | -A               | 指定客户端代理标头(User-Agent)                    | curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' <url> |                                                              |
    | -H               | 指定客户端标头                                    | curl -H 'User-Agent: php/1.0' <url>                          |                                                              |
    | -b               | 向服务器发送Cookie                                | curl -b 'foo1=bar1;foo2=bar2' <url>                          |                                                              |
    | -c               | 将服务器设置的Cookie写入一个文件                  | curl -c cookies.txt <url>                                    |                                                              |
    | -d               | 发送POST请求的数据体                              | curl -d 'username=foo' -d 'password=foo&code=001' -X POST  <url><br />读取本地文件的数据<br />curl -d '@data.txt' <url> | -d 会自动在标头加上 Content-Type: application/x-www-form-urlencoded, 并将请求转换为 POST 方法, 可以省略 -X POST |
    | --data-urlencode | 等同于 -d, 区别是数据会进行 URL 编码              | curl --data-urlencode 'comment=i have space' <url>           |                                                              |
    | -e               | 设置客户端Referer标头                             | curl -e 'https://google.com?q=exampl' https://www.example.com |                                                              |
    | -F               | 向服务器上传二进制文件                            | curl -F 'file=@photo.png;type=image/png;filename=newname.png' <url> | 标头会加上 Content-Type: multipart/form-data, 将 photo.png 作为 file 字段上传 |
    | -G               | 构造URL的查询字符串(GET请求)                      | curl -G -d 'q1=1' -d 'q2=2' <url>                            | url?q1=1&q2=2, 省略 -G 会变是 POST 请求                      |
    | -I, --head       | 发送 HEAD 请求                                    | curl -I <url>                                                |                                                              |
    | -k               | 跳过 SSL 检测                                     | curl -k <url>                                                | 不检查服务器的 SSL 证书是否正确                              |
    | -L               | 请求跟随服务器的重定向                            | curl -L <url>                                                | curl 默认不跟随重定向                                        |
    | --limit-rate     | 显示HTTP请求和回应的带宽, 模拟慢网速的环境        | curl --limit-rate 200k <url>                                 |                                                              |
    | -o               | 将服务器的响应结果保存成文件                      | curl -o data.html <url>                                      |                                                              |
    | -O               | 将服务器结果保存成文件, 将URL的最后部分当场文件名 | curl -O localhost:3000/install.sh                            |                                                              |
    | -s               | 不输出错误和进度信息                              | curl -s <url>                                                |                                                              |
    | -S               | 不输出内容, 只有当错误时才输出                    | curl -S <url>                                                |                                                              |
    | -u               | 设置服务器认证的用户名和密码                      | curl -u 'username:password' <url><br />curl https://username:password@google.com/login<br />curl -u 'username' https://google.com/login 会提示用户输入密码 |                                                              |
    | -v               | 输出通信的整个过程                                | curl -v <url>                                                |                                                              |
    | --trace          | 输出通信的整个过程(二进制数据)                    | curl --trace - <url>                                         |                                                              |

* 创建文件

  * | command                    | description         | tips |
    | -------------------------- | ------------------- | ---- |
    | touch test.txt             |                     |      |
    | touch test1.txt test2.txt  |                     |      |
    | echo "content" > test.txt  | > 直接覆盖原文件    |      |
    | echo "content" >> test.txt | >> 在原文件末尾追加 |      |

* shell

  * 双引号内特殊字符保留原字符特性, 单引号内特殊字符仅为一般文本

  * 取消变量 unset 变量名

  * \$var/\${var} 使用变量

  * $(uname -r) / `uname -r` 借助其他指令

  * 等号左右不能有空格

  * 生成随机数 echo ${RANDOM}

  * 特殊变量

    * $? 指令回传值, 成功为 0

  * 在 PATH 添加目录

    * ok
      * PATH=$PATH:/xxx/xxx
      * PATH=${PATH}:/xxx/xxx
      * PATH="$PATH":/xxx/xxx

  * export 导出到环境变量, 直接赋值仅在当前的 shell 内使用

  * PS1 提示字符格式

  * 变量内容的删除/取代/替换

    * \# 删除由左往右最少匹配次数的内容

    * \#\# 删除由左往右最多匹配次数的内容

    * % 删除由右往左最少

    * %% 删除由右往左最多

    * ---

    * / 替换内容(最短匹配(一次))

    * // 替换内容(全部)

    * ---

    * 默认值
    
  * shift 推出参数

  * <<< ?????????????????????????

* read 读取键盘输入的变量

* declare 类型声明
  * -i
  * -r
  * -x/+x
  * -a
  
* ulimit

* alias, unalias

* printf

* awk 按行执行指令 awk 'condition1{action1} condition2{action2}' filename
  * $0 当前行的数据
  * $1, 2, ... N 第 N 栏数据
  * 内置变量
    * BEGIN
    * NF 每一行($0)拥有的字段总数
    * NR 当前处理的是第几行
    * FS 当前的分割字符, 默认是空白
  
* history

* 指令运行顺序
  1. 绝对或相对路径
  2. alias
  3. builtin 指令
  4. $PATH
  
* source 读入环境配置文件

* \> 覆盖, \>\> 累加
  * 标准输入 0 < / <<
  * 标准输出 1 > / >>
  * 标准错误输出 2 2> / 2 >>
  * 2>&1 2 的结果输出到 1
  
* /dev/null

* 单纯地连续执行 cmd; cmd

* && 前一个命令执行成功后再执行后一个命令

* || 前一个命令执行失败才执行后一个命令, 如果前面一个命令执行成功则不执行后一个命令

* 管道符 |, | 后的指令必须要是能够接受 stdin 的指令

* sort 排序
  
  * -r 反向排序
  
* last

* tr 替换或者删除文字
  
  * -d 删除
  
* uniq 删除重复行
  * -i 忽略大小写
  * -c 显示计数
  
* grep

* col -x tab => 空格

* cut
  * -d 分隔符
  * -f 获取第几段内容
  * -c 区间 2,5 获取第二个到第五个字符
  
* tee 将数据流分送到文件和屏幕(stdout)

* join (最好先排序)

* paste

* expand/unexpand tab => 空格

* split 文件分割
  * -b 指定文件大小单位
  * -l 以行数进行分割
  * 文件合成 cat xxx* > xxx
  
* xargs

* ps

  * -e / -A / agx 查看所有进程
  * -p / p / --pid 查看指定 pid 的进程
  * -s / --sid 查看指定 sid 的进程
  * aux / auxg / agx u / -e u

* pidof

### FQA

-  Syntax error: "(" unexpected 报错

  ```bash
  sudo dpkg-reconfigure dash
  ```

  