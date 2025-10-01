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

Предлагаю узнать о жизненном цикла (ЖЦ) у данного компоенента. Это последовательность состояний, через которые проходит объект. 

Давайте попрядку. После создания объект оказывациеся в `начальном сосотянии`.  Затем переходит в `состояние 1`, потом `состояние 2` и тд. Конечно, могут быть и условия перехода из состояний в состояния или возврата в прошлое. В итоге это приходит к конечному.

![](https://edu.tbank.ru/files/4483c25f-d568-4232-bb32-d751860790b0)
