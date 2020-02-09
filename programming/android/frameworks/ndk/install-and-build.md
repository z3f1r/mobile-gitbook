# Install and Build

Устнавливаем Android SDK, Android NDK \(можно через Android Studio\)  
допустим установилось в ${HOME}/Library/Android/sdk/  
Android NDK установится в ndk-bundle  
Platform Tools лежит в platform-tools

Что в ndk-bundle:  
toolchains - уже собранные для некоторых платформ наборы библиотек для разработки \(как я понял\)  
Собрать свой toolchain:  
$ANDROID\_NDK/build/tools/[make-standalone-toolchain.sh](http://make-standalone-toolchain.sh) --arch=arm64 --install-dir=/path/to/my\_toolchain --platform=android-28

