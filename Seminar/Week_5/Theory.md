# **ТЕОРИЯ ФУНКЦИИ**

---

# КАКВО Е ФУНКЦИЯ?

## 1) Основни понятия

Функцията е **независим блок от код**, който може да се извика от главната програма или от други функции. Тя извършва специфична задача и може да върне резултат.

### Защо използваме функции?

1. **Преизползваемост на код** - избягваме дублиране на еднотипен код
2. **Модулност** - разделяме сложни задачи на по-малки, управляеми части
3. **Четимост** - кодът става по-организиран и лесен за разбиране
4. **Поддръжка** - промените на едно място се отразяват навсякъде където функцията се използва
5. **Тестване** - лесно можем да тестваме отделни части от програмата

---

## 2) Структура на функция

### Синтаксис:

```c++
<тип_връщана_стойност> <име_функция>(<параметри>) {
    <тяло_на_функцията>
    return <стойност>;  // ако типът не е void
}
```

### Компоненти:

1. **Тип връщана стойност** - какъв тип данни връща функцията (int, double, bool, void, char, ...)
2. **Име на функцията** - идентификатор, който използваме за извикване
3. **Параметри** - входни данни за функцията (може да няма параметри)
4. **Тяло** - кодът, който се изпълнява при извикване
5. **Return** - връщане на резултат (задължителен за всички типове освен void)

### Пример - проста функция:

```c++
#include <iostream>

void printHello() {
    std::cout << "Hello, World!" << std::endl;
}

int main() {
    printHello();  // Извикване на функцията
    printHello();
    printHello();
    return 0;
}
```

**Изход:**

```
Hello, World!
Hello, World!
Hello, World!
```

---

## 3) Функции с параметри

Параметрите позволяват да предаваме информация към функцията. При повече от един параметър, те се разделят със запетая.

### Примери:

```c++
// Функция с един параметър
void printNumber(int n) {
    std::cout << "The number is: " << n << std::endl;
}

// Функция с два параметъра
int sum(int a, int b) {
    return a + b;
}

// Функция с три параметъра от различни типове
void displayInfo(char initial, int age, double height) {
    std::cout << "Initial: " << initial << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Height: " << height << " m" << std::endl;
}
```

### Използване:

```c++
int main() {
    printNumber(42);

    int result = sum(5, 3);
    std::cout << "5 + 3 = " << result << std::endl;

    displayInfo('M', 20, 1.75);

    return 0;
}
```

---

## 4) Връщане на стойност

Функциите могат да връщат резултат чрез ключовата дума `return`.

### Важни правила:

1. Типът на връщаната стойност **трябва да съвпада** с декларирания тип на функцията
2. След `return` изпълнението на функцията **веднага приключва**
3. Функция може да има **множество return** изрази (но само един се изпълнява)
4. Функции от тип `void` **не връщат стойност**

### Примери:

```c++
// Функция която връща int
int square(int n) {
    return n * n;
}

// Функция с множество return изрази
int absoluteValue(int n) {
    if (n < 0) {
        return -n;  // Връща положителната стойност
    }
    return n;  // Връща самото число
}

// Функция която връща bool
bool isEven(int n) {
    return n % 2 == 0;
}

// Функция която не връща стойност
void printSeparator() {
    std::cout << "==================" << std::endl;
    // Няма return (или return без стойност)
}
```

### Пълен пример:

```c++
#include <iostream>

double calculateArea(double length, double width) {
    double area = length * width;
    return area;
}

int main() {
    double l = 5.5;
    double w = 3.2;

    double result = calculateArea(l, w);
    std::cout << "Area: " << result << " sq.m" << std::endl;

    // Директно използване в израз
    std::cout << "Double area: " << calculateArea(l, w) * 2 << std::endl;

    return 0;
}
```

---

## 5) Предаване на параметри

В C++ има два основни начина за предаване на параметри:

### A) Предаване по стойност (Pass by Value)

Създава се **копие** на аргумента. Промените във функцията **НЕ засягат** оригиналната променлива.

```c++
void increment(int n) {
    n = n + 1;  // Променя само локалното копие
    std::cout << "Inside function: " << n << std::endl;
}

int main() {
    int x = 5;
    increment(x);
    std::cout << "Outside function: " << x << std::endl;  // x е все още 5
    return 0;
}
```

**Изход:**

```
Inside function: 6
Outside function: 5
```

### B) Предаване по референция (Pass by Reference)

Работи директно с **оригиналната променлива**. Промените във функцията **засягат** оригинала.

```c++
void increment(int& n) {  // & означава референция
    n = n + 1;  // Променя оригиналната променлива
    std::cout << "Inside function: " << n << std::endl;
}

int main() {
    int x = 5;
    increment(x);
    std::cout << "Outside function: " << x << std::endl;  // x е 6
    return 0;
}
```

**Изход:**

```
Inside function: 6
Outside function: 6
```

### Кога използваме pass by reference?

1. Когато искаме функцията да **модифицира** оригиналната променлива
2. Когато работим с **големи обекти** (масиви, структури) - избягваме копиране
3. Когато искаме да върнем **множество стойности** чрез параметрите

### Пример - размяна на стойности:

```c++
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 10, y = 20;
    std::cout << "Before: x = " << x << ", y = " << y << std::endl;

    swap(x, y);

    std::cout << "After: x = " << x << ", y = " << y << std::endl;
    return 0;
}
```

**Изход:**

```
Before: x = 10, y = 20
After: x = 20, y = 10
```

---

## 6) Функции с масиви

Когато предаваме масив на функция, той **винаги се предава по референция** (всъщност като указател). Това означава, че функцията може да модифицира оригиналния масив.

### Важно: Трябва да предаваме и размера!

```c++
// Функция за отпечатване на масив
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

// Функция за удвояване на елементите
void doubleElements(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        arr[i] *= 2;  // Променя оригиналния масив!
    }
}

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    std::cout << "Original: ";
    printArray(numbers, 5);

    doubleElements(numbers, 5);

    std::cout << "Modified: ";
    printArray(numbers, 5);

    return 0;
}
```

**Изход:**

```
Original: 1 2 3 4 5
Modified: 2 4 6 8 10
```

### Защитаване на масив от промяна:

```c++
// const предотвратява модификация
int sumArray(const int arr[], int size) {
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += arr[i];
        // arr[i] = 0;  // ГРЕШКА! Не можем да променяме const масив
    }
    return sum;
}
```

---

## 7) Прототипи на функции (Function Prototypes)

Понякога искаме да дефинираме функция **след** `main()`, но да я извикаме **в** `main()`. За това използваме **прототип** (декларация).

### Без прототип (ГРЕШКА):

```c++
#include <iostream>

int main() {
    int result = add(5, 3);  // ГРЕШКА! add() не е дефинирана
    std::cout << result << std::endl;
    return 0;
}

int add(int a, int b) {
    return a + b;
}
```

### С прототип (ПРАВИЛНО):

```c++
#include <iostream>

// Прототип (декларация)
int add(int a, int b);

int main() {
    int result = add(5, 3);  // OK!
    std::cout << result << std::endl;
    return 0;
}

// Дефиниция
int add(int a, int b) {
    return a + b;
}
```

### Стил на писане:

```c++
// Прототип - само сигнатурата
double calculateArea(double radius);

// Можем да пропуснем имената на параметрите
double calculateArea(double);

// Но е по-добре да ги оставим за яснота
double calculateArea(double radius);
```

---

## 8) Рекурсия

Рекурсията е техника, при която **функция извиква сама себе си**. Всяка рекурсивна функция трябва да има:

1. **Базов случай** (base case) - условие при което спира
2. **Рекурсивен случай** - извикване на себе си с по-малка задача

### Пример - факториел:

```c++
// n! = n × (n-1) × (n-2) × ... × 2 × 1
// 5! = 5 × 4 × 3 × 2 × 1 = 120

int factorial(int n) {
    // Базов случай
    if (n <= 1) {
        return 1;
    }
    // Рекурсивен случай
    return n * factorial(n - 1);
}
```

**Как работи:**

```
factorial(5)
= 5 * factorial(4)
= 5 * (4 * factorial(3))
= 5 * (4 * (3 * factorial(2)))
= 5 * (4 * (3 * (2 * factorial(1))))
= 5 * (4 * (3 * (2 * 1)))
= 5 * (4 * (3 * 2))
= 5 * (4 * 6)
= 5 * 24
= 120
```

### Пример - Fibonacci:

```c++
// Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
// F(n) = F(n-1) + F(n-2)

int fibonacci(int n) {
    // Базови случаи
    if (n == 0) return 0;
    if (n == 1) return 1;

    // Рекурсивен случай
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### Пример - степенуване:

```c++
// a^n = a × a^(n-1)
double power(double base, int exponent) {
    // Базов случай
    if (exponent == 0) {
        return 1;
    }
    // Рекурсивен случай
    return base * power(base, exponent - 1);
}
```

### ⚠️ Внимание с рекурсията!

1. **Винаги трябва да има базов случай**, иначе получаваме безкрайна рекурсия
2. **Stack overflow** - твърде много рекурсивни извиквания препълват стека
3. **Неефективност** - някои рекурсивни функции извикват себе си много пъти с едни и същи аргументи

---

## 9) Scope (Обхват на променливи)

### Локални променливи

Променливи дефинирани **вътре във функция** са видими само в тази функция.

```c++
void function1() {
    int x = 10;  // Локална за function1
    std::cout << x << std::endl;
}

void function2() {
    int x = 20;  // Различна локална променлива
    std::cout << x << std::endl;
}

int main() {
    function1();  // Отпечатва: 10
    function2();  // Отпечатва: 20
    // std::cout << x;  // ГРЕШКА! x не съществува тук
    return 0;
}
```

### Глобални променливи

Променливи дефинирани **извън всички функции** са достъпни навсякъде.

```c++
#include <iostream>

int globalVar = 100;  // Глобална променлива

void printGlobal() {
    std::cout << "Global: " << globalVar << std::endl;
}

void modifyGlobal() {
    globalVar = 200;  // Модифицира глобалната променлива
}

int main() {
    std::cout << "Initial: " << globalVar << std::endl;  // 100
    modifyGlobal();
    std::cout << "After: " << globalVar << std::endl;    // 200
    printGlobal();                                        // 200
    return 0;
}
```

### ⚠️ Добри практики:

1. **Избягвайте глобални променливи** когато е възможно
2. Предпочитайте **предаване на параметри**
3. Глобалните променливи правят кода **по-труден за debugging**

---

## 10) Претоварване на функции (Function Overloading)

C++ позволява множество функции да имат **едно и също име**, но **различни параметри**.

```c++
// Различен брой параметри
int add(int a, int b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}

// Различен тип параметри
double add(double a, double b) {
    return a + b;
}

int main() {
    std::cout << add(5, 3) << std::endl;           // Извиква първата
    std::cout << add(5, 3, 2) << std::endl;        // Извиква втората
    std::cout << add(5.5, 3.2) << std::endl;       // Извиква третата
    return 0;
}
```

### Правила:

1. Функциите трябва да се различават по **брой** или **тип** параметри
2. **НЕ можем** да претоварваме само по връщан тип
3. Компилаторът избира **най-подходящата** версия

---

## 11) Стойности по подразбиране (Default Arguments)

Можем да зададем стойности по подразбиране за параметри:

```c++
void printMessage(std::string msg, int times = 1) {
    for (int i = 0; i < times; i++) {
        std::cout << msg << std::endl;
    }
}

int main() {
    printMessage("Hello");        // times = 1 (по подразбиране)
    printMessage("Hi", 3);         // times = 3
    return 0;
}
```

### Правила:

1. Параметрите по подразбиране трябва да са **в края**
2. Не можем да имаме параметър без стойност **след** параметър със стойност

```c++
// ПРАВИЛНО
void func(int a, int b = 5, int c = 10);

// ГРЕШНО
void func(int a = 5, int b, int c = 10);  // b няма стойност по подразбиране
```

---

## 12) Примери за често използвани функции

### Проверка дали число е просто:

```c++
bool isPrime(int n) {
    if (n <= 1) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;

    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
```

### Намиране на НОД (Greatest Common Divisor):

```c++
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

### Обръщане на число:

```c++
int reverseNumber(int n) {
    int reversed = 0;
    while (n > 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }
    return reversed;
}
```

### Проверка дали число е палиндром:

```c++
bool isPalindrome(int n) {
    return n == reverseNumber(n);
}
```

---

## 13) Добри практики при писане на функции

1. **Едно име = една задача** - функцията трябва да прави едно нещо добре
2. **Описателни имена** - `calculateAverage()` вместо `calc()` или `f1()`
3. **Малък размер** - ако функцията е твърде дълга, разделете я
4. **Коментари** - обяснете какво прави функцията, особено ако е сложна
5. **Избягвайте странични ефекти** - функцията не трябва да променя глобални променливи неочаквано
6. **Валидация на входа** - проверявайте дали параметрите са валидни

### Пример за добре документирана функция:

```c++
/**
 * Изчислява n-тото число на Fibonacci
 * @param n - индексът на числото (n >= 0)
 * @return n-тото число на Fibonacci
 * Примери: fibonacci(0) = 0, fibonacci(5) = 5, fibonacci(10) = 55
 */
int fibonacci(int n) {
    if (n < 0) {
        return -1;  // Грешка - невалиден вход
    }
    if (n == 0) return 0;
    if (n == 1) return 1;

    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}
```
