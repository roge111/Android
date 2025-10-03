# Android
Всем привет. Прежде, чем читать данныую статью, я всем советую прочитать про котлин: https://github.com/roge111/Kotlin

# Компоненты приложения
Компоненты приложения - необходимые для приложения блоки. Их бывает четыре тип:
- `Activity`
- `Service`
- `BroadcastReceiver`
- `ContentProvider`

Самый часто используемы тип - `Activity`. 

`Манифест приложения` — файл в формате `xml` с информацией о компонентах приложения и разрешениях, которые необходимы. Там же указывается минимальная поддерживаемая версия Android и информация для публикации в маркетах приложений `Rustore`, `Google Play`, `AppGallery` и т. д. Такой файл должен быть в папке `main`. Все компоненты должны быть указаны в файле, иначе приложение будет падать с ошибкой

### Язык разметки XML
----
XML-файл должен начинаться с одной и той же строки:
```
<?xml version="1.0" encoding="utf-8"?>
```
Здесь мы указываем версию и кодировку. Это XML-пролог.

Язык немного похож на HTML. Тут есть теги. Между открывающим и закрывающим тегами могут располагаться другие теги.


Тег может не иметь значения, вложенных в него тегов или атрибутов.

В одном XML документе может быть только один корневой тег. 

### Activity
---
Это компонент, который отвечает за создание контейнера, куда можем положить наш интерфейс. Простые приложения состоят из одной активности, более сложные могут иметь несколько окон.
Чтобы создать компонент `Activity`:
1) Создать класс-наследник `AppCompatActivity`
2) Добавить объявление `Activity` в манифест.
### Создадим первый компонет
----

Для начала создадим проект:
1) `File` → `New` → `New Project`
2) В появившемся окне выберите `Empty Views Activity`

Теперь дождемся полного создания. В левом верхнем углу будет надпись `Project`. Она должна сменить на `Android`, что будет показывать вам только те пакеты, папки и файлы, необходимые для приложения. Если этого не произойдет, то мы сами нажимаем на `Project`, в выпавшем списке выбираем `Android`. 

Далее в директории `kotlin-java` в пакете, не помеченном ни `androidTest`, ни `test`, создаем класс Котлина. По стандарту входной точкой приложения называют `MainActivity`. Внутри создаем класс и наследуем от `AppCompatActivity`.
```
package com.example.myapplication
import androidx.appcompat.app.AppCompatActivity


class MainActivity : AppCompatActivity() {

}
```

В `AndroidManifest.xml` пишем следующее:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:label="@string/app_name"
        android:theme="@style/Theme.MyApplication">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

В `application` пишем `lable`, где указываем называние, `theme` - стиль. 


### Context
---

`Context` — это абстрактный класс, который предоставляет доступ к ресурсам приложения, системным сервисам и информации о среде выполнения. Ресурсы приложения хранятся в папке `res` и не являются кодом.
Все компоненты — это наследники `Context`.

`Context` обладает следующими методами:
- `getString(id: Int)` — позволяет получить строковый ресурс по его ID;
- `getDrawable(id: Int)` — позволяет получить drawable-иконку по его ID;
- `resources` — поле, которое предоставляет доступ к данным папки `Resources`;
- `assets` — поле, которое предоставляет доступ к данным папки Assets и другие.

Помимо компонентов, в Context можно получить `context-приложения`. Делается это через `applicationContext` любого компонента и из UI-элементов (элементы интерфейса). `Application Context` работает всё время работы приложения, существует в единственном экземпляре, доступен везде (из любого компонента). То есть реализует паттерн "Одиночка"

Разные контексты имеют разный набор возможностей. Например, контекст приложений не может выполнить то, что может контекст активности. Например, отрисовку и взаимодействие с UI-элементами.

От Context наследуется `ContextWrapper` — обертка над Context. Она переопределяет Context, не меняя его самого. От обертки идет наследование `Application`, `Service`, `ContextThemeWrapper`, а от последнего наследуется `Activity`.

`ContextThemeWrapper` — это обертка Context, позволяющая использовать темы. Стили из нее распространяются по всей иерархии UI-элементов, созданных при помощи ContextThemeWrapper. И как раз Activity уже и позволяет нам работать с тем, что пользователь видит на экране.



# Интерфейс

Интерфейс приложения можно задать тремя способами:
1) XML-файл
2) Kotlin-код
3) Используя библиотеку `Jampack Compose`

### XML-файл
---

Для того чтобы сделать интерфейс через xml-файл, для начала создадим его. Для этого в папке `res` находим папку `layout`. Если какой-то из этих папок нет, то создаем их. Кликаем по правой клавише, выбираем `New` -> `Layout Resource File`. Задаем имя, например, `activity_main` и корневым элементом делаем `LinearLayout`. В данном файле будет располагаться иерархия тегов, чье название будет соответствовать названию класса, от которого наследуются. 

### Классы
---

Есть два класса, которые помогают в создании интерфейса:
1) `View` — класс, что отвечает за отображение информации на экране и не может иметь дочерних тегов. Например:
  - `TextView` — текстовая информация
  - `Button` — кнопка
  - `ProgressBar` — отображает состояние закрузки
2) `ViewGroup` — класс, что отвечает за расположение `View` на экране, обычно имеет дочерние теги. Примеры:
  - `FrameLayout` — располагает дочерние View друг поверх друга
  - `LinearLayout` — располагает дочерние View по порядку по горизонтали/вертикали.
### Теги
У тегов в данном XML-файле могут быть свои обязательные атрибуты. К обязательным относятся атрибуты размера:
- `android:layout_width` — атрибут ширины элемента;
- `android:layout_height` — атрибут высоты элемента.

Необязательные атрибуты:
- id элемента
- отсутп за счет модифицируемого UI-элемента
- margin - отступ за счет родительского UI-элемента

### Практика
---

По умолчанию у нас в файле после создания будет такой код:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

</LinearLayout>
```
Здесь мы видим значение `match_parent` в атрибутах высоты и ширины элемента `LinearLayout`. Данное значение говорит, что у элемента максимальные размеры по вертикали/горизонтали. В нашем случае наш элемент будет на весь экран.

Теперь напишем текстовый тег `<TextView>` и зададим параметры высоты и ширины `wrap_content` — размер будет соответствовать содержимому.

Так выглядит тег:

```
<TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```
Текст можно задать при помощи атрибута `android:text`, а размер шрифта можно регулировать с помощью `android:textSize`. Единица измерения рекомендуется sp, что позволяет автоматически учитывать настройки размера шрифта пользователя.


Создадим кнопку:
```
    <Button
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginLeft="16dp"
        android:paddingLeft="16dp"
        android:id="@+id/button"
        android:text="Hello world" />
```


Отобразим это на нашем экране. Вернемся в наш компонент `MainActivity.kt`. 


```
package com.example.myapp


import android.os.Bundle
import android.os.PersistableBundle
import androidx.activity.ComponentActivity
import com.example.myapp.databinding.ActivityMainBinding


class MainActivity : ComponentActivity() {
//    Переопределим метод onCreate, в нем мы преобразуем верстку в иерархиб объектов
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Для получения иерархии вызовим у сгенированного класса метод inflate.
        // Имя класса скалдывается из названия объект (activity_main) по camelCase и Binfing
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }
}
```
Теперь наш интерфейс отображается на экране телефона. Добавим обработку нажатия кнопки:

В нашей иерархии `binding` вызовем соответствующий id кнопки — `button`. И вызовем метод `setOnClickListener`. Сделаем так, что при нажатии в тексте будет увеличиваться число.
```
var count = 0
        binding.button.setOnClickListener{
            count ++
            binding.text.text = "Hello world $count"

        }
```



Полный код:

```
package com.example.myapp

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.example.myapp.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    //    Переопределим метод onCreate, в нем мы преобразуем верстку в иерархиб объектов
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Для получения иерархии вызовим у сгенированного класса метод inflate.
        // Имя класса скалдывается из названия объект (activity_main) по camelCase и Binfing
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)


        var count = 0
        binding.button.setOnClickListener{
            count ++
            binding.text.text = "Hello world $count"

        }
    }
}
```


Теперь можно запускать.


# Жизненный цикл Activity

Предлагаю узнать о жизненном цикле (ЖЦ) у данного компонента. Это последовательность состояний, через которые проходит объект. 

Давайте по порядку. После создания объект оказывается в `начальном состоянии`. Затем переходит в `состояние 1`, потом в `состояние 2` и т. д. Конечно, могут быть и условия перехода из состояний в состояния или возврата в прошлое. В итоге это приходит к конечному.

<img width="640" height="551" alt="image" src="https://github.com/user-attachments/assets/9d59c9b3-81d7-4890-8f66-ddb5023cf498" />

Если обратить внимание, то при переходе из состояния в состояние происходит обратный вызов — колбэк ЖЦ. Колбэки ЖЦ — это обычные функции, для каждого состояния обычно есть свой колбэк. Это то, какие действия будут осуществляться при переходе.

У Activity есть свой ЖЦ

1) `onCreate` — запускается, когда запускаем компонент. Тут мы отображаем интерфейс, ресурсы некоторые, восстанавливается состояние.
2) `onStart` — вызывается, когда Activity становится видимой.
  — В этом методе мы подготавливаем Activity к показу пользователю.
3) `onResume` — вызывается, когда Activity становится доступной для взаимодействия с пользователем (обрабатываются клики и т. д.). Запускаются интерактивные процессы.
4) `onPause` — вызывается, когда Activity становится недоступной для взаимодействия, но все еще видна. Останавливаются интерактивные процессы.
5) `onStop` — вызывается, когда Activity перестает быть видна, освобождаются ресурсы, которые временно не нужны.
6) `onSaveInstanceState` - вызывается, когда Activity планирует сохранить состояние. Этого нет на рисунке. Об этом рассказано чуть ниже.
7) `onRestart` — вызывается перед тем, как Activity вновь будет видна.
8) `onDestroy` — вызывается перед тем, как Activity будет уничтожена. Освобождаются ресурсы и т. д.

Самый простой пример. Когда мы слушаем музыку, а потом включаем в «Телеграме» включаем видеосообщение, то музыка останавливается, а после видеосообщение — возобновляется. 
1) Вы запускаете видео в Telegram → система дает фокус Telegram.
2) Ваше музыкальное приложение теряет фокус → onPause().
3) Музыкальное приложение полностью скрыто → onStop().
4) Музыкальное приложение НЕ уничтожается — остается в фоне.
5) Видеосообщение закрывается, возвращается фокус и срабатывает `onRestart` и далее по цепочке.

### Сохранение состояния Activity
---

Это процесс сохранения и восстановления данных, чтобы обеспечить непрерывный пользовательский опыт при изменении конфигурации устройства и при временном выходе приложения из фокуса. Например, того же музыкального приложения. Когда мы переходим в «Телеграмм» и включаем видео, то музыкальное приложение теряет фокус. И в этот момент в качестве данных сохраняет время, на котором остановилась музыка, какая музыка и прочее. А когда фокус возвращается, то эти данные восстанавливаются. 

Смена конфигурации Android — это изменение настроек устройства: ориентация экрана, язык, размер экрана и т. д. Например, поворот экрана — в таком случае приложения пересоздает Activity для применения новых настроек. Если мы хотим сохранить данные при пересоздании, то важно изучить, что такое класс `Bundle`.

### Класс Bundle
---

Это класс, используемый для передачи данных между компонентами приложения и для сохранения состояния. Хранятся данные как словари (или Map) «ключ-значение». Объем данных ограничивается `IPC` (межпроцессорным взаимодействием). Этот лимит составляет примерно 1 МБ, потому хранить нужно только легкие данные, иначе будет крэш приложения.

Сохраняем состояние в 2 шага:
1) Переопределяем метод `onSaveInstanceState`. В нем потребуется записать необходимые данные в Bundle. Этот метод гарантированно вызывается до уничтожения экрана (до `onDestroy`).
2) Восстановить состояние в методах `onRestoreInstanceState()` или `onCreate()`. Обычно используют `onCreate()`. Для этого нужно достать значение из Bundle по ключу.

```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {
    // Переменная для хранения состояния
    private var counter: Int = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Восстановление состояния, если оно существует
        savedInstanceState?.let {
            counter = it.getInt(KEY_COUNTER, 0)
        }

        // Установка значения счетчика в TextView
        updateCounterText()

        // Увеличение счетчика при нажатии на кнопку
        incrementButton.setOnClickListener {
            counter++
            updateCounterText()
        }
    }

    // Сохранение состояния
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putInt(KEY_COUNTER, counter)
    }

    // Обновление текста счетчика в TextView
    private fun updateCounterText() {
        counterTextView.text = counter.toString()
    }

// Ключи для сохранения состояния
    private companion object {
        const val KEY_COUNTER = "counter"
    }
}
```

# Навигация между Activity


Поскольку приложение может состоять из нескольких окон, необходимо знать, как навигироваться между страницами, то есть переключаться между Activity.
### Intetn
---

`Intent` — это класс, что используют для сообщения системе, что мы хотим запустить Activity. Используется для запуска и других компонентов приложения. 
Есть два вида `Intent`:
1) Явный — используется для запуска конкретного компонента. В его конструктор должна передаваться информация о запускаемом классе и контекст (1).
2) Нявный — используется для открытия компонента, который может выполнить действие, переданное в конструктор. Действие можно уточнить за счет категории, которую можно передать в конструктор. После запуска такого интента система найдет сама подходящий компонент. (2)
```
(1) - пример янвого интента
val intent = Intent(context, SecondActivity::class.java)
startActivity(intent)
```

```
(2) - пример НЕявного интента
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("geo:55.7558,37.6176"))
startActivity(intent)
// Покажет выбор: Яндекс.Карты, Google Maps, другие навигаторы
```

`ACTION_VIEW` — это метод, который позволяет находить необходимое приложение, которое выполнит действие. В примере система сама ищет приложение, которое может принять координаты. А потом мы можем сделать так, что пользователю выйдет диалоговое окно с выбором приложения. 


Мы можем выбрать разные классы для action. В примере мы выбрали `Intent`. А есть еще:
- `MediaStore` — действия, специфичные для медиа
- `Settings` — действия, специфичные для настроек
- `ContactsContract` — действия, специфичные для контактов.

Такое называется intetn-filter. 

На занятии мы запустили приложение, где в манифесте были указаны `action = android.intent.action.MAIN` и `category = android.intent.category.LAUNCHER`, что позволило запустить Activity как стартовый экран приложения. А `android.intent.category.LAUNCHER` — это категория, которая указывает, что Activity должна отображаться в лаунчере приложения, то есть в списке приложений на устройстве.

### Передача данных
---
И тут тоже используется `Bundle`, который находиться между объявлением `intent` и `startActivity`

```
 val intent = Intent(this, SecondActivity::class.java)
            
 intent.putExtra("EXTRA_MESSAGE", "Hello from MainActivity")
 intent.putExtra("EXTRA_NUMBER", 123)
 
 startActivity(intent)
 ```

Здесь по ключу `EXTRA_MESSAGE` мы сохранили текст "Hello from MainActivity", а по ключу `EXTRA_NUMBER`  - 123 и сделали явный интент на `SecodActivity` (то есть переход).


Заберем данные в `SecondActivity`:

```
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        val message = intent.getStringExtra("EXTRA_MESSAGE")
        val number = intent.getIntExtra("EXTRA_NUMBER", 0)
}
```


### Флаги навигации между Activity

Все запускаемы приложения формируют стек, поведением котрого можно управлять при помощи флаов навигации:
- `standart` - по умолчания каждая новая Activity будет запускаться как новая, даже если она уже усещствует
- `singleTop` - если Activity уже находиться на вершине стека задач, то при повторном запуске не будет сосздан новый экземлпяр, а будет вызвна `onNewIntent`
- `singeltTask` - если Activity уже существует в стеке задач, она будет переиспользована и все Activity выше нее будут удалены
- `singleInstance` - похож на предыдщуее, но гаратирует, что эта Activity будет единственой в своей задаче

Флаг запуска Activity можно указать либо в манифесте при помощи поля launchMode:
```
<activity
    android:name=".MainActivity"
    android:launchMode="singleTop">
</activity>
```
Либо в коде, задав поле flags для Intent (flags отличаются от launchMode по названию):
```
val intent = Intent(this, SecondActivity::class.java) 
intent.flags = Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP
startActivity(intent)
```

# Как работает ACTION?

Хочется немного остановиться и немного рассказать о том, как `action` находит подходящее приложение. 

При создании приложения в манифесте идет объявление того, что умеет это приложение.
Например:

```
<!-- В Яндекс.Браузере: -->
<activity android:name=".YandexBrowserActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:scheme="http" />
        <data android:scheme="https" />
    </intent-filter>
</activity>
```

Здесь мы видим, что приложение `Яндекс Браузер` может обрабатывать "http" и "https". В свою очередь, `action android:name="android.intent.action.VIEW"` говорит о том, что приложение может показывать/просматривать данные. И может обрабатывать, например, ссылки или файлы. А `<category android:name="android.intent.category.DEFAULT" />` — говорит, что Activity обязательна для неявных Intent. Ну, то есть включает видимость для других приложений.

Далее, когда мы пишем `val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://google.com"))`, система Android включает фильтры — `intent-filter`. При установке приложения система андройда автоматически сканирует манифест и строит базу данных интент-фильтров. Пример:
```
Системная база данных:
- Chrome:      ACTION_VIEW + (http|https)
- Firefox:     ACTION_VIEW + (http|https)  
- Яндекс:      ACTION_VIEW + (http|https)
- Instagram:   ACTION_SEND + image/*
- Камера:      ACTION_IMAGE_CAPTURE
- Телефон:     ACTION_DIAL + tel
```

Тут для каждого приложения отображаются его возможности. Эти данные и берутся из манифеста. И как только мы делаем в коде `intent`, система начинает поиск:
```
// Ваш запрос:
Intent(ACTION_VIEW, "https://google.com")

// Система ищет совпадения по:
// 1. Action: ACTION_VIEW
// 2. Data: scheme="https", host="google.com"
// 3. Category: DEFAULT (подразумевается)
```

Далее система возвращает список подходящих приложений.


В системе андройда работает специальная служба `PackageManagerService`, которая:
- запрашивает `AndroidManifest.xml` при установке приложения
- строит индекс всех Intent Filter'ов.
- При запросе быстро находит нужные приложения
