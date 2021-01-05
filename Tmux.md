## tmux

进入 tmux

退出 exit

前缀键

新建会话 tmux new -s <session-name>

当前会话与窗口分离 tmux detach

查看所有会话 tmux ls

接入会话 tmux attach -t <session-index> 或 <session-name>

杀死会话 tmux kill-session -t <session-index> 或 <session-name>

切换会话 tmux switch -t <session-index> 或 <session-name>

| 命令                                                         | 说明                 | 快捷键    | 备注                 |
| ------------------------------------------------------------ | -------------------- | --------- | -------------------- |
|                                                              | 前缀键               | C-z       |                      |
|                                                              | 进入窗口选择界面     | C-z w     |                      |
|                                                              | 显示面板序号         | C-z q     |                      |
|                                                              | 选择不同布局         | C-z space |                      |
|                                                              | 进入命令模式         | C-z :     |                      |
| tmux new -s <session-name>                                   | 新建会话             | C-z ,     |                      |
| tmux detach                                                  | 当前会话与窗口分离   | C-z d     |                      |
| tmux attach -t <session-name> 或 <session-index>             | 接入会话             |           |                      |
| tmux kill-session -t <session-name> 或 <session-index>       | 杀死会话             |           |                      |
| tmux rename-session -t <session-name> 或 <session-index> <new-name> | 重命名会话           | C-z $     |                      |
| tmux ls                                                      | 查看所有会话         | C-z s     |                      |
|                                                              | 切换窗口             | C-z 0 - 9 |                      |
|                                                              | 重命名窗口           | C-z ,     |                      |
|                                                              | 创建新窗口           | C-z c     |                      |
|                                                              | 关闭当前窗口         | C-z &     |                      |
|                                                              | 当前面板转到新的窗口 | C-z !     |                      |
|                                                              | 关闭面板             | C-z x     |                      |
|                                                              |                      |           |                      |
| tmux split-window -v                                         | 水平拆分窗口         | C-z -     |                      |
| tmux split-window -h                                         | 垂直拆分窗口         | C-z \     |                      |
| tmux select-pane -U                                          | 光标切换到上方窗口   | M k       | M 为 Meta 键, 有修改 |
| tmux select-pane -D                                          | 光标切换到下方窗口   | M j       |                      |
| tmux select-pane -L                                          | 光标切换到左边窗口   | M h       |                      |
| tmux select-pane -R                                          | 光标切换到右边窗口   | M l       |                      |
| tmux resize-pane -L                                          | 调整窗口大小(向左)   | M H       |                      |
| tmux resize-pane -D                                          | 调整窗口大小(向下)   | M J       |                      |
| tmux resize-pane -R                                          | 调整窗口大小(向右)   | M  L      |                      |
| tmux resize-pane -U                                          | 调整窗口大小(向上)   | M K       |                      |