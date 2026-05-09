# Abstract Factory (Абстрактная фабрика)
**Категория:** Creational (Порождающий)

## 📌 Суть (Intent)
Паттерн **Abstract Factory** позволяет создавать **СЕМЕЙСТВА (families)** связанных объектов, не привязываясь к их конкретным классам.

## 🧠 Фундаментальная идея (из Лекции 15 / Лаба 15)
**Проблема (Пример: GUI интерфейс):** Вы пишете кроссплатформенное приложение. У вас есть Кнопка (`Button`) и Чекбокс (`Checkbox`). Приложение должно работать на Mac и на Windows. Вы создали `MacButton`, `WinButton`, `MacCheckbox`, `WinCheckbox`. 
Проблема: Если клиентский код напишет `new WinButton()` и `new MacCheckbox()`, стили интерфейса перемешаются. Нам нужно гарантировать, что если мы находимся на Windows, создаются **только** Windows-элементы!
**Решение:** Создать `AbstractFactory` — интерфейс, который описывает создание целого *семейства* (возвращает `Button` и `Checkbox`). И создать конкретные фабрики `MacFactory` и `WinFactory`. Клиент получит в руки нужную фабрику при старте программы, и дальше будет просить у нее `createButton()`, не зная, для какой ОС эта кнопка.

## 🛠 Архитектура (Роли)
1. **Abstract Products (Абстрактные продукты):** Семейство базовых интерфейсов для продуктов (`Button`, `Checkbox`).
2. **Concrete Products (Конкретные продукты):** Реализации абстрактных продуктов для конкретной вариации (`WinButton`, `MacButton`).
3. **Abstract Factory (Абстрактная фабрика):** Интерфейс, в котором объявлены методы для создания **всех** абстрактных продуктов из семейства (`createButton()`, `createCheckbox()`).
4. **Concrete Factories (Конкретные фабрики):** Реализуют методы создания продуктов для одной конкретной вариации. `WinFactory` вернет `new WinButton()` и `new WinCheckbox()`.

## 💻 Пошаговая реализация и Примеры

### Пример: Кроссплатформенный UI (Лаба 15)

    // 1. АБСТРАКТНЫЕ ПРОДУКТЫ
    class Button { public: virtual void paint() = 0; };
    class Checkbox { public: virtual void paint() = 0; };

    // 2. КОНКРЕТНЫЕ ПРОДУКТЫ (Семейство Windows)
    class WinButton : public Button {
    public: void paint() override { cout << "Render Win Button\n"; }
    };
    class WinCheckbox : public Checkbox {
    public: void paint() override { cout << "Render Win Checkbox\n"; }
    };

    // КОНКРЕТНЫЕ ПРОДУКТЫ (Семейство Mac)
    class MacButton : public Button {
    public: void paint() override { cout << "Render Mac Button\n"; }
    };
    class MacCheckbox : public Checkbox {
    public: void paint() override { cout << "Render Mac Checkbox\n"; }
    };

    // 3. АБСТРАКТНАЯ ФАБРИКА
    class GUIFactory {
    public:
        virtual Button* createButton() = 0;
        virtual Checkbox* createCheckbox() = 0;
    };

    // 4. КОНКРЕТНЫЕ ФАБРИКИ
    class WinFactory : public GUIFactory {
    public:
        Button* createButton() override { return new WinButton(); }
        Checkbox* createCheckbox() override { return new WinCheckbox(); }
    };

    class MacFactory : public GUIFactory {
    public:
        Button* createButton() override { return new MacButton(); }
        Checkbox* createCheckbox() override { return new MacCheckbox(); }
    };

    // Клиентский код: работает только с абстракциями!
    class Application {
        GUIFactory* factory;
    public:
        Application(GUIFactory* f) : factory(f) {}
        void createUI() {
            Button* b = factory->createButton();
            Checkbox* c = factory->createCheckbox();
            b->paint(); c->paint();
        }
    };

    // Точка входа
    GUIFactory* factory;
    if (OS == "Windows") factory = new WinFactory();
    else factory = new MacFactory();
    
    Application app(factory);
    app.createUI(); // Гарантированно создадутся элементы одного семейства!

## ⚖️ Плюсы и Минусы
**Плюсы:**
* Гарантирует сочетаемость (совместимость) продуктов из одного семейства (мы никогда не получим Mac кнопку рядом с Windows чекбоксом).
* Избавляет клиентский код от жесткой привязки к конкретным классам продуктов.
* **Single Responsibility:** Код инициализации продуктов вынесен в отдельное место.

**Минусы:**
* ОЧЕНЬ много классов. Добавление новой фабрики — легко. Но добавление **нового типа продукта** (например, `RadioButton`) — это кошмар. Придется менять интерфейс `AbstractFactory` и добавлять реализацию во **все** существующие конкретные фабрики (нарушение Open/Closed).

## 🔍 Как отличить от других паттернов
* **Abstract Factory vs Factory Method:**
  * `Factory Method` создает **один** конкретный объект (Использует *Наследование*: класс переопределяет один метод, чтобы вернуть нужный транспорт).
  * `Abstract Factory` создает **семейство** объектов (Использует *Композицию*: клиент получает объект фабрики и дергает у него множество методов). Внутри Абстрактной Фабрики обычно используются Фабричные методы для каждого продукта.
* **Abstract Factory vs Builder:** Фабрика выплевывает объекты пачками мгновенно. Builder строит ОДИН сложный объект шаг за шагом.