# apkanalyzer

С помощью этого инструменты можно просмотреть разницу между двумя apk-файлами, получить общую информацию о приложении.

Пример использования:

```text
Фулл док: https://developer.android.com/studio/command-line/apkanalyzer

Общий синтаксис: 
apkanalyzer [global-options] subject verb [options] apk-file [apk-file2]

global-options:
-h, --human-readable - Prints sizes in human-readable format. 

subject: - с чем взаимодействуем
apk       - Analyze APK file attributes such as application ID, version code, and version name
files     - Analyze the files inside the APK file
manifest  - Analyze the contents of the manifest inside the APK file
dex       - Analyze the DEX files inside the APK file
resources - View text, image, and string resources

verb - что хотим узнать о subject (см ниже)

Примеры использования:
apk summary apk-file - узнать appId, версию
apk features [--not-required] apk-file - с чем хочет взаимодействовать приложение
apk compare [options] apk1 apk2 - показать файлы, которые отличаются
        options:    
            --different-only: Print directories and files with differences.
            --files-only: Do not print directory entries.
            --patch-size: Show an estimate of the file-by-file patch instead of a raw difference.

files list apk-file - список файлов внутри apk
files cat --file <path, ex: /AndroidManifest.xml> apk-file - прочитать файл

manifest print apk-file
manifest application-id apk-file
manifest version-name apk-file
manifest version-code apk-file
manifest min-sdk apk-file
manifest target-sdk apk-file
manifest permissions apk-file
manifest debuggable apk-file

dex list apk-file
dex packages [option1 option2 ...] apk-file - вывести классы, объявленные в бинаре
    In the output, P, C, M, and F indicate packages, classes, methods, and fields, respectively. 
    And x, k, r, and d indicate removed, kept, referenced and defined nodes, respectively.
    
    Add the following options to refine the output:

    --defined-only: Include only classes defined in the APK in the output.
    --files: Specify the DEX file names to include. Default: all DEX files.
    --proguard-folder file: Specify the Proguard output folder to search for mappings.
    --proguard-mappings file: Specify the Proguard mappings file.
    --proguard-seeds file: Specify the Proguard seeds file.
    --proguard-usages file: Specify the Proguard usages file.
    --show-removed: Show classes and members that were removed by Proguard.

```

