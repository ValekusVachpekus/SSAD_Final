# SSAD Final Exam: Ultimate Design Patterns Recap

Этот репозиторий содержит максимально подробные конспекты по всем паттернам проектирования, изученным в курсе Software Systems Analysis and Design (SSAD). Материал скомпилирован из лекций, туториалов и лабораторных работ.

## Что такое Паттерны Проектирования?
Паттерн проектирования — это архитектурная схема (определенная организация классов, объектов и методов), которая предоставляет стандартизированное решение частой проблемы в ООП. 
*Важно:* Паттерн — это не алгоритм. Алгоритм — это набор шагов. Паттерн — это высокоуровневое описание структуры (чертеж).

## Таксономия (Типы паттернов)
1. **Creational (Порождающие):** Отвечают за безопасное и гибкое создание объектов.
2. **Structural (Структурные):** Описывают, как комбинировать классы и объекты в более крупные структуры (деревья, обертки и т.д.).
3. **Behavioral (Поведенческие):** Отвечают за алгоритмы и распределение обязанностей (взаимодействие) между объектами.

## Фундамент: SOLID Принципы
Все паттерны строятся на этих 5 принципах:
* **S - Single Responsibility (Единственная ответственность):** Класс должен иметь только одну причину для изменения (выполнять одну задачу).
* **O - Open/Closed (Открытость/Закрытость):** Программные сущности открыты для расширения (через интерфейсы/наследование), но закрыты для модификации исходного кода.
* **L - Liskov Substitution (Подстановка Барбары Лисков):** Объект базового класса должен безболезненно заменяться объектом производного класса (полиморфизм не должен ломать логику).
* **I - Interface Segregation (Разделение интерфейсов):** Клиенты не должны зависеть от методов, которые они не используют (лучше много мелких интерфейсов, чем один огромный).
* **D - Dependency Inversion (Инверсия зависимостей):** Модули верхних уровней не должны зависеть от нижних. Оба должны зависеть от абстракций.

---

## Навигация по паттернам (по неделям)

### Week 9: Creational & Behavioral Basics
* [Singleton (Одиночка)](./Singleton.md) - *Creational*
* [State (Состояние)](./State.md) - *Behavioral*
* [Prototype (Прототип)](./Prototype.md) - *Creational*
* [Builder (Строитель)](./Builder.md) - *Creational*

### Week 10: Algorithms & Structures
* [Strategy (Стратегия)](./Strategy.md) - *Behavioral*
* [Adapter (Адаптер)](./Adapter.md) - *Structural*
* [Composite (Компоновщик)](./Composite.md) - *Structural*

### Week 11: Wrappers & Controllers
* [Facade (Фасад)](./Facade.md) - *Structural*
* [Decorator (Декоратор)](./Decorator.md) - *Structural*
* [Proxy (Заместитель)](./Proxy.md) - *Structural*

### Week 12: Memory & Decoupling
* [Bridge (Мост)](./Bridge.md) - *Structural*
* [Flyweight (Легковес)](./Flyweight.md) - *Structural*
* [Factory Method (Фабричный метод)](./Factory_Method.md) - *Creational*

### Week 13: Operations & Traversing
* [Interpreter (Интерпретатор)](./Interpreter.md) - *Behavioral*
* [Iterator (Итератор)](./Iterator.md) - *Behavioral*
* [Visitor (Посетитель)](./Visitor.md) - *Behavioral*

### Week 14: Requests & Communication
* [Command (Команда)](./Command.md) - *Behavioral*
* [Chain of Responsibility (Цепочка ответственности)](./Chain_of_Responsibility.md) - *Behavioral*
* [Observer (Наблюдатель)](./Observer.md) - *Behavioral*

### Week 15: State Saving & Architecture
* [Template Method (Шаблонный метод)](./Template_Method.md) - *Behavioral*
* [Mediator (Посредник)](./Mediator.md) - *Behavioral*
* [Memento (Снимок)](./Memento.md) - *Behavioral*
* [Abstract Factory (Абстрактная фабрика)](./Abstract_Factory.md) - *Creational*