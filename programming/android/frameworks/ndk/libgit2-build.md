---
description: Пример сборки библиотеки (libgit2.so) под андроид
---

# libgit2 build

```text
Этапы компиляции libgit2.so для Андроид

HOME=/Users/o.petrakov
ANDROID_SDK=$HOME/Library/Android/sdk
ANDROID_NDK=$ANDROID_SDK/ndk-bundle
1. Создание Android Toolchain под конкретную архитектуру и Android API (Пример: arm64/android-api-28)
$ $ANDROID_NDK/build/tools/make-standalone-toolchain.sh --arch=arm64 --install-dir=$HOME/Work/test_bgit/android-ndk-28 --platform=android-28 --force
ANDROID_NDK_HOME=$HOME/Work/test_bgit/android-ndk-28
2. Собираем  ZLib
$ wget https://www.zlib.net/zlib-1.2.11.tar.gz
$ tar -xvf zlib-1.2.11.tar.gz
$ mv zlib-1.2.11.tar zlib && cd zlib
export CC="aarch64-linux-android-gcc"
export CXX="aarch64-linux-android-g++"
export RANLIB="aarch64-linux-android-ranlib"
export LD="aarch64-linux-android-ld"
export AR="aarch64-linux-android-ar"
export ARFLAGS="cr"
export CHOST="aarch64-linux-android"
./configure
make

3. Собираем OpenSSL
$ git clone https://github.com/openssl/openssl.git
./Configure
            --prefix=$HOME/Work/test_bgit/openssl_build
            --openssldir=$HOME/Work/test_bgit/openssl_build/ssl
            --with-zlib-include=
            --with-zlib-lib=

Пример: 
export PATH:"$ANDROID_NDK_HOME/bin:${PATH}"
unset <all var from zlib: CC, CXX, .., CHOST>
./Configure android-arm64 --prefix=$HOME/Work/test_bgit/openssl_build --openssldir=$HOME/Work/test_bgit/openssl_build/ssl --with-zlib-include=/path/to/zlib --with-zlib-lib=/path/to/zlib/libz.so
make

4. Собираем LibGit2
git clone https://github.com/libgit2/libgit2.git
mkdir libgit2_build && cd libgit2_build
Создаем файл для CMake: touch toolchain.cmake
В него пишем:
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_SYSTEM_VERSION Android)

SET(CMAKE_C_COMPILER    $ENV{ANDROID_NDK_HOME}/bin/aarch64-linux-android-gcc)
SET(CMAKE_CXX_COMPILER $ENV{ANDROID_NDK_HOME}/bin/aarch64-linux-android-g++)
SET(CMAKE_FIND_ROOT_PATH $ENV{ANDROID_NDK_HOME}/sysroot/)

SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

Если не установлен pkg-config -> ставим (установится в  /usr/local/bin): $ brew install pkg-config

Далее конфигурируем с помощью cmake проект:
cmake .. -DCMAKE_TOOLCHAIN_FILE=toolchain.cmake -DPKG_CONFIG_EXECUTABLE=/usr/local/bin/pkg-config -DUSE_HTTPS=OpenSSL -DOPENSSL_ROOT_DIR=/Users/o.petrakov/Work/test_bgit/openssl -DOPENSSL_LIBRARIES=/Users/o.petrakov/Work/test_bgit/openssl -DOPENSSL_INCLUDE_DIR=/Users/o.petrakov/Work/test_bgit/openssl/include -DOPENSSL_CRYPTO_LIBRARY=/Users/o.petrakov/Work/test_bgit/openssl/libcrypto.so -DOPENSSL_SSL_LIBRARY=/Users/o.petrakov/Work/test_bgit/openssl/libssl.so
Где:
.. - путь до сорцов (куда забирали с гита исходники)
CMAKE_TOOLCHAIN_FILE - путь до файла для cmake (см выше, что туда писать)
PKG_CONFIG_EXECUTABLE - путь до pkg-config - бинарь для поиска зависимостей в системе
USE_HTTPS=OpenSSL - что использовать для шифрования трафика
OPENSSL_ROOT_DIR - путь до каталога с собранным OpenSSL
OPENSSL_LIBRARIES - путь до каталога, где лежат libcrypto.so, libssl.so
OPENSSL_INCLUDE_DIR - путь до каталога с header-файлами собранного OpenSSL
OPENSSL_CRYPTO_LIBRARY - полный путь до libcrypto.so
OPENSSL_SSL_LIBRARY - полный путь до libssl.so

Собираем: $ make
Теперь в папке build лежит libgit2.so для нашего android приложения

5. Бинарь гита для Android:
ставим Termux, устанавливаем git
вытаскиваем бинарь из /data/data/com.termux/files/usr/bin/git -> <container>/files/usr/libexec/git-core/git - это наш бинарь
переносим библиотеки из <container>/files/usr/lib: libpcre2-*.so и libiconv.so -> /system/lib64/ (предварительно потребуется перевести фс в режим записи: mount -o rw,remount /system)
```

