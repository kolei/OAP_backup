[содержание](/readme.md)

https://startandroid.ru/ru/uroki/vse-uroki-spiskom.html

1. [Установка Android Studio](#Установка-Android-Studio)
2. [Создание нового проекта](#Создание-нового-проекта)
3. [Знакомство с интерфейсом Android Studio](#Знакомство-с-интерфейсом-Android-Studio)
4. [Добавление кнопки и обработчика события onClick для нее](#Добавление-кнопки-и-обработчика-события-onClick-для-нее)
5. [Проект "калькулятор"](#Проект-"калькулятор")
6. [Проект "Погода"](#Проект-"Погода")

    * [Получение текущей локации](#Получение-текущей-локации)


# Установка Android Studio

> Внешний вид Android Studio может измениться в следующих версиях, методичка написана на основе версии 3.5 (Windows)

1. [Скачать дистрибутив (версия ОС определяется автоматически)](https://developer.android.com/studio/)

2. Установить Android Studio (все настройки по-умолчанию)

3. Установить эмулятор

    * откройте AVD Manager 
    ![запуск AVD manager](/img/as003.png)

    * кликните "Create Virtual Device"
    ![создание эмулятора](/img/as004.png)

    * выберите устройство
    ![](/img/as005.png)

    * скачайте образ (у меня уже скачан для Q) и нажмите "Next"
    ![](/img/as006.png)


# Создание нового проекта

1. Создайте новый проект (File - New - New Project...)
![создание нового проекта](/img/as007.png)    

2. Выберите "Empty Activity"
![создание нового проекта](/img/as001.png)    

3. Задайте название (Name) и местоположение (Save location) проекта. Остальные настройки по-умолчанию
![настройка проекта](/img/as002.png)


# Знакомство с интерфейсом Android Studio

1. В панели "Project" отображается структура нашего проекта:
![знакомство с интерфейсом](/img/as008.png)

    * внешний вид описывается в activity_main.xml, который расположен в app/res/layout

    * программный код (MainActivity) для этого *activity* расположен в app/java/com.example.test1, где "com.example.test1" название пакета

    * в центральном окне открыты закладки для activity_main и MainActivity

    * в режиме **Design** на закладке activity_main отображается внешний вид выбранного экрана, палитра компонентов (с нее можно перетаскивать компоненты на форму) и аттрибуты выбранного компонента (в нашем случае TextView)

    * на закладке MainActivity отображается код приложения
    ![знакомство с интерфейсом](/img/as009.png)
    <br/>, где
        * super.onCreate(savedInstanceState) - вызов конструктора базового класса
        * setContentView(R.layout.activity_main) - инициализация activity

# Добавление кнопки и обработчика события onClick для нее

1. С панели перетаскиваем объект "Button" на форму
![добавление кнопки](/img/as010.png)  
При добавлении компонента на форму система выдает предупреждение, что нет привязок по горизонтали и вертикали - если их не задать, то все компоненты будут иметь координаты 0,0 и будут отрисовываться в левом верхнем углу. Чтобы этого не происходило нужно добавить привязку либо к сторонам формы, либо к соседним объектам. (Привязка нужна по *горизонтали* и *вертикали*, т.е. не обязательно делать все четыре привязки, достаточно двух)
![добавление кнопки](/img/as013.png)  
Привязку можно задать либо на самом объекте  
![добавление кнопки](/img/as014.png)    
либо в аттрибутах  
![добавление кнопки](/img/as015.png)    

2. Для объекта можно поменять цвет фона, цвет и стиль текста  
![добавление кнопки](/img/as016.png)    

3. В свойствах кнопки заполняем поле id. **Идентификатор визуального компонента одновременно является названием объекта в коде, поэтому в названии можно использовать только буквы, цифры и знак "_".** Я назвал кнопку *btn_one*.

4. Переходим на закладку "MainActivity.kt" и набираем первые буквы id кнопки
![добавление кнопки](/img/as011.png), 
<br/>выбираем полное имя из подсказки - Android Studio автоматически добавит в импорт все объекты нашей формы, т.о. мы можем обращаться к объекту не объявляя его явно<br/>
![добавление кнопки](/img/as012.png)<br/>
    > В jave для создания ссылки на объект нужно было использовать функцию 
    >```java
    >val btn_one = findViewById(R.id.btn_one)
    >```

5. Создаем обработчик на событие "click". Обработчик (слушатель) событий назначается с помощью метода setOnClickListener. Есть несколько вариантов его использования:<br/>
    
    * первый вариант: обработчик пишется прямо в месте объявления (в конструкторе формы), подходит для мелких действий
    ```kt
    btn_one.setOnClickListener {
        textView.text="hello"
    }
    ```

    * второй вариант: создается функция обработчик

    ```java
    ...
    btn_one.setOnClickListener(this::onClick)
    ...
    
    fun onClick(view: View){
        textView.text = "hello"
    }
    ```

    * третий вариант: добавить форме интерфейс "View.OnClickListener" и реализовать событие "onClick"
    ```java
    class MainActivity : AppCompatActivity(), View.OnClickListener {
        override fun onClick(v: View?) {
            textView.text = "hello"
        }

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            btn_one.setOnClickListener(this)
        }
    }
    ```
    Можно один и тот же обработчик событий назначить нескольким объектам

    ```java
        btn_one.setOnClickListener(this)
        btn_two.setOnClickListener(this)
    ```

    * четвертый вариант: в коде создать функцию обработчик и в аттрибутах кнопки свойству "onClick" присвоить эту функцию:
    ```java
    fun onClick(v: View) {
        var tmp = textView.text.toString()
        // when - это аналог switch
        when(v.id) {
            //R.id.btn_one - идентификатор кнопки
            R.id.btn_one -> textView.text = tmp+'1'
            R.id.btn_bs->{
                if(tmp.isNotEmpty()) {
                    textView.text = tmp.take(tmp.length-1)
                }
            }
        }
    }
    ```
    ![](/img/as017.png)

# Проект "калькулятор"

1. Установите, если еще нет, Adobe XD https://www.adobe.com/ru/products/xd.html

2. Склонируйте (или скачайте) дизайн калькулятора https://github.com/kolei/wsr-calc

3. Реализуйте дизайн в Android Studio (максимально близко к дизайну расположить и раскрасить компоненты: текст и кнопки) и напишите логику (обработчики нажатий на все кнопки)

## Оптимизация размещения кнопок

Позиционировать каждую кнопку нудно и дизайн получается девайсо-зависимым

Попробуем "резиновую" верстку с помощью LinearLyout

1. Очистим Activity от всех компонентов, поместим на форму LinearLayout (vertical)
![](/img/as019.png)  
и "привяжем" к краям родителя

2. Поместим на форму TextView для дисплея калькулятора - ширина компонента автоматически выравнивается по родительскому LinearLayout, высоту задаим позже

3. Поместим на форму LinearLayout (horizontal)
![](/img/as020.png)  
высоту зададим как у кнопки из дизайна

4. поместим в этот контейнер кнопку
![](/img/as021.png)  
и установим layout_height = match_parent  
после этого скопируем кнопку и вставим 3 копии  
![](/img/as022.png)  

5. повторим пп 3-4 для всех рядов кнопок калькулятора

6. Высоту TextView подгоним так, чтобы был заполнен весь Activity


## Конкатенация строк

Вообще строки склеиваются знаком "+", но свойство text объекта textView имеет тип CharSequence и конкатенацию не поддерживает, приходится делать двойное преобразование:

```java
btn_one.setOnClickListener{
    // объявляем временную строковую переменную
    var tmp = textView.text.toString()
    textView.text = tmp+'1'
}
```

## Удаление последнего символа

Есть несколько методов для получения подстроки, например, **take** возвращает первые **n** символов строки:

```java
btn_bs.setOnClickListener {
    if(textView.text.length>0)
        textView.text = textView.text.take(textView.text.length-1)
}
```

> Котлин компилируемый язык, в Android Studio не удобно проверять как будет работать какая-то функция. Для проверки можно использовать онлайн "проигрыватель" https://play.kotlinlang.org/


## Добавление альбомной ориентации

В режиме "design" кликаем кнопку "Orientation..." выбираем "Create Landscape Variation"    
![](/img/as023.png)

Система автоматически создаст Layout с альбомной ориентацией.   
![](/img/as024.png)

> Учитывайте, что конструктор общий для всех ориентаций - при обращении к несуществующему объекту произойдет исключение. Что-бы этого не происходило, нужно либо обработчики событий для кнопок оформить отдельными функциями, либо проверять наличие объекта кнопки перед вызовом ``setOnClickListener``



# Проект "Погода"

Цели:
* получить текущую локацию
* по сети получить погоду для текущей локации
* отобразить погоду на форме

## Получение текущей локации

### Стандартные средства

[Тут](https://developers.google.com/android/reference/com/google/android/gms/location/package-summary) описаны стандартные интерфейсы для работы с геолокацией


[На основе этого примера можно посмотреть как это работает](https://en.proft.me/2019/01/3/how-get-location-latitude-longitude-android-kotlin/)

1. В манифест добавляем разрешения для работы с геолокацией  
```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

![](/img/as025.png)


2. В build.graddle (Module: app) добавляем зависимость  
```
implementation 'com.google.android.gms:play-services-location:11.8.0'
```

![](/img/as026.png)


Полный текст программы:

```kt
package com.example.wheather

import android.Manifest
import android.app.AlertDialog
import android.content.pm.PackageManager
import android.location.Location
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat
import com.google.android.gms.location.FusedLocationProviderClient
import com.google.android.gms.location.LocationServices
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    var fusedLocationClient: FusedLocationProviderClient? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // инициализируем объект
        fusedLocationClient = LocationServices.
            getFusedLocationProviderClient(this)

        // запрашиваем разрешение
        if (checkPermission(
            Manifest.permission.ACCESS_COARSE_LOCATION,
            Manifest.permission.ACCESS_FINE_LOCATION)) 
        {
            fusedLocationClient?.lastLocation?.
                addOnSuccessListener(this,
                    // Got last known location. In some rare
                    // situations this can be null.
                    {location : Location? ->
                        // полученные координаты выводим на экран
                        if(location == null) {
                            textView.text = "location == null"
                        } else location.apply {
                            textView.text = location.toString()
                        }
                    })
        }
    }

    private fun checkPermission(vararg perm:String) : Boolean {
        val PERMISSION_ID = 42

        val havePermissions = perm.toList().all {
            ContextCompat.checkSelfPermission(this,it) ==
                    PackageManager.PERMISSION_GRANTED
        }

        if (!havePermissions) {
            if(perm.toList().any {
                ActivityCompat.
                    shouldShowRequestPermissionRationale(this, it)
            }){
                val dialog = AlertDialog.Builder(this)
                    .setTitle("Permission")
                    .setMessage("Permission needed!")
                    .setPositiveButton("OK", {id, v ->
                        ActivityCompat.requestPermissions(
                            this, perm, PERMISSION_ID)
                    })
                    .setNegativeButton("No", {id, v -> })
                    .create()
                dialog.show()
            } else {
                ActivityCompat.requestPermissions(this, perm, PERMISSION_ID)
            }
            return false
        }
        return true
    }
}
```

### Сторонние библиотеки

В стандартной реализации, как обычно, слишком много букв, к счастью есть [библиотека](https://github.com/BirjuVachhani/locus-android), в которой вся рутина скрыта:

1. Добавляем репозиторий в build.graddle (Project)

```
maven { url 'https://jitpack.io' }
```

![](/img/as027.png)


2. Добавляем зависимости в build.graddle (Module app)

```
implementation 'com.google.android.gms:play-services-location:17.0.0'
implementation 'com.github.BirjuVachhani:locus-android:3.0.1'
```

![](/img/as028.png)


3. В конструктор добавляем запрос геолокации:

```kt
Locus.getCurrentLocation(this) { result ->
    result.location?.let {
        tv.text = "${it.latitude}, ${it.longitude}"
    } ?: run {
        tv.text = result.error?.message
    }
}
```


Полный текст программы:

```kt
package com.example.locator2

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import com.birjuvachhani.locus.Locus
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Locus.getCurrentLocation(this) { result ->
            result.location?.let {
                tv.text = "${it.latitude}, ${it.longitude}"
            } ?: run {
                tv.text = result.error?.message
            }
        }

    }
}
```

## Http запросы (не закончено)

В Kotlin-е есть встроенные функции работы с http-запросами, но стандартный код для сетевых запросов сложен, излишен и в реальном мире почти не используется. Используются библиотеки. Самые популярные: [OkHttp](https://square.github.io/okhttp/) и Retrofit.

Рассмотрим работу к **OkHttp**

https://square.github.io/okhttp/recipes/ - примеры синхронных и асинхронных запросов на котлине

### Подключение библиотеки к проекту:
   
![](/img/as018.png)

1. На закладке **Project** в **Gradle Scripts** открываем файл **build.gradle (Module: app)**

2. В файле находим секцию **dependencies** (зависимости)

3. Добавляем нашу библиотеку ``implementation 'com.squareup.okhttp3:okhttp:4.2.1'``. На момент написания методички последняя версия была 4.2.1, вы можете уточнить актуальную версию на сайте.

4. Синхронизируйте измения (Gradle скачает обновившиеся зависимости)
    
5. В манифест добавляем права на доступ в интернет
```
<uses-permission android:name="android.permission.INTERNET" />
```    

6. В функцию определения координат вместо вывода координат на экран вставаляем вызов функции, запрашивающей погоду для этих координат


```kt
Locus.getCurrentLocation(this) { result ->
    result.location?.let {
        //tv.text = "${it.latitude}, ${it.longitude}"

        getWheather(it.longitude, it.latitude)

    } ?: run {
        tv.text = result.error?.message
    }
}
```

7. Функция запроса погоды

```kt
// http клиент
private val client = OkHttpClient()

fun getWheather(lon: Double, lat: Double) {
    val token = "d4c9eea0d00fb43230b479793d6aa78f"
    val url = "https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${token}"

    val request = Request.Builder().url(url).build()

    client.newCall(request).enqueue(object : Callback {

        override fun onFailure(call: Call, e: IOException) {
            tv.text = e.toString()
        }

        override fun onResponse(call: Call, response: Response) {
            response.use {
                if (!response.isSuccessful) throw IOException("Unexpected code $response")

                // так можно достать заголовки http-ответа
                //for ((name, value) in response.headers) {
                //  println("$name: $value")
                //}

                // полученный результат выводим на экран (JSON)
                tv.text = response.body!!.string()
            }
        }
    })
}
```

[содержание](/readme.md)