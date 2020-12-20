# Handlers & Выполнение кода вне главного потока

```kotlin
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        my_button.setOnClickListener {
            val handler = Handler() // этот инструмент нужен для того, чтобы
            // выполнять код из одного треда в главном (напрямую так делать нельзя)
            // например, обновить текстовое поле во вьюхе
            
            // Запускаем еще один поток
            Thread(Runnable {
                val test = 123
                /*
                doSomething()
                */
                
                // изменяем что-то в главном треде
                handler.post(Runnable {
                    outputView.text = "my text and $test"
                })
            }).start()
        }
    }
}
```

