# Объединить несколько JAR

```
jar -xvf jar1.jar tmp
jar -xvf jar2.jar tmp

cd tmp
jar -cvf jar3.jar .
```

