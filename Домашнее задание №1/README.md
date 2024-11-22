1. Узнать IP-адрес интерфейса, подключенного к сети Интернет:
```bash
[root@devops-slonit ~]# ip a | grep 'inet ' | grep -v 127.0.0.1
    inet 93.183.72.91/24 brd 93.183.72.255 scope global noprefixroute enp0s5
```

2. Создать именованный пайп ( named pipe ). Вывести в файл через созданный pipe вывод команды ss -plnt:
```bash
[root@devops-slonit ~]# mkfifo my_pipe
[root@devops-slonit ~]# ss -plnt > my_pipe &
[1] 33364
[root@devops-slonit ~]# cat my_pipe > output.txt
[1]+  Done                    ss -plnt > my_pipe
[root@devops-slonit ~]# cat output.txt
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=1123,fd=5))
```

3. При помощи именованного пайпа заархивировать всё, что в него отправляем. Например, содержимое файла /var/log/messages. На выходе получить архив tar или любой другой:
```bash
[root@devops-slonit ~]# mkfifo my_pipe
[root@devops-slonit ~]# cat /var/log/messages > my_pipe &
[3] 48921
[root@devops-slonit ~]# cat my_pipe | gzip -f > archiv.gz
[3]+  Done                    cat /var/log/messages > my_pipe
```

4. Вывести дату в unixtime. На вход команды date через пайп подать свой формат выводимой даты:
```bash
[root@devops-slonit ~]# date +%s | xargs -I{} date -d @{} +"%H:%M %d.%m.%Y"
16:19 22.11.2024
```

5. При помощи HEREDOC записать в файл многострочное сообщение:
```bash
[root@devops-slonit ~]# cat << EOF > 5.txt
> this is the first line
> Hello Slonit!
> EOF
```
