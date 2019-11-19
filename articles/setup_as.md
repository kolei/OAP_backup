[содержание](/readme.md)

https://startandroid.ru/ru/uroki/vse-uroki-spiskom.html

1. [Установка Android Studio](#Установка-Android-Studio)
2. [Создание нового проекта](#Создание-нового-проекта)
3. [Знакомство с интерфейсом Android Studio](#Знакомство-с-интерфейсом-Android-Studio)
4. [Добавление кнопки и обработчика события onClick для нее](#Добавление-кнопки-и-обработчика-события-onClick-для-нее)
5. [Проект "калькулятор"](#Проект-калькулятор)
6. [Проект "Погода"](#Проект-Погода)

    * [Получение текущей локации](#Получение-текущей-локации)
    * [Http запросы](#Http-запросы)
    * [Вывод иконки погоды](#Вывод-иконки-погоды)
    * [Splash screen](#Splash-screen)
    * [Получение массива данных (данные о погоде за несколько дней)](#Получение-массива-данных-данные-о-погоде-за-несколько-дней)
    * [Создание дополнительной формы (список городов)](#Создание-дополнительной-формы-список-городов)

7. [Проект "Достопримечательности"](#Проект-Достопримечательности)


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



# Проект **Погода**

Цели:
* получить текущую локацию
* по сети получить погоду для текущей локации
* отобразить погоду на форме
* создание splash-screen
* создание дополнительных форм, переход между формами, обмен данными между формами

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

## Http запросы

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

## Вывод иконки погоды

Для отображения иконки погоды используем компонент ImageView и библиотеку Glide. Для установки библиотеки:

    * добавить репозиторий mavenCentral() в build.graddle (Project)
    * добавить зависимость ``implementation 'com.github.bumptech.glide:glide:4.10.0'`` в build.graddle (Module)

Функция запроса погоды

```kt
// http клиент
private val client = OkHttpClient()

fun getWheather(lon: Double, lat: Double) {
    val token = "d4c9eea0d00fb43230b479793d6aa78f"
    val url = "https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${token}"

    val request = Request.Builder().url(url).build()

    client.newCall(request).enqueue(object : Callback {

        override fun onFailure(call: Call, e: IOException) {
            setText( e.toString() )
        }

        override fun onResponse(call: Call, response: Response) {
            response.use {
                if (!response.isSuccessful) throw IOException("Unexpected code $response")

                // так можно достать заголовки http-ответа
                //for ((name, value) in response.headers) {
                //  println("$name: $value")
                //}

                //строку преобразуем в JSON-объект
                var jsonObj = JSONObject(response.body!!.string())


                // обращение к визуальному объекту из потока может вызвать исключение
                // нужно присвоение делать в UI-потоке
                setText( jsonObj )
            }
        }
    })
}

fun setText(t: JSONObject){
    runOnUiThread { 
        // достаем из ответа сервера название иконки погоды
        val wheather = t.getJSONArray("weather")
        val icoName = wheather.getJSONObject(0).getString("icon")
        val icoUrl = "https://openweathermap.org/img/w/${icoName}.png"

        // аналогично достаньте значение температуры и выведите на экран

        // загружаем иконку и выводим ее на icon (ImageView)
        Glide.with(this).load( icoUrl ).into( icon )
    }
}

```

## Splash screen

Запрос данных может занять несколько секунд, чтобы пользователь не смотрел на пустую форму можно добавить загрузочный экран, который будет отображать какую-то картинку, пока данные не получены.

1. В каталог ``res/drawable`` положить картинку (с расширением png)
2. на форму кинуть ImageView, задать фоном загруженную картинку  и развернуть на весь экран
3. После отрисовки полученных данных скрыть картинку ``.isVisible = false``
4. Чтобы пользователь успел полюбоваться вашим Splash-скрином, можно сделать скрытие по таймеру:

```kt
// объявляем приватную переменую ``ready``, которая будет установлена при получении данных о погоде
private var ready = false

// и счетчик
private var couter = 0

...

// в конструктор добавляем счечик (маскимальное время ожидания, периодические события)
object : CountDownTimer(5000,1000){
    override fun onTick(millisUntilFinished: Long) {
        // если данные получены и прошло 3 сек, то скрываем splash screen и останавливаем счетчик
        counter++
        if(counter>3 && ready){
            splash_screen.isVisible = false
            this.cancel()
        }
    }

    override fun onFinish(){
        splash_screen.isVisible = false
    }
}.start()

...

Glide.with(this).load( icoUrl ).into(icon)

// в конце функции получения данных выставляем флаг готовности
ready = true

```

## Получение массива данных (данные о погоде за несколько дней)

Цели:

* работа с разными вариантами данных
* работа с циклами
* поиск визуального объекта по его ``id``

### Данные о погоде за 5 дней

Описание API находится [тут](https://openweathermap.org/forecast5)

### Работа с циклами

При получении данных за 5 дней API возвращает JSON-массив данных

```kt

...

var list = t.getJSONArray("list")

// объект JSONArray не реализует интерфейс Iterable, поэтому нужно обращаться к элементам массива по индексу
for(i in 0..list.length()-1){
    var item = list.getJSONObject(i)
    
    // дальше обрабатываем как обычно
    ...

}

```

### Поиск визуального объекта по его ``id``

Для отображения нескольких записей о погоде можно использовать два варианта: 

* в дизайнере создать n-визуальных объектов с ``id`` вида ``item_0``, ``item_1`` ..``item_(n-1)``. И потом при отрисовке получать ссылку на объект по его ``id``

> Для этого проекта не пригодилось, но для ознакомления оставлю  
>```kt
>val id = resources.
>    getIdentifier("item_${i}", "id ", getPackageName())
>
>if(id>0){
>    val cur_item = findViewById<TextView>(id)
>
>    cur_item.text = температура
>}
>```

* динамически создавать визуальные объекты для каждой записи о погоде

На это марианте остановлюсь подробнее:

1. Добавляем на форму горизонтальный скролл и зададим id вложенному LinearLayout

![](/img/as029.png)

2. Добавим класс, потомок LinearLayout

![](/img/as030.png)

![](/img/as031.png)

```kt
class CustomLayout : LinearLayout {
    // описываем публичные аттрибуты для текста и картинки
    var tempTextView: TextView? = null
    var icoImageView: ImageView? = null

    // пишем конструктор
    constructor(context: Context?): super(context){
        this.orientation = LinearLayout.VERTICAL
        this.minimumWidth = 150

        // создаем TextView
        tempTextView = TextView(context)
        tempTextView!!.textAlignment = View.TEXT_ALIGNMENT_CENTER
        this.addView(tempTextView)

        // создаем ImageView
        icoImageView = ImageView(context)
        this.addView(icoImageView)

    }
}
```

Далее в основном коде в цикле обработки данных о погоде динамически создаем наш CustomLayout для каждого элемента и помещаем его в созданный скролл

```kt
for(i in 0..list.length()-1){
    val item = CustomLayout(this)
    if(item!=null) {
        container.addView(item)
        item.tempTextView!!.text = i.toString()
        Glide.with(this).load(icoUrl).into(item.icoImageView!!)
    }
}
```

## Создание дополнительной формы (список городов)

1. Создаем новую форму (Activity)

![Создаем новую форму](/img/as032.png)

![Задаем название](/img/as033.png)

2. На форму кидаем вертикальный LinearLayout, в него TextView и ListView. ListView обзываем как *cityList*

![Задаем название](/img/as035.png)


3. На основную форму добавляем кнопу перехода на экран выбора города и обработчик для нее:

```kt
selectCity.setOnClickListener {
    // при клике переходим на форму выбора города
    startActivity( Intent(this, CityListActivity::class.java) )
}
```

4. Класс CityListActivity

Подробнее см [тут](http://developer.alexanderklimov.ru/android/listactivity.php)

```kt
// создаем массив городов
private var names = arrayOf(
    "Moscow",
    "Yoshkar-Ola",
    "Kazan"
)
```

```kt
// в конструкторе создаем адаптер для списка городов
// где R.layout.city_list_item - название НОВОГО layout для элемента списка
cityList.adapter = ArrayAdapter(
    this,
    R.layout.city_list_item, names
)
```

Android Studio покажет ошибку, что не знает что такое ``city_list_item`` - добавляем реализацию:

![Создание Layout для элемента списка](/img/as036.png)

**Внимание!** RootElement поменять на TextView

![Создание Layout для элемента списка](/img/as037.png)

```kt
// создаем обработчик событий выбора элемента списка
cityList.setOnItemClickListener { parent, view, position, id ->
    val mainIntent = Intent(this, MainActivity::class.java)
    val cityName = names[id.toInt()]

    // запоминаем выбранное название города
    mainIntent.putExtra("city_name", cityName)

    // возвращаемся на основной экран (Activity)
    startActivity( mainIntent )
}
```

В MainActivity считываем название выбранного города

```kt
val cityName = intent.getStringExtra("city_name")
```

и делаем проверку, если город есть, то запрашиваем данные о погоде по названию города, если нет, то по старому варианту через определение координат

## Передача параметров в форму

```kt
// перед переходом в Activity можно передать параметры
val mainIntent = Intent(this, MainActivity::class.java)

val cityName = "Moscow"

mainIntent.putExtra("city_name", cityName)

startActivity( mainIntent )

...

// и в целевом активити, соответственно, извлечь эти данные
val newCityName = intent.getStringExtra("city_name")
```

## Хранение данных

Для хранения больших массивов данных нужно использовать SQLite или аналоги, но для простых приложений можно использовать *Preferences* - хранение пар *ключ* -> *значение*:

```kt
// аттрибут класса - массив городов - пустой массив
private var names = emptyArray<String>()


// запрашиваем приватное хранилище с названием "settings" (если нет, то создаст автоматически, количество хранилищ не ограничено)
val myPreferences = getSharedPreferences("settings", MODE_PRIVATE)

// запрашиваем из хранилища список городов (можно задать значение по-умолчанию)
// андроид не позволяет хранить массивы, поэтому список хранится как строка с разделителями
val oldCityListString = myPreferences.getString("cityList", "Moscow|Kazan|Yoshkar-Ola")

// заполняем массив городов
names = cityListString!!.split("|").toTypedArray()
```

Для записи данных в хранилище нужно создать объект "редактор" и после записи сохранить изменения:

```kt
val editor = myPreferences.edit()
try {
    editor.putString("cityList", oldCityListString+"|"+newCityName )
}finally {
    editor.commit()
}
```

## Контрольное задание по проекту **Погода**

Создать приложение состоящее из трех форм (Activity)

**Первый эркан**: "Заставка" (splash-screen). Показывать не менее 3-х секунд. Чтобы без толку не висело - в этом экране запрашиваем текущую локацию. Если координаты определены, то переходим на второй экран, если ошибка, то на третий

**Второй экран**: основная форма с показом погоды за текущий день и список за 5 дней. На экране должна быть кнопка выбора города, по клику на которой открывается третий экран

**Третий экран**: выбор города (в него попадаем при ошибке геолокации или при переходе со второго экрана). При выборе города переходим на *второй экран* и показываем погоду для выбранного города


# Проект "Достопримечательности"

Цели:
* Научитсья работать с Google Maps
* Авторизация на сервере
* Построение маршрутов

## Авторизация на сервере

> Для сетевых запросов далее будем пользоваться библиотекой [Fuel](https://github.com/kittinunf/fuel). 

### Установка библиотеки

В зависимости приложения добавляем ``implementation 'com.github.kittinunf.fuel:fuel-android:2.2.1'`` (актуальную версию библиотеки уточняйте на сайте разработчика)

### Примеры запросов

Запросы бывают двух типов: синхронные (приложение ждет ответа от сервера, останавливая работу - такие запросы имеет смысл использовать только при авторизации, когда дальше просто нельзя двигаться) и асинхронные (запрос посылается в фоне, выполнение программы продолжается - такой режим предпочтительнее, т.к. не "замораживает" интерфейс)

#### Синхронный запрос

> Адрес сервера динамический, уточняйте в начале лекции

```kt
val (request, response, result) = "http://192.168.1.18/login"
    .httpPost()
    .responseString()

```       

Приложение может не запуститься, выдав ошибку "Cleartext HTTP traffic to ... not permitted" - в последних версиях Андроида по-умолчанию запрещено работать без ssl. Для разрешения открытого траффика добавьте в манифест в секцию **application** аттрибут ``android:usesCleartextTraffic="true"``

import com.github.kittinunf.result.Result

[содержание](/readme.md)