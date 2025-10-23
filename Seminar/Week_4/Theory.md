# **ТЕОРИЯ МАСИВИ**

---

# _КАКВО Е МАСИВ?_

## Основни понятия

Масивът е една от най-фундаменталните структури от данни в програмирането. Той представлява **колекция от еднотипни елементи, разположени последователно в паметта**, обединени под едно общо име.

### Защо са важни масивите?

1. **Организация на данни** - позволяват ни да съхраняваме множество стойности от един и същ тип
2. **Ефективен достъп** - можем да достъпим всеки елемент директно чрез индекс за константно време O(1)
3. **Лесна обработка** - идеални за работа с цикли и алгоритми
4. **Пестене на памет** - елементите са последователно разположени в паметта

## 1) Деклариране и инициализация на масив

### Основен синтаксис:

```c++
<тип> <име>[<големина>];
```

**Пример:**

```c++
int A[20];  // Array of type int with 20 elements named "A"
```

### Важни правила:

- **Индексирането започва от 0** - първият елемент е на индекс 0, последният е на индекс (n-1)
- **Фиксиран размер** - размерът на масива не може да се променя след деклариране
- **Еднотипни елементи** - всички елементи трябва да са от един и същ тип
- **Последователно разположени** - елементите са подредени един след друг в паметта

### Достъп до елементи:

```c++
int A[20];
std::cin >> A[2];   // We read the value for the THIRD cell (because indexing starts from 0)
std::cout << A[2];  // We print the corresponding value

std::cin >> A[20]; // !!! ERROR! Indices are from 0 to 19 (20 in total, but start from 0)
```

### Различни начини за инициализация:

#### 1. Деклариране без инициализация:

```c++
int arr[5];  // Elements contain random values (garbage from memory)
```

#### 2. Инициализация при деклариране:

```c++
int arr[5] = {1, 2, 3, 4, 5};
// arr[0] = 1, arr[1] = 2, ..., arr[4] = 5
```

#### 3. Частична инициализация:

```c++
int arr[5] = {1, 2};
// arr[0] = 1, arr[1] = 2, arr[2] = 0, arr[3] = 0, arr[4] = 0
// Remaining elements are automatically initialized to 0
```

#### 4. Автоматично определяне на размер:

```c++
int arr[] = {1, 2, 3, 4, 5};
// The compiler automatically determines the size (5 elements)
```

#### 5. Инициализация с нули:

```c++
int arr[100] = {0};
// All 100 elements will be initialized to 0
```

#### 6. Инициализация с едно и също число:

```c++
int arr[5] = {7, 7, 7, 7, 7};  // All elements are 7

// CAUTION! The following does NOT work:
int arr[5] = {7};  // Only arr[0] = 7, others are 0!
```

## 2) Работа с масиви и цикли

Масивите са отлична комбинация с цикли. Това ни позволява компактен и четим код:

### Пример - въвеждане и извеждане:

```c++
int A[100];

// Long notation (WRONG approach):
std::cin >> A[0];
std::cin >> A[1];
std::cin >> A[2];
// ...
std::cin >> A[99];  // This is 100 lines of code!

// Short notation (CORRECT approach):
for(int i = 0; i < 100; i++){
    std::cin >> A[i];
}
```

### Пълна програма - пример:

```c++
#include <iostream>

int main(){
    int n = 5;
    int a[n];  // Create array with length n

    // Enter all values in the array with iteration
    std::cout << "Enter " << n << " numbers: ";
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }

    // Print all values with separator ", "
    std::cout << "Array elements: ";
    for(int i = 0; i < n; i++){
        std::cout << a[i];
        if(i < n - 1) std::cout << ", ";  // No comma after last element
    }
    std::cout << std::endl;

    return 0;
}
```

**Вход:**

```
1
2
3
4
5
```

**Изход:**

```
Array elements: 1, 2, 3, 4, 5
```

## 3) Char масиви (C-стринг) и терминираща нула

Символните низове (strings) в C++ могат да се представят като масиви от символи, като последният елемент **ВИНАГИ** трябва да е специалният символ `'\0'` (терминираща нула).

### Защо е нужна терминиращата нула?

Терминиращата нула `'\0'` обозначава **край на низа**. Без нея функциите за работа с низове няма да знаят къде свършва низът и ще продължат да четат паметта отвъд края му, което води до undefined behavior.

### Примери:

#### Автоматично добавяне на '\0':

```c++
char name[] = "FMI";
// The compiler automatically adds '\0' at the end
// Actually the array is: {'F', 'M', 'I', '\0'} - 4 elements!
```

#### Ръчно добавяне на '\0':

```c++
char name[] = {'F', 'M', 'I', '\0'};
// We must manually add '\0' at the end
```

#### ГРЕШКА - липсваща терминираща нула:

```c++
char name[3] = {'F', 'M', 'I'};  // DANGEROUS! Missing '\0'
std::cout << name;  // Will print "FMI" + random garbage from memory
```

### Работа със символни низове:

```c++
#include <iostream>
#include <cstring>  // For strlen, strcpy and others

int main(){
    char text[100];

    std::cout << "Enter your name: ";
    std::cin >> text;  // Reads until first whitespace

    std::cout << "Hello, " << text << "!" << std::endl;
    std::cout << "Your name has " << strlen(text) << " characters." << std::endl;

    return 0;
}
```

### Четене на цял ред с интервали:

```c++
char text[100];
std::cin.getline(text, 100);  // Reads up to 99 characters or until Enter
```

### Полезни функции от <cstring>:

```c++
#include <cstring>

char str1[50] = "Hello";
char str2[50] = "World";

// strlen() - length of string
int len = strlen(str1);  // len = 5

// strcpy() - copy string
strcpy(str1, str2);  // str1 now contains "World"

// strcat() - concatenate strings
strcat(str1, str2);  // str1 now contains "HelloWorld"

// strcmp() - compare strings
int result = strcmp(str1, str2);  // 0 if equal, <0 if str1 < str2, >0 if str1 > str2
```

## 4) Предаване на масиви във функции

Когато предаваме масив на функция, всъщност предаваме **указател към първия елемент**. Това означава, че функцията работи директно с оригиналния масив (не се прави копие).

### Синтаксис:

```c++
void processArray(int arr[], int size) {
    for(int i = 0; i < size; i++) {
        arr[i] *= 2;  // Changes the original array!
    }
}

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    processArray(numbers, 5);
    // numbers now contains: {2, 4, 6, 8, 10}
}
```

### Защо е важно да предаваме размера?

Масивът "не знае" колко елемента има. Когато го предаваме на функция, той се "разпада" до указател, затова **винаги трябва да предаваме и размера**:

```c++
// CORRECT:
void printArray(int arr[], int size) {
    for(int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
}

// WRONG - we cannot determine the size:
void printArray(int arr[]) {
    // sizeof(arr) will give the size of pointer, not array!
}
```

### Пример - намиране на сума:

```c++
int sumArray(int arr[], int size) {
    int sum = 0;
    for(int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    int total = sumArray(numbers, 5);
    std::cout << "Sum: " << total << std::endl;  // Sum: 15
    return 0;
}
```

### Const масиви в функции:

Ако не искаме функцията да променя масива, използваме `const`:

```c++
double average(const int arr[], int size) {
    int sum = 0;
    for(int i = 0; i < size; i++) {
        sum += arr[i];
        // arr[i] = 0; // ERROR! Cannot modify const array
    }
    return static_cast<double>(sum) / size;
}
```

## 5) Многомерни масиви

Можем да създаваме масиви с повече от едно измерение - най-често двумерни масиви (матрици).

### Деклариране и инициализация:

```c++
// Two-dimensional array (matrix) 3x4
int matrix[3][4];  // 3 rows, 4 columns

// Initialization during declaration:
int matrix[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

### Визуализация на двумерен масив:

```
matrix[3][4] изглежда така:

          col 0  col 1  col 2  col 3
row 0  |   [ ]    [ ]    [ ]    [ ]  |
row 1  |   [ ]    [ ]    [ ]    [ ]  |
row 2  |   [ ]    [ ]    [ ]    [ ]  |
```

### Работа с двумерни масиви:

```c++
#include <iostream>

int main(){
    int matrix[3][3];

    // Input:
    std::cout << "Enter 3x3 matrix:\n";
    for(int i = 0; i < 3; i++){
        for(int j = 0; j < 3; j++){
            std::cin >> matrix[i][j];
        }
    }

    // Output:
    std::cout << "Matrix:\n";
    for(int i = 0; i < 3; i++){
        for(int j = 0; j < 3; j++){
            std::cout << matrix[i][j] << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

### Транспониране на матрица:

```c++
void transpose(int mat[3][3], int result[3][3]) {
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            result[j][i] = mat[i][j];
        }
    }
}
```

### Умножение на матрици:

```c++
void multiplyMatrices(int a[3][3], int b[3][3], int result[3][3]) {
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            result[i][j] = 0;
            for(int k = 0; k < 3; k++) {
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }
}
```

## 6) Важни операции с масиви

### Копиране на масив:

```c++
int original[5] = {1, 2, 3, 4, 5};
int copy[5];

// Manual copy
for(int i = 0; i < 5; i++) {
    copy[i] = original[i];
}

// Using memcpy (for built-in types):
#include <cstring>
memcpy(copy, original, 5 * sizeof(int));
```

### Обръщане на масив:

```c++
int arr[5] = {1, 2, 3, 4, 5};
int n = 5;

for(int i = 0; i < n/2; i++) {
    int temp = arr[i];
    arr[i] = arr[n - 1 - i];
    arr[n - 1 - i] = temp;
}
// Result: {5, 4, 3, 2, 1}
```

### Намиране на максимум и минимум:

```c++
int arr[5] = {3, 7, 2, 9, 1};
int n = 5;

int max = arr[0];
int min = arr[0];
int maxIndex = 0;
int minIndex = 0;

for(int i = 1; i < n; i++) {
    if(arr[i] > max) {
        max = arr[i];
        maxIndex = i;
    }
    if(arr[i] < min) {
        min = arr[i];
        minIndex = i;
    }
}
```

### Сумиране на елементи:

```c++
int arr[5] = {1, 2, 3, 4, 5};
int sum = 0;

for(int i = 0; i < 5; i++) {
    sum += arr[i];
}
// sum = 15
```

### Линейно търсене:

```c++
int linearSearch(int arr[], int size, int target) {
    for(int i = 0; i < size; i++) {
        if(arr[i] == target) {
            return i;  // Found! Return index
        }
    }
    return -1;  // Not found
}
```

### Bubble Sort (сортиране):

```c++
void bubbleSort(int arr[], int n) {
    for(int i = 0; i < n - 1; i++) {
        for(int j = 0; j < n - i - 1; j++) {
            if(arr[j] > arr[j + 1]) {
                // Swap
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

### Премахване на дубликати:

```c++
int removeDuplicates(int arr[], int n) {
    if(n == 0 || n == 1) return n;

    int uniqueIndex = 0;
    for(int i = 0; i < n - 1; i++) {
        if(arr[i] != arr[i + 1]) {
            arr[uniqueIndex] = arr[i];
            uniqueIndex++;
        }
    }
    arr[uniqueIndex] = arr[n - 1];
    return uniqueIndex + 1;  // New size
}
```

## 7) Масиви и памет

### Stack vs Heap:

```c++
// Stack allocation (automatic):
int arr[100];  // Fast, automatic cleanup, fixed size

// Heap allocation (dynamic):
int* arr = new int[100];  // Flexible size, manual cleanup required
// ... use array ...
delete[] arr;  // MUST delete manually!
```

### Опасности с динамична памет:

```c++
// Memory leak:
int* arr = new int[100];
// ... forget to delete ...
// Memory is lost!

// Solution:
int* arr = new int[100];
// ... use array ...
delete[] arr;
arr = nullptr;  // Good practice
```

### Размер на масив в паметта:

```c++
int arr[10];
std::cout << sizeof(arr);  // 40 bytes (10 * 4 bytes for int)

double arr2[10];
std::cout << sizeof(arr2);  // 80 bytes (10 * 8 bytes for double)
```

## 8) Често срещани грешки с масиви

### ❌ Грешка 1: Достъп извън границите

```c++
int arr[5] = {1, 2, 3, 4, 5};
std::cout << arr[5];  // ERROR! No element with index 5
std::cout << arr[10]; // ERROR! Far outside bounds
// This is undefined behavior - can crash or give random values
```

### ❌ Грешка 2: Забравена терминираща нула

```c++
char name[3] = {'A', 'B', 'C'};  // Missing '\0'
std::cout << name;  // Undefined behavior!
```

### ❌ Грешка 3: Неинициализиран масив

```c++
int arr[5];
std::cout << arr[0];  // Prints random value (garbage)

// Solution:
int arr[5] = {0};  // Initialize all to 0
```

### ❌ Грешка 4: Предаване на масив като стойност

```c++
// CANNOT BE DONE:
int arr2[5] = arr1;  // ERROR! Cannot copy arrays like this

// Solution:
for(int i = 0; i < 5; i++) {
    arr2[i] = arr1[i];
}
```

### ❌ Грешка 5: Използване на sizeof за предаден масив

```c++
void printSize(int arr[]) {
    std::cout << sizeof(arr);  // Prints size of POINTER, not array!
}

int main() {
    int arr[10];
    std::cout << sizeof(arr);  // 40 bytes (correct)
    printSize(arr);  // 8 bytes (size of pointer) - WRONG!
}
```

### ❌ Грешка 6: Забравяне на delete[] при динамична памет

```c++
void function() {
    int* arr = new int[1000];
    // ... use array ...
    // Forgot to delete[] arr;
}  // Memory leak!
```

### ❌ Грешка 7: Използване на delete вместо delete[]

```c++
int* arr = new int[10];
delete arr;  // WRONG! Should be delete[]
// This causes undefined behavior

int* arr = new int[10];
delete[] arr;  // CORRECT!
```

## 9) Напреднали техники

### Указатели и масиви:

Масивите и указателите са тясно свързани в C++:

```c++
int arr[5] = {1, 2, 3, 4, 5};
int* ptr = arr;  // ptr points to first element

// These are equivalent:
std::cout << arr[0];   // 1
std::cout << ptr[0];   // 1
std::cout << *ptr;     // 1
std::cout << *(arr);   // 1

// Pointer arithmetic:
std::cout << *(ptr + 2);  // 3 (third element)
std::cout << arr[2];      // 3 (same thing)
```

### Константни масиви:

```c++
const int arr[5] = {1, 2, 3, 4, 5};
arr[0] = 10;  // ERROR! Cannot modify const array
```

### Масиви от структури:

```c++
struct Point {
    int x, y;
};

Point points[3] = {{1, 2}, {3, 4}, {5, 6}};

for(int i = 0; i < 3; i++) {
    std::cout << "(" << points[i].x << ", " << points[i].y << ")" << std::endl;
}
```

## 10) Съвети за добра практика

1. **Винаги инициализирайте масивите** - избягвайте работа с неинициализирани данни

   ```c++
   int arr[100] = {0};  // Safe
   ```

2. **Проверявайте границите** - уверете се, че индексът е в диапазона [0, n-1]

   ```c++
   if(index >= 0 && index < size) {
       // Safe to access arr[index]
   }
   ```

3. **Използвайте константи за размера** - вместо магически числа

   ```c++
   const int SIZE = 100;
   int arr[SIZE];
   ```

4. **Предавайте размера на функции** - масивът не "знае" колко е голям

   ```c++
   void process(int arr[], int size) { /* ... */ }
   ```

5. **Внимавайте с char масивите** - винаги добавяйте '\0' в края

   ```c++
   char str[10] = "hello";  // Compiler adds '\0'
   ```

6. **Не забравяйте delete[]** - при динамична памет

   ```c++
   int* arr = new int[100];
   // ... use ...
   delete[] arr;
   ```

7. **Използвайте `std::array` или `std::vector`** - за по-модерен и безопасен код

   ```c++
   #include <array>
   #include <vector>

   std::array<int, 5> arr = {1, 2, 3, 4, 5};  // Fixed size, safer
   std::vector<int> vec = {1, 2, 3, 4, 5};    // Dynamic size, easier
   ```

8. **Документирайте предположенията** - използвайте коментари
   ```c++
   // Assumes: arr is sorted in ascending order
   // Assumes: size > 0
   int binarySearch(int arr[], int size, int target) { /* ... */ }
   ```

## 11) Полезни pattern-и и идиоми

### Pattern 1: Пълнене на масив с определена стойност

```c++
const int SIZE = 100;
int arr[SIZE];
std::fill_n(arr, SIZE, -1);  // Fill with -1
```

### Pattern 2: Two Pointers техника

```c++
// Find pair with given sum
bool findPair(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while(left < right) {
        int sum = arr[left] + arr[right];
        if(sum == target) return true;
        if(sum < target) left++;
        else right--;
    }
    return false;
}
```

### Pattern 3: Sliding Window

```c++
// Maximum sum of k consecutive elements
int maxSumKConsecutive(int arr[], int n, int k) {
    int maxSum = 0;
    for(int i = 0; i < k; i++) {
        maxSum += arr[i];
    }

    int currentSum = maxSum;
    for(int i = k; i < n; i++) {
        currentSum = currentSum + arr[i] - arr[i - k];
        maxSum = std::max(maxSum, currentSum);
    }
    return maxSum;
}
```

## Заключение

Масивите са фундаментална структура от данни в C++. Разбирането на тяхното поведение и ефективна употреба е ключово за всеки програмист. След като овладеете масивите, ще можете да работите с по-сложни структури и алгоритми.

### Ключови моменти за запомняне:

✅ Масивите са последователни блокове от памет  
✅ Индексирането започва от 0  
✅ Винаги предавайте размера заедно с масива на функции  
✅ Char масивите изискват '\0' терминатор  
✅ Не забравяйте delete[] за динамично заделена памет  
✅ Внимавайте за достъп извън границите  
✅ Използвайте модерни алтернативи когато е възможно (std::array, std::vector)

Със солидно разбиране на масивите, вие сте готови да се задълбочите в по-сложни теми като динамично управление на паметта, структури от данни и алгоритми!
