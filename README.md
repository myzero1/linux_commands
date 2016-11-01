# linux_commands
常用的linux命令

批量修改文件命令
```bash
sed -i "s|oldstring|newstring|g" $(grep oldstring -rl dir)
```
查看监听端口
```bash
netstat -ntlp
```
