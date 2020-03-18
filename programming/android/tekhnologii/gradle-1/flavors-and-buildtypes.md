# Flavors & BuildTypes

## Определения

`flavor dimension` - под этим можно понимать категорию переменных, некоторый переменный набор. Пример: 

```text
flavorDimensions "site", "endpoint", "market"
```

`productFlavors` - здесь определяются возможные значения для `dimension`. Пример:

```text
productFlavors {
        prod {
            dimension 'endpoint'
            applicationId 'blabla1'
        }

        staging {
            dimension 'endpoint'
            applicationId 'blabla2'
            здесь могут быть объявлены любые переменные (те же, что в defaultConfig)
        }

        google {
            dimension 'market'
        }

        amazon {
            dimension 'market'
        }

    }
```

Здесь: в категории endpoint могут содержатся следующие наборы параметров: prod, staging



