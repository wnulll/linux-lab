# Linux 常用命令

> 约定：`$` 开头是命令，`#` 开头是注释；`<>` 表示需要替换的内容。

---

## 一、目录与文件浏览

| 命令 | 作用 |
|---|---|
| `pwd` | 显示当前所在目录的绝对路径 |
| `ls` | 列出目录内容 |
| `cd` | 切换目录 |

```bash
# 显示当前位置
$ pwd
/home/czh

# 列出当前目录
$ ls
# 显示详细信息（权限、大小、时间）
$ ls -l
# 显示隐藏文件
$ ls -a
# 详细信息 + 人类可读的大小
$ ls -lh

# 进入某个目录
$ cd /usr/local
# 返回上一级
$ cd ..
# 回到自己的主目录
$ cd ~
# 回到上一次所在的目录
$ cd -
```

---

## 二、创建与删除

| 命令 | 作用 |
|---|---|
| `mkdir` | 创建目录 |
| `touch` | 创建空文件 / 更新时间戳 |
| `rmdir` | 删除空目录 |
| `rm` | 删除文件或目录 |

```bash
# 创建目录
$ mkdir lab
# 递归创建多级目录
$ mkdir -p lab/linux/notes

# 创建空文件
$ touch hello.txt

# 删除空目录
$ rmdir lab/empty

# 删除文件（-i 会先询问）
$ rm hello.txt
$ rm -i hello.txt

# 递归删除目录及其内容（⚠️ 危险，删了不可恢复）
$ rm -r lab
```

> ⚠️ `rm -rf /` 之类千万不要执行。删除前先用 `ls` 确认路径。

---

## 三、复制、移动与重命名

```bash
# 复制文件
$ cp a.txt b.txt
# 复制目录（需要 -r）
$ cp -r dir1 dir2

# 移动 / 重命名
$ mv a.txt b.txt          # 同目录下改名
$ mv a.txt /tmp/          # 移动到 /tmp
$ mv dir1 /opt/           # 移动目录
```

---

## 四、查看文件内容

| 命令 | 作用 |
|---|---|
| `cat` | 一次性输出整个文件 |
| `less` | 分页查看（可上下翻） |
| `head` | 查看开头若干行 |
| `tail` | 查看末尾若干行 |
| `wc` | 统计行数 / 字数 / 字节数 |

```bash
$ cat hello.txt
$ less /etc/passwd          # 空格翻页，q 退出

$ head -n 20 access.log     # 前 20 行
$ tail -n 20 access.log     # 后 20 行
$ tail -f access.log        # 持续跟踪新增内容（看日志常用）

$ wc -l hello.txt           # 行数
```

---

## 五、查找与搜索

```bash
# 按文件名查找
$ find . -name "*.md"
$ find /home -type f -name "*.txt"

# 在文件内容里搜索
$ grep "error" access.log
# 忽略大小写 + 显示行号
$ grep -in "error" access.log
# 递归搜索整个目录
$ grep -r "TODO" ./src
```

---

## 六、权限管理

```bash
# 查看当前文件权限
$ ls -l
# -rw-r--r-- 1 czh czh 1024 Nov  3 10:00 hello.txt
#  ↑↑↑ ↑↑↑ ↑↑↑
#  属主 属组 其他人

# 数字方式改权限（r=4 w=2 x=1）
$ chmod 755 run.sh          # 属主 rwx，其他 r-x
$ chmod 644 hello.txt       # 属主 rw-，其他 r--

# 符号方式改权限
$ chmod +x run.sh           # 增加执行权限
$ chmod -w hello.txt        # 去掉写权限

# 改文件属主
$ sudo chown czh:czh hello.txt
```

---

## 七、进程管理

```bash
# 查看当前所有进程
$ ps aux

# 动态查看进程与资源占用（q 退出）
$ top
$ htop                      # 更友好的版本，可能需要安装

# 结束进程（按 PID）
$ kill 1234
$ kill -9 1234              # 强制结束

# 按名字结束
$ killall firefox
```

---

## 八、系统信息与资源

```bash
$ uname -a                  # 内核与系统信息
$ whoami                    # 当前用户
$ date                      # 当前时间
$ uptime                    # 开机时长与负载

$ df -h                     # 磁盘空间（人类可读）
$ du -sh /var/log           # 某目录占用的总大小

$ free -h                   # 内存使用情况
```

---

## 九、压缩与解压

```bash
# 打包 + 压缩为 .tar.gz
$ tar -czvf backup.tar.gz dir/

# 解压 .tar.gz
$ tar -xzvf backup.tar.gz

# 查看压缩包内容（不解压）
$ tar -tzvf backup.tar.gz
```

参数记忆：`c` 创建、`x` 解压、`t` 列出、`z` gzip、`v` 显示过程、`f` 指定文件名。

---

## 十、网络相关

```bash
$ ping -c 4 baidu.com       # 测试连通性
$ ip addr                   # 查看本机 IP（新系统）
$ ifconfig                  # 旧命令

$ ssh user@192.168.1.10     # 远程登录

$ wget https://example.com/file.zip      # 下载
$ curl -O https://example.com/file.zip   # 下载（也可用来测试接口）
$ curl https://api.github.com            # 直接输出返回内容
```

---

## 十一、帮助与手册

```bash
$ man ls                    # 查看完整手册（q 退出）
$ ls --help                 # 查看简短帮助
$ whatis ls                 # 一句话说明
```

---

## 十二、快捷键（命令行里很实用）

| 按键 | 作用 |
|---|---|
| `Tab` | 自动补全命令或路径 |
| `↑` / `↓` | 翻历史命令 |
| `Ctrl + C` | 中断当前命令 |
| `Ctrl + L` | 清屏（等于 `clear`） |
| `Ctrl + A` / `Ctrl + E` | 跳到行首 / 行尾 |
| `Ctrl + R` | 搜索历史命令 |

---

## 小结

这次实验里我实际用到的流程大致是：

```bash
# 1. 看自己在哪
$ pwd
# 2. 建一个工作目录
$ mkdir -p ~/lab/linux
# 3. 进去
$ cd ~/lab/linux
# 4. 建文件、写内容
$ touch notes.md
# 5. 看内容
$ cat notes.md
# 6. 查权限
$ ls -l
```

