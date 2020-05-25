---
description: Принадлежит Microsoft
---

# Hockey App

## Deprecated с Ноября 2019 года

Ныне AppCenter \(как замена\). Однако, какие-то компании еще используют HockeyApp

## Про проблемы с токенами

Источник: [https://www.allysonomalley.com/2020/01/06/saying-goodbye-to-my-favorite-5-minute-p1/](https://www.allysonomalley.com/2020/01/06/saying-goodbye-to-my-favorite-5-minute-p1/)

У Hockey App есть два API: **Client API** и **Developer API**.

Любой пользователь, добавленный в оорганизацию с правами **Developer** или выше, может создавать **API Tokens**. Несмотря на то, что эти токены можно настроить с ограничением прав доступа \(read-only, upload-only, enable to release, etc\), но автор статьи не сталкивалась с этим на практике.

Логично, конечно, что нельзя давать права на создания токенов со всеми типами привилегий. Однако этому не удилено должное внимание в документации Hockey App. 

Токен помещается в заголовок



