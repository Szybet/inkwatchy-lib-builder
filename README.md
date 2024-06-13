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
mkdir app/
docker run -it -v $PWD/app:/app/ eal:v1 /bin/bash
```

prepare a bit
```
mkdir out
git clone https://github.com/espressif/esp32-arduino-lib-builder.git
```
possible fixes for problems:

edit this line in `build.sh`
```
python3 ./tools/gen_platformio_manifest.py -o "$TOOLS_JSON_OUT/" -s "$ibr" -c "$ic"
```
to this:
```
python3 ./tools/gen_platformio_manifest.py -o "$TOOLS_JSON_OUT" -s "$ibr" -c "$ic"
```

Also this commit for `esp32-arduino-lib-builder` repo: `4b47bebf9d49e6fc96a71732360d91f00bf980ae`

Maybe do a clean build
```
cd esp32-arduino-lib-builder/
./build.sh -t esp32 -D default -c /app/out/ -A idf-release/v5.1 -i 2381f35409be734fff693c08a7f4b003096fb4c4 -e
```
**Important: `-c /app/out` with `/` at the end of the path**

If something crashes, check if it still runs
```
docker ps
```
