# Iterator (Итератор / Курсор)
**Категория:** Behavioral (Поведенческий)

## 📌 Суть (Intent)
Паттерн **Iterator** позволяет последовательно обходить элементы сложных коллекций (агрегатов: списков, деревьев, графов) **не раскрывая их внутреннего представления** (массив это, связный список или хэш-таблица).

## 🧠 Фундаментальная идея (из Лекции 13)
**Проблема:** У вас есть коллекция (например, `Composite` из прошлой лекции или бинарное дерево). Вы хотите обойти все её элементы. Если это обычный массив, вы напишете `for (int i=0) arr[i]`. Но как обходить дерево? В глубину (DFS) или в ширину (BFS)? Если логику обхода встроить прямо в дерево (методы `tree.first()`, `tree.next()`), то класс дерева раздуется, а главное — нельзя будет запустить два параллельных обхода одного и того же дерева (так как указатель на текущий элемент будет один на весь класс дерева).

**Решение:** Вынести логику обхода в отдельный класс `Iterator`. Итератор будет хранить состояние обхода (указатель на текущий элемент). Коллекция будет иметь только один метод: `createIterator()`, который возвращает объект итератора.

## 🛠 Архитектура (Роли)
1. **Iterator (Интерфейс):** Определяет методы для обхода: `first()`, `next()`, `isDone()`, `currentItem()`.
2. **Concrete Iterator (Конкретный итератор):** Реализует обход конкретной коллекции (например, `DFSTreeIterator`). Хранит внутри себя состояние (указатель на индекс или стек для обхода графа).
3. **Iterable Collection / Aggregate (Интерфейс):** Интерфейс коллекции, описывающий метод создания итератора.
4. **Concrete Collection (Конкретная коллекция):** Возвращает объект конкретного итератора, привязанного к ней (`return new DFSTreeIterator(this)`).

## 💻 Пошаговая реализация и Примеры

### Пример: Обход связного графа/дерева (Лаба 13)
В C++ STL итераторы реализованы через перегрузку операторов `++` (вместо `next()`), `*` (вместо `currentItem()`) и `!=` (вместо `isDone()`). Но концепция остается той же.

    // 1. Интерфейс Итератора
    template <typename T>
    class Iterator {
    public:
        virtual void first() = 0;
        virtual void next() = 0;
        virtual bool isDone() = 0;
        virtual T currentItem() = 0;
    };

    // 2. Коллекция
    class ListCollection {
        std::vector<int> data; // Внутреннее устройство скрыто
        friend class ListIterator; // Итератор должен иметь доступ к данным
    public:
        ListCollection(std::vector<int> d) : data(d) {}
        Iterator<int>* createIterator();
    };

    // 3. Конкретный итератор
    class ListIterator : public Iterator<int> {
        ListCollection* collection;
        int currentPosition;
    public:
        ListIterator(ListCollection* c) : collection(c), currentPosition(0) {}
        
        void first() override { currentPosition = 0; }
        void next() override { currentPosition++; }
        bool isDone() override { return currentPosition >= collection->data.size(); }
        int currentItem() override { return collection->data[currentPosition]; }
    };

    Iterator<int>* ListCollection::createIterator() {
        return new ListIterator(this); // Возвращаем Итератор
    }

    // В main():
    ListCollection list({1, 2, 3});
    Iterator<int>* it = list.createIterator();
    for (it->first(); !it->isDone(); it->next()) {
        cout << it->currentItem() << endl; // Клиенту не важно, vector там или что-то еще!
    }

## ⚖️ Плюсы и Минусы
**Плюсы:**
* **Single Responsibility Principle:** Класс коллекции хранит данные, а класс итератора — логику обхода.
* **Open/Closed Principle:** Можно добавлять новые виды итераторов (в обратном порядке, по четным элементам), не меняя код самой коллекции.
* Можно итерировать одну и ту же коллекцию параллельно из разных потоков (каждый итератор хранит своё состояние `currentPosition`).

**Минусы:**
* Применять итераторы для простых коллекций (вроде `std::vector`) иногда менее эффективно, чем прямой доступ по индексу.

## 🔍 Как отличить от других паттернов
* Итератор в C++ это основа цикла `for(auto x : arr)`. Если вы видите перегрузку `operator++` и `operator*` в контексте обхода — это 100% реализация паттерна Iterator.