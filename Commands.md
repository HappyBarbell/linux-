
mv "文件名/文件夹名"md ~"文件位置"

cd ..
cd ~/
alias查看哪些设置了别名
nano ~/.bashrc
source ~/.bashrc

```
idf.py fullclean
idf.py set-target esp32s3
idf.py build
```


在wsl端设置板子
先win powershell输入  
```
usbipd list
usbipd bind --busid <你的BUSID>
usbipd attach --wsl --busid <你的BUSID>
```
再wsl终端
```
ls /dev/ttyUSB* /dev/ttyACM*
sudo chmod 666 /dev/ttyACM0
idf.py -p /dev/ttyACM0 flash
```

