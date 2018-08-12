= Install =
Put the Nitrogen8M into fastboot mode by holding the fastboot button
and pressing and releasing the reset button

fastboot flash gpt gpt.img
fastboot flash rootfs rootfs.img

= Boot =
In u-boot, run

ext4load mmc 0 ${loadaddr} /boot/Image
ext4load mmc 0 ${fdt_addr} /boot/imx8mq-nitrogen8m.dtb
setenv bootargs console=${console},115200 root=/dev/mmcblk0p1 rootwait rw video=HDMI-A-1:1920x1080-32@60 drm kms helper.edid firmware=HDMI-A-1:edid/1920x1080.bin
booti ${loadaddr} - ${fdt_addr}


= Default u-boot environment, just in case something needs to be restored =
arch=arm
baudrate=115200
board=nitrogen8m
board_name=nitrogen8m
boot_a_script=load ${devtype} ${devnum}:${distro_bootpart} ${scriptaddr} ${prefix}${script}; source ${scriptaddr}
boot_efi_binary=load ${devtype} ${devnum}:${distro_bootpart} ${kernel_addr_r} efi/boot/bootaa64.efi; if fdt addr ${fdt_addr_r}; then bootefi ${kernel_addr_r} ${fdt_addr_r};else bootefi ${kernel_addri
boot_extlinux=sysboot ${devtype} ${devnum}:${distro_bootpart} any ${scriptaddr} ${prefix}extlinux/extlinux.conf
boot_net_usb_start=usb start
boot_prefixes=/ /boot/
boot_script_dhcp=boot.scr.uimg
boot_scripts=boot.scr.uimg boot.scr
boot_targets=mmc0 usb0 
bootcmd=run distro_bootcmd
bootcmd_mmc0=setenv devnum 0; run mmc_boot
bootcmd_usb0=setenv devnum 0; run usb_boot
bootdelay=2
bootfstype=ext4
console=ttymxc0
cpu=8MQ
devnum=0
devplist=1
devtype=mmc
distro_bootcmd=for target in ${boot_targets}; do run bootcmd_${target}; done
efi_dtb_prefixes=/ /dtb/ /dtb/current/
env_dev=0
env_part=1
ethaddr=00:19:b8:05:f1:b7
ethprime=FEC
fdt_addr=0x43000000
fdt_high=0xffffffffffffffff
fdtcontroladdr=be8fff80
imx_cpu=8MQ
initrd_high=0xffffffffffffffff
load_efi_dtb=load ${devtype} ${devnum}:${distro_bootpart} ${fdt_addr_r} ${prefix}${efi_fdtfile}
loadaddr=0x40480000
mmc_boot=if mmc dev ${devnum}; then setenv devtype mmc; run scan_dev_for_boot_part; fi
mmcdev=0
mmcroot=/dev/mmcblk0p2 rootwait rw
netargs=setenv bootargs console=${console},115200 root=/dev/nfs rw ip=dhcp nfsroot=${tftpserverip}:${nfsroot},v3,tcp
netboot=run netargs; if test -z "${fdt_file}" -a -n "${soc}"; then setenv fdt_file ${soc}-${board}${boardver}.dtb; fi; if test ${ip_dyn} = yes; then setenv get_cmd dhcp; else setenv get_cmd tftp; fi;
scan_dev_for_boot=echo Scanning ${devtype} ${devnum}:${distro_bootpart}...; for prefix in ${boot_prefixes}; do run scan_dev_for_extlinux; run scan_dev_for_scripts; done;run scan_dev_for_efi;
scan_dev_for_boot_part=part list ${devtype} ${devnum} -bootable devplist; env exists devplist || setenv devplist 1; for distro_bootpart in ${devplist}; do if fstype ${devtype} ${devnum}:${distro_booe
scan_dev_for_efi=setenv efi_fdtfile ${fdtfile}; for prefix in ${efi_dtb_prefixes}; do if test -e ${devtype} ${devnum}:${distro_bootpart} ${prefix}${efi_fdtfile}; then run load_efi_dtb; fi;done;if tee
scan_dev_for_extlinux=if test -e ${devtype} ${devnum}:${distro_bootpart} ${prefix}extlinux/extlinux.conf; then echo Found ${prefix}extlinux/extlinux.conf; run boot_extlinux; echo SCRIPT FAILED: conti
scan_dev_for_scripts=for script in ${boot_scripts}; do if test -e ${devtype} ${devnum}:${distro_bootpart} ${prefix}${script}; then echo Found U-Boot script ${prefix}${script}; run boot_a_script; eche
scriptaddr=0x40480000
serial#=0019b805f1b7
soc=imx8mq
uboot_defconfig=nitrogen8m
upgradeu=setenv boot_scripts upgrade.scr; boot;echo Upgrade failed!; setenv boot_scripts boot.scr
usb_boot=usb start; if usb dev ${devnum}; then setenv devtype usb; run scan_dev_for_boot_part; fi
vendor=boundary

