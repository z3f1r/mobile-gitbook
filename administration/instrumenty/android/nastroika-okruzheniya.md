---
description: >-
  Как настроить окружение и подтянуть все зависимости для android (в основе
  докер образ https://hub.docker.com/r/thyrlian/android-sdk/ )
---

# Настройка окружения



```bash
export USER=ctfzone

cd /home/${USER}/scripts

# install java (заменил на 11), essential tools, Qt
dpkg --add-architecture i386
apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc1 lib32ncurses5 lib32z1 zlib1g:i386
apt-get install -y --no-install-recommends openjdk-11-jdk
apt-get install -y --no-install-recommends git wget unzip
apt-get install -y --no-install-recommends qt5-default
apt-get install -y --no-install-recommends python3-pip

# export _JAVA_OPTIONS="-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap"  # = ""
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# install android sdk
export ANDROID_HOME=/home/${USER}/android-sdk
mkdir -p ${ANDROID_HOME}/cmdline-tools
wget -q https://dl.google.com/android/repository/commandlinetools-linux-6200805_latest.zip && unzip *tools*linux*.zip -d ${ANDROID_HOME}/cmdline-tools && rm *tools*linux*.zip

export PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator
export LD_LIBRARY_PATH=${ANDROID_HOME}/emulator/lib64:${ANDROID_HOME}/emulator/lib64/qt/lib

# echo
echo "Add to ~/.bash_profile"
echo "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
echo "export ANDROID_HOME=/home/${USER}/android-sdk"
echo "export PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator"
echo "LD_LIBRARY_PATH=${ANDROID_HOME}/emulator/lib64:${ANDROID_HOME}/emulator/lib64/qt/lib"

# accept the license agreements of the SDK components
chmod +x license_accepter.sh && ./license_accepter.sh $ANDROID_HOME

# Доставляем тулзы ддля запуска emulator, adb ...
sdkmanager "build-tools;29.0.3"
sdkmanager "platforms;android-25"
sdkmanager "platform-tools"
sdkmanager "emulator"
sdkmanager "system-images;android-25;google_apis;x86_64"

```

{% file src="../../../.gitbook/assets/license\_accepter.sh" caption="Это license\_accepter.sh - для автоматического подтверждения лицензии в системе \(без этого android-sdk не взлетит\)" %}



