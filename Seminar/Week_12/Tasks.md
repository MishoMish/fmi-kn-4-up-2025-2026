# **ЗАДАЧИ - СЕДМИЦА 12**

## **Структури, Класове и Enums - Основни задачи**

---

## **ЗАДАЧА 0: Warming Up - Какво ще се изведе?**

Без да пускате програмата, определете какво ще се изведе:

```cpp
struct Point {
    int x;
    int y;
};

int main() {
    Point p1 = {10, 20};
    Point p2 = p1;

    p2.x = 30;

    std::cout << p1.x << " " << p2.x << std::endl;

    return 0;
}
```

<details>
<summary><b>📝 Отговор</b></summary>

```
10 30
```

**Обяснение:**

- `p2 = p1` копира стойностите на p1 в p2
- Промяната на `p2.x` НЕ засяга `p1.x`
- p1 и p2 са отделни променливи в паметта

</details>

---

## **ЗАДАЧА 1: Структура Point**

Създайте структура `Point` с координати `x` и `y`. Напишете функция `distance`, която изчислява разстоянието между две точки.

**Формула:** $ d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2} $

```cpp
// Входни данни:
Point p1 = {0, 0};
Point p2 = {3, 4};

// Изход:
5.0
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

struct Point {
    double x;
    double y;
};

double distance(const Point& p1, const Point& p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx * dx + dy * dy);
}

int main() {
    Point p1 = {0, 0};
    Point p2 = {3, 4};

    std::cout << "Distance: " << distance(p1, p2) << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 2: Структура Student**

Създайте структура `Student` с полета:

- `name` (име) - char масив
- `fn` (факултетен номер) - int
- `grade` (среден успех) - double

Напишете функция `printStudent`, която извежда информацията форматирано.

```cpp
// Вход:
Student s = {"Ivan Petrov", 12345, 5.75};

// Изход:
Name: Ivan Petrov
FN: 12345
Grade: 5.75
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

struct Student {
    char name[100];
    int fn;
    double grade;
};

void printStudent(const Student& s) {
    std::cout << "Name: " << s.name << std::endl;
    std::cout << "FN: " << s.fn << std::endl;
    std::cout << "Grade: " << s.grade << std::endl;
}

int main() {
    Student s;
    strcpy(s.name, "Ivan Petrov");
    s.fn = 12345;
    s.grade = 5.75;

    printStudent(s);

    return 0;
}
```

</details>

---

## **ЗАДАЧА 3: Структура Rectangle**

Създайте структура `Rectangle` с полета `width` и `height`. Напишете функции:

- `area()` - изчислява лицето
- `perimeter()` - изчислява периметъра

```cpp
// Вход:
Rectangle r = {5, 3};

// Изход:
Area: 15
Perimeter: 16
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

struct Rectangle {
    int width;
    int height;
};

int area(const Rectangle& r) {
    return r.width * r.height;
}

int perimeter(const Rectangle& r) {
    return 2 * (r.width + r.height);
}

int main() {
    Rectangle r = {5, 3};

    std::cout << "Area: " << area(r) << std::endl;
    std::cout << "Perimeter: " << perimeter(r) << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 4: Масив от структури**

Напишете програма, която работи с масив от студенти. Намерете студента с най-висок среден успех.

```cpp
// Вход:
Student students[3] = {
    {"Alice", 101, 5.50},
    {"Bob", 102, 6.00},
    {"Charlie", 103, 5.75}
};

// Изход:
Best student: Bob (6.00)
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

struct Student {
    char name[100];
    int fn;
    double grade;
};

int findBestStudent(const Student students[], int size) {
    int bestIndex = 0;

    for (int i = 1; i < size; i++) {
        if (students[i].grade > students[bestIndex].grade) {
            bestIndex = i;
        }
    }

    return bestIndex;
}

int main() {
    Student students[3] = {
        {"Alice", 101, 5.50},
        {"Bob", 102, 6.00},
        {"Charlie", 103, 5.75}
    };

    int bestIdx = findBestStudent(students, 3);

    std::cout << "Best student: " << students[bestIdx].name
              << " (" << students[bestIdx].grade << ")" << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 5: Влагане на структури**

Създайте структура `Date` с полета `day`, `month`, `year`. Създайте структура `Person` с име и рожденна дата (Date). Напишете функция, която изчислява възрастта.

```cpp
// Вход:
Person p = {"Ivan", {15, 6, 2000}};
Date today = {18, 12, 2025};

// Изход:
Age: 25
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

struct Date {
    int day;
    int month;
    int year;
};

struct Person {
    char name[100];
    Date birthDate;
};

int calculateAge(const Person& p, const Date& today) {
    int age = today.year - p.birthDate.year;

    // Ако рожденният ден още не е минал тази година
    if (today.month < p.birthDate.month ||
        (today.month == p.birthDate.month && today.day < p.birthDate.day)) {
        age--;
    }

    return age;
}

int main() {
    Person p;
    strcpy(p.name, "Ivan");
    p.birthDate = {15, 6, 2000};

    Date today = {18, 12, 2025};

    std::cout << "Age: " << calculateAge(p, today) << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 6: Клас Counter**

Създайте клас `Counter` с private член `count`. Имплементирайте методи:

- `increment()` - увеличава брояча
- `decrement()` - намалява брояча
- `getValue()` - връща текущата стойност

```cpp
// Използване:
Counter c;
c.increment();
c.increment();
c.decrement();
std::cout << c.getValue() << std::endl;  // 1
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class Counter {
private:
    int count;

public:
    Counter() {
        count = 0;
    }

    void increment() {
        count++;
    }

    void decrement() {
        count--;
    }

    int getValue() const {
        return count;
    }
};

int main() {
    Counter c;
    c.increment();
    c.increment();
    c.decrement();

    std::cout << c.getValue() << std::endl;  // 1

    return 0;
}
```

</details>

---

## **ЗАДАЧА 7: Клас BankAccount**

Създайте клас `BankAccount` с:

- private член `balance`
- методи `deposit()`, `withdraw()`, `getBalance()`
- валидация при теглене (не може да се тегли повече от наличното)

```cpp
// Използване:
BankAccount acc;
acc.deposit(1000);
acc.withdraw(300);
std::cout << acc.getBalance() << std::endl;  // 700
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class BankAccount {
private:
    double balance;

public:
    BankAccount() {
        balance = 0;
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }

    double getBalance() const {
        return balance;
    }
};

int main() {
    BankAccount acc;
    acc.deposit(1000);

    if (acc.withdraw(300)) {
        std::cout << "Withdrawal successful" << std::endl;
    }

    std::cout << "Balance: " << acc.getBalance() << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 8: Клас с конструктор и деструктор**

Създайте клас `DynamicArray`, който заделя динамична памет в конструктора и я освобождава в деструктора.

```cpp
// Използване:
DynamicArray arr(10);
arr.set(0, 42);
std::cout << arr.get(0) << std::endl;  // 42
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class DynamicArray {
private:
    int* data;
    int size;

public:
    // Конструктор
    DynamicArray(int n) {
        size = n;
        data = new int[size];

        // Инициализация с 0
        for (int i = 0; i < size; i++) {
            data[i] = 0;
        }

        std::cout << "Array created with size " << size << std::endl;
    }

    // Деструктор
    ~DynamicArray() {
        delete[] data;
        std::cout << "Array destroyed" << std::endl;
    }

    void set(int index, int value) {
        if (index >= 0 && index < size) {
            data[index] = value;
        }
    }

    int get(int index) const {
        if (index >= 0 && index < size) {
            return data[index];
        }
        return -1;  // Грешка
    }

    int getSize() const {
        return size;
    }
};

int main() {
    DynamicArray arr(10);
    arr.set(0, 42);
    arr.set(5, 100);

    std::cout << arr.get(0) << std::endl;  // 42
    std::cout << arr.get(5) << std::endl;  // 100

    return 0;
}  // Деструкторът се извиква автоматично тук
```

</details>

---

## **ЗАДАЧА 9: Enum Days**

Създайте enum class `Weekday` с дните от седмицата. Напишете функция `isWeekend`, която проверява дали денят е почивен.

```cpp
// Използване:
Weekday today = Weekday::Saturday;
if (isWeekend(today)) {
    std::cout << "It's weekend!" << std::endl;
}
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

enum class Weekday {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};

bool isWeekend(Weekday day) {
    return day == Weekday::Saturday || day == Weekday::Sunday;
}

const char* weekdayToString(Weekday day) {
    switch (day) {
        case Weekday::Monday: return "Monday";
        case Weekday::Tuesday: return "Tuesday";
        case Weekday::Wednesday: return "Wednesday";
        case Weekday::Thursday: return "Thursday";
        case Weekday::Friday: return "Friday";
        case Weekday::Saturday: return "Saturday";
        case Weekday::Sunday: return "Sunday";
        default: return "Unknown";
    }
}

int main() {
    Weekday today = Weekday::Saturday;

    std::cout << "Today is " << weekdayToString(today) << std::endl;

    if (isWeekend(today)) {
        std::cout << "It's weekend!" << std::endl;
    } else {
        std::cout << "It's a workday" << std::endl;
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 10: Enum в структура**

Създайте enum class `Color` и структура `Car` с цвят, модел и година. Напишете функция, която сортира масив от коли по година.

```cpp
// Вход:
Car cars[3] = {
    {Color::Red, "BMW", 2020},
    {Color::Blue, "Audi", 2018},
    {Color::Black, "Mercedes", 2022}
};

// Изход (след сортиране):
Audi 2018
BMW 2020
Mercedes 2022
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

enum class Color {
    Red,
    Blue,
    Black,
    White,
    Silver
};

struct Car {
    Color color;
    char model[50];
    int year;
};

void sortCarsByYear(Car cars[], int size) {
    // Bubble sort
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (cars[j].year > cars[j + 1].year) {
                // Размяна
                Car temp = cars[j];
                cars[j] = cars[j + 1];
                cars[j + 1] = temp;
            }
        }
    }
}

void printCars(const Car cars[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << cars[i].model << " " << cars[i].year << std::endl;
    }
}

int main() {
    Car cars[3];

    cars[0].color = Color::Red;
    strcpy(cars[0].model, "BMW");
    cars[0].year = 2020;

    cars[1].color = Color::Blue;
    strcpy(cars[1].model, "Audi");
    cars[1].year = 2018;

    cars[2].color = Color::Black;
    strcpy(cars[2].model, "Mercedes");
    cars[2].year = 2022;

    std::cout << "Before sorting:" << std::endl;
    printCars(cars, 3);

    sortCarsByYear(cars, 3);

    std::cout << "\nAfter sorting:" << std::endl;
    printCars(cars, 3);

    return 0;
}
```

</details>

---

## **ЗАДАЧА 11: Клас Point с методи**

Създайте клас `Point` с методи:

- `translate(int dx, int dy)` - премества точката
- `distanceTo(const Point& other)` - разстояние до друга точка
- `display()` - извежда координатите

```cpp
// Използване:
Point p1(0, 0);
Point p2(3, 4);
p1.translate(1, 1);
std::cout << p1.distanceTo(p2) << std::endl;
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

class Point {
private:
    double x;
    double y;

public:
    Point(double xVal = 0, double yVal = 0) : x(xVal), y(yVal) {}

    void translate(double dx, double dy) {
        x += dx;
        y += dy;
    }

    double distanceTo(const Point& other) const {
        double dx = other.x - x;
        double dy = other.y - y;
        return sqrt(dx * dx + dy * dy);
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")" << std::endl;
    }

    double getX() const { return x; }
    double getY() const { return y; }
};

int main() {
    Point p1(0, 0);
    Point p2(3, 4);

    std::cout << "p1: ";
    p1.display();

    p1.translate(1, 1);

    std::cout << "p1 after translation: ";
    p1.display();

    std::cout << "Distance: " << p1.distanceTo(p2) << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 12: Структура Complex**

Създайте структура `Complex` за комплексни числа с real и imaginary част. Напишете функции за:

- Събиране на комплексни числа
- Изваждане на комплексни числа
- Умножение на комплексни числа

**Формули:**

- $(a + bi) + (c + di) = (a + c) + (b + d)i$
- $(a + bi) - (c + di) = (a - c) + (b - d)i$
- $(a + bi) \times (c + di) = (ac - bd) + (ad + bc)i$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

struct Complex {
    double real;
    double imag;
};

Complex add(const Complex& c1, const Complex& c2) {
    Complex result;
    result.real = c1.real + c2.real;
    result.imag = c1.imag + c2.imag;
    return result;
}

Complex subtract(const Complex& c1, const Complex& c2) {
    Complex result;
    result.real = c1.real - c2.real;
    result.imag = c1.imag - c2.imag;
    return result;
}

Complex multiply(const Complex& c1, const Complex& c2) {
    Complex result;
    result.real = c1.real * c2.real - c1.imag * c2.imag;
    result.imag = c1.real * c2.imag + c1.imag * c2.real;
    return result;
}

void printComplex(const Complex& c) {
    if (c.imag >= 0) {
        std::cout << c.real << " + " << c.imag << "i" << std::endl;
    } else {
        std::cout << c.real << " - " << -c.imag << "i" << std::endl;
    }
}

int main() {
    Complex c1 = {3, 2};   // 3 + 2i
    Complex c2 = {1, 7};   // 1 + 7i

    Complex sum = add(c1, c2);
    Complex diff = subtract(c1, c2);
    Complex prod = multiply(c1, c2);

    std::cout << "c1 = ";
    printComplex(c1);

    std::cout << "c2 = ";
    printComplex(c2);

    std::cout << "Sum = ";
    printComplex(sum);

    std::cout << "Difference = ";
    printComplex(diff);

    std::cout << "Product = ";
    printComplex(prod);

    return 0;
}
```

</details>

---

## **ЗАДАЧА 13: Клас String (опростен)**

Създайте клас `MyString`, който съхранява динамично заделен символен масив. Имплементирайте:

- Конструктор, който заделя памет
- Деструктор, който освобождава паметта
- Метод `length()` - връща дължината
- Метод `print()` - извежда стринга

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

class MyString {
private:
    char* data;
    int len;

public:
    MyString(const char* str) {
        len = strlen(str);
        data = new char[len + 1];
        strcpy(data, str);
    }

    ~MyString() {
        delete[] data;
    }

    int length() const {
        return len;
    }

    void print() const {
        std::cout << data << std::endl;
    }

    const char* getData() const {
        return data;
    }
};

int main() {
    MyString s("Hello, World!");

    s.print();
    std::cout << "Length: " << s.length() << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 14: Enum с switch**

Създайте enum class `TrafficLight` с Red, Yellow, Green. Напишете функция, която връща следващото състояние на светофара.

```cpp
// Red → Yellow → Green → Red ...
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

enum class TrafficLight {
    Red,
    Yellow,
    Green
};

TrafficLight nextState(TrafficLight current) {
    switch (current) {
        case TrafficLight::Red:
            return TrafficLight::Yellow;
        case TrafficLight::Yellow:
            return TrafficLight::Green;
        case TrafficLight::Green:
            return TrafficLight::Red;
        default:
            return TrafficLight::Red;
    }
}

const char* stateToString(TrafficLight state) {
    switch (state) {
        case TrafficLight::Red: return "Red";
        case TrafficLight::Yellow: return "Yellow";
        case TrafficLight::Green: return "Green";
        default: return "Unknown";
    }
}

int main() {
    TrafficLight light = TrafficLight::Red;

    for (int i = 0; i < 10; i++) {
        std::cout << stateToString(light) << std::endl;
        light = nextState(light);
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 15: Клас Rectangle с енкапсулация**

Създайте клас `Rectangle` с private полета width и height. Имплементирайте:

- Getters и setters с валидация (ширина и височина > 0)
- Методи за area() и perimeter()
- Метод isSquare() - проверява дали е квадрат

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class Rectangle {
private:
    int width;
    int height;

public:
    Rectangle(int w = 1, int h = 1) {
        setWidth(w);
        setHeight(h);
    }

    void setWidth(int w) {
        if (w > 0) {
            width = w;
        } else {
            width = 1;
        }
    }

    void setHeight(int h) {
        if (h > 0) {
            height = h;
        } else {
            height = 1;
        }
    }

    int getWidth() const {
        return width;
    }

    int getHeight() const {
        return height;
    }

    int area() const {
        return width * height;
    }

    int perimeter() const {
        return 2 * (width + height);
    }

    bool isSquare() const {
        return width == height;
    }

    void display() const {
        std::cout << "Rectangle: " << width << "x" << height << std::endl;
        std::cout << "Area: " << area() << std::endl;
        std::cout << "Perimeter: " << perimeter() << std::endl;
        std::cout << "Is square: " << (isSquare() ? "Yes" : "No") << std::endl;
    }
};

int main() {
    Rectangle r1(5, 3);
    r1.display();

    std::cout << std::endl;

    Rectangle r2(4, 4);
    r2.display();

    return 0;
}
```

</details>

---

**Успех със задачите! 🚀**

_Следващи стъпки: TasksForHome.md за допълнителна практика_
