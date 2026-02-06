---
title: Т-Банк. Android
description: Подготовка к собеседованию в Т-Банк Android разработчик. Тест по android Т-Банк.
---

# Вопросы и ответы

## 1. Как вызвать перерасчет размеров Custom View?

- [ ] invalidate()
- [ ] onMeasure()
- [X] requestLayout()
- [ ] onLayout()

??? explanation "Объяснение"

    * invalidate() - используется, когда нужно только перерисовать ваш элемент (текст, цвет и т.д.)
    * onMeasure() - определяет размер и расположение вашего View на экране
    * requestLayout() - используется, когда нужно изменить размеры вашего View
    * onLayout() - вызывается при каждом изменении размера и позиции View, в том числе при его создании и перерисовке

    [Источник](https://habr.com/ru/articles/727744/){:target="_blank" rel="noopener"}

## 2. Можно ли записывать данные в переменную, помеченную модификатором out?

- [ ] Да
- [X] Нет
- [ ] Да, если переменная не используется для чтения
- [ ] Да, если подтип является базовым

??? explanation "Объяснение"
        
    ```kotlin
    interface FileReader<out T> {
        fun read(): T
        fun write(data: T) // Ошибка
    }
    ```
    !!! warning "Компилятор выкинет ошибку"
        Type parameter 'T' is declared as 'out' but occurs in 'in' position in type 'T (of interface FileReader<out T>)'

    [Источник](https://kotlinlang.org/docs/generics.html#declaration-site-variance){:target="_blank" rel="noopener"}

## 3. Что работает быстрее Serializable или Parcelable?

- [ ] Одинаково
- [ ] Serializable быстрее, так как является системным решением
- [X] Parcelable быстрее, так как разработан специально для Android
- [ ] Serializable быстрее, так как используется повсеместно

??? explanation "Объяснение"

    Parcelable быстрее, так как он не использует рефлексию

    ![Serializable vs Parcelable](https://miro.medium.com/1*d4iAcVhmfIrbGR4c0yqCvw.png)

    [Источник](https://stackoverflow.com/questions/3323074/android-difference-between-parcelable-and-serializable){:target="_blank" rel="noopener"}

## 4. Необходимо ли объявлять Broadcast Receiver в манифесте?

- [ ] Да
- [ ] Нет
- [X] Да, если Broadcast Receiver может быть запущен вне зависимости от статуса основного приложения

??? explanation "Объяснение"

    Broadcast Receiver необходимо объявлять в AndroidManifest.xml, если он должен работать независимо от жизненного цикла приложения — например,
    получать системные события, даже когда приложение закрыто.

    * Динамическая (registerReceiver()): работает только пока приложение активно (Activity/Service запущены). Не требует манифеста.
    ```kotlin
    class MainActivity : AppCompatActivity() {
        private var networkReceiver: BroadcastReceiver? = null
        
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            
            networkReceiver = object : BroadcastReceiver() {
                override fun onReceive(context: Context?, intent: Intent?) {
                    Log.d("Network", "Connection changed")
                }
            }
            
            val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION)
            registerReceiver(networkReceiver, filter)
        }
        
        override fun onDestroy() {
            super.onDestroy()
            unregisterReceiver(networkReceiver)
        }
    }
    ```
    * Статическая (манифест): работает всегда, даже если app не запущено. Обязательна для фоновых событий.
    ```xml
    <receiver 
        android:name=".NetworkReceiver"
        android:exported="true">
        <intent-filter>
            <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
        </intent-filter>
    </receiver>
    ```

    [Источник](https://bimlibik.github.io/posts/broadcasts/){:target="_blank" rel="noopener"}

## 5. Вид сервиса, ЖЦ которого зависит от использующих его компонентов

- [ ] Background Service
- [ ] Foreground Service
- [X] Bound Service
- [ ] Intent Service

??? explanation "Объяснение"

    * Background Service - Работает независимо от UI. Используется для длительных фоновых задачи (синхронизация, загрузка данных)
    * Foreground Service - Background Service с уведомлением в статус-баре. Не убивается системой. Используется для воспроизведения музыки
    * Bound Service - Сервис, привязанный к компонентам через bindService(). ЖЦ зависит от клиентов — уничтожается при onUnbind() всех привязок. Используется для обмена данными между Activity/Service (медиаплеер, кэш)
    * Intent Service([This class was deprecated in API level 30](https://developer.android.com/reference/android/app/IntentService){:target="_blank" rel="noopener"}) - Background Service с встроенной очередью задач. Используется для одноразовых фоновых задач (парсинг, отправка)

    [Источник](https://swiftbook.org/tutorials/servisy-android-nachalo-raboty/){:target="_blank" rel="noopener"}

## 6. Можно ли совершить транзакцию с Fragment после onStop() у Activity?

- [ ] Да, в любом случае
- [ ] Нет, будет ошибка
- [X] Да, если не сохранять состояние
- [ ] Да, выполнение методов ЖЦ будет прервано и начато заново

??? explanation "Объяснение"

    После onStop() Activity может вызвать onSaveInstanceState(), после чего FragmentManager запрещает транзакции. 
    Попытка выполнить commit() вызовет **IllegalStateException: Can not perform this action after onSaveInstanceState**

    [Источник](https://stackoverflow.com/questions/48283619/executing-fragment-transactions-after-onpause-onstop){:target="_blank" rel="noopener"}

    [Подробнее](https://habr.com/ru/articles/247561/){:target="_blank" rel="noopener"}

## 7. Гарантирует ли @Volatile корректные операции чтения-записи при многопоточном использовании?

- [ ] Гарантирует
- [X] Не гарантирует
- [ ] Не гарантирует, если потоков больше 5
- [ ] Нет верного ответа

??? explanation "Объяснение"

    В Java и Kotlin есть ключевое слово volatile (в Kotline в виде аннотации @volatile), которые применяются к полям и гарантируют, что считываемое значение поступает из основной памяти, а не из кэша процессора, поэтому все участники процесса будут ожидать окончания параллельной записи, прежде чем считать значение.

    [Источник](https://proglib.io/p/kotlin-java-tips){:target="_blank" rel="noopener"}

    [Подробнее](https://pvs-studio.ru/ru/blog/posts/java/1128/){:target="_blank" rel="noopener"}

## 8. Можно ли передать в Bundle кастомный класс?

- [ ] Да, без ограничений
- [X] Да, если класс наследует Serializable или Parcelable
- [ ] Нет
- [ ] Нет, если класс не наследует Parcelable

??? explanation "Объяснение"

    ```kotlin
    @Parcelize
    data class User(val name: String) : Parcelable

    val bundle = Bundle().apply {
        putParcelable("user", User("John"))
    }
    intent.putExtras(bundle)
    ```
    Без Serializable или Parcelable будет **IllegalArgumentException**

    [Источник](https://ru.stackoverflow.com/questions/1142466/%D0%9F%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0-%D0%BA%D0%B0%D1%81%D1%82%D0%BE%D0%BC%D0%BD%D1%8B%D1%85-%D1%8D%D0%BB%D0%B5%D0%BC%D0%B5%D0%BD%D1%82%D0%BE%D0%B2-%D0%B2-bundle-android){:target="_blank" rel="noopener"}

## 9. Какой слой в Clean Architecture определяет бизнес-логику приложения?

- [ ] Presentation
- [ ] App
- [ ] Data
- [X] Domain

??? explanation "Объяснение"

    ![Android app architecture](https://habrastorage.org/getpro/habr/upload_files/8b8/53f/422/8b853f42274d6976b7d4c8ac4d4305c9.png){ width="50%" }

    [Источник](https://education.yandex.ru/handbook/flutter/article/clean-architecture){:target="_blank" rel="noopener"}

    [Источник](https://developer.android.com/topic/architecture?hl=ru){:target="_blank" rel="noopener"}


## 10. Может ли Domain-слой в Clean Architecture использовать Android-сущности?

- [ ] Да, но это не является хорошей практикой
- [ ] Может, если приложение не является кросс-платформенным
- [ ] Да
- [X] Нет

??? explanation "Объяснение"

    На самом деле никто не запрещает делать зависимость от android сущностей в domain, но это противоречит Clean Architecture. 
    Поэтому ответ скорее `нет`, чем `Да, но это не является хорошей практикой`.

    [Источник](https://education.yandex.ru/handbook/flutter/article/clean-architecture){:target="_blank" rel="noopener"}

    [Источник](https://developer.android.com/topic/architecture?hl=ru){:target="_blank" rel="noopener"}

## 11. Сколько экземпляров object (из Kotlin) существует в приложении?

- [X] 1
- [ ] 1, но необходимо соблюдать осторожность при сериализации/десериализации такого объекта
- [ ] Столько, сколько будет вызовов
- [ ] 4 - для каждого системного компонента

??? explanation "Объяснение"

    В Kotlin ключевое слово Object используется для создания объекта-одиночки (singleton). 
    Объект-одиночка представляет собой класс, у которого может быть только один экземпляр, и этот экземпляр предоставляет глобальную точку доступа к нему.

    [Источник](https://apptractor.ru/info/techhype/kotlin-object.html){:target="_blank" rel="noopener"}

## 12. Всегда ли объекты, на которые есть ссылки, не будут собраны GC?

- [ ] Да, если ссылок больше 2
- [ ] Да, так как объекты используются
- [X] Нет, GC соберет такие объекты, если на них нет ссылок из GC Root
- [ ] Нет верного ответа

??? explanation "Объяснение"

    * **Объекты User не будут собраны, потому что есть ссылка из GC Root**
    ```kotlin
    class User(val name: String) {
        var friend: User? = null
    }

    class App {
        companion object {
            @JvmStatic 
            var currentUser: User? = null  // GC Root
        }
    }

    // достижимы из GC Root
    val user1 = User("Alice")
    val user2 = User("Bob")
    user1.friend = user2
    user2.friend = user1
    App.currentUser = user1  // Ссылка из GC Root
    ```

    * **Объекты User будут собраны, потому что нет ссылки из GC Root**
    ```kotlin
    val user1 = User("Alice")
    val user2 = User("Bob")
    user1.friend = user2
    user2.friend = user1
    ```

    ![Garbage collector](https://habrastorage.org/getpro/habr/upload_files/169/4f6/6b1/1694f66b1967e917e9fccf45ddbe4581.png)

    [Источник](https://habr.com/ru/amp/publications/749568/){:target="_blank" rel="noopener"}

## 13. Сколько раз вызывается onDraw() у View?

- [X] До 1000/кол-во FPS
- [ ] 2 раза
- [ ] 1 раз в секунду
- [ ] Каждый раз при validate() 

??? explanation "Объяснение"

    Метод onDraw() вызывается при каждой перерисовке View, а также при принудительном вызове invalidate(), но метода validate() не существует

    **Жизненный цикл View**

    ![View lifecycle](https://habrastorage.org/getpro/habr/upload_files/ec3/496/516/ec34965166cad6fa0392b422391c6480.png){ width="50%" }

    [Источник](https://apptractor.ru/info/techhype/kotlin-object.html){:target="_blank" rel="noopener"}

## 14. Какой модификатор есть в Kotlin, но нет в Java?

- [ ] protected
- [X] internal
- [ ] private
- [ ] public

## 15. Сколько типов ViewHolder можно использовать в одном RecyclerView?

- [ ] 1
- [ ] Не более 5
- [ ] 2
- [X] Нет ограничений

??? explanation "Объяснение"

    RecyclerView поддерживает неограниченное количество типов ViewHolder через механизм getItemViewType()

    [Источник](https://stackoverflow.com/questions/26245139/how-to-create-recyclerview-with-multiple-view-types){:target="_blank" rel="noopener"}

## 16. Что инициализируется раньше: Application или ContentProvider?

- [ ] Одновременно
- [ ] Зависит от порядка вызова в коде
- [ ] Application
- [X] ContentProvider

??? explanation "Объяснение"

    Экземпляр ContentProvider создаётся системой, а не нами. Инициализация Content Provider. Если в приложении задекларирован ContentProvider, то он инициализируется до вызова onCreate у Application. Некоторые библиотеки используют этот хак, чтобы проводить необходимые им работы на старте, чтобы не утруждать разработчика — например, Firebase.

    [Источник](https://developer.alexanderklimov.ru/android/theory/contentprovider.php){:target="_blank" rel="noopener"}

    [Источник](https://habr.com/ru/companies/yandex/articles/721984/){:target="_blank" rel="noopener"}

## 17. MeasureSpec.AT_MOST - какой аналог из xml-объявления?

- [X] wrap_content
- [ ] 150dp
- [ ] match_parent
- [ ] 0dp

??? explanation "Объяснение"

    MeasureSpec.AT_MOST - Размер View может быть таким или меньше, если по результатам измерений оно окажется меньше

    [Источник](https://stackoverflow.com/questions/16022841/when-will-measurespec-unspecified-and-measurespec-at-most-be-applied){:target="_blank" rel="noopener"}

## 18. Какой метод ЖЦ Service вызывается только один раз?

- [ ] onStartCommand()
- [ ] onBind()
- [X] onCreate()
- [ ] Все методы вызываются больше одного раза

??? explanation "Объяснение"

    **Жизненный цикл Сервиса**

    ![Service lifecycle](https://media.geeksforgeeks.org/wp-content/uploads/20200912005544/Lifecycleofandroidservices-660x407.png)

    [Источник](https://habr.com/ru/articles/349102/){:target="_blank" rel="noopener"}

## 19. В чем разница между горячими и холодными потоками данных?

- [ ] Они одинаковые
- [X] Холодный поток стартует только при подписке, а горячий активен всегда
- [ ] Холодный поток активен всегда, а горячий стартует только при подписке
- [ ] Горячий поток кэширует значения

??? explanation "Объяснение"

    Горячие потоки генерируют данные независимо от наличия подписчиков, а холодные потоки ленивы и начинают генерировать их только по запросу.

    * **Холодный поток** (Flow)
    ```kotlin
    val coldFlow = flow {
        emit(1)
        delay(1000)
        emit(2)
    }

    coldFlow.collect { println(it) }  // 1, 2
    coldFlow.collect { println(it) }  // 1, 2 
    ```

    * **Горячий поток** (StateFlow, SharedFlow)
    ```kotlin
    val stateFlow = MutableStateFlow(0)
    stateFlow.value = 1  // Изменилось до подписки

    stateFlow.collect { println(it) }  // 1
    stateFlow.value = 2
    stateFlow.collect { println(it) }  // 2
    ```

    [Источник](https://habr.com/ru/articles/922962/){:target="_blank" rel="noopener"}

## 20. В каких случаях необходимо применять модификатор inline?    

- [ ] Если необходимо использовать Generic тип
- [ ] Если лямбда будет использована в циклах
- [ ] Если необходим нелокальный return
- [X] Все варианты

??? explanation "Объяснение"

    inline в Kotlin встраивает код функции в место вызова, избегая создания анонимных классов для лямбд.

    [Источник](https://kotlinlang.org/docs/inline-functions.html){:target="_blank" rel="noopener"}
