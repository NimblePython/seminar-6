# Задача на полиморфизмом, виртуальные методы и различные виды приведения типов в C++, включая `static_cast` и `dynamic_cast`

## Задача: "Иерархия персонажей в игре"

Представьте себе иерархию персонажей в ролевой игре. Все персонажи делятся на классы: Герой, Монстр, и разные их подтипы. Задача заключается в создании базовой иерархии классов и применении `static_cast` и `dynamic_cast` для работы с этими классами.

## Условие задачи

1. Создайте базовый класс `Character` (персонаж), в котором будут общие характеристики для всех персонажей (например, имя и уровень здоровья).
2. От класса `Character` унаследуйте два класса: `Hero` (Герой) и `Monster` (Монстр).
3. От класса `Hero` создайте два класса: `Warrior` (Воин) и `Mage` (Маг).
4. От класса `Monster` создайте два класса: `Goblin` (Гоблин) и `Dragon` (Дракон).

## Функционал

- В базовом классе `Character` реализуйте виртуальные методы для атаки и защиты, которые будут переопределены в наследниках.
- В классе `Hero` добавьте метод для исцеления.
- В классе `Monster` добавьте метод для использования особой атаки.
- Создайте функцию, которая получает указатель на объект класса `Character` и пытается преобразовать его с использованием `dynamic_cast` в указатели на разные классы-наследники (например, проверить, является ли это Герой, Воин или Маг, а также Монстр, Гоблин или Дракон).
- Реализуйте сценарий, в котором `static_cast` используется для приведения указателей в случаях, когда тип объекта известен на этапе компиляции, а `dynamic_cast` для безопасного приведения с проверкой типов во время выполнения.

## Пример кода

```cpp
#include <iostream>
#include <string>
#include <typeinfo>

class Character {
public:
    virtual ~Character() = default;
    virtual void attack() = 0;
    virtual void defend() = 0;
    std::string name;
    int health;

    Character(const std::string& name, int health) : name(name), health(health) {}
};

class Hero : public Character {
public:
    Hero(const std::string& name, int health) : Character(name, health) {}
    void heal() {
        std::cout << name << " исцеляет себя!" << std::endl;
    }
};

class Warrior : public Hero {
public:
    Warrior(const std::string& name, int health) : Hero(name, health) {}

    void attack() override {
        std::cout << name << " атакует мечом!" << std::endl;
    }

    void defend() override {
        std::cout << name << " блокирует щитом!" << std::endl;
    }
};

class Mage : public Hero {
public:
    Mage(const std::string& name, int health) : Hero(name, health) {}

    void attack() override {
        std::cout << name << " атакует огненным шаром!" << std::endl;
    }

    void defend() override {
        std::cout << name << " использует магический барьер!" << std::endl;
    }
};

class Monster : public Character {
public:
    Monster(const std::string& name, int health) : Character(name, health) {}
    void specialAttack() {
        std::cout << name << " использует особую атаку!" << std::endl;
    }
};

class Goblin : public Monster {
public:
    Goblin(const std::string& name, int health) : Monster(name, health) {}

    void attack() override {
        std::cout << name << " атакует кинжалом!" << std::endl;
    }

    void defend() override {
        std::cout << name << " уклоняется!" << std::endl;
    }
};

class Dragon : public Monster {
public:
    Dragon(const std::string& name, int health) : Monster(name, health) {}

    void attack() override {
        std::cout << name << " выпускает огонь!" << std::endl;
    }

    void defend() override {
        std::cout << name << " использует чешую как броню!" << std::endl;
    }
};

void identifyCharacter(Character* character) {
    if (Hero* hero = dynamic_cast<Hero*>(character)) {
        std::cout << character->name << " — это герой." << std::endl;
        if (Warrior* warrior = dynamic_cast<Warrior*>(hero)) {
            std::cout << character->name << " — это воин." << std::endl;
        } else if (Mage* mage = dynamic_cast<Mage*>(hero)) {
            std::cout << character->name << " — это маг." << std::endl;
        }
    } else if (Monster* monster = dynamic_cast<Monster*>(character)) {
        std::cout << character->name << " — это монстр." << std::endl;
        if (Goblin* goblin = dynamic_cast<Goblin*>(monster)) {
            std::cout << character->name << " — это гоблин." << std::endl;
        } else if (Dragon* dragon = dynamic_cast<Dragon*>(monster)) {
            std::cout << character->name << " — это дракон." << std::endl;
        }
    } else {
        std::cout << character->name << " — это неизвестный тип персонажа." << std::endl;
    }
}

int main() {
    Warrior warrior("Алексей", 100);
    Mage mage("Моргана", 80);
    Goblin goblin("Гракс", 50);
    Dragon dragon("Смауг", 200);

    Character* characters[] = { &warrior, &mage, &goblin, &dragon };

    for (Character* character : characters) {
        identifyCharacter(character);
    }

    // Пример использования static_cast
    Hero* hero = static_cast<Hero*>(&warrior);
    hero->heal();

    return 0;
}
```

## Задания

1. Дополните иерархию новыми типами персонажей Elf и Troll.
2. Проведите эксперименты с `static_cast` и `dynamic_cast`, добавьте случаи с неправильным приведением типов.
3. Объясните, когда лучше использовать `static_cast`, а когда — `dynamic_cast`, с учетом производительности и безопасности.
