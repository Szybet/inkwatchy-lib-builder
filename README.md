# esp32-arduino-lib-builder-2024-06 
Because the commands in that repo doesn't work, or on the esp-idf docs site...

Install on your host system: 
```
sudo pacman -S docker docker-buildx
sudo systemctl enable --now docker
```

Then

https://stackoverflow.com/questions/65896681/exec-docker-credential-desktop-exe-executable-file-not-found-in-path
```
You should delete the line with credsStore from ~/.docker/config.json or rename credsStore to credStore.
```

If you already created such image:
```
docker images
docker rmi -f hash
```
Or
```
docker rmi -f eal:v1
```

Then
```
docker build --no-cache -t eal:v1 .
```

Enter the image
```
sudo rm -rf app/
mkdir app/
chmod 777 app/
setfacl -Rdm u::rwx,g::rwx,o::rwx app/
docker run -it -v $PWD/app:/app/ eal:v1 /bin/bash
```

prepare a bit
```
mkdir -p out/package/
git clone https://github.com/espressif/esp32-arduino-lib-builder.git
cd esp32-arduino-lib-builder/
git switch release/v4.4
```

Maybe build
```
touch /usr/bin/arch
echo '#!/bin/bash' >> /usr/bin/arch
echo "uname -m" >> /usr/bin/arch
chmod +x /usr/bin/arch
./build.sh -t esp32 -c /app/out/ -A idf-release/v4.4 -I release/v4.4
```
or

Also MAYBE this commit for `esp32-arduino-lib-builder` repo: `4b47bebf9d49e6fc96a71732360d91f00bf980ae`

```
./build.sh -t esp32 -c /app/out/ -A idf-release/v4.4 -I release/v4.4
```
**Important: `-c /app/out/` with `/` at the end of the path?**

If something crashes, check if it still runs
```
docker ps
```
If git fails, maybe
```
git config --global core.compression 0
```
If git complains about safe dir...
```
git config --system --replace-all safe.directory '*'
```