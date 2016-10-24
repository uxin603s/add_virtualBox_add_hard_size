# add_virtualBox_add_hard_size

去virtualBox的資料夾找 VBoxManage.exe路徑

把固態複製成動態方便調整大小
VBoxManage clonehd {來源VDI} {目的VDI}

size單位為M
VBoxManage modifymedium disk {目的VDI} --resize {size}

ex:"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" modifymedium disk C:\Users\chi\.VirtualBox\funfunquiz.vdi --resize 20480
以上範例為20G 20*1024

查看硬碟空間是否有增加
fdisk -l /dev/sda

df -h

切割VBoxManage新增的空間
fdisk /dev/sda
n {new partition}
p {primary partition}
3 {partition number}

t {change partition id}
3 {partition number}
8e {Linux LVM partition}
w

fdisk -l /dev/sda

reboot

查看VolGroup的名稱
vgdisplay

查看邏輯卷
lvscan

新增物理卷
pvcreate /dev/sda3

將物理卷新增至VolGroup
vgextend {VolGroup的名稱} /dev/sda3

將物理卷擴增至邏輯卷
lvextend /dev/VolGroup/lv_root /dev/sda3

調整邏輯卷
resize2fs /dev/VolGroup/lv_root

參考資料:https://cnzhx.net/blog/resizing-lvm-centos-virtualbox-guest/

