### nvim

命令

| 命令 | 说明                        | 快捷键                   |      |
| ---- | --------------------------- | ------------------------ | ---- |
| :sp  | 水平分屏                    |                          |      |
| :vsp | 垂直分屏                    |                          |      |
|      | 当前面板向右/左加宽         | ctrl-w >/< \| ctrl-w ]/[ |      |
|      | 当前面板向右/左加宽 n       | ctrl-w [n] >/<           |      |
|      | 将当前面板移到最上/下/左/右 | ctrl-w K/J/H/L           |      |

快捷键

| 快捷键       | 说明                                  | 频率(数字越小越频繁) | plugin        |
| ------------ | ------------------------------------- | -------------------- | ------------- |
| SPC f b      | 跳转到被 mark 的位置                  |                      |               |
| SPC t c      | 切换主题                              |                      |               |
| SPC f f      | 查找文件                              | 1                    |               |
| SPC f h      | 最近打开的文件                        |                      |               |
| SPC f a      | 查找单词                              | 1                    |               |
| SPC f w      | 查找单词                              | 1                    |               |
| SPC f v      | 查找单词                              | 1                    |               |
| SPC s l      | 最近的 session                        |                      |               |
| SPC c n      |                                       |                      |               |
| <leader>bx   | 关闭当前标签                          | 2                    |               |
| <leader> 1-0 | 切换标签                              | 1                    |               |
| SPC f W      | 切换窗口                              |                      |               |
| <leader> ce  | 查看目录下文件的 warning, error       | 4                    |               |
| <leader> ;   | 查看 coc 扩展                         |                      | coc.nvim      |
| <leader>.    | 查看 coc 命令                         |                      | coc.nvim      |
| <leader>cS   | 查看启动的服务                        |                      | vim-clap      |
| <leader>ct   | 单词大纲                              | 5                    | vim-clap      |
| <leader>i    | 函数变量索引                          | 2                    | vista.vim     |
| <leader>cr   | 快速启动                              |                      | vim-quickrun  |
| <leader>t    | 浮动命令行                            | 2                    | vim-floaterm  |
| <leader>m    | 操作(undo)统计                        |                      | vim-mundo     |
| gsj          | 快速移动 easymotion(向前)             |                      | easymotion    |
| gsk          | 快速移动 easymotion(向后)             |                      | easymotion    |
| gsf          | 快速移动 easymotion(根据单词)         |                      | easymotion    |
| sa           | 首尾添加(), ""等                      |                      | vim-sandwich  |
| sd           | 首尾删除()等                          |                      | vim-sandwich  |
| sdb          | 删除单词()等                          |                      | vim-sandwich  |
| srb          | 替换单词首尾()为其他                  |                      | vim-sandwich  |
| dsf          | 删除最外层{}变量名?                   |                      | dsf.vim       |
| csf          | 删除上一层变量名                      |                      | dsf.vim       |
| sj           | 单行代码与多行代码之间切换(代码折叠?) |                      | splitjoin.vim |
| sk           | 展开代码                              |                      | splitjoin.vim |
| <leader> e   | 展开/收起文件目录                     | 1                    | defx.nvim     |
| gci          | 添加注释                              |                      | caw.vim       |
| gcui         | 删除注释                              |                      | caw.vim       |
| gcc          | 添加/删除注释                         | 1                    | caw.vim       |
| gcI          | 在当前行第一个字符处添加注释          |                      | caw.vim       |
| gcuI         | 删除当前行第一个字符处的注释          |                      | caw.vim       |
| gca          | 在当前行后面添加注释                  | 2                    | caw.vim       |
| gcua         | 删除当前行后面的注释                  |                      | caw.vim       |
| gcw          | 添加块注释                            |                      | caw.vim       |
| gcuw         | 删除块注释                            |                      | caw.vim       |