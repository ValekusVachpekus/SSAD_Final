# State (Состояние)
**Категория:** Behavioral (Поведенческий)

## 📌 Суть (Intent)
Позволяет объекту **изменять свое поведение** при изменении внутреннего состояния. Со стороны кажется, будто объект изменил свой класс. Основан на концепции **Конечного Автомата (Finite State Machine)**.

## 🛠 Проблема и Решение
**Проблема:** Класс имеет огромное количество условных операторов (`if (state == SOLID) else if (state == LIQUID)`). При добавлении нового состояния придется переписывать все методы класса.
**Решение:** 
Выделить каждое состояние в отдельный класс. Контекст (сам объект) хранит ссылку на текущее состояние и делегирует ему работу.

### Пример кода (Лекция: Агрегатные состояния воды / Лаба 9: Document Editor)
В Лабе 9 был редактор документов, который мог быть в состояниях `Draft`, `Review`, `Published`.
```cpp
class State {
public:
    virtual void review() = 0;
    virtual ~State() = default;
};

class Draft : public State {
public:
    void review() override { std::cout << "Draft: Reviewing changes\n"; }
};

class Published : public State {
public:
    void review() override { std::cout << "Published: Review not allowed\n"; }
};

class Document {
private:
    std::unique_ptr<State> state; // Контекст хранит текущее состояние
public:
    Document(std::unique_ptr<State> initialState) : state(std::move(initialState)) {}
    
    void setState(std::unique_ptr<State> newState) {
        state = std::move(newState);
    }
    
    void review() {
        state->review(); // Делегирование! Нет никаких if/else
    }
};
```

## ⚖️ Плюсы и Минусы
**Плюсы:**
* Избавляет от огромных блоков `switch/case` или `if/else`.
* **Single Responsibility:** Код каждого состояния изолирован в своем классе.
* **Open/Closed:** Легко добавить новые состояния (создаем новый класс), не меняя старый код.

**Минусы:**
* Если состояний мало (2-3) и они редко меняются, паттерн может быть избыточным (Overkill).

## 🔍 Как отличить от других паттернов
* **State vs Strategy:** Структурно они идентичны (UML диаграммы почти совпадают). **Разница в Intent (намерении):**
  * В **Strategy** клиент сам выбирает, какую стратегию задать объекту (например, метод сортировки). Стратегии обычно не знают друг о друге и редко меняются в процессе работы.
  * В **State** контекст или сами состояния инициируют **переходы (transitions)** из одного состояния в другое (Draft -> Review). Состояния знают друг о друге.