---
description: 'Здесь будут заметки по местонахождению в фс файлов сервисов, приложений и тп'
---

# Файловая система

Keychain DB: `/private/var/Keychains/` _, а копия здесь: `/Library/Keychains/`_  
CallHistoryDB: `/private/var/mobile/Library/CallHistoryDB/CallHistory.storedata`

Заметки хранятся здесь  
`/private/var/mobile/Containers/Shared/AppGroup//NoteStore.sqlite`  
_и здесь: `/User/Library/Notes/`_

iOS 9.  
_Системные приложения: `/Applications`  
Установленные: `/var/containers/Bundle/Application`  
iOS 11._  
Системные либы/фреймворки: `/System/Library/PrivateFrameworks/`  
_-//- приложений: `/System/Library/Frameworks`  
Часть фреймворков хранится в `/System/Library/AccessibilityBundles/`_

На iOS по пути `<container>/Documents/<some_name>` - может лежать база. Она заполняется с помощью класса \(или интерфейса\) [NSPersistentStore](https://developer.apple.com/documentation/coredata/nspersistentstore?language=objc)   
__Создается следующей функцией: [https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468860-addpersistentstorewithtype?language=objc](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468860-addpersistentstorewithtype?language=objc)  
Пример, как с этим всем работают: [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/InitializingtheCoreDataStack.html](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/InitializingtheCoreDataStack.html)

