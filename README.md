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

下载git上面的文件库，如docker的文件都是放在亚马逊的s2上的国内访问很慢，但是每次重连的时候会有短暂的速度，所有写这个脚本来下载.脚本名字wget.sh
```bash
url="https://github.com/boot2docker/boot2docker/releases/download/v1.12.5/boot2docker.iso"
#url="https://github.com/walkor/Workerman/archive/master.zip"

for((i=1;i<=300;i++));
#do echo $(expr $i \* 4);
do
        #echo $i
        wget -c "$url" 2>outputfile &
        sleep 60;
        ps -A |grep wget| cut -c 1-6|xargs kill -9
        end=$(cat outputfile | grep 'The file is already fully retrieved; nothing to do.');
        if [[ $end ]]; then
                i=99999999;
        fi
done

bash wget.sh 运行下载脚本
tail -f outputfile  查看进度
ps -aux | grep wget.sh 查看脚本的pid
kill -f pid  结束下载脚本
```
