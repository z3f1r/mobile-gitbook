# Память процессов

Process Memory   
ddms.bat - дампим процесс \(после логаута, например\)   
hprof-conv.exe sorce dest - конвертируем в читаемый формат   
Открываем полученный файл в Eclipse memory analyzer   
Click on Open Dominator for entire heap   
Click on "Group result by Group by package"   
Navigate to the class that holds sensitive data, and see if the data is still available in clear text and if it can be retrieved.

Если видим наши переменные \(пароли и тп\) - херово

