[содержание](/readme.md)

1. [Установка Android Studio](#Установка-Android-Studio)
2. [Создание нового проекта](#Создание-нового-проекта)
3. [Знакомство с интерфейсом Android Studio](#Знакомство-с-интерфейсом-Android-Studio)
4. [Добавление кнопки и обработчика события onClick для нее](#Добавление-кнопки-и-обработчика-события-onClick-для-нее)
5. [Проект "калькулятор"](#Проект-"калькулятор")


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

2. В свойствах кнопки заполняем поле id. **Идентификатор визуального компонента одновременно является названием объекта в коде, поэтому в названии можно использовать только буквы, цифры и знак "_".** Я назвал кнопку *btn_one*.

3. Переходим на закладку "MainActivity.kt" и набираем первые буквы id кнопки
![добавление кнопки](/img/as011.png), 
<br/>выбираем полное имя из подсказки - Android Studio автоматически добавит в импорт все объекты нашей формы, т.о. мы можем обращаться к объекту не объявляя его явно<br/>
![добавление кнопки](/img/as012.png)<br/>
    > В jave для создания ссылки на объект нужно было использовать функцию 
    >```java
    >val btn_one = findViewById(R.id.btn_one)
    >```

4. Создаем обработчик на событие "click". Обработчик (слушатель) событий назначается с помощью метода setOnClickListener. Есть несколько вариантов его использования:<br/>
    
    * первый вариант: обработчик пишется прямо в месте объявления (в конструкторе формы), подходит для мелких действий
    ```kt
    btn_one.setOnClickListener {
        textView.text="hello"
    }
    ```

    * второй вариант: создается функция обработчик

    ```kt
    ...
    btn_one.setOnClickListener(this::onClick)
    ...
    
    fun onClick(view: View){
        textView.text = "hello"
    }
    ```

    * третий вариант: добавить форме интерфейс "View.OnClickListener" и реализовать событие "onClick"

    ```kt
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

    ```kt
        btn_one.setOnClickListener(this)
        btn_two.setOnClickListener(this)
    ```

# Проект "калькулятор"

1. Установите, если еще нет, Adobe XD https://www.adobe.com/ru/products/xd.html

2. Склонируйте дизайн калькулятора https://github.com/kolei/wsr-calc

3. Реализуйте дизайн в Android Studio и напишите логику

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

Есть несколько методов для получения подстроки, например, take возвращает первые n симворлов строки:

```java
btn_bs.setOnClickListener {
    if(textView.text.length>0)
        textView.text = textView.text.take(textView.text.length-1)
}
```

[содержание](/readme.md)