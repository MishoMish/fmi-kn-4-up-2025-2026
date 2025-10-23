# **ТЕОРИЯ УКАЗАТЕЛИ (POINTERS)**

---# **ТЕОРИЯ ПОЙНТЪРИ**

---

# _КАКВО СА УКАЗАТЕЛИ?_

# _КАКВО Е ПОЙНТЪР?_

## Основни понятия

## 1) Пойнтър:

Указателят (pointer) е променлива, която съхранява **адреса в паметта** на друга променлива. Това е едно от най-мощните и същевременно опасни средства в C++, което ни дава директен достъп до паметта.Пойнтърът е променлива, която съхранява адреса на друга променлива, вместо самата стойност на променливата. Пойнтърите са полезни за динамично управление на паметта, работа с масиви и достъп до ресурси извън стека.

### Защо са важни указателите?Пример:

````c++

1. **Директен достъп до паметта** - можем да работим ефективно с големи структури от данниint x = 10;      // обикновена променлива

2. **Динамично управление на паметта** - можем да заделяме памет по време на изпълнениеint* p = &x;     // пойнтър променлива, сочеща към адреса на "x"

3. **Предаване по референция** - можем да променяме параметри на функции// Деклариране: <тип>* <име> = &<променлива>;

4. **Ефективност** - вместо да копираме големи обекти, предаваме само техния адрес```

5. **Структури от данни** - свързани списъци, дървета, графи - всички използват указатели

Тук `p` съдържа адреса на `x` в паметта и ни позволява да достъпваме `x` през него. Това се нарича "достъп чрез дереференция":

## 1) Деклариране и инициализация на указатели```c++

std::cout << *p;  // Достъп до стойността на "x" чрез пойнтър, изход: 10

### Основен синтаксис:```

```c++

<тип>* <име>;## 2) Памет: Стек и Хийп

```Пойнтърите могат да сочат към променливи, които се намират или в стека, или в хип паметта (heap).



**Примери:**- **Стек** (Stack): Това е областта в паметта, където се съхраняват локални променливи и функциите се извикват по реда на изпълнение. При напускане на функцията, стекът автоматично освобождава заетата памет.

```c++- **Хийп** (Heap): Област за динамично разпределяне на паметта, където можем да заделим и освобождаваме памет ръчно, чрез функции като `new` и `delete` в C++.

int* ptr;     // Pointer to int

double* dPtr; // Pointer to doubleПример за динамично заделяне на памет с пойнтър:

char* cPtr;   // Pointer to char```c++

```int* arr = new int[5];  // Динамично заделен масив от 5 елемента

arr[0] = 1;             // Присвояване на стойности в масива

### Важни оператори:arr[1] = 2;



- **`&` (address-of operator)** - взима адреса на променливаdelete[] arr;           // Освобождаване на паметта след използване

- **`*` (dereference operator)** - достъпва стойността на адреса, към който сочи указателя```



```c++## 3) Пойнтъри и Масиви

int num = 42;Пойнтърите са тясно свързани с масивите. Името на масива действа като пойнтър към първия му елемент.

int* ptr = &num;  // ptr stores the address of num

Пример:

std::cout << num;   // 42 - value of num```c++

std::cout << &num;  // 0x7ffe... - address of numint numbers[3] = {10, 20, 30};

std::cout << ptr;   // 0x7ffe... - same address (stored in ptr)int* p = numbers;      // "numbers" е пойнтър към първия елемент на масива

std::cout << *ptr;  // 42 - value at the address (dereferencing)

```std::cout << *p;       // Извежда 10 (стойността на numbers[0])

std::cout << *(p + 1); // Извежда 20 (стойността на numbers[1])

### Визуализация:```



```Работата с масиви и пойнтъри е бърза и ефективна, особено в комбинация с цикли за достъп и промяна на стойности.

Memory Layout:


Address     Value
0x1000      42         <- num lives here
0x2000      0x1000     <- ptr stores the address of num

ptr = 0x1000  (адресът на num)
*ptr = 42     (стойността на num)
````

## 2) Основни операции с указатели

### Инициализация:

```c++
int x = 10;
int* ptr1 = &x;        // Good - points to x
int* ptr2 = nullptr;   // Good - explicitly null (C++11)
int* ptr3 = NULL;      // OK - old style null
int* ptr4;             // BAD - uninitialized (wild pointer!)
```

### Промяна на стойността чрез указател:

```c++
int num = 5;
int* ptr = &num;

std::cout << num;    // 5
*ptr = 10;           // Change value through pointer
std::cout << num;    // 10 - num is changed!
```

### Сравняване на указатели:

```c++
int x = 10, y = 20;
int* ptr1 = &x;
int* ptr2 = &y;
int* ptr3 = &x;

if(ptr1 == ptr2) { /* false */ }  // Different addresses
if(ptr1 == ptr3) { /* true */ }   // Same address
if(ptr1 == nullptr) { /* false */ }
```

### Пълна програма - пример:

```c++
#include <iostream>

int main() {
    int number = 100;
    int* ptr = &number;

    std::cout << "Value of number: " << number << std::endl;
    std::cout << "Address of number: " << &number << std::endl;
    std::cout << "Value stored in ptr: " << ptr << std::endl;
    std::cout << "Value pointed by ptr: " << *ptr << std::endl;

    // Change value through pointer
    *ptr = 200;
    std::cout << "New value of number: " << number << std::endl;

    return 0;
}
```

**Изход:**

```
Value of number: 100
Address of number: 0x7ffee4b3a8ac
Value stored in ptr: 0x7ffee4b3a8ac
Value pointed by ptr: 100
New value of number: 200
```

## 3) Указатели и масиви

Масивите и указателите са тясно свързани в C++. Името на масива всъщност е **константен указател** към първия му елемент.

### Основна връзка:

```c++
int arr[5] = {10, 20, 30, 40, 50};
int* ptr = arr;  // arr is automatically converted to pointer to first element

// These are equivalent:
std::cout << arr[0];    // 10
std::cout << *arr;      // 10
std::cout << *ptr;      // 10
std::cout << ptr[0];    // 10
```

### Pointer Arithmetic (аритметика с указатели):

При работа с указатели можем да извършваме аритметични операции:

```c++
int arr[5] = {10, 20, 30, 40, 50};
int* ptr = arr;

std::cout << *ptr;       // 10 (first element)
std::cout << *(ptr + 1); // 20 (second element)
std::cout << *(ptr + 2); // 30 (third element)

ptr++;  // Move to next element
std::cout << *ptr;       // 20
```

### Важно за паметта:

Когато правим `ptr + 1`, указателят **НЕ** се увеличава с 1 байт, а с `sizeof(int)` байта (обикновено 4):

```
Memory Layout for int arr[5]:

Address    Value
0x1000     10     <- ptr (arr[0])
0x1004     20     <- ptr + 1 (arr[1])
0x1008     30     <- ptr + 2 (arr[2])
0x100C     40     <- ptr + 3 (arr[3])
0x1010     50     <- ptr + 4 (arr[4])

Each int takes 4 bytes!
```

### Обхождане на масив с указател:

```c++
int arr[5] = {1, 2, 3, 4, 5};

// Method 1: Index notation
for(int i = 0; i < 5; i++) {
    std::cout << arr[i] << " ";
}

// Method 2: Pointer arithmetic
for(int* ptr = arr; ptr < arr + 5; ptr++) {
    std::cout << *ptr << " ";
}

// Method 3: Pointer with offset
int* ptr = arr;
for(int i = 0; i < 5; i++) {
    std::cout << *(ptr + i) << " ";
}
```

## 4) Динамично управление на паметта

Една от най-мощните приложения на указателите е динамичното заделяне на памет.

### Stack vs Heap:

```
STACK (Автоматична памет):
- Бърза алокация/деалокация
- Ограничен размер (~1-8 MB)
- Автоматично се почиства при излизане от scope
- Използва се за локални променливи

HEAP (Динамична памет):
- По-бавна алокация/деалокация
- Много по-голям размер (до достъпната RAM)
- Ръчно управление (new/delete)
- Използва се за големи структури и динамични данни
```

### Операторите new и delete:

#### Заделяне на единична променлива:

```c++
// Allocate memory
int* ptr = new int;       // Allocate space for one int
*ptr = 42;                // Assign value

std::cout << *ptr;        // 42

// Free memory
delete ptr;               // MUST free memory manually!
ptr = nullptr;            // Good practice - avoid dangling pointer
```

#### Заделяне на масив:

```c++
// Allocate array
int size = 10;
int* arr = new int[size];  // Allocate array of 10 ints

// Use array
for(int i = 0; i < size; i++) {
    arr[i] = i * 10;
}

// Free array memory
delete[] arr;              // MUST use delete[] for arrays!
arr = nullptr;
```

### Пълна програма - пример с динамична памет:

```c++
#include <iostream>

int main() {
    int n;
    std::cout << "Enter array size: ";
    std::cin >> n;

    // Dynamic allocation
    int* arr = new int[n];

    // Input values
    std::cout << "Enter " << n << " numbers: ";
    for(int i = 0; i < n; i++) {
        std::cin >> arr[i];
    }

    // Calculate sum
    int sum = 0;
    for(int i = 0; i < n; i++) {
        sum += arr[i];
    }

    std::cout << "Sum: " << sum << std::endl;

    // Clean up
    delete[] arr;
    arr = nullptr;

    return 0;
}
```

### Защо е важно delete?

Когато не освободим паметта, получаваме **memory leak** (изтичане на памет):

```c++
void badFunction() {
    int* ptr = new int[1000];
    // ... use ptr ...
    // OOPS! Forgot to delete[] ptr
}  // Memory is leaked - cannot be used anymore!

// If we call this function 1000 times, we lose ~4 MB of memory!
```

## 5) Указатели към указатели

Можем да имаме указатели, които сочат към други указатели:

```c++
int num = 42;
int* ptr = &num;          // Pointer to int
int** pptr = &ptr;        // Pointer to pointer to int

std::cout << num;         // 42 - value
std::cout << *ptr;        // 42 - dereference once
std::cout << **pptr;      // 42 - dereference twice
```

### Визуализация:

```
Memory Layout:

Address    Value
0x1000     42           <- num
0x2000     0x1000       <- ptr (points to num)
0x3000     0x2000       <- pptr (points to ptr)

num = 42
*ptr = 42
**pptr = 42
```

### Приложение - динамична 2D матрица:

```c++
int rows = 3, cols = 4;

// Allocate array of pointers
int** matrix = new int*[rows];

// Allocate each row
for(int i = 0; i < rows; i++) {
    matrix[i] = new int[cols];
}

// Use matrix
matrix[0][0] = 10;
matrix[1][2] = 25;

// Clean up - IMPORTANT ORDER!
for(int i = 0; i < rows; i++) {
    delete[] matrix[i];  // First delete rows
}
delete[] matrix;  // Then delete array of pointers
```

## 6) Указатели и функции

Указателите ни позволяват да променяме параметри на функции и да работим ефективно с големи данни.

### Предаване на указател на функция:

```c++
void doubleValue(int* ptr) {
    *ptr = *ptr * 2;  // Modify the original value
}

int main() {
    int num = 5;
    doubleValue(&num);
    std::cout << num;  // 10 - original value changed!
    return 0;
}
```

### Връщане на указател от функция:

```c++
int* createArray(int size) {
    int* arr = new int[size];  // Allocate on heap
    for(int i = 0; i < size; i++) {
        arr[i] = i;
    }
    return arr;  // Return pointer to dynamically allocated memory
}

int main() {
    int* myArray = createArray(5);

    for(int i = 0; i < 5; i++) {
        std::cout << myArray[i] << " ";  // 0 1 2 3 4
    }

    delete[] myArray;  // Don't forget to free!
    return 0;
}
```

### Swap функция с указатели:

```c++
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 10, y = 20;
    std::cout << "Before: x=" << x << ", y=" << y << std::endl;

    swap(&x, &y);

    std::cout << "After: x=" << x << ", y=" << y << std::endl;
    return 0;
}
```

**Изход:**

```
Before: x=10, y=20
After: x=20, y=10
```

## 7) Const и указатели

Има различни начини да използваме `const` с указатели:

### 1. Указател към константа (pointer to const):

```c++
int num = 10;
const int* ptr = &num;  // Can't modify *ptr

std::cout << *ptr;  // OK - can read
*ptr = 20;          // ERROR! Can't modify value through ptr
ptr = &other;       // OK - can point to different address
```

### 2. Константен указател (const pointer):

```c++
int num = 10;
int* const ptr = &num;  // Can't change ptr address

*ptr = 20;          // OK - can modify value
ptr = &other;       // ERROR! Can't point to different address
```

### 3. Константен указател към константа:

```c++
int num = 10;
const int* const ptr = &num;  // Can't change anything!

std::cout << *ptr;  // OK - can read
*ptr = 20;          // ERROR! Can't modify value
ptr = &other;       // ERROR! Can't change address
```

### Мнемоника:

Четете отдясно наляво:

- `const int* ptr` - ptr е указател към const int
- `int* const ptr` - ptr е const указател към int
- `const int* const ptr` - ptr е const указател към const int

## 8) Често срещани грешки с указатели

### ❌ Грешка 1: Дериференциране на nullptr

```c++
int* ptr = nullptr;
std::cout << *ptr;  // CRASH! Dereferencing null pointer
```

**Решение:**

```c++
int* ptr = nullptr;
if(ptr != nullptr) {
    std::cout << *ptr;  // Safe
}
```

### ❌ Грешка 2: Wild pointer (неинициализиран указател)

```c++
int* ptr;  // Contains garbage address!
*ptr = 10; // CRASH! Writing to random memory
```

**Решение:**

```c++
int* ptr = nullptr;  // Always initialize!
```

### ❌ Грешка 3: Dangling pointer (висящ указател)

```c++
int* ptr = new int(10);
delete ptr;
std::cout << *ptr;  // UNDEFINED BEHAVIOR! Accessing freed memory
```

**Решение:**

```c++
int* ptr = new int(10);
delete ptr;
ptr = nullptr;  // Set to null after delete
if(ptr != nullptr) {
    std::cout << *ptr;
}
```

### ❌ Грешка 4: Memory leak

```c++
void function() {
    int* ptr = new int[1000];
    // Forgot delete[] ptr;
}  // Memory leaked!
```

**Решение:**

```c++
void function() {
    int* ptr = new int[1000];
    // ... use ptr ...
    delete[] ptr;
    ptr = nullptr;
}
```

### ❌ Грешка 5: Използване на delete вместо delete[]

```c++
int* arr = new int[10];
delete arr;  // WRONG! Use delete[] for arrays
```

**Решение:**

```c++
int* arr = new int[10];
delete[] arr;  // CORRECT!
```

### ❌ Грешка 6: Връщане на указател към локална променлива

```c++
int* badFunction() {
    int num = 42;
    return &num;  // DANGER! num is destroyed after return
}

int main() {
    int* ptr = badFunction();
    std::cout << *ptr;  // UNDEFINED BEHAVIOR!
}
```

**Решение:**

```c++
int* goodFunction() {
    int* num = new int(42);  // Allocate on heap
    return num;
}

int main() {
    int* ptr = goodFunction();
    std::cout << *ptr;
    delete ptr;  // Remember to free!
}
```

### ❌ Грешка 7: Двойно изтриване (double delete)

```c++
int* ptr = new int(10);
delete ptr;
delete ptr;  // ERROR! Already deleted
```

**Решение:**

```c++
int* ptr = new int(10);
delete ptr;
ptr = nullptr;  // Deleting nullptr is safe (does nothing)
delete ptr;     // OK now
```

## 9) Напреднали техники и pattern-и

### Техника 1: Функция за размяна (swap)

```c++
template<typename T>
void swap(T* a, T* b) {
    T temp = *a;
    *a = *b;
    *b = temp;
}
```

### Техника 2: Динамично преоразмеряване на масив

```c++
int* resize(int* oldArr, int oldSize, int newSize) {
    int* newArr = new int[newSize];

    int copySize = (oldSize < newSize) ? oldSize : newSize;
    for(int i = 0; i < copySize; i++) {
        newArr[i] = oldArr[i];
    }

    delete[] oldArr;
    return newArr;
}
```

### Техника 3: Smart Pointers (C++11 и по-нови)

Вместо ръчно управление, използвайте smart pointers:

```c++
#include <memory>

// Unique pointer - single ownership
std::unique_ptr<int> ptr1 = std::make_unique<int>(42);
// No need to delete - automatic cleanup!

// Shared pointer - shared ownership
std::shared_ptr<int> ptr2 = std::make_shared<int>(100);
std::shared_ptr<int> ptr3 = ptr2;  // Both point to same memory
// Memory freed when last shared_ptr is destroyed
```

### Pattern: RAII (Resource Acquisition Is Initialization)

```c++
class IntArray {
private:
    int* data;
    int size;
public:
    IntArray(int n) : size(n) {
        data = new int[n];
    }

    ~IntArray() {
        delete[] data;  // Automatic cleanup!
    }

    int& operator[](int index) {
        return data[index];
    }
};

// Usage:
void function() {
    IntArray arr(100);
    arr[0] = 10;
    // No need to manually delete - destructor does it!
}
```

## 10) Указатели и структури

Указателите са особено полезни при работа със структури:

```c++
struct Student {
    char name[50];
    int age;
    double grade;
};

int main() {
    // Stack allocation
    Student s1 = {"Ivan", 20, 5.5};

    // Heap allocation
    Student* ptr = new Student;

    // Access members
    (*ptr).age = 21;      // Dereference then access
    ptr->age = 21;        // Arrow operator (preferred)
    ptr->grade = 5.75;

    std::cout << "Age: " << ptr->age << std::endl;
    std::cout << "Grade: " << ptr->grade << std::endl;

    delete ptr;
    return 0;
}
```

### Динамичен масив от структури:

```c++
int n = 5;
Student* students = new Student[n];

for(int i = 0; i < n; i++) {
    students[i].age = 20 + i;
    students[i].grade = 5.0 + i * 0.1;
}

delete[] students;
```

## 11) Съвети за добра практика

1. **Винаги инициализирайте указателите**

   ```c++
   int* ptr = nullptr;  // Good
   int* ptr;            // Bad - wild pointer!
   ```

2. **Проверявайте за nullptr преди дериференциране**

   ```c++
   if(ptr != nullptr) {
       *ptr = 10;
   }
   ```

3. **Задавайте nullptr след delete**

   ```c++
   delete ptr;
   ptr = nullptr;
   ```

4. **Използвайте delete[] за масиви**

   ```c++
   int* arr = new int[10];
   delete[] arr;  // Not delete arr!
   ```

5. **Избягвайте memory leaks**

   ```c++
   // Every new must have corresponding delete
   int* ptr = new int(10);
   // ... use ptr ...
   delete ptr;
   ```

6. **Предпочитайте smart pointers**

   ```c++
   std::unique_ptr<int> ptr = std::make_unique<int>(42);
   // Automatic cleanup!
   ```

7. **Внимавайте с pointer arithmetic**

   ```c++
   int arr[5] = {1, 2, 3, 4, 5};
   int* ptr = arr;
   // ptr + i is valid only for 0 <= i < 5
   ```

8. **Документирайте ownership**
   ```c++
   // Returns pointer - caller must delete!
   int* create() {
       return new int(10);
   }
   ```

## 12) Указатели vs Референции

В C++ имаме и референции, които са алтернатива на указателите:

```c++
int num = 10;

// Pointer
int* ptr = &num;
*ptr = 20;

// Reference
int& ref = num;
ref = 30;
```

### Разлики:

| Характеристика             | Указател                    | Референция             |
| -------------------------- | --------------------------- | ---------------------- |
| Синтаксис                  | `int* ptr`                  | `int& ref`             |
| Задължителна инициализация | Не                          | Да                     |
| Може да е null             | Да                          | Не                     |
| Може да се променя         | Да (може да сочи към друго) | Не (винаги към същото) |
| Дериференциране            | Явно `*ptr`                 | Неявно `ref`           |
| Pointer arithmetic         | Да                          | Не                     |
| Използва се за             | Динамична памет, масиви     | Параметри на функции   |

### Кога използваме указатели:

- Динамично управление на памет
- Масиви и pointer arithmetic
- Когато трябва да е възможно null
- Когато искаме да променяме към какво сочим

### Кога използваме референции:

- Параметри на функции (избягваме копиране)
- Връщане на стойности от функции
- Operator overloading
- Когато null не е валидна опция

## Заключение

Указателите са мощно, но и опасно средство в C++. Те ни дават директен контрол над паметта, което е едновременно техните сила и слабост.

### Ключови моменти за запомняне:

✅ Указателят съхранява адрес в паметта  
✅ Използвайте `&` за вземане на адрес и `*` за дериференциране  
✅ Винаги инициализирайте указателите (поне с nullptr)  
✅ Всеки `new` трябва да има съответстващ `delete`  
✅ Използвайте `delete[]` за масиви, заделени с `new[]`  
✅ Задавайте `nullptr` след `delete`  
✅ Предпочитайте smart pointers когато е възможно  
✅ Внимавайте за memory leaks, dangling pointers и undefined behavior

С добро разбиране на указателите, вие имате мощен инструмент за ефективно програмиране, но трябва да го използвате внимателно и отговорно!
