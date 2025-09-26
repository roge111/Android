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

### Activity
---
Это компонент, который отвечает за создание контейнера, куда можем положить наш интерфейс. Простые приложения состоят из одной активности, более сложные могут иметь несколько окон.
Чтобы создать компонент `Activity`:
1) Создать класс-наследник `AppCompatActivity`
2) Добавить объявление `Activity` в манифест.

### Создадим первый компонет
----

Для начала создаим проект:
1) `File` → `New` → `New Project`
2) В появившемся окне выберите `Empty Views Activity`

Теперь дождемся полного создания. В левом верхнем угул будет надпись `Project`. Она должна сменить на `Android`, что будет показывать вам только те пакет, папки и файл, необходимые для приложения. Если этого не произойдет, то мы сами нажимаем на `Project`, в выпавшем списке выбираем `Android`. 


Далее в директории `kotlin+java` в пакете не помеченым ни `androidTest`, ни `test` создаем класс котлина. По стандарту входной точкой приложения называют `MainActivity`. Внтури создаем класс и наследуем от `AppCompatActivity`.
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
