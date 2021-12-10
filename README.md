我在nas上的zfs上安装gentoo时遇到了zfs比硬盘先加载的问题，导致系统开机无法自动识别zfs```!! tank/os/gentoo is not a filesystem```
解决办法：<BS>
1. 添加/etc/portage/patches/sys-kernel/genkernel/sleep-wait-zfs.patch文件，内容如下：
```diff --git a/defaults/initrd.scripts b/defaults/initrd.scripts
index 7eac892..c18b65e 100644
--- a/defaults/initrd.scripts
+++ b/defaults/initrd.scripts
@@ -1643,6 +1643,7 @@ start_volumes() {
       	if [ "${USE_ZFS}" = '1' ]
       	then
               	# Avoid race involving asynchronous module loading
+               sleep 15s
               	if [ ! -e /dev/zfs ]
               	then
                       	bad_msg "Cannot import ZFS pool because /dev/zfs is missing"
```
2. emerge -av1 genkernel
3. genkernel --install --zfs --microcode-initramfs --compress-initramfs-type=zstd initramfs
