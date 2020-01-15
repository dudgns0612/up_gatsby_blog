---
title: 'CentOS7 boot kernel 변경하기'
date: 2019-11-14
category: 'linux'
draft: false
---

![](./images/banner/centOS.png)

## CentOS7에서 boot kernel 설정

CentOS7에서는 GRUB2 통하여 아래와 같은 과정을 통해 Boot kernel을 변경

1. 현재 선택 boot kernel 확인
```sh
# grub2-editenv list
saved_entry=CentOS Linux (3.10.0-1062.4.1.el7.x86_64) 7 (Core)
```

2. boot kernel 목록 확인
```sh
# cat /etc/grub2.cfg | grep "^menuentry" | cut -d "-" -f1-2
menuentry 'CentOS Linux (3.10.0-1062.4.1.el7.x86_64) 7 (Core)'
menuentry 'CentOS Linux (3.10.0-1062.el7.x86_64) 7 (Core)'
menuentry 'CentOS Linux (0-rescue)
```

3.  원하는 boot kernel 선택
```sh
# grub2-set-default "CentOS Linux (3.10.0-1062.el7.x86_64) 7 (Core)"
```

4. 변경된 boot kernel 확인
```sh
# grub2-editenv list
saved_entry=CentOS Linux (3.10.0-1062.el7.x86_64) 7 (Core)
```

5. 변경된 boot config 적용
```sh
# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-1062.4.1.el7.x86_64
Found linux image: /boot/vmlinuz-3.10.0-1062.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-1062.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-726b01fcb3d64f63aad7d0aea490aba9
Found initrd image: /boot/initramfs-0-rescue-726b01fcb3d64f63aad7d0aea490aba9.img
done
```

6. system 재부팅
```sh
shutdown -r now
```