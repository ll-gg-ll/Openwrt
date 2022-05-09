<p align="center">
    <a href="https://github.com/Hongseme/OpenWrt/blob/main/README.md">README</a> | <a href="https://github.com/Hongseme/OpenWrt/blob/main/README-R4S.md">R4S</a> | <a href="https://github.com/Hongseme/OpenWrt/blob/main/README-360v6.md">360v6</a>
</p>

# 360v6
安装脚本使用的是最新版源的插件，自带源安装有坑

### 更换源
```conf
src/gz reboot_base http://mirrors.cloud.tencent.com/lede/releases/19.07.8/packages/aarch64_cortex-a53/base
src/gz reboot_luci http://mirrors.cloud.tencent.com/lede/releases/18.06.9/packages/aarch64_cortex-a53/luci
src/gz reboot_packages http://mirrors.cloud.tencent.com/lede/releases/19.07.8/packages/aarch64_cortex-a53/packages
src/gz reboot_routing http://mirrors.cloud.tencent.com/lede/releases/19.07.8/packages/aarch64_cortex-a53/routing
src/gz reboot_telephony http://mirrors.cloud.tencent.com/lede/releases/19.07.8/packages/aarch64_cortex-a53/telephony

```
### git、git-http
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/git.sh)"
```

### luci-app-diskman
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/luci-app/diskman.sh)"
```

### luci-app-frpc、luci-app-frps
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/luci-app/frpc-frps.sh)"
```

### luci-app-filebrowser
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/luci-app/filebrowser.sh)"
```

### luci-app-alist
```shell
sh -c "$(curl -sSL https://cdn.jsdelivr.net/gh/Hongseme/OpenWrt@main/360v6/luci-app/alist.sh)"
```
