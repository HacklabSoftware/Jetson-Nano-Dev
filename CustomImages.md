### Create your own image for jetson nano board


#### Method 1 : Download the prebuilt images
To download prebuilt images, go [here](https://github.com/pythops/jetson-nano-image/releases)

To be able to decompress the images, you must have [lrzip](https://github.com/ckolivas/lrzip) installed.

Once the images are decompressed, follow the Flashing section to flash the images 

The default username is jetson and the password is jetson

#### Method 2 : Build the jetson image your self

For Building the jetson image, all you need to have is the following tools installed on your machine.

- [podman](https://github.com/containers/podman/blob/main/install.md)
  ```
  sudo apt-get -y install podman
  ```
- [just](https://github.com/casey/just)

  You must have the Prebuilt-MPR set up on your system in order to run this command.
  ```
  wget -qO - 'https://proget.makedeb.org/debian-feeds/prebuilt-mpr.pub' | gpg --dearmor | sudo tee /usr/share/keyrings/prebuilt-mpr-archive-keyring.gpg 1> /dev/null
  echo "deb [arch=amd64 signed-by=/usr/share/keyrings/prebuilt-mpr-archive-keyring.gpg] https://proget.makedeb.org prebuilt-mpr $(lsb_release -cs)" | sudo tee /etc/apt/sources.list.d/prebuilt-mpr.list
  sudo apt update
  ```

  Install Just
  ```
  sudo apt install just

  ```
  
- [jq](https://github.com/jqlang/jq)
  ```
  git clone https://github.com/jqlang/jq
  git submodule update --init # if building from git to get oniguruma
  autoreconf -fi              # if building from git
  ./configure --with-oniguruma=builtin
  make -j8
  make check
  ```
  
  
- qemu-user-static

Some ubuntu users report that you may need `makedb` as well which is provided by `libnss-db`.
```
apt install -y libnss-db
```

Start by cloning the repo from github
```
git clone https://github.com/pythops/jetson-nano-image
cd jetson-nano-image
```

Then create a new rootfs using the following command:
```
just build-jetson-rootfs
```
This will create the rootfs in the rootfs directory.

You can modify the Containerfile.rootfs file to add any tool or configuration that you will need in the final image.

Next, use the following command to build the Jetson image:
```
just build-jetson-image <board> <revision>
```

Specify the Jetson board you want, which can be either jetson-nano or jetson-nano-2gb.

For the jetson nano you can specify the revision: 200 or 300. For example, for the jetson nano board 200 revision, use the following command:
```
just build-jetson-image jetson-nano 200
```
For the Jetson Nano 2GB, there is no need to provide the revision, so just use the following command:
```
just build-jetson-image jetson-nano-2gb
```
The Jetson image will be built and saved in the current directory in a file named jetson.img

Once the images are built, follow the Flashing section to flash the images 

