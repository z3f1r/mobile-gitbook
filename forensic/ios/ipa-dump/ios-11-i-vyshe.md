# iOS 11 и выше

bfinject - дамп ios приложения \(работает только с Electra\)

Для расшифрования бинарника iOS приложения необходим team-identifier \(Он одинаков на всех платформах для конкретного приложения\). Он лежит в бинарнике \(бинарь зашифрован, но подпись \(частью чего явл team-identifier\) - нет\). У гугл-приложений это значение одно на все и хранится оно в начале app-identifier.

Если bfinject-скрипт не смог сам извлечь team-identifier, то найдя его в бинаре мы сами должны проставить его в скрипт в переменную TEAMID!

Пример подписи

```text
 $> /electra/usr/local/bin/jtool.liberios --ent /var/containers/Bundle/Application/D6A07246-234D-40E2-94FC-0BBCEAB16D1C/Snapchat.app/Snapchat
```

```text
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
        <dict>
                <key>com.apple.developer.networking.wifi-info</key>
                <true/>

                <key>keychain-access-groups</key>
                <array>
                        <string>3MY7A92V5W.com.toyopagroup.picaboo</string>
                </array>

                <key>com.apple.developer.team-identifier</key>
                <string>424M5254LK</string>

                <key>application-identifier</key>
                <string>3MY7A92V5W.com.toyopagroup.picaboo</string>

                <key>aps-environment</key>
                <string>production</string>
                  <key>com.apple.developer.networking.HotspotConfiguration</key>
                <true/>

                <key>com.apple.developer.associated-domains</key>
                <array>
                        <string>applinks:www.snapchat.com</string>
                </array>

                <key>com.apple.security.application-groups</key>
                <array>
                        <string>group.snapchat.picaboo</string>
                </array>

        </dict>
</plist>
```

{% file src="../../../.gitbook/assets/bfinject\_fixed.tar" %}

Проекты Clutch и Rasticrac заброшены. Начиная с 11 ios можно использовать [https://github.com/BishopFox/bfinject/blob/master/README.md\#decrypt-app-store-apps](https://github.com/BishopFox/bfinject/blob/master/README.md#decrypt-app-store-apps) для дампа бинарей. \(Что б заработал, необходимо фиксануть скрипт: bfinject\_fixed\) На телефоне бросаю в /var/tmp/\*

