# Цикли в C++ - Задълбочена теория

## Основни понятия

### Какво е цикъл?

Циклът е управляваща структура в програмирането, която позволява **многократно изпълнение** на блок код, докато определено условие е изпълнено. Циклите са основен инструмент за **автоматизация на повторяещи се действия**.

Представете си, че искате да отпечатате числата от 1 до 1000. Без цикли би трябвало да напишете 1000 реда `cout` - с цикъл можете да го направите с 3-4 реда код!

### Защо използваме цикли?

1. **Икономия на код** - избягваме повторения
2. **Гъвкавост** - можем да контролираме колко пъти се изпълнява кода
3. **Ефективност** - лесно обработваме големи количества данни
4. **Четимост** - кодът е по-ясен и по-кратък

## Видове цикли в C++

В C++ имаме три основни вида цикли:

- `for` - когато знаем точно колко пъти искаме да повторим
- `while` - когато повтаряме докато е изпълнено условие
- `do-while` - като `while`, но се изпълнява поне веднъж

## 1. FOR цикъл

### Синтаксис и структура

```cpp
for (initialization; condition; step) {
    // code to execute
}
```

**Компоненти:**

1. **Инициализация** - изпълнява се веднъж в началото
2. **Условие** - проверява се преди всяка итерация
3. **Стъпка** - изпълнява се след всяка итерация

### Как работи for циклът?

```cpp
for (int i = 1; i <= 5; i++) {
    cout << i << " ";
}
```

**Стъпка по стъпка:**

1. `int i = 1` - създава променлива `i` и я инициализира с 1
2. `i <= 5` - проверява дали `i` е по-малко или равно на 5
3. Ако условието е ✅ - изпълнява тялото на цикъла
4. `i++` - увеличава `i` с 1
5. Връща се на стъпка 2

**Резултат:** `1 2 3 4 5 `

### Примери с for цикъл

#### Основни примери

```cpp
// Print numbers from 1 to 10
for (int i = 1; i <= 10; i++) {
    cout << i << " ";
}
```

```cpp
// Print numbers from 10 to 1 (backwards)
for (int i = 10; i >= 1; i--) {
    cout << i << " ";
}
```

```cpp
// Print even numbers from 2 to 20
for (int i = 2; i <= 20; i += 2) {
    cout << i << " ";
}
```

#### Напреднали примери

```cpp
// Nested for loops - multiplication table
for (int i = 1; i <= 10; i++) {
    for (int j = 1; j <= 10; j++) {
        cout << i << " * " << j << " = " << i * j << endl;
    }
}
```

```cpp
// Working with different steps
for (int i = 0; i < 100; i += 5) {
    cout << i << " "; // 0 5 10 15 20 ... 95
}
```

```cpp
// Iterating through characters
for (char c = 'A'; c <= 'Z'; c++) {
    cout << c << " "; // A B C D ... Z
}
```

### Вариации на for цикъла

#### Празни компоненти

```cpp
int i = 0;
for (; i < 5; i++) {  // Empty initialization
    cout << i << " ";
}
```

```cpp
int i = 0;
for (; i < 5; ) {  // Empty step
    cout << i << " ";
    i++;  // Manual increment
}
```

#### Множество променливи

```cpp
for (int i = 0, j = 10; i < j; i++, j--) {
    cout << "i=" << i << ", j=" << j << endl;
}
```

## 2. WHILE цикъл

### Синтаксис и структура

```cpp
while (condition) {
    // code to execute
}
```

**Как работи:**

1. Проверява условието
2. Ако е ✅ - изпълнява тялото
3. Връща се на стъпка 1
4. Ако е ❌ - излиза от цикъла

### Основни примери

```cpp
// Print numbers from 1 to 5
int i = 1;
while (i <= 5) {
    cout << i << " ";
    i++;
}
```

```cpp
// Read numbers until 0 is entered
int number;
cout << "Enter a number (0 to exit): ";
cin >> number;
while (number != 0) {
    cout << "You entered: " << number << endl;
    cout << "Enter a number (0 to exit): ";
    cin >> number;
}
```

### Напреднали примери с while

#### Работа с неизвестен брой итерации

```cpp
// Find the first power of 2 greater than 1000
int power = 1;
int exponent = 0;
while (power <= 1000) {
    power *= 2;
    exponent++;
}
cout << "2^" << exponent << " = " << power << endl;
```

#### Валидация на вход

```cpp
int age;
cout << "Enter age (18-100): ";
cin >> age;
while (age < 18 || age > 100) {
    cout << "Invalid age! Try again: ";
    cin >> age;
}
cout << "Valid age: " << age << endl;
```

#### Работа с цифри на число

```cpp
int number = 12345;
int digitSum = 0;
while (number > 0) {
    digitSum += number % 10;  // Add the last digit
    number /= 10;             // Remove the last digit
}
cout << "Sum of digits: " << digitSum << endl;
```

## 3. DO-WHILE цикъл

### Синтаксис и структура

```cpp
do {
    // code to execute
} while (condition);
```

**Ключова разлика:** Тялото се изпълнява **поне веднъж**, независимо от условието!

### Основни примери

```cpp
// Print numbers from 1 to 5
int i = 1;
do {
    cout << i << " ";
    i++;
} while (i <= 5);
```

### Практически примери на do-while

#### Меню система

```cpp
int choice;
do {
    cout << "\n=== MENU ===" << endl;
    cout << "1. Option 1" << endl;
    cout << "2. Option 2" << endl;
    cout << "3. Exit" << endl;
    cout << "Choose option: ";
    cin >> choice;

    switch (choice) {
        case 1:
            cout << "You chose option 1" << endl;
            break;
        case 2:
            cout << "You chose option 2" << endl;
            break;
        case 3:
            cout << "Goodbye!" << endl;
            break;
        default:
            cout << "Invalid option!" << endl;
    }
} while (choice != 3);
```

#### Игра "Познай числото"

```cpp
#include <random>
int secretNumber = rand() % 100 + 1;
int guess;
int attempts = 0;

cout << "Guess the number between 1 and 100!" << endl;
do {
    cout << "Enter your guess: ";
    cin >> guess;
    attempts++;

    if (guess > secretNumber) {
        cout << "Too high! Try again." << endl;
    } else if (guess < secretNumber) {
        cout << "Too low! Try again." << endl;
    } else {
        cout << "Congratulations! You guessed it in " << attempts << " attempts!" << endl;
    }
} while (guess != secretNumber);
```

## Кога кой цикъл да използваме?

### FOR цикъл - използвайте когато:

- Знаете точния брой итерации
- Работите с брояч (counter)
- Обхождате масиви или контейнери
- Правите математически изчисления със стъпка

**Примери:**

- Отпечатване на числа от 1 до N
- Таблици за умножение
- Генериране на последователности

### WHILE цикъл - използвайте когато:

- Броят итерации зависи от условие
- Четете данни докато не се въведе специална стойност
- Валидирате вход
- Работите с неизвестен брой елементи

**Примери:**

- Четене на файл докато не свърши
- Игри с неопределен край
- Търсене на решение

### DO-WHILE цикъл - използвайте когато:

- Трябва да изпълните кода поне веднъж
- Създавате меню системи
- Валидирате вход след първо въвеждане

**Примери:**

- Потребителски интерфейси
- Игри с повторение
- Системи за вход

## Контролни оператори в циклите

### BREAK - прекратяване на цикъл

```cpp
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;  // Exit the loop when i = 5
    }
    cout << i << " ";  // Prints: 1 2 3 4
}
```

### CONTINUE - прескачане на итерация

```cpp
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    cout << i << " ";  // Prints: 1 3 5 7 9
}
```

### Вложени цикли с break и continue

```cpp
for (int i = 1; i <= 3; i++) {
    cout << "Outer loop: " << i << endl;
    for (int j = 1; j <= 5; j++) {
        if (j == 3) {
            break;  // Terminates only the inner loop
        }
        cout << "  Inner: " << j << endl;
    }
}
```

## Често срещани грешки и как да ги избегнем

### 1. Безкрайни цикли

```cpp
// WRONG - infinite loop
int i = 1;
while (i <= 10) {
    cout << i << " ";
    // We forgot i++
}

// CORRECT
int i = 1;
while (i <= 10) {
    cout << i << " ";
    i++;  // Don't forget to change the condition!
}
```

### 2. Грешки с границите (Off-by-one errors)

```cpp
// WRONG - prints from 0 to 9 (10 numbers)
for (int i = 0; i <= 9; i++) {
    cout << i << " ";
}

// CORRECT - prints from 1 to 10 (10 numbers)
for (int i = 1; i <= 10; i++) {
    cout << i << " ";
}
```

### 3. Променяне на брояча в тялото на for цикъл

```cpp
// DANGEROUS - modifying i in the body
for (int i = 1; i <= 10; i++) {
    cout << i << " ";
    i++;  // This makes the loop skip numbers
}

// CORRECT - don't modify the counter in the body
for (int i = 1; i <= 10; i++) {
    cout << i << " ";
    // Don't modify i here!
}
```

## Ефективност и оптимизация

### Избор на правилен тип данни

```cpp
// For large numbers use appropriate type
for (long long i = 0; i < 1000000000LL; i++) {
    // Working with very large number of iterations
}
```

### Минимизиране на изчисления в цикъла

```cpp
// INEFFICIENT
for (int i = 0; i < 1000; i++) {
    int limit = expensiveCalculation();  // Calculated 1000 times
    if (i < limit) {
        // ...
    }
}

// EFFICIENT
int limit = expensiveCalculation();  // Calculated only once
for (int i = 0; i < 1000; i++) {
    if (i < limit) {
        // ...
    }
}
```

## Практически примери и алгоритми

### 1. Finding factorial

```cpp
int factorial(int n) {
    int result = 1;
    for (int i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}

// Alternative with while
int factorial_while(int n) {
    int result = 1;
    while (n > 0) {
        result *= n;
        n--;
    }
    return result;
}
```

### 2. Числа на Фибоначи

```cpp
void fibonacci(int n) {
    int a = 0, b = 1;
    cout << a << " " << b << " ";

    for (int i = 2; i < n; i++) {
        int next = a + b;
        cout << next << " ";
        a = b;
        b = next;
    }
}
```

### 3. Проверка за просто число

```cpp
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

### 4. Обръщане на число (палиндром)

```cpp
int reverseNumber(int n) {
    int reversed = 0;
    while (n > 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }
    return reversed;
}

bool isPalindrome(int n) {
    return n == reverseNumber(n);
}
```

### 5. Най-голям общ делител (НОД)

```cpp
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

## Вложени цикли и многомерни проблеми

### Отпечатване на фигури

```cpp
// Triangle of stars
void printTriangle(int height) {
    for (int i = 1; i <= height; i++) {
        for (int j = 1; j <= i; j++) {
            cout << "* ";
        }
        cout << endl;
    }
}

// Square frame
void printSquareFrame(int size) {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            if (i == 0 || i == size-1 || j == 0 || j == size-1) {
                cout << "* ";
            } else {
                cout << "  ";
            }
        }
        cout << endl;
    }
}
```

### Работа с матрици

```cpp
// Reading a matrix
void readMatrix(int matrix[10][10], int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            cout << "Element [" << i << "][" << j << "]: ";
            cin >> matrix[i][j];
        }
    }
}

// Sum of matrix elements
int matrixSum(int matrix[10][10], int rows, int cols) {
    int sum = 0;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            sum += matrix[i][j];
        }
    }
    return sum;
}
```

## Съвети за добра практика

### 1. Използвайте смислени имена за променливите

```cpp
// BAD
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        // Unclear what i and j are
    }
}

// GOOD
for (int row = 0; row < height; row++) {
    for (int col = 0; col < width; col++) {
        // Clear that we're working with rows and columns
    }
}
```

### 2. Ограничете обхвата на променливите

```cpp
// Variable i is visible only in the loop
for (int i = 0; i < 10; i++) {
    // Use i here
}
// i is not accessible here
```

### 3. Избягвайте дълбоко вложени цикли

```cpp
// If you have more than 3 levels of nesting,
// consider splitting into functions
for (...) {
    for (...) {
        for (...) {
            // Maybe extract part of the logic into a separate function
        }
    }
}
```

### 4. Тествайте с крайни случаи

- Тествайте с n = 0
- Тествайте с n = 1
- Тествайте с отрицателни числа (ако е възможно)
- Тествайте с много големи числа

## Заключение

Циклите са фундаментална част от програмирането. Овладяването им е ключово за решаването на сложни проблеми. Не забравяйте:

1. **Избирайте правилния тип цикъл** за задачата
2. **Внимавайте с условията** за спиране
3. **Тествайте с различни стойности**
4. **Пишете чист и четим код**
5. **Избягвайте безкрайните цикли**

Практикувайте с различни задачи, за да развиете интуиция за това кога и как да използвате всеки тип цикъл!
