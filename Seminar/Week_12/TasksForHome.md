# **ЗАДАЧИ ЗА ВКЪЩИ - СЕДМИЦА 12**

## **Структури, Класове и Enums - Практика за упражнение**

---

## **ЗАДАЧА 1: Структура Book**

Създайте структура `Book` с полета:

- `title` (заглавие)
- `author` (автор)
- `year` (година на издаване)
- `price` (цена)

Напишете функции за:

1. Четене на масив от книги
2. Сортиране по цена
3. Намиране на най-скъпата книга
4. Извеждане на книги, издадени след дадена година

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

struct Book {
    char title[100];
    char author[50];
    int year;
    double price;
};

void readBook(Book& b) {
    std::cout << "Title: ";
    std::cin.getline(b.title, 100);
    std::cout << "Author: ";
    std::cin.getline(b.author, 50);
    std::cout << "Year: ";
    std::cin >> b.year;
    std::cout << "Price: ";
    std::cin >> b.price;
    std::cin.ignore();  // Изчистваме буфера
}

void readBooks(Book books[], int n) {
    for (int i = 0; i < n; i++) {
        std::cout << "\nBook " << i + 1 << ":" << std::endl;
        readBook(books[i]);
    }
}

void sortByPrice(Book books[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (books[j].price > books[j + 1].price) {
                Book temp = books[j];
                books[j] = books[j + 1];
                books[j + 1] = temp;
            }
        }
    }
}

int findMostExpensive(const Book books[], int n) {
    int maxIdx = 0;
    for (int i = 1; i < n; i++) {
        if (books[i].price > books[maxIdx].price) {
            maxIdx = i;
        }
    }
    return maxIdx;
}

void printBooksAfterYear(const Book books[], int n, int year) {
    std::cout << "\nBooks published after " << year << ":" << std::endl;
    bool found = false;

    for (int i = 0; i < n; i++) {
        if (books[i].year > year) {
            std::cout << books[i].title << " (" << books[i].year << ")" << std::endl;
            found = true;
        }
    }

    if (!found) {
        std::cout << "No books found." << std::endl;
    }
}

void printBook(const Book& b) {
    std::cout << "\"" << b.title << "\" by " << b.author
              << " (" << b.year << ") - $" << b.price << std::endl;
}

int main() {
    const int MAX_BOOKS = 100;
    Book library[MAX_BOOKS];
    int n;

    std::cout << "How many books? ";
    std::cin >> n;
    std::cin.ignore();

    if (n > 0 && n <= MAX_BOOKS) {
        readBooks(library, n);

        int expensiveIdx = findMostExpensive(library, n);
        std::cout << "\nMost expensive book:" << std::endl;
        printBook(library[expensiveIdx]);

        sortByPrice(library, n);
        std::cout << "\nBooks sorted by price:" << std::endl;
        for (int i = 0; i < n; i++) {
            printBook(library[i]);
        }

        int year;
        std::cout << "\nEnter year: ";
        std::cin >> year;
        printBooksAfterYear(library, n, year);
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 2: Структура Time**

Създайте структура `Time` с полета `hours`, `minutes`, `seconds`. Напишете функции за:

1. Валидиране на време (0 ≤ h < 24, 0 ≤ m,s < 60)
2. Добавяне на секунди към време
3. Разлика между две времена (в секунди)
4. Сравняване на две времена

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

struct Time {
    int hours;
    int minutes;
    int seconds;
};

bool isValidTime(const Time& t) {
    return t.hours >= 0 && t.hours < 24 &&
           t.minutes >= 0 && t.minutes < 60 &&
           t.seconds >= 0 && t.seconds < 60;
}

void normalize(Time& t) {
    // Нормализираме секундите
    if (t.seconds >= 60) {
        t.minutes += t.seconds / 60;
        t.seconds %= 60;
    }

    // Нормализираме минутите
    if (t.minutes >= 60) {
        t.hours += t.minutes / 60;
        t.minutes %= 60;
    }

    // Нормализираме часовете
    t.hours %= 24;
}

Time addSeconds(Time t, int sec) {
    t.seconds += sec;
    normalize(t);
    return t;
}

int timeToSeconds(const Time& t) {
    return t.hours * 3600 + t.minutes * 60 + t.seconds;
}

int timeDifference(const Time& t1, const Time& t2) {
    return timeToSeconds(t1) - timeToSeconds(t2);
}

int compareTime(const Time& t1, const Time& t2) {
    int diff = timeDifference(t1, t2);
    if (diff < 0) return -1;  // t1 < t2
    if (diff > 0) return 1;   // t1 > t2
    return 0;                 // t1 == t2
}

void printTime(const Time& t) {
    printf("%02d:%02d:%02d\n", t.hours, t.minutes, t.seconds);
}

int main() {
    Time t1 = {14, 30, 45};
    Time t2 = {16, 15, 20};

    std::cout << "Time 1: ";
    printTime(t1);

    std::cout << "Time 2: ";
    printTime(t2);

    Time t3 = addSeconds(t1, 3700);  // Добавяме 1 час и 1 минута
    std::cout << "Time 1 + 3700 seconds: ";
    printTime(t3);

    int diff = timeDifference(t2, t1);
    std::cout << "Difference: " << diff << " seconds" << std::endl;

    int cmp = compareTime(t1, t2);
    if (cmp < 0) {
        std::cout << "Time 1 is earlier than Time 2" << std::endl;
    } else if (cmp > 0) {
        std::cout << "Time 1 is later than Time 2" << std::endl;
    } else {
        std::cout << "Times are equal" << std::endl;
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 3: Клас Vector2D**

Създайте клас `Vector2D` за 2D вектор с координати x и y. Имплементирайте:

1. Конструктори (по подразбиране и с параметри)
2. Методи за събиране и изваждане на вектори
3. Метод за скаларно произведение (dot product)
4. Метод за дължина на вектора
5. Метод за нормализация (единичен вектор)

**Формули:**

- Дължина: $|\vec{v}| = \sqrt{x^2 + y^2}$
- Скаларно произведение: $\vec{a} \cdot \vec{b} = a_x \cdot b_x + a_y \cdot b_y$
- Нормализация: $\hat{v} = \frac{\vec{v}}{|\vec{v}|}$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

class Vector2D {
private:
    double x;
    double y;

public:
    Vector2D() : x(0), y(0) {}

    Vector2D(double xVal, double yVal) : x(xVal), y(yVal) {}

    double getX() const { return x; }
    double getY() const { return y; }

    void setX(double val) { x = val; }
    void setY(double val) { y = val; }

    Vector2D add(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    Vector2D subtract(const Vector2D& other) const {
        return Vector2D(x - other.x, y - other.y);
    }

    double dotProduct(const Vector2D& other) const {
        return x * other.x + y * other.y;
    }

    double length() const {
        return sqrt(x * x + y * y);
    }

    Vector2D normalize() const {
        double len = length();
        if (len == 0) {
            return Vector2D(0, 0);
        }
        return Vector2D(x / len, y / len);
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")" << std::endl;
    }
};

int main() {
    Vector2D v1(3, 4);
    Vector2D v2(1, 2);

    std::cout << "v1 = ";
    v1.display();

    std::cout << "v2 = ";
    v2.display();

    Vector2D sum = v1.add(v2);
    std::cout << "v1 + v2 = ";
    sum.display();

    Vector2D diff = v1.subtract(v2);
    std::cout << "v1 - v2 = ";
    diff.display();

    std::cout << "v1 · v2 = " << v1.dotProduct(v2) << std::endl;
    std::cout << "|v1| = " << v1.length() << std::endl;

    Vector2D normalized = v1.normalize();
    std::cout << "v1 normalized = ";
    normalized.display();
    std::cout << "Length of normalized = " << normalized.length() << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 4: Клас Fraction**

Създайте клас `Fraction` за работа с дроби. Имплементирайте:

1. Конструктор с валидация (знаменател != 0)
2. Методи за опростяване на дроби (чрез НОД)
3. Методи за аритметични операции (+, -, \*, /)
4. Метод за конвертиране към double

**Формули:**

- $\frac{a}{b} + \frac{c}{d} = \frac{ad + bc}{bd}$
- $\frac{a}{b} \times \frac{c}{d} = \frac{ac}{bd}$
- $\frac{a}{b} \div \frac{c}{d} = \frac{ad}{bc}$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

class Fraction {
private:
    int numerator;
    int denominator;

    // НОД (Greatest Common Divisor)
    int gcd(int a, int b) const {
        a = abs(a);
        b = abs(b);
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    void simplify() {
        if (denominator == 0) {
            std::cout << "Error: denominator cannot be 0" << std::endl;
            denominator = 1;
        }

        int g = gcd(numerator, denominator);
        numerator /= g;
        denominator /= g;

        // Ако знаменателят е отрицателен, преместваме знака в числителя
        if (denominator < 0) {
            numerator = -numerator;
            denominator = -denominator;
        }
    }

public:
    Fraction(int num = 0, int den = 1) : numerator(num), denominator(den) {
        simplify();
    }

    Fraction add(const Fraction& other) const {
        int num = numerator * other.denominator + other.numerator * denominator;
        int den = denominator * other.denominator;
        return Fraction(num, den);
    }

    Fraction subtract(const Fraction& other) const {
        int num = numerator * other.denominator - other.numerator * denominator;
        int den = denominator * other.denominator;
        return Fraction(num, den);
    }

    Fraction multiply(const Fraction& other) const {
        int num = numerator * other.numerator;
        int den = denominator * other.denominator;
        return Fraction(num, den);
    }

    Fraction divide(const Fraction& other) const {
        int num = numerator * other.denominator;
        int den = denominator * other.numerator;
        return Fraction(num, den);
    }

    double toDouble() const {
        return static_cast<double>(numerator) / denominator;
    }

    void display() const {
        if (denominator == 1) {
            std::cout << numerator;
        } else {
            std::cout << numerator << "/" << denominator;
        }
    }
};

int main() {
    Fraction f1(3, 4);    // 3/4
    Fraction f2(2, 3);    // 2/3

    std::cout << "f1 = ";
    f1.display();
    std::cout << std::endl;

    std::cout << "f2 = ";
    f2.display();
    std::cout << std::endl;

    Fraction sum = f1.add(f2);
    std::cout << "f1 + f2 = ";
    sum.display();
    std::cout << " = " << sum.toDouble() << std::endl;

    Fraction diff = f1.subtract(f2);
    std::cout << "f1 - f2 = ";
    diff.display();
    std::cout << " = " << diff.toDouble() << std::endl;

    Fraction prod = f1.multiply(f2);
    std::cout << "f1 * f2 = ";
    prod.display();
    std::cout << " = " << prod.toDouble() << std::endl;

    Fraction quot = f1.divide(f2);
    std::cout << "f1 / f2 = ";
    quot.display();
    std::cout << " = " << quot.toDouble() << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 5: Enum Direction + структура Position**

Създайте:

1. `enum class Direction {North, South, East, West}`
2. Структура `Position` с x и y координати
3. Функция `move(Position& pos, Direction dir, int steps)` - премества позицията

```cpp
// Пример:
Position p = {0, 0};
move(p, Direction::North, 5);  // p става {0, 5}
move(p, Direction::East, 3);   // p става {3, 5}
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

enum class Direction {
    North,
    South,
    East,
    West
};

struct Position {
    int x;
    int y;
};

void move(Position& pos, Direction dir, int steps) {
    switch (dir) {
        case Direction::North:
            pos.y += steps;
            break;
        case Direction::South:
            pos.y -= steps;
            break;
        case Direction::East:
            pos.x += steps;
            break;
        case Direction::West:
            pos.x -= steps;
            break;
    }
}

const char* directionToString(Direction dir) {
    switch (dir) {
        case Direction::North: return "North";
        case Direction::South: return "South";
        case Direction::East: return "East";
        case Direction::West: return "West";
        default: return "Unknown";
    }
}

void printPosition(const Position& pos) {
    std::cout << "(" << pos.x << ", " << pos.y << ")" << std::endl;
}

int main() {
    Position p = {0, 0};

    std::cout << "Starting position: ";
    printPosition(p);

    move(p, Direction::North, 5);
    std::cout << "After moving North 5: ";
    printPosition(p);

    move(p, Direction::East, 3);
    std::cout << "After moving East 3: ";
    printPosition(p);

    move(p, Direction::South, 2);
    std::cout << "After moving South 2: ";
    printPosition(p);

    move(p, Direction::West, 1);
    std::cout << "After moving West 1: ";
    printPosition(p);

    return 0;
}
```

</details>

---

## **ЗАДАЧА 6: Клас Matrix (опростена 2x2 матрица)**

Създайте клас `Matrix2x2` за 2x2 матрица. Имплементирайте:

1. Конструктор с 4 елемента
2. Метод за умножение на матрици
3. Метод за транспониране
4. Метод за изчисляване на детерминанта

**Формули:**

- Детерминанта: $\det(A) = a_{11} \cdot a_{22} - a_{12} \cdot a_{21}$
- Умножение: $(AB)_{ij} = \sum_k A_{ik} \cdot B_{kj}$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class Matrix2x2 {
private:
    double data[2][2];

public:
    Matrix2x2() {
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                data[i][j] = 0;
            }
        }
    }

    Matrix2x2(double a11, double a12, double a21, double a22) {
        data[0][0] = a11;
        data[0][1] = a12;
        data[1][0] = a21;
        data[1][1] = a22;
    }

    double get(int i, int j) const {
        if (i >= 0 && i < 2 && j >= 0 && j < 2) {
            return data[i][j];
        }
        return 0;
    }

    void set(int i, int j, double value) {
        if (i >= 0 && i < 2 && j >= 0 && j < 2) {
            data[i][j] = value;
        }
    }

    Matrix2x2 multiply(const Matrix2x2& other) const {
        Matrix2x2 result;

        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                double sum = 0;
                for (int k = 0; k < 2; k++) {
                    sum += data[i][k] * other.data[k][j];
                }
                result.data[i][j] = sum;
            }
        }

        return result;
    }

    Matrix2x2 transpose() const {
        return Matrix2x2(data[0][0], data[1][0],
                        data[0][1], data[1][1]);
    }

    double determinant() const {
        return data[0][0] * data[1][1] - data[0][1] * data[1][0];
    }

    void display() const {
        std::cout << "┌             ┐" << std::endl;
        std::cout << "│ " << data[0][0] << "  " << data[0][1] << " │" << std::endl;
        std::cout << "│ " << data[1][0] << "  " << data[1][1] << " │" << std::endl;
        std::cout << "└             ┘" << std::endl;
    }
};

int main() {
    Matrix2x2 A(1, 2, 3, 4);
    Matrix2x2 B(5, 6, 7, 8);

    std::cout << "Matrix A:" << std::endl;
    A.display();

    std::cout << "Matrix B:" << std::endl;
    B.display();

    Matrix2x2 C = A.multiply(B);
    std::cout << "A * B:" << std::endl;
    C.display();

    Matrix2x2 At = A.transpose();
    std::cout << "A transposed:" << std::endl;
    At.display();

    std::cout << "det(A) = " << A.determinant() << std::endl;
    std::cout << "det(B) = " << B.determinant() << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 7: Клас Employee**

Създайте клас `Employee` с:

- private полета: name, id, salary
- public методи: constructors, getters, setters
- метод `giveRaise(double percentage)` - увеличава заплатата
- метод `display()` - извежда информацията

Създайте масив от служители и намерете този с най-висока заплата.

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

class Employee {
private:
    char name[100];
    int id;
    double salary;

public:
    Employee() {
        strcpy(name, "");
        id = 0;
        salary = 0;
    }

    Employee(const char* n, int empId, double sal) {
        strcpy(name, n);
        id = empId;
        salary = sal;
    }

    const char* getName() const {
        return name;
    }

    int getId() const {
        return id;
    }

    double getSalary() const {
        return salary;
    }

    void setName(const char* n) {
        strcpy(name, n);
    }

    void setId(int empId) {
        id = empId;
    }

    void setSalary(double sal) {
        if (sal >= 0) {
            salary = sal;
        }
    }

    void giveRaise(double percentage) {
        if (percentage > 0) {
            salary += salary * percentage / 100.0;
        }
    }

    void display() const {
        std::cout << "ID: " << id << std::endl;
        std::cout << "Name: " << name << std::endl;
        std::cout << "Salary: $" << salary << std::endl;
    }
};

int findHighestPaid(const Employee employees[], int count) {
    int maxIdx = 0;
    for (int i = 1; i < count; i++) {
        if (employees[i].getSalary() > employees[maxIdx].getSalary()) {
            maxIdx = i;
        }
    }
    return maxIdx;
}

int main() {
    Employee employees[5] = {
        Employee("John Smith", 101, 50000),
        Employee("Jane Doe", 102, 65000),
        Employee("Bob Johnson", 103, 55000),
        Employee("Alice Williams", 104, 70000),
        Employee("Charlie Brown", 105, 48000)
    };

    std::cout << "All employees:" << std::endl;
    for (int i = 0; i < 5; i++) {
        std::cout << "\nEmployee " << i + 1 << ":" << std::endl;
        employees[i].display();
    }

    int highestIdx = findHighestPaid(employees, 5);
    std::cout << "\n\nHighest paid employee:" << std::endl;
    employees[highestIdx].display();

    // Даваме 10% увеличение на всички
    std::cout << "\n\nGiving 10% raise to all employees..." << std::endl;
    for (int i = 0; i < 5; i++) {
        employees[i].giveRaise(10);
    }

    std::cout << "\nAfter raises:" << std::endl;
    for (int i = 0; i < 5; i++) {
        std::cout << employees[i].getName() << ": $"
                  << employees[i].getSalary() << std::endl;
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 8: Структура Polynomial (полином)**

Създайте структура `Polynomial` за представяне на полином (до степен 5).
Имплементирайте функции за:

1. Оценка на полином при дадено x
2. Събиране на два полинома
3. Умножение на полином по константа

**Формула:** $P(x) = a_0 + a_1x + a_2x^2 + ... + a_nx^n$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

const int MAX_DEGREE = 5;

struct Polynomial {
    double coefficients[MAX_DEGREE + 1];  // a0, a1, a2, ..., a5
    int degree;
};

void initPolynomial(Polynomial& p) {
    for (int i = 0; i <= MAX_DEGREE; i++) {
        p.coefficients[i] = 0;
    }
    p.degree = 0;
}

void setCoefficient(Polynomial& p, int power, double value) {
    if (power >= 0 && power <= MAX_DEGREE) {
        p.coefficients[power] = value;
        if (power > p.degree && value != 0) {
            p.degree = power;
        }
    }
}

double evaluate(const Polynomial& p, double x) {
    double result = 0;
    for (int i = 0; i <= p.degree; i++) {
        result += p.coefficients[i] * pow(x, i);
    }
    return result;
}

Polynomial addPolynomials(const Polynomial& p1, const Polynomial& p2) {
    Polynomial result;
    initPolynomial(result);

    int maxDeg = (p1.degree > p2.degree) ? p1.degree : p2.degree;
    result.degree = maxDeg;

    for (int i = 0; i <= maxDeg; i++) {
        result.coefficients[i] = p1.coefficients[i] + p2.coefficients[i];
    }

    return result;
}

Polynomial multiplyByConstant(const Polynomial& p, double k) {
    Polynomial result = p;
    for (int i = 0; i <= p.degree; i++) {
        result.coefficients[i] *= k;
    }
    return result;
}

void printPolynomial(const Polynomial& p) {
    bool first = true;

    for (int i = p.degree; i >= 0; i--) {
        if (p.coefficients[i] != 0) {
            if (!first && p.coefficients[i] > 0) {
                std::cout << " + ";
            } else if (p.coefficients[i] < 0) {
                std::cout << " - ";
            }

            double absCoef = fabs(p.coefficients[i]);

            if (i == 0) {
                std::cout << absCoef;
            } else if (i == 1) {
                if (absCoef == 1) {
                    std::cout << "x";
                } else {
                    std::cout << absCoef << "x";
                }
            } else {
                if (absCoef == 1) {
                    std::cout << "x^" << i;
                } else {
                    std::cout << absCoef << "x^" << i;
                }
            }

            first = false;
        }
    }

    if (first) {
        std::cout << "0";
    }

    std::cout << std::endl;
}

int main() {
    // P1(x) = 2x^3 + 3x^2 - 5x + 1
    Polynomial p1;
    initPolynomial(p1);
    setCoefficient(p1, 0, 1);
    setCoefficient(p1, 1, -5);
    setCoefficient(p1, 2, 3);
    setCoefficient(p1, 3, 2);

    // P2(x) = x^2 - 2x + 4
    Polynomial p2;
    initPolynomial(p2);
    setCoefficient(p2, 0, 4);
    setCoefficient(p2, 1, -2);
    setCoefficient(p2, 2, 1);

    std::cout << "P1(x) = ";
    printPolynomial(p1);

    std::cout << "P2(x) = ";
    printPolynomial(p2);

    std::cout << "\nP1(2) = " << evaluate(p1, 2) << std::endl;
    std::cout << "P2(2) = " << evaluate(p2, 2) << std::endl;

    Polynomial sum = addPolynomials(p1, p2);
    std::cout << "\nP1(x) + P2(x) = ";
    printPolynomial(sum);

    Polynomial scaled = multiplyByConstant(p1, 3);
    std::cout << "\n3 * P1(x) = ";
    printPolynomial(scaled);

    return 0;
}
```

</details>

---

## **ЗАДАЧА 9: Enum Season + клас Date**

Създайте:

1. `enum class Season {Winter, Spring, Summer, Autumn}`
2. Клас `Date` с day, month, year
3. Метод `getSeason()` в класа Date

```cpp
// Зима: Dec, Jan, Feb
// Пролет: Mar, Apr, May
// Лято: Jun, Jul, Aug
// Есен: Sep, Oct, Nov
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

enum class Season {
    Winter,
    Spring,
    Summer,
    Autumn
};

class Date {
private:
    int day;
    int month;
    int year;

public:
    Date(int d = 1, int m = 1, int y = 2000) : day(d), month(m), year(y) {
        validate();
    }

    void validate() {
        if (month < 1) month = 1;
        if (month > 12) month = 12;
        if (day < 1) day = 1;
        if (day > 31) day = 31;
    }

    Season getSeason() const {
        if (month == 12 || month == 1 || month == 2) {
            return Season::Winter;
        } else if (month >= 3 && month <= 5) {
            return Season::Spring;
        } else if (month >= 6 && month <= 8) {
            return Season::Summer;
        } else {
            return Season::Autumn;
        }
    }

    const char* getSeasonName() const {
        Season s = getSeason();
        switch (s) {
            case Season::Winter: return "Winter";
            case Season::Spring: return "Spring";
            case Season::Summer: return "Summer";
            case Season::Autumn: return "Autumn";
            default: return "Unknown";
        }
    }

    void display() const {
        printf("%02d/%02d/%04d\n", day, month, year);
    }

    int getDay() const { return day; }
    int getMonth() const { return month; }
    int getYear() const { return year; }
};

int main() {
    Date dates[] = {
        Date(15, 1, 2025),   // Winter
        Date(20, 4, 2025),   // Spring
        Date(10, 7, 2025),   // Summer
        Date(5, 10, 2025)    // Autumn
    };

    for (int i = 0; i < 4; i++) {
        dates[i].display();
        std::cout << "Season: " << dates[i].getSeasonName() << std::endl;
        std::cout << std::endl;
    }

    return 0;
}
```

</details>

---

## **ЗАДАЧА 10: Клас Stack (опростен)**

Създайте клас `Stack` за стек от integers с фиксиран размер. Имплементирайте:

1. `push(int value)` - добавя елемент
2. `pop()` - премахва и връща последния елемент
3. `top()` - връща последния елемент без да го премахва
4. `isEmpty()` и `isFull()` - проверки
5. `size()` - брой елементи

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class Stack {
private:
    static const int MAX_SIZE = 100;
    int data[MAX_SIZE];
    int topIndex;

public:
    Stack() {
        topIndex = -1;
    }

    bool isEmpty() const {
        return topIndex == -1;
    }

    bool isFull() const {
        return topIndex == MAX_SIZE - 1;
    }

    int size() const {
        return topIndex + 1;
    }

    bool push(int value) {
        if (isFull()) {
            std::cout << "Stack overflow!" << std::endl;
            return false;
        }

        topIndex++;
        data[topIndex] = value;
        return true;
    }

    bool pop(int& value) {
        if (isEmpty()) {
            std::cout << "Stack underflow!" << std::endl;
            return false;
        }

        value = data[topIndex];
        topIndex--;
        return true;
    }

    bool top(int& value) const {
        if (isEmpty()) {
            std::cout << "Stack is empty!" << std::endl;
            return false;
        }

        value = data[topIndex];
        return true;
    }

    void display() const {
        if (isEmpty()) {
            std::cout << "Stack is empty" << std::endl;
            return;
        }

        std::cout << "Stack (top to bottom): ";
        for (int i = topIndex; i >= 0; i--) {
            std::cout << data[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    Stack s;

    std::cout << "Pushing 10, 20, 30, 40, 50" << std::endl;
    s.push(10);
    s.push(20);
    s.push(30);
    s.push(40);
    s.push(50);

    s.display();
    std::cout << "Size: " << s.size() << std::endl;

    int value;
    if (s.top(value)) {
        std::cout << "Top element: " << value << std::endl;
    }

    std::cout << "\nPopping elements:" << std::endl;
    while (!s.isEmpty()) {
        if (s.pop(value)) {
            std::cout << "Popped: " << value << std::endl;
        }
    }

    s.display();

    return 0;
}
```

</details>

---

**Успех с домашните! 💪**

_Следваща стъпка: HardTasks.md за още предизвикателства_
