<p align="center">
  <strong>README</strong> | <a href="https://github.com/Hongseme/OpenWrt/blob/main/README-R4S.md">R4S</a> | <a href="https://github.com/Hongseme/OpenWrt/blob/main/README-360v6.md">360v6</a>
</p>

# Openwrt packages
所有插件都为网上收集的开源仓库插件
- R4S适用于固件项目：https://github.com/QiuSimons/YAOF
- 360V6适用于lean固件 v22.3.8


### 安装基本工具包
> 含：`vim`、`cfdisk`、`rsync`、`git`、`git-http`、`usbutils`、`pciutils`、`lsof`、`coreutils-nohup`
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/base.sh)"
```

### 安装开发工具包
> 含：`golang`、 `erlang`、`rust`

```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/dev.sh)"
```

### 安装zsh、zsh-plugins、主题
- 安装zsh
```shell
# R4S
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/R4S/zsh.sh)"
# 360v6
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/zsh.sh)"
```
- 安装zsh-plugins、主题
```shell
# plugins
# R4S
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/R4S/zsh-plugins.sh)"
# 360v6
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/zsh-plugins.sh)"

# theme - Argon
wget --no-check-certificate https://github.com/jerrykuku/luci-theme-argon/releases/download/v1.7.0/luci-theme-argon_1.7.0-20200909_all.ipk
opkg install luci-theme-argon*.ipk
```

### 安装neofetch
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/neofetch.sh)"
```

### filebrowser
> 默认账号密码：`admin`
> web: `http://ip:8080`
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/filebrowser.sh)"
```

### speedtest-go
> web: `http://ip:8989`
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/speedtest-go.sh)"
```

## 工具脚本
- acme
```shell
curl https://get.acme.sh | sh
```

- DDNSTO
```shell
sh -c "$(curl -sSL http://fw.koolcenter.com/binary/ddnsto/openwrt/install_ddnsto.sh)"
```
- Linkease
```shell
sh -c "$(curl -sSL http://fw.koolcenter.com/binary/LinkEase/Openwrt/install_linkease.sh)"
```

## 常用Nginx配置
  - alist nginx config
  ```shell
  server {
	listen 8080 ssl http2;
	listen [::]:8080 ssl http2;

	server_name xxx.com;
	set $base /var/www/xx.com;
	root $base/public;

	# SSL
	ssl_certificate /path/crt.crt;
	ssl_certificate_key /path/key.key;

	location / {
      		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      		proxy_set_header Host $http_host;
      		proxy_set_header X-Real-IP $remote_addr;
      		proxy_redirect off;
      		proxy_pass http://127.0.0.1:5244;
     		# 上传的最大文件尺寸
      		client_max_body_size 20000m;
     }
}
  ```
  - filebrowser nginx config
  ```shell
  server {
	listen 8888 ssl http2;
	listen [::]:8888 ssl http2;

	server_name xx.com;
	set $base /var/www/xx.com;
	root $base/public;

	# SSL
	ssl_certificate /path/crt.crt;
	ssl_certificate_key /path/key.key;

	location / {
      		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      		proxy_set_header Host $http_host;
     		proxy_set_header X-Real-IP $remote_addr;
      		proxy_redirect off;
      		proxy_pass http://127.0.0.1:8080;
      		# 上传的最大文件尺寸
      		client_max_body_size 20000m;
    }
}
  ```

## 小提示
1. 如果你路由空间不算大，建议把包安装在外置磁盘空间
```shell
echo "dest usb /mnt/opkg" >> /etc/opkg.conf
# 重启shell会重置，把下面这条放在`/etc/profile` 就可以保存了
# 注意`$PATH`是原来的变量值
export PATH=$PATH:/mnt/opkg/usr/bin:/mnt/opkg/usr/sbin

# 安装package时指定安装目录
opkg install git -d usb
```
2. 直接扩容根分区`/`，根目录准备，确保使用以下命令来复制根文件系统，其中`/dev/sda1` 是挂载分区，注意格式化为ext4，执行完上面操作好，去挂载点保存好分区挂载配置后重启
```shell
mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/sda1 /tmp/extroot
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot
```
