**eim下载问题**
交互问题在wsl中使用eim命令打开gui下载出现问题（可能是图形工具没有准备好，以及cpu资源紧张）
解决：使用命令行 `eim install` 解决

---

**Windows系统间传输问题**
用属性-高级设置连接问题
解决：填对方电脑的用户名和密码（开机密码）

---

**Linux文件直接复制到本机Windows**
解决：直接复制到mnt
```
cp -r ~/esp/console_example /mnt/e/u_w/
```

---

**ESESIXPACKAGE拓展名文件不能被ESIX识别**
解决：换成esix拓展名可被识别

---

**WSL连接本机网络被拒绝**
开tun/虚拟网卡模式也不行，从powershell打开ubuntu终端时发现"wsl: 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理"
解决：win键搜索wsl setting，把第一栏nat模式改为mirror

---

**json设置无效（划白线）**

---

**idf自带宏标红**
解决：检查export.sh了没，build后重新打开可解决

---

**形如 `#Failed to resolve component 'esp_log' required by component 'main': unknown name` 的报错**
组件名错误或者没有填在cmakelist中
解决：
- ctrl+点击头文件名，跳转到的文件若在component中
- 鼠标悬停在跳转到的文件标签上，显示的component目录下的根目录级（若其中含cmakelist）的名字即为组件名
- 填在项目文件夹中main文件夹的cmakelist里的PRIV_REQUIRES和REQUIRES后面

---

**ESP32-S3 Waveshare触摸屏板烧录后monitor立刻断开、无法重连**
两个原因叠加。

原因一（板子特有）：触摸屏初始化函数 `waveshare_esp32_s3_touch_reset()` 向CH422G写0x2C/0x2E，bit5=1，EXIO5被拉高，GPIO19/20（USB D+/D-）被物理切换到CAN收发器，USB断线，ESP32-S3 USB外设触发 `USB_UART_CHIP_RESET`，死循环。
解决：写CH422G前强制清除bit5，所有直接写CH422G的地方改为调用此函数：
```c
static void ch422g_set_output(uint8_t val) {
    uint8_t cfg = 0x01;
    i2c_master_write_to_device(I2C_MASTER_NUM, 0x24, &cfg, 1, I2C_MASTER_TIMEOUT_MS / portTICK_PERIOD_MS);
    val &= ~0x20;  // bit5=EXIO5强制为0，USB不被切到CAN
    i2c_master_write_to_device(I2C_MASTER_NUM, 0x38, &val, 1, I2C_MASTER_TIMEOUT_MS / portTICK_PERIOD_MS);
}
```

原因二（配置问题）：默认主控制台是UART0物理引脚，USB-JTAG只是次级控制台，monitor看不到任何输出。
解决：`sdkconfig.defaults` 加入以下两行，然后 `idf.py fullclean && idf.py build`（必须fullclean才能删掉旧sdkconfig重新生成）：
```
CONFIG_ESP_CONSOLE_USB_SERIAL_JTAG=y
CONFIG_ESP_CONSOLE_SECONDARY_NONE=y
```
