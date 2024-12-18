# Лабораторная работа 3
## Диаграмма контейнеров
![](C4%20-%20компонент.png)
## Диаграмма компонентов
![Ведущий контроллер](С4%20-%20компоненты%20ведущий.png)
![Ведомый контроллер](С4%20-%20компоненты%20ведомый.png)

## Диаграмма последовательностей
![Диаграмма последовательностей](Диаграмма%20последовательности.png)
## Модель БД
![Модель БД](UML%20диаграмма.png)
## Применение основных принципов разработки
### KISS
Было
```
superMegaAmazingPrintFunction("hello");

int AddOneToNumber(int x) {
    return x + 1;
}
```
Стало
```
print("hello");

int Inc(int x) {
    return x + 1;
}
```
### YAGNI
Было
```
void print(object o) {
    Console.Write(o);
}

void printLine(object o) {
    Console.WriteLine(o);
}

void printTab() {
    Console.WriteLine("\t");
}

void Main() {
    print("Hello world!");
}
```
Стало
```
void print(object o) {
    Console.Write(o);
}

void Main() {
    print("Hello world!");
}
```
### DRY
Было
```
int x = 0;

for (int i = 0; i < 10; i++) {
    x += i;
}

print(x);
x = 0;

for (int i = 0; i < 20; i++) {
    x += i;
}

print(x);
x = 0;

for (int i = 0; i < 30; i++) {
    x += i;
}

print(x);
```
Стало
```
int Fibonachi(int x) {
    int res = 0;
    for (int i = 0; i < x; i++) {
        res += i;
    }
    return res;
}

print(Fibonachi(10));
print(Fibonachi(20));
print(Fibonachi(30));
```
### SOLID
#### S: Single Responsibility Principle
Было
```
class Command {
    void Execute() { }
    void Load() { }
    void Save() { }
}
```
Стало
```
class Command {
    void Execute() { }
}

class CommandDB {
    void Load() { }
    void Save() { }
}
```
Вынесли функции загрузки/сохранения в отдельный класс
#### S: Open-Closed Principle
Было
```
class Pokemon {
    Pokemon(string kind) { this.kind = kind; }
}

Pokemon[] pokemons = {
    new Pokemon("Fire");
    new Pokemon("Water");
    new Pokemon("Electric");
}

void NamePokemonsWeaknesses(Pokemon[] pokemons) {
    foreach (var pokemon in pokemons) {
        switch (pokemon.kind) {
            "Fire": print("Water"); break;
            "Water": print("Electric"); break;
            "Electric": print("Ground"); break;
        }
    }
}
```
Стало
```
abstract class Pokemon {
    abstract SayWeaknesses();
}

class FirePokemon : Pokemon {
    SayWeaknesses() {
        print("Water");
    }
}

class WaterPokemon : Pokemon {
    SayWeaknesses() {
        print("Electric");
    }
}

class ElectricPokemon : Pokemon {
    SayWeaknesses() {
        print("Ground");
    }
}

Pokemon[] pokemons = {
    new FirePokemon();
    new WaterPokemon();
    new ElectricPokemon();
}

void NamePokemonsWeaknesses(Pokemon[] pokemons) {
    foreach (var pokemon in pokemons) {
        pokemon.SayWeaknesses();
    }
}
```
#### S: Liskov Substitution Principle
Было
```
abstract class Pokemon {
    abstract SayWeaknesses();
}

class FirePokemon : Pokemon {
    SayWeaknesses() {
        print("Water");
    }
}

class WaterPokemon : Pokemon {
    SayWeaknesses() {
        print("Electric");
    }
}

class ElectricPokemon : Pokemon {
    SayWeaknesses() {
        print("Ground");
    }
}

Pokemon[] pokemons = {
    new FirePokemon();
    new WaterPokemon();
    new ElectricPokemon();
}

void NamePokemonsFriends(Pokemon[] pokemons) {
    foreach (var pokemon in pokemons) {
        if (typeof(pokemon) == FirePokemon) {
            NameFireFriends(pokemon);
        } else if (typeof(pokemon) == WaterPokemon) {
            NameWaterFriends(pokemon);
        } else if (typeof(pokemon) == ElectricPokemon) {
            NameElectricFriends(pokemon);
        }
    }
}
```
Стало
```
abstract class Pokemon {
    abstract SayWeaknesses();
    abstract NameFriends();
}

class FirePokemon : Pokemon {
    SayWeaknesses() {
        print("Water");
    }

    NameFriends() { }
}

class WaterPokemon : Pokemon {
    SayWeaknesses() {
        print("Electric");
    }

    NameFriends() { }
}

class ElectricPokemon : Pokemon {
    SayWeaknesses() {
        print("Ground");
    }

    NameFriends() { }
}

Pokemon[] pokemons = {
    new FirePokemon();
    new WaterPokemon();
    new ElectricPokemon();
}

void NamePokemonsWeaknesses(Pokemon[] pokemons) {
    foreach (var pokemon in pokemons) {
        pokemon.NameFriends();
    }
}
```
#### S: Interface Segregation Principle
Было
```
interface IFood {
    void FireFood();
    void WaterFood();
    void ElectricFood();
}

abstract class Pokemon {
    abstract SayWeaknesses();
}

class FirePokemon : Pokemon, IFood {
    SayWeaknesses() {
        print("Water");
    }

    void FireFood() { }
    void WaterFood() { }
    void ElectricFood() { }
}

class WaterPokemon : Pokemon, IFood {
    SayWeaknesses() {
        print("Electric");
    }

    void FireFood() { }
    void WaterFood() { }
    void ElectricFood() { }
}

class ElectricPokemon : Pokemon, IFood {
    SayWeaknesses() {
        print("Ground");
    }

    void FireFood() { }
    void WaterFood() { }
    void ElectricFood() { }
}
```
Стало
```
interface IFireFood {
    void FireFood();
}

interface IWaterFood {
    void WaterFood();
}

interface IElectricFood {
    void ElectricFood();
}

abstract class Pokemon {
    abstract SayWeaknesses();
}

class FirePokemon : Pokemon, IFireFood {
    SayWeaknesses() {
        print("Water");
    }

    void FireFood() { }
}

class WaterPokemon : Pokemon, IWaterFood {
    SayWeaknesses() {
        print("Electric");
    }

    void WaterFood() { }
}

class ElectricPokemon : Pokemon, IElectricFood {
    SayWeaknesses() {
        print("Ground");
    }

    void ElectricFood() { }
}
```
#### S: Dependency Inversion Principle
Было
```
class Stolovaya {
    void Obsluzhit(Kassir kassir) { }
    void Obsluzhit(Pomochnik kassir) { }
    void Obsluzhit(ChelSUlitsy kassir) { }
}
```
Стало
```
interface IKassir {
    ...
}

class Stolovaya {
    void Obsluzhit(IKassir kassir) { }
}
```