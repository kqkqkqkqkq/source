---
title: Яндекс. Android
description: Подготовка к собеседованию в Яндекс Android разработчик.
---

# Полезные материалы

Чтобы успешно пройти собеседование на Android‑разработчика в Яндекс, нужно хорошо разбираться в нескольких ключевых областях. 
Мы проанализировали успешные и не очень успешные прохождения собеседований и выделили основные маркеры знаний и типичные пробелы. 
Вот что вам нужно знать и уметь:

## Раздел 1. Синтаксис Kotlin, классы, коллекции, ООП — углубления

#### Inline/noinline/crossinline:

* inline — инлайнинг лямбд для снижения аллокаций и вызовов; полезно в часто вызываемых утилитах.
* noinline — запрет инлайна для отдельных параметров‑лямбд.
* crossinline — запрет нелокального return внутри лямбды.

#### Reified type parameters:

* позволяют работать с типом‑параметром во время выполнения (is, when, Class<T>, старт Activity<T>).
* работают только в inline‑функциях; помогают обойти стирание типов JVM.

#### Коллекции под капотом:

* Kotlin‑коллекции на JVM — обертки над java.util.*. Понимайте различия ArrayList vs LinkedList, HashSet/LinkedHashSet/TreeSet, HashMap/LinkedHashMap/TreeMap.
* Сложности операций: поиск в HashMap/HashSet — амортизированно O(1), в TreeMap/TreeSet — O(log n), вставка в ArrayList — амортизированно O(1), в середину — O(n).
* equals/hashCode: корректность ключей в HashMap/HashSet; не меняйте поля, влияющие на hashCode/equals, после помещения в коллекцию.
* Порядок обхода: Hash* — не гарантирует порядок, LinkedHash* — порядок вставки, Tree* — сортировка по компаратору.
* Capacity/growth: издержки перераспределения у ArrayList/HashMap.
* Последовательности (Sequence) vs коллекции: ленивые цепочки против немедленных; профит на больших пайплайнах, но возможны лишние аллокации итераторов — профилируйте.
* Иммутабельные структуры (kotlinx.collections.immutable) — persistent‑коллекции: меньше копий при изменениях состояния UI.


## Раздел 2. Многопоточность, Coroutines — практическая задача и нюансы

#### Практическая задача: параллельная загрузка по нескольким ссылкам

* within coroutineScope { val results = urls.map { async(Dispatchers.IO) { load(it) } }.awaitAll() } и затем отображение на Main.
* Обработка ошибок: supervisorScope для изоляции падений отдельных задач; retry с ограничением; timeout с withTimeout.
* Отмена: propagate cancellation, не блокируйте UI (не используйте runBlocking в UI).
* Structured concurrency: не используйте GlobalScope; передавайте scope из ViewModel.
* Синхронизация: Mutex/Semaphore для rate limiting/"защиты от двойного клика".
* Не блокируйте корутины: используйте suspend API, а не блокирующие вызовы.

## Раздел 3. Android SDK, архитектура, жизненный цикл — Clean Architecture

#### Изучите Clean Architecture (наши задачи часто на ней построены):

* Слои: Presentation (ViewModel/Compose), Domain (use cases, бизнес‑правила), Data (репозитории, источники).
* Зависимости направлены внутрь: Presentation → Domain ← Data (через интерфейсы репозиториев).
* Репозитории инкапсулируют сети/БД/кэш, use case — единица бизнес‑логики; DI (Hilt/Koin) для провайдинга зависимостей.
* Преимущества: тестируемость, изоляция платформенного кода, удобное масштабирование.
* Потоки/диспатчеры: переключение контекстов в use case или репозитории, UI остаётся «чистым».

## Раздел 4–5. UI (View/Compose) — жизненный цикл отрисовки и реакция на состояние

#### View-пайплайн:

* Фазы: measure → layout → draw. requestLayout инициирует measure/layout, invalidate — перерисовку (draw).
* Кастомные View: переопределяйте onMeasure/onLayout/onDraw корректно; избегайте аллокаций в onDraw.
* RecyclerView: диффинг (ListAdapter/DiffUtil), пулы вьюхолдеров, payloads.

#### Compose: что происходит при изменении state

* Снапшоты состояния → планируется recomposition для затронутых композов.
* Recomposition не всегда тянет remeasure/relayout — только если размеры/ограничения изменились.
* remember кэширует значения в composition; rememberSaveable — через SavedStateHandle/Bundle.
* Стабильность и равенство: используйте data/val, immutability, equals — меньше бесполезных рекомпозиций.
* Keys: стабильные ключи в Lazy*; derivedStateOf для вычисляемых значений, зависящих от других state.

# До подготовки 1 неделя

## День 1: Kotlin, ООП, inline/reified

Добавьте: inline/noinline/crossinline, reified type parameters и ограничения JVM type erasure.
Практика: написать inline‑функцию с reified T (например, generic навигация/парсинг JSON), показать отличие с Class<T>

## День 2: Коллекции — «под капотом»

Разберите внутренние структуры: HashMap/TreeMap/LinkedHashMap; сложности операций; equals/hashCode контракты. erasure.
Практика: профилировать цепочки map/filter для List vs Sequence; посмотреть влияние больших коллекций и промежуточных копий.

## День 3: Корутины — параллельная загрузка

Реализуйте: параллельную загрузку по списку URL через async/awaitAll + отображение на UI.
Добавьте: supervisorScope, retry/backoff, withTimeout, отмена по событию UI, защита от двойного клика через Mutex/Semaphore.

## День 4: Android SDK, ЖЦ, Clean Architecture

Помимо базовых компонентов, добавьте: схема слоев CA (Presentation/Domain/Data), интерфейсы репозиториев, один use case.
Практика: маленький модуль — ViewModel → UseCase → Repository → fake datasource; внедрите через Hilt/Koin.

## День 5: UI View/Compose — фазы отрисовки и стейт

View: показать, когда дергаются measure/layout/draw, чем requestLayout отличается от invalidate.
Compose: продемонстрировать цепочку state → recomposition → (re)measure/layout/draw, применить derivedStateOf, стабильные ключи в LazyColumn.

## День 6: Код‑ревью и архитектурные обоснования

Включите в чек‑лист: нарушения equals/hashCode в ключах коллекций, лишние рекомпозиции из‑за мутабельных моделей, GlobalScope, блокирующие вызовы в suspend.
Объясняйте решения через принципы CA и последствия для тестирования и поддержки.

## День 7: Итоговая мини‑работа

Соберите демо: экран списка (Compose), загрузка данных параллельно (корутины), хранение состояния, слой UseCase/Repository, ошибки/ретраи.
Подготовьте устные ответы: почему CA, где переключаете диспатчеры, как избегаете лишних рекомпозиций, почему Sequence/почему нет.

# Полезные материалы к добавленным темам

* Kotlin inline и reified: docs.kotl.in → Inline functions; Reified type parameters.
* Коллекции под капотом: официальные Kotlin docs + Java Collections Framework (Oracle), статьи о HashMap internals.
* Coroutines: Structured concurrency, Cancellation, Exception handling, Channels/Flow (kotlinlang.org + Android Developers).
* View rendering pipeline: Android Rendering and View Lifecycle (developer.android.com).
* Compose state и recomposition: State and Jetpack Compose, Thinking in Compose, Performance best practices.

# Видеоматериалы

* Лекции Школы мобильной разработки, Android — 2024 / [перейти](https://www.youtube.com/watch?v=tZiViucVTyA&list=PLXtiZNKIobF5E1JgDaisqnVJfbZeUFYkm)
* Лекции Школы мобильной разработки — 2023 / [перейти](https://www.youtube.com/watch?v=u7MsQG-Ulfw&list=PLXtiZNKIobF7dtH0dVaEKuFTTk7KYG305)
* Лекции Школы мобильной разработки — 2022 / [перейти](https://www.youtube.com/playlist?list=PLQC2_0cDcSKAcQQjPdi77FUF8LYoLZHoO)
* Видеоэнциклопедия Android-разработки. Ф. Вальтер / [перейти](https://www.youtube.com/c/CodinginFlow)

[Источник](https://yandex.ru/yaintern/mobile)