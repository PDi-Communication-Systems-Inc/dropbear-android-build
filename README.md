# Dropbear for Android

## Android Toolchain  
This is an example for Android 5.0 on arm.  


1. Get Android NDK
    ```shell
    curl -O https://dl.google.com/android/repository/android-ndk-r15c-linux-x86_64.zip
    unzip android-ndk-r15c-linux-x86_64.zip
    ```
2. Build the NDK
    ```shell
    cd android-ndk-r15c/build/tools
    ./make_standalone_toolchain.py --install-dir=../../../android-ndk --arch=arm --api=21 --deprecated-headers
3. Add the new android-ndk/bin to the $PATH
    ```shell
    export <full path>/android-ndk/bin:$PATH
    ```

## Compiling
1. Download source
    ```shell
    git clone --recursive https://github.com/PDi-Communication-Systems-Inc/dropbear-android-build.git
    ```

2. Edit the HOST variables in build.sh to match your environment if required  
3. Compile
    ```shell
    cd dropbear-android-build/dropbear
    ../build.sh <arch>
    ```

    You will find the output in `dropbear-android-build/target/<arch>`.  
    By default it is intended to be run from `/data/dropbear`, this can be changed using command line arguments.
4. Running

    ```shell
    ln -s dropbearmulti dropbear
    ln -s dropbearmulti dropbearkey
    ln -s dropbearmulti dbclient
    ln -s dropbearmulti scp
    # Generate host key
    ./dropbearkey -f dropbear_rsa_host_key -t rsa -s 4096
    mv dropbear_rsa_host_key /system/etc/dropbear/
    # Starting server
    ./dropbear -A -N root -U 0 -G 0 -R <path of authorized_keys>
    ```

### Auth
This supports encrypted passwords using `crypt()`
Just add `-e` to the end of your server arguments and provide a password generated with `mkpasswd -mSHA-512`  
Note: `-e` MUST be specified last. Somehow it's being reset otherwise.

### SCP
For SCP to work you will need to symlink `dropbearmulti` to `/system/xbin/scp`

### See Also
Description of original Android patch is [here.](http://web.archive.org/web/20150915202138/http://blog.xulforum.org/index.php?post/2013/12/19/Compiling-Dropbear-for-a-Nexus-7-tablet)

