

eim下载问题：交互问题在wsl中使用eim命令打开gui下载出现问题（可能是图形工具没有准备好，以及cpu资源紧张）使用命令行eim install解决


Windows系统间传输问题：用属性-高级设置连接问题，是填对方电脑的用户名和密码（开机密码）

Linux文件直接复制到本机windows，直接复制到mnt
cp -r ~/esp/console_example /mnt/e/u_w/

ESESIXPACKAGE拓展名文件不能被ESIX识别，换成esix拓展名可被识别

linux连接本机网络被拒绝：开tun/虚拟网卡模式模式

json设置无效（划白线）

出现一些idf自带宏标红，检查export.sh了没，build后重新打开可解决

形如 #Failed to resolve component 'esp_log' required by component 'main': unknown name的报错，是组件名错误或者没有填在cmakelist中
查找组件名：ctrl+点击头文件名，跳转到的文件若在component中，鼠标悬停在跳转到的文件标签上，显示的component目录下的根目录级（若其中含cmakelist）的名字即为组件名
查找后需要填在在项目文件夹中main文件夹的cmakelist里的PRIV_REQUIRES和REQUIRES后面



