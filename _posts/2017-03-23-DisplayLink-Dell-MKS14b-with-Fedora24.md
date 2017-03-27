---
published: ture
layout: post
title: "DisplayLink - Dell MKS14b dock with Fedora 24"
author: MochaPenguin
categories: [Howto, Articles]
tags:
- DisplayLink
- Fedora24
---

Very pleased to report success on connecting my TP300LA running Fedora 24 to a USB 3.0 DisplayLink dock. This would be a very handy way of connecting monitors. At present I can only use the laptop with a monitor connected to its HDMI port.

OS - Fedora 24 running kernel 4.9.13-101.fc24.x86_64

Dock used: [Dell MKS14b](http://accessories.euro.dell.com/sna/productdetail.aspx?c=uk&l=en&s=bsd&cs=ukbsdt1&sku=452-bbkd)

Installed the [displaylink-rpm](https://github.com/displaylink-rpm/displaylink-rpm) driver following the instructions posted for Fedora 25 by [reypm](https://github.com/reypm) at this [post](https://github.com/DisplayLink/evdi/issues/64#issuecomment-284399558). He experienced some issues while attempting to install the DisplayLink driver on Fedora 25. The instructions for Fedora 25 worked without a glitch on Fedora 24.

DisplayLink driver rpm link: [fedora-24-displaylink-rpm-1.3.43-2.x86_64.rpm](https://github.com/displaylink-rpm/displaylink-rpm/releases/download/v1.3.52/fedora-24-displaylink-1.3.43-2.x86_64.rpm)

Checking the status of displaylink.service after installing the driver
~~~
[root@tp300la displaylink-driver-1.2.65]# systemctl status displaylink.service
● displaylink.service - DisplayLink Manager Service
   Loaded: loaded (/usr/lib/systemd/system/displaylink.service; static; vendor preset: disabled)
   Active: inactive (dead)
~~~

Connected the USB 3.0 cable from the dock after installing the DisplayLink driver and checking the displaylink.service was active

Plugging in the USB cable from the dock

~~~
[root@tp300la displaylink-driver-1.2.65]# systemctl status displaylink.service
● displaylink.service - DisplayLink Manager Service
   Loaded: loaded (/usr/lib/systemd/system/displaylink.service; static; vendor preset: disabled)
   Active: active (running) since Thu 2017-03-23 13:41:57 GMT; 5min ago
  Process: 24805 ExecStartPre=/sbin/modprobe evdi (code=exited, status=0/SUCCESS)
 Main PID: 24822 (DisplayLinkMana)
    Tasks: 26 (limit: 512)
   CGroup: /system.slice/displaylink.service
           └─24822 /usr/libexec/displaylink/DisplayLinkManager
~~~

~~~
[ 5043.605949] usb 1-1.3: New USB device found, idVendor=0451, idProduct=8044
[ 5043.605951] usb 1-1.3: New USB device strings: Mfr=0, Product=0, SerialNumber=1
[ 5043.605952] usb 1-1.3: SerialNumber: NNNNNNNNNNNN
[ 5043.606309] usb 1-1.3: Not enough bandwidth for new device state.
[ 5043.606315] usb 1-1.3: can't set config #1, error -28
[ 5043.669754] usb 2-1.3: new SuperSpeed USB device number 19 using xhci_hcd
[ 5043.681922] usb 2-1.3: New USB device found, idVendor=0451, idProduct=8046
[ 5043.681924] usb 2-1.3: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[ 5043.682440] usb 2-1.3: Not enough bandwidth for new device state.
[ 5043.682447] usb 2-1.3: can't set config #1, error -28
[ 5043.745725] usb 1-1.4: new low-speed USB device number 32 using xhci_hcd
[ 5043.826237] usb 1-1.4: New USB device found, idVendor=046d, idProduct=c016
[ 5043.826242] usb 1-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[ 5043.826245] usb 1-1.4: Product: Optical USB Mouse
[ 5043.826247] usb 1-1.4: Manufacturer: Logitech
[ 5043.826793] usb 1-1.4: Not enough bandwidth for new device state.
[ 5043.826806] usb 1-1.4: can't set config #1, error -28
[ 5044.050445] evdi: [W] evdi_painter_disconnect:462 (dev=-1) An unknown connection to ffff96485851e000 tries to close us
[ 5044.050447] evdi: [W] evdi_painter_disconnect:463  - ignoring
[ 5044.063811] evdi: [D] evdi_painter_connect:443 (dev=1) Connected with ffff96485851e000
[ 5044.063813] evdi: [D] evdi_detect:75 (dev=1) Painter is connected
[ 5044.063852] evdi: [D] evdi_painter_crtc_state_notify:360 (dev=1) Notifying crtc state: 3
[ 5044.064608] evdi: [D] evdi_detect:75 (dev=1) Painter is connected
[ 5044.064612] evdi: [D] evdi_painter_get_edid_copy:192 (dev=1) 00 ff ff
[ 5044.099866] evdi: [D] evdi_painter_mode_changed_notify:376 (dev=1) Notifying mode changed: 1920x1200@60; bpp 32;
[ 5044.099868] evdi: [D] evdi_painter_mode_changed_notify:377 pixel format 875713112
[ 5044.099873] evdi: [D] evdi_painter_crtc_state_notify:360 (dev=1) Notifying crtc state: 0
[ 5044.099876] evdi: [D] evdi_painter_dpms_notify:347 (dev=1) Notifying dpms mode: 0
[ 5044.280517] evdi: [D] evdi_detect:75 (dev=1) Painter is connected
[ 5044.280520] evdi: [D] evdi_painter_get_edid_copy:192 (dev=1) 00 ff ff
~~~


disconnecting the USB cable from the dock

~~~
[ 5102.015916] usb 1-1: USB disconnect, device number 29
[ 5102.015920] usb 1-1.2: USB disconnect, device number 30
[ 5102.063364] usb 1-1.3: USB disconnect, device number 31
[ 5102.063491] usb 1-1.4: USB disconnect, device number 32
[ 5102.122400] xhci_hcd 0000:00:14.0: Cannot set link state.
[ 5102.122405] usb usb2-port1: cannot disable (err = -32)
[ 5102.122410] usb 2-1: USB disconnect, device number 17
[ 5102.122411] usb 2-1.1: USB disconnect, device number 18
[ 5102.123103] cdc_ncm 2-1.1:1.5 enp0s20u1u1i5: unregister 'cdc_ncm' usb-0000:00:14.0-1.1, CDC NCM
[ 5102.143956] usb 2-1.3: USB disconnect, device number 19
[ 5102.145449] evdi: [D] evdi_painter_disconnect:483 (dev=1) Disconnected from ffff96485851e000
[ 5102.145452] evdi: [D] evdi_detect:78 Painter is disconnected
[ 5102.145488] evdi: [W] evdi_painter_disconnect:462 (dev=-1) An unknown connection to ffff96485851e000 tries to close us
[ 5102.145490] evdi: [W] evdi_painter_disconnect:463  - ignoring
[ 5102.147704] evdi: [D] evdi_detect:78 Painter is disconnected
[ 5102.177013] evdi: [D] evdi_painter_crtc_state_notify:360 (dev=-1) Notifying crtc state: 3
[ 5102.177019] evdi: [W] evdi_painter_send_crtc_state:266 Painter is not connected!
[ 5102.194840] audit: type=1130 audit(1490277065.997:5177): pid=1 uid=0 auid=4294967295 ses=4294967295 msg='unit=NetworkManager-dispatcher comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
[ 5102.348666] evdi: [D] evdi_detect:78 Painter is disconnected
~~~

~~~
[root@tp300la displaylink-driver-1.2.65]# systemctl status displaylink.service
● displaylink.service - DisplayLink Manager Service
   Loaded: loaded (/usr/lib/systemd/system/displaylink.service; static; vendor p
   Active: inactive (dead)
~~~

# Screenshots

![Connected to a secondary monitor](https://lh3.googleusercontent.com/uLuib-awiK9hRE5_dFu5Q_vPhlNaGrwxC3jeZrFHRHHAr3RxNLKD9kXNObIlxj6lJjzzfu19a9TTiL_pxlyKkboIXBwj8KE04jD2xkzWi-E71BOQB18UehBwKJHrtxDmaKSCMh9jZyhCd6eSrLmpGWeOjQrO7cstSlx73dkFKJGGLS1eHp2Dr2WSVYCezhiiUtUoMoQZb0ckHQVLhbOvwZwoWm1WjGhyn04On5dPd6RXBmcsyemv_rQPl2oqHavL_jOslZkyKTua-ONm8lWw2Xkoo1IRSnl1eXYu9ycec_eL0tcVEddUMMdT6ZQOm5oKD_Ju9ejp1lj30XyyBkZ8HsQLYgdIHZu-UNYmV44DpLKeeF0p77UlkniA1HvSIA_VPKKFcaoVqr4XEI6w7Ce0uYufIItK9f0IPWQWMUyYqp0gAVV8ztLw57pZw8Gol9phZ9pCPU4bUT45H9kWQf1UkSpXqAm8oik4MkdA0AVC9-m3AsutC8g_gQRVsNAPdIX7nsBBDSjFJuEuNt18tf_HdgG39alnp9DINaIVQRv2F7FFM_1dRWkWQxZnuxWEgmUK_kdCIeB9aRfbBCaMhpi5WT6ULHH9yLmZiL66tkA4wf_gL-fxIkrD5d1YRqUP_5qYBoHbxOVk4i-umpv_K_cDF6G2YXOh7ULGdpP_NXteqw=w740-h347-no){:width="700"}

<div class="separator" style="clear: both; text-align: center;"><a href="https://1.bp.blogspot.com/-h6H8Ou1IfJU/WNR3Fk0rLTI/AAAAAAAAAiY/OMwpMub57Xwi8c726D7qUCDE3IT1JwQlwCPcB/s1600/displaylink-fc24-001.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://1.bp.blogspot.com/-h6H8Ou1IfJU/WNR3Fk0rLTI/AAAAAAAAAiY/OMwpMub57Xwi8c726D7qUCDE3IT1JwQlwCPcB/s400/displaylink-fc24-001.png" width="400" height="188" /></a></div>


<div class="separator" style="clear: both; text-align: center;"><a href="https://2.bp.blogspot.com/-_lfwRqQxtjM/WNR3M56_e_I/AAAAAAAAAik/IrWhPLIQ0I8RW27-_fQkSQT2IcbFPG5bwCPcB/s1600/displaylink-fc24-002.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://2.bp.blogspot.com/-_lfwRqQxtjM/WNR3M56_e_I/AAAAAAAAAik/IrWhPLIQ0I8RW27-_fQkSQT2IcbFPG5bwCPcB/s400/displaylink-fc24-002.png" width="400" height="222" /></a></div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://4.bp.blogspot.com/-wQPxVTh6TRo/WNR33pNS5pI/AAAAAAAAAiw/buXvYrNKwd8SqoVnXHSv6cJqWgRCcIukACPcB/s1600/displaylink-fc24-004.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://4.bp.blogspot.com/-wQPxVTh6TRo/WNR33pNS5pI/AAAAAAAAAiw/buXvYrNKwd8SqoVnXHSv6cJqWgRCcIukACPcB/s400/displaylink-fc24-004.png" width="400" height="315" /></a></div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://4.bp.blogspot.com/-TmjUsWaxxZo/WNR38wKCUzI/AAAAAAAAAi4/nYwYmNPNnRsxj4EP_5rFSO5OhayQPSL3ACPcB/s1600/displaylink-fc24-005.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://4.bp.blogspot.com/-TmjUsWaxxZo/WNR38wKCUzI/AAAAAAAAAi4/nYwYmNPNnRsxj4EP_5rFSO5OhayQPSL3ACPcB/s640/displaylink-fc24-005.png" width="640" height="234" /></a></div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://2.bp.blogspot.com/-ACcibjH4enI/WNR48H2dzUI/AAAAAAAAAjY/mJgsDQc-8m8RKAH1TFX318IkhLiTqL00ACPcB/s1600/displaylink-fc24-006.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://2.bp.blogspot.com/-ACcibjH4enI/WNR48H2dzUI/AAAAAAAAAjY/mJgsDQc-8m8RKAH1TFX318IkhLiTqL00ACPcB/s400/displaylink-fc24-006.png" width="400" height="146" /></a></div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://2.bp.blogspot.com/-BeEdxu0xtoo/WNR48LVlYpI/AAAAAAAAAjY/A8oDeKWFZX4BN5h0B7uE9EADeWXv6lakwCPcB/s1600/displaylink-fc24-007.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://2.bp.blogspot.com/-BeEdxu0xtoo/WNR48LVlYpI/AAAAAAAAAjY/A8oDeKWFZX4BN5h0B7uE9EADeWXv6lakwCPcB/s400/displaylink-fc24-007.png" width="400" height="280" /></a></div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://3.bp.blogspot.com/-8MMpz_09pgQ/WNR48PejdZI/AAAAAAAAAjY/xLB335IF1loNMqXM5GKrTNg4gdDuUXAVwCPcB/s1600/displaylink-fc24-008.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://3.bp.blogspot.com/-8MMpz_09pgQ/WNR48PejdZI/AAAAAAAAAjY/xLB335IF1loNMqXM5GKrTNg4gdDuUXAVwCPcB/s400/displaylink-fc24-008.png" width="400" height="213" /></a>
</div>

<div class="separator" style="clear: both; text-align: center;"><a href="https://1.bp.blogspot.com/-BuHQ5lUCQzQ/WNR48GHHvVI/AAAAAAAAAjY/mVXVvjEO8nEDqaOMOwkJzzLDb0BdYZdGACPcB/s1600/displaylink-fc24-009.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://1.bp.blogspot.com/-BuHQ5lUCQzQ/WNR48GHHvVI/AAAAAAAAAjY/mVXVvjEO8nEDqaOMOwkJzzLDb0BdYZdGACPcB/s400/displaylink-fc24-009.png" width="400" height="92" /></a></div>
