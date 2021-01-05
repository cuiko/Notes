# Linux 命令笔记

- lsof -i:[端口号]
- tar -zxvf 解压文件
- tar -zcvf 压缩文件

* kill -9 [pid]

* chmod -v 755 4:读/2:写/1:执行, user/group/other

* chown -R user:group dir

* nohup [xxx.sh] &

* mdfind -name 文件名 [mac 限定]

* sudo --preserve-env=PATH ./install.sh

* ln -s 源目录 目标目录 创建软链接

* cat /etc/shells

* chsh -s /bin/zsh

* echo $SHELL

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

    