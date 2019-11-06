# Swift

### Как найти все функции в списке строк: 

**\_\_objc\_methname**

### Описание функций, которые встречаются при декомпилировании swift-кода

swift\_once\(_predicate, void \(_fn\)\(void _\), void_ context\), predicate - глобальная константа, fn - функция. Суть - вызвать swift-функцию fn ровно один раз. Возможно, результат отправляется в predicate, хз. Пример: swift\_once\(&qword\_1018CB818, initConsts\)  
swift\_unknownRetain\(\*object\) - Возможно, пытается создать объект по указателю, но это не точно.

