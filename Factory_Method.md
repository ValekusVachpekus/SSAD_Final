# Factory Method (Фабричный метод / Виртуальный конструктор)
**Категория:** Creational (Порождающий)

## 📌 Суть (Intent)
Паттерн **Factory Method** предоставляет интерфейс для создания объектов в суперклассе, но позволяет подклассам (Concrete Creators) изменять тип создаваемых продуктов.
*Суть:* Вместо вызова `new Product()` напрямую, мы вызываем метод `createProduct()`.

## 🧠 Фундаментальная идея (из Туториала 12)
**Проблема:** Ваша программа доставки (Логистика) работает только с грузовиками (`Truck`). Весь код намертво завязан на класс `Truck`. Завтра бизнес требует добавить доставку по морю (`Ship`). Вам придется лезть во весь код приложения и писать: `if (type == SEA) new Ship(); else new Truck();`.
**Решение:** Заменить вызовы `new` на вызов специального "Фабричного метода". Создать абстрактный класс `Logistics` с методом `createTransport()`. А в наследниках (`RoadLogistics` и `SeaLogistics`) переопределить этот метод так, чтобы он возвращал `new Truck()` или `new Ship()`. Основная логика программы будет работать через базовый интерфейс `Transport`.

## 🛠 Архитектура (Роли)
1. **Product (Продукт / Интерфейс):** Общий интерфейс всех создаваемых объектов (`Transport` -> метод `deliver()`).
2. **Concrete Products (Конкретные продукты):** Реализации продуктов (`Truck`, `Ship`).
3. **Creator (Создатель):** Абстрактный класс (или класс с дефолтной реализацией). Объявляет фабричный метод, возвращающий `Product`. *Важно:* Основная роль Создателя — это бизнес-логика, использующая продукт, а не только его создание.
4. **Concrete Creators (Конкретные создатели):** Переопределяют фабричный метод, чтобы возвращать конкретные продукты (`RoadLogistics` возвращает `Truck`).

## 💻 Пошаговая реализация и Примеры

### Пример: Логистика (Лаба 12)

    // 1. ИНТЕРФЕЙС ПРОДУКТА
    class ITransport {
    public:
        virtual void deliver() = 0;
        virtual ~ITransport() = default;
    };

    // 2. КОНКРЕТНЫЕ ПРОДУКТЫ
    class Truck : public ITransport {
    public:
        void deliver() override { cout << "Deliver by land in a box.\n"; }
    };

    class Ship : public ITransport {
    public:
        void deliver() override { cout << "Deliver by sea in a container.\n"; }
    };

    // 3. СОЗДАТЕЛЬ (Базовый класс с Фабричным методом)
    class Logistics {
    public:
        // Тот самый фабричный метод!
        virtual ITransport* createTransport() = 0; 
        
        // Базовая бизнес-логика, которая использует абстрактный продукт
        void planDelivery() {
            ITransport* t = createTransport();
            cout << "Logistics: Planning delivery... ";
            t->deliver(); // Полиморфизм в действии
            delete t;
        }
        virtual ~Logistics() = default;
    };

    // 4. КОНКРЕТНЫЕ СОЗДАТЕЛИ
    class RoadLogistics : public Logistics {
    public:
        ITransport* createTransport() override { return new Truck(); }
    };

    class SeaLogistics : public Logistics {
    public:
        ITransport* createTransport() override { return new Ship(); }
    };

    // В main():
    Logistics* logistics = new RoadLogistics();
    logistics->planDelivery(); // Выведет логику Truck
    // Хотим переключить на море?
    Logistics* logistics2 = new SeaLogistics(); // Менять логику внутри Logistics не пришлось!

## ⚖️ Плюсы и Минусы
**Плюсы:**
* Избавляет класс от жесткой привязки к конкретным классам продуктов (DIP — Dependency Inversion).
* **Single Responsibility Principle:** Код создания продуктов вынесен в одно место.
* **Open/Closed Principle:** Можно вводить новые типы продуктов (например, `Plane`), просто создав новый класс продукта и новый класс его создателя (`AirLogistics`), не меняя существующий код клиента.

**Минусы:**
* Может привести к созданию больших параллельных иерархий классов (для каждого продукта нужен свой класс-создатель).

## 🔍 Как отличить от других паттернов
* **Factory Method vs Abstract Factory:** 
  * `Factory Method` создает **один** конкретный объект через переопределение метода (использует наследование).
  * `Abstract Factory` создает **семейства** взаимосвязанных объектов (использует композицию). Абстрактная фабрика часто сама является набором из нескольких Фабричных методов.