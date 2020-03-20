# Foreground Services \(или просто Services\)

## Intro

man \(RU\): [https://developer.android.com/guide/components/services](https://developer.android.com/guide/components/services)

Для инициируемых задач. Такие сервисы не убиваются системой \(потому что имеют высокий приоритет\). Их работа будет отображаться в области уведомлений.

Служба выполняется в **основном потоке**. Поэтому надо создать новый поток в службе, если она выполняет интенсивные или блокирующие операции.

## Типы служб

Фактически служба может принимать две формы:

### Запущенная

Служба является «запущенной», когда компонент приложения \(например, операция\) запускает ее вызовом [`startService()`](https://developer.android.com/reference/android/content/Context#startService%28android.content.Intent%29). После запуска служба может работать в фоновом режиме в течение неограниченного времени, даже если уничтожен компонент, который ее запустил. Обычно запущенная служба выполняет одну операцию и не возвращает результатов вызывающему компоненту. Например, она может загружать или выгружать файл по сети. Когда операция выполнена, служба должна остановиться самостоятельно.

Для обработки запросов - переопределить onStartCommand\(\)

### Привязанная

Служба является «привязанной», когда компонент приложения привязывается к ней вызовом [`bindService()`](https://developer.android.com/reference/android/content/Context#bindService%28android.content.Intent,%20android.content.ServiceConnection,%20int%29). Привязанная служба предлагает интерфейс клиент-сервер, который позволяет компонентам взаимодействовать со службой, отправлять запросы, получать результаты и даже делать это между разными процессами посредством межпроцессного взаимодействия \(IPC\). Привязанная служба работает только пока к ней привязан другой компонент приложения. К службе могут быть привязаны несколько компонентов одновременно, но когда все они отменяют привязку, служба уничтожается.

Для обработки запросов - переопределить onBind\(\)

## Важные методы

### onStartCommand

Система вызывает этот метод, когда другой компонент, например, операция, запрашивает запуск этой службы, вызывая [`startService()`](https://developer.android.com/reference/android/content/Context#startService%28android.content.Intent%29). После выполнения этого метода служба запускается и может в течение неограниченного времени работать в фоновом режиме. Если вы реализуете такой метод, вы обязаны остановить службу посредством вызова [`stopSelf()`](https://developer.android.com/reference/android/app/Service#stopSelf%28%29) или [`stopService()`](https://developer.android.com/reference/android/content/Context#stopService%28android.content.Intent%29). \(Если требуется только обеспечить привязку, реализовывать этот метод не обязательно\).

### onBind

Система вызывает этот метод, когда другой компонент хочет выполнить привязку к службе \(например, для выполнения удаленного вызова процедуры\) путем вызова [`bindService()`](https://developer.android.com/reference/android/content/Context#bindService%28android.content.Intent,%20android.content.ServiceConnection,%20int%29). В вашей реализации этого метода вы должны обеспечить интерфейс, который клиенты используют для взаимодействия со службой, возвращая [`IBinder`](https://developer.android.com/reference/android/os/IBinder). Всегда необходимо реализовывать этот метод, но если вы не хотите разрешать привязку, необходимо возвращать значение null.

### onCreate

Система вызывает этот метод при первом создании службы для выполнения однократных процедур настройки \(перед вызовом [`onStartCommand()`](https://developer.android.com/reference/android/app/Service#onStartCommand%28android.content.Intent,%20int,%20int%29) или [`onBind()`](https://developer.android.com/reference/android/app/Service#onBind%28android.content.Intent%29)\). Если служба уже запущена, этот метод не вызывается.

### onDestroy

Система вызывает этот метод, когда служба более не используется и выполняется ее уничтожение. Ваша служба должна реализовать это для очистки ресурсов, таких как потоки, зарегистрированные приемники, ресиверы и т. д. Это последний вызов, который получает служба.

## Нюансы, связанные с уничтожением

Если компонент запускает службу посредством вызова [`startService()`](https://developer.android.com/reference/android/content/Context#startService%28android.content.Intent%29) \(что приводит к вызову [`onStartCommand()`](https://developer.android.com/reference/android/app/Service#onStartCommand%28android.content.Intent,%20int,%20int%29)\), то служба продолжает работу, пока она не остановится самостоятельно с помощью [`stopSelf()`](https://developer.android.com/reference/android/app/Service#stopSelf%28%29) или другой компонент не остановит ее посредством вызова [`stopService()`](https://developer.android.com/reference/android/content/Context#stopService%28android.content.Intent%29).

Если компонент вызывает [`bindService()`](https://developer.android.com/reference/android/content/Context#bindService%28android.content.Intent,%20android.content.ServiceConnection,%20int%29) для создания службы \(и [`onStartCommand()`](https://developer.android.com/reference/android/app/Service#onStartCommand%28android.content.Intent,%20int,%20int%29) _не_ вызывается\), то служба работает, пока к ней привязан компонент. Как только выполняется отмена привязки службы ко всем клиентам, система уничтожает службу.

Система Android будет принудительно останавливать службу только в том случае, когда не хватает памяти, и необходимо восстановить системные для операции, которая отображается на переднем плане. Если служба привязана к операции, которая отображается на переднем плане, менее вероятно, что она будет уничтожена, и если служба объявлена для [выполнения на переднем плане](https://developer.android.com/guide/components/services#Foreground) \(как обсуждалось выше\), она почти никогда не будет уничтожаться. В противном случае, если служба была запущена и является длительной, система со временем будет опускать ее положение в списке фоновых задач, и служба станет очень чувствительной к уничтожению — если ваша служба запущена, вы должны предусмотреть изящную обработку ее перезапуска системой. Если система уничтожает вашу службу, она перезапускает ее, как только снова появляется доступ к ресурсам \(хотя это также зависит от значения, возвращаемого методом [`onStartCommand()`](https://developer.android.com/reference/android/app/Service#onStartCommand%28android.content.Intent,%20int,%20int%29), как обсуждается ниже\). Дополнительная информация о ситуациях, в которых система может уничтожить службу приведена в документе [Процессы и потоки](https://developer.android.com/guide/components/processes-and-threads) .

## IntentService

Это подкласс класса [`Service`](https://developer.android.com/reference/android/app/Service), который использует рабочий поток для обработки всех запросов запуска поочередно. Это оптимальный вариант, если вам не требуется, чтобы ваша служба обрабатывала несколько запросов одновременно. Достаточно реализовать метод [`onHandleIntent()`](https://developer.android.com/reference/android/app/IntentService#onHandleIntent%28android.content.Intent%29), который получает намерение для каждого запроса запуска, позволяя выполнять фоновую работу.

Очень важно: IntentService - это один сервис! Если поступают два запроса ему, то он их обрабатывает последовательно!

### onHandleIntent

Нам надо переопределить только этот метод для обработки входяих интентов. Все остальное за нас уже сделано. 

