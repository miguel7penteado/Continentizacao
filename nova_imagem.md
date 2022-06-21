

```cmd
cmd /E /C "prompt $T$$ & echo.%TIME%$ & dir /AL /S .\ | find "SYMLINK" & for %Z in (.) do rem/ "

takeown /f C:\minha\pasta /r
icacls C:\minha\pasta /grant "%USERDOMAIN%\%USERNAME%":(F) /t

icacls C:\minha\pasta /setowner "NT SERVICE\TrustedInstaller" /t
icacls C:\minha\pasta /remove:g "%USERDOMAIN%\%USERNAME%":(F) /t

mcr.microsoft.com/windows:10.0.17763.2928-amd64
mcr.microsoft.com/windows/nanoserver:win10-21h1-preview

```

```bash
apt-get install qemu-utils

qemu-img convert -O raw disco.vmdk container/image.raw
parted -s container/image.raw

mkdir /mnt/container
mount -o loop,ro,offset=1045876 container/image.raw /mnt/container
cd /mnt/container
ls container/
tar   -C /mnt/container -czf image.tar.gz container/.
docker import image.tar.gz
docker images
docker run -i -t 891dcfcad752 /bin/bash

```
