
mv "文件名/文件夹名"md ~"文件位置"

cd ..
cd ~/
alias查看哪些设置了别名
nano ~/.bashrc
source ~/.bashrc

云端服务器ECS
看服务状态
```
systemctl status cloud_api
```
看实时日志
```
journalctl -u cloud_api -f
```
改完代码重启
```
systemctl restart cloud_api
```
手动停
```
systemctl stop cloud_api
```

```
idf.py fullclean
idf.py set-target esp32s3
idf.py build
```


在wsl端设置板子
先win powershell输入  
```
usbipd list
usbipd bind --busid 1-3
usbipd attach --wsl --busid 1-3
```
再wsl终端
```
ls /dev/ttyUSB* /dev/ttyACM*
sudo chmod 666 /dev/ttyACM0
idf.py -p /dev/ttyACM0 flash
```

nrf
1. 编译build
 ```  
west build -b xiao_ble/nrf52840/sense
```
2. 打包成 DFU 固件
```
adafruit-nrfutil dfu genpkg --dev-type 0x0052 --application build\zephyr\zephyr.hex firmware.zip
```
3. 烧录
先双击板子上的 RESET 进 bootloader(出现f盘后)，看设备管理器记下新出现的 COM 号，把下面的 COM<x> 换成它：
```
adafruit-nrfutil --verbose dfu serial --package firmware.zip -p COM11 -b 115200 --singlebank
```
串口监视
```
python -m serial.tools.miniterm COM13 115200
```

4.戴上取下重录命令
```
cd E:\nrf\posture_band_nrf\tools
python ble_detect.py
python wear_recorder.py
```


