# Builder (Строитель)
**Категория:** Creational (Порождающий)

## 📌 Суть (Intent)
Позволяет создавать сложные объекты **шаг за шагом**. Позволяет использовать один и тот же код строительства для получения разных представлений (вариаций) объекта.

## 🛠 Проблема и Решение
**Проблема:** "Телескопический конструктор" (Telescoping constructor). Когда у класса десятки параметров (многие из которых опциональны), приходится создавать кучу перегрузок конструктора: `Pizza()`, `Pizza(cheese)`, `Pizza(cheese, pepperoni)` и т.д.
**Решение:** Вынести конструирование объекта в отдельный класс `Builder`. Строительство идет поэтапно (`buildStepA()`, `buildStepB()`), а в конце вызывается `getResult()`.

### Архитектура (Роли)
1. **Builder (Интерфейс):** Описывает общие шаги (`buildCPU`, `buildRAM`).
2. **ConcreteBuilder:** Реализует шаги для конкретной вариации (например, `GamingPCBuilder`).
3. **Product:** Сам создаваемый объект (`PC` или `Document`).
4. **Director (Опционально):** Класс, который знает *порядок* вызова шагов Builder'а.

### Пример кода (Лаба 9: Сборка ПК / Document Editor)
```cpp
class PC {
    std::string m_cpu, m_ram;
public:
    void setCPU(std::string cpu) { m_cpu = cpu; }
    void setRAM(std::string ram) { m_ram = ram; }
};

class PCBuilder { // Интерфейс
public:
    virtual void buildCPU() = 0;
    virtual void buildRAM() = 0;
    virtual PC getResult() = 0;
};

class GamingPCBuilder : public PCBuilder {
    PC m_pc;
public:
    void buildCPU() override { m_pc.setCPU("Intel i9"); }
    void buildRAM() override { m_pc.setRAM("32GB DDR5"); }
    PC getResult() override { return m_pc; }
};

class Director {
    PCBuilder* m_builder;
public:
    void setBuilder(PCBuilder* builder) { m_builder = builder; }
    
    // Директор задает ПОРЯДОК шагов
    PC construct() {
        m_builder->buildCPU();
        m_builder->buildRAM();
        return m_builder->getResult();
    }
};
```

## ⚖️ Плюсы и Минусы
**Плюсы:**
* Позволяет создавать объекты пошагово (в том числе рекурсивно).
* Переиспользование одного и того же кода Директора для разных Билдеров.
* **Single Responsibility:** Изолирует сложный код сборки от бизнес-логики самого класса.
* Обеспечивает **Иммутабельность (Immutability):** объект можно сделать полностью готовым и неизменяемым после вызова `getResult()`.

**Минусы:**
* Увеличивает общую сложность кода (требуется создавать много дополнительных классов).

## 🔍 Как отличить от других паттернов
* **Builder vs Abstract Factory:** 
  * `Builder` фокусируется на создании **одного сложного объекта** шаг за шагом. Возвращает продукт только на последнем шаге.
  * `Abstract Factory` фокусируется на создании **семейств связанных объектов** (кнопки, чекбоксы). Возвращает продукты немедленно по одному.