# **ЗАДАЧИ ЗА ВКЪЩИ - СЕДМИЦА 11**

## **Адресна аритметика - Практика за упражнение**

---

## **ЗАДАЧА 1: Извличане на подмасив**

Напишете функция `extractSubarray`, която копира част от масив (от начален до краен индекс) в нов динамичен масив.

```cpp
// Вход:
int arr[10] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
int* subarray = extractSubarray(arr, 2, 5, resultSize);

// Резултат:
subarray: [30, 40, 50, 60]
resultSize: 4

// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

**Прототип:**
```cpp
int* extractSubarray(int* arr, int startIdx, int endIdx, int& resultSize);
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* extractSubarray(int* arr, int startIdx, int endIdx, int& resultSize) {
    resultSize = endIdx - startIdx + 1;
    int* subarray = new int[resultSize];
    
    int* src = arr + startIdx;
    int* dest = subarray;
    int* end = arr + endIdx + 1;
    
    while (src < end) {
        *dest = *src;
        src++;
        dest++;
    }
    
    return subarray;
}

// Използване:
int main() {
    int arr[10] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
    int size;
    int* sub = extractSubarray(arr, 2, 5, size);
    
    for (int i = 0; i < size; i++) {
        std::cout << *(sub + i) << " ";
    }
    std::cout << std::endl;
    
    delete[] sub;
    return 0;
}
```

</details>

---

## **ЗАДАЧА 2: Merge на два сортирани масива**

Напишете функция `mergeSorted`, която обединява два сортирани масива в един сортиран масив използвайки указатели.

```cpp
// Вход:
int arr1[4] = {1, 3, 5, 7};
int arr2[5] = {2, 4, 6, 8, 10};
int* merged = mergeSorted(arr1, 4, arr2, 5);

// Резултат:
[1, 2, 3, 4, 5, 6, 7, 8, 10]

// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* mergeSorted(int* arr1, int size1, int* arr2, int size2) {
    int totalSize = size1 + size2;
    int* result = new int[totalSize];
    
    int* ptr1 = arr1;
    int* ptr2 = arr2;
    int* resPtr = result;
    
    int* end1 = arr1 + size1;
    int* end2 = arr2 + size2;
    
    // Докато има елементи и в двата масива
    while (ptr1 < end1 && ptr2 < end2) {
        if (*ptr1 <= *ptr2) {
            *resPtr = *ptr1;
            ptr1++;
        } else {
            *resPtr = *ptr2;
            ptr2++;
        }
        resPtr++;
    }
    
    // Копираме останалите от arr1
    while (ptr1 < end1) {
        *resPtr = *ptr1;
        ptr1++;
        resPtr++;
    }
    
    // Копираме останалите от arr2
    while (ptr2 < end2) {
        *resPtr = *ptr2;
        ptr2++;
        resPtr++;
    }
    
    return result;
}
```

</details>

---

## **ЗАДАЧА 3: Премахване на дублирани елементи**

Напишете функция `removeDuplicates`, която премахва дублираните елементи от **сортиран** масив и връща новия размер. Работете на място (in-place).

```cpp
// Вход:
int arr[10] = {1, 1, 2, 2, 2, 3, 4, 4, 5, 5};
int newSize = removeDuplicates(arr, 10);

// След изпълнение:
arr: [1, 2, 3, 4, 5, ?, ?, ?, ?, ?]
newSize: 5
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int removeDuplicates(int* arr, int size) {
    if (size <= 1) return size;
    
    int* write = arr + 1;  // Къде пишем уникални елементи
    int* read = arr + 1;   // Откъде четем
    int* end = arr + size;
    
    while (read < end) {
        // Ако текущият елемент е различен от предишния уникален
        if (*read != *(write - 1)) {
            *write = *read;
            write++;
        }
        read++;
    }
    
    return write - arr;  // Нов размер
}
```

**Визуализация:**

```
Начало: [1, 1, 2, 2, 2, 3, 4, 4, 5, 5]
        write
        read
         ↓

Стъпка 1: read = 1 (дубликат) → skip
        [1, 1, 2, 2, 2, 3, 4, 4, 5, 5]
            write
               read

Стъпка 2: read = 2 (уникален) → copy
        [1, 2, 2, 2, 2, 3, 4, 4, 5, 5]
               write
                  read

... и т.н.
```

</details>

---

## **ЗАДАЧА 4: Разделяне на четни и нечетни**

Напишете функция `partitionEvenOdd`, която пренарежда масива така, че всички четни числа да са в началото, а нечетните - в края. Връща броя на четните числа.

```cpp
// Вход:
int arr[8] = {1, 2, 3, 4, 5, 6, 7, 8};
int evenCount = partitionEvenOdd(arr, 8);

// Възможен резултат:
arr: [2, 4, 6, 8, 1, 3, 5, 7]  (или друг ред)
evenCount: 4
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int partitionEvenOdd(int* arr, int size) {
    int* left = arr;
    int* right = arr + size - 1;
    
    while (left < right) {
        // Търсим нечетно число отляво
        while (left < right && *left % 2 == 0) {
            left++;
        }
        
        // Търсим четно число отдясно
        while (left < right && *right % 2 == 1) {
            right--;
        }
        
        // Разменяме ги
        if (left < right) {
            int temp = *left;
            *left = *right;
            *right = temp;
            left++;
            right--;
        }
    }
    
    // Броим четните
    int count = 0;
    for (int* ptr = arr; ptr < arr + size; ptr++) {
        if (*ptr % 2 == 0) {
            count++;
        } else {
            break;  // След първото нечетно спираме
        }
    }
    
    return count;
}
```

</details>

---

## **ЗАДАЧА 5: Ротация на масив**

Напишете функция `rotateLeft`, която ротира масив наляво с `k` позиции използвайки указатели.

```cpp
// Вход:
int arr[6] = {1, 2, 3, 4, 5, 6};
rotateLeft(arr, 6, 2);

// След изпълнение:
[3, 4, 5, 6, 1, 2]
```

**Hint:** Използвайте временен масив или алгоритъма за обръщане.

<details>
<summary><b>📝 Решение 1 (с временен масив)</b></summary>

```cpp
void rotateLeft(int* arr, int size, int k) {
    k = k % size;  // Ако k > size
    if (k == 0) return;
    
    int* temp = new int[k];
    
    // Копираме първите k елемента
    for (int i = 0; i < k; i++) {
        *(temp + i) = *(arr + i);
    }
    
    // Местим останалите наляво
    for (int i = k; i < size; i++) {
        *(arr + i - k) = *(arr + i);
    }
    
    // Копираме запазените в края
    for (int i = 0; i < k; i++) {
        *(arr + size - k + i) = *(temp + i);
    }
    
    delete[] temp;
}
```

</details>

<details>
<summary><b>📝 Решение 2 (алгоритъм с обръщане)</b></summary>

```cpp
void reverse(int* start, int* end) {
    while (start < end) {
        int temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }
}

void rotateLeft(int* arr, int size, int k) {
    k = k % size;
    if (k == 0) return;
    
    // 1. Обърни първите k елемента
    reverse(arr, arr + k - 1);
    
    // 2. Обърни останалите size - k елемента
    reverse(arr + k, arr + size - 1);
    
    // 3. Обърни целия масив
    reverse(arr, arr + size - 1);
}
```

**Пример:**
```
Начало: [1, 2, 3, 4, 5, 6], k=2

Стъпка 1: [2, 1, 3, 4, 5, 6]  (обърнахме първите 2)
Стъпка 2: [2, 1, 6, 5, 4, 3]  (обърнахме останалите 4)
Стъпка 3: [3, 4, 5, 6, 1, 2]  (обърнахме всички)
```

</details>

---

## **ЗАДАЧА 6: Намиране на втория най-голям елемент**

Напишете функция `findSecondMax`, която намира втория най-голям елемент в масив (различен от максималния).

```cpp
// Вход:
int arr[6] = {10, 50, 30, 70, 20, 70};
int secondMax = findSecondMax(arr, 6);

// Изход:
50
```

**Забележка:** Ако всички елементи са еднакви или има само един уникален, върнете -1 или друга индикация за грешка.

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int findSecondMax(int* arr, int size) {
    if (size < 2) return -1;
    
    int max = INT_MIN;
    int secondMax = INT_MIN;
    
    for (int* ptr = arr; ptr < arr + size; ptr++) {
        if (*ptr > max) {
            secondMax = max;
            max = *ptr;
        } else if (*ptr > secondMax && *ptr < max) {
            secondMax = *ptr;
        }
    }
    
    return (secondMax == INT_MIN) ? -1 : secondMax;
}
```

**Обяснение:**
- Поддържаме два максимума: `max` и `secondMax`
- Ако намерим по-голям от `max`, `max` става `secondMax`
- Ако е между `secondMax` и `max`, обновяваме `secondMax`

</details>

---

## **ЗАДАЧА 7: Интервално сумиране**

Напишете функция `rangeSum`, която изчислява сумата на елементите в даден интервал `[start, end]`.

```cpp
// Вход:
int arr[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int sum = rangeSum(arr, 2, 7);

// Изход:
33  // 3+4+5+6+7+8 = 33
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int rangeSum(int* arr, int start, int end) {
    int sum = 0;
    int* ptr = arr + start;
    int* endPtr = arr + end + 1;
    
    while (ptr < endPtr) {
        sum += *ptr;
        ptr++;
    }
    
    return sum;
}
```

</details>

---

## **ЗАДАЧА 8: Преброй на елементи в интервал**

Напишете функция `countInRange`, която брои колко елемента от масив попадат в интервала `[min, max]`.

```cpp
// Вход:
int arr[10] = {5, 12, 7, 18, 3, 25, 9, 14, 2, 20};
int count = countInRange(arr, 10, 5, 15);

// Изход:
6  // Елементите 5, 12, 7, 9, 14 попадат в [5, 15]
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int countInRange(int* arr, int size, int minVal, int maxVal) {
    int count = 0;
    
    for (int* ptr = arr; ptr < arr + size; ptr++) {
        if (*ptr >= minVal && *ptr <= maxVal) {
            count++;
        }
    }
    
    return count;
}
```

</details>

---

## **ЗАДАЧА 9: Циклична проверка**

Напишете функция `isCyclicShift`, която проверява дали един масив е циклично изместване на друг.

```cpp
// Вход:
int arr1[5] = {1, 2, 3, 4, 5};
int arr2[5] = {3, 4, 5, 1, 2};
bool result = isCyclicShift(arr1, arr2, 5);

// Изход:
true  // arr2 е arr1 ротиран с 2 позиции наляво
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
bool isCyclicShift(int* arr1, int* arr2, int size) {
    // Проверяваме всички възможни ротации
    for (int shift = 0; shift < size; shift++) {
        bool matches = true;
        
        for (int i = 0; i < size; i++) {
            if (*(arr1 + i) != *(arr2 + (i + shift) % size)) {
                matches = false;
                break;
            }
        }
        
        if (matches) return true;
    }
    
    return false;
}
```

**Обяснение:**
- Проверяваме дали arr1 може да се получи от arr2 с някаква ротация
- За всяка възможна ротация `shift`, сравняваме елементите
- `(i + shift) % size` дава циклична индексация

</details>

---

## **ЗАДАЧА 10: Префиксна сума**

Напишете функция `computePrefixSum`, която създава нов масив с префиксни суми. Префиксната сума на позиция `i` е сумата на всички елементи до тази позиция включително.

```cpp
// Вход:
int arr[5] = {1, 2, 3, 4, 5};
int* prefixSum = computePrefixSum(arr, 5);

// Резултат:
[1, 3, 6, 10, 15]
// 1, 1+2, 1+2+3, 1+2+3+4, 1+2+3+4+5

// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* computePrefixSum(int* arr, int size) {
    int* prefixSum = new int[size];
    
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += *(arr + i);
        *(prefixSum + i) = sum;
    }
    
    return prefixSum;
}

// Използване:
int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    int* prefix = computePrefixSum(arr, 5);
    
    for (int i = 0; i < 5; i++) {
        std::cout << *(prefix + i) << " ";
    }
    std::cout << std::endl;
    
    delete[] prefix;
    return 0;
}
```

</details>

---

## **ЗАДАЧА 11: Намиране на k-ти най-малък елемент (с сортиране)**

Напишете функция `findKthSmallest`, която намира k-тия най-малък елемент в масив (без да променя оригиналния масив).

```cpp
// Вход:
int arr[7] = {7, 10, 4, 3, 20, 15, 8};
int kth = findKthSmallest(arr, 7, 3);

// Изход:
7  // Третият най-малък елемент след сортиране: [3, 4, 7, ...]
```

**Hint:** Създайте временно копие и го сортирайте.

<details>
<summary><b>📝 Решение</b></summary>

```cpp
// Помощна функция за сортиране (bubble sort)
void bubbleSort(int* arr, int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (*(arr + j) > *(arr + j + 1)) {
                int temp = *(arr + j);
                *(arr + j) = *(arr + j + 1);
                *(arr + j + 1) = temp;
            }
        }
    }
}

int findKthSmallest(int* arr, int size, int k) {
    if (k < 1 || k > size) return -1;  // Невалидно k
    
    // Създаваме копие
    int* temp = new int[size];
    for (int i = 0; i < size; i++) {
        *(temp + i) = *(arr + i);
    }
    
    // Сортираме
    bubbleSort(temp, size);
    
    // Вземаме k-тия елемент (k-1 индекс)
    int result = *(temp + k - 1);
    
    delete[] temp;
    return result;
}
```

</details>

---

## **ЗАДАЧА 12: Разлика между съседни елементи**

Напишете функция `consecutiveDifference`, която създава нов масив с разликите между съседни елементи.

```cpp
// Вход:
int arr[5] = {10, 20, 15, 30, 25};
int* diff = consecutiveDifference(arr, 5);

// Резултат:
[10, -5, 15, -5]
// 20-10=10, 15-20=-5, 30-15=15, 25-30=-5

// Размерът на резултата е size-1
// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* consecutiveDifference(int* arr, int size, int& resultSize) {
    resultSize = size - 1;
    int* diff = new int[resultSize];
    
    for (int i = 0; i < resultSize; i++) {
        *(diff + i) = *(arr + i + 1) - *(arr + i);
    }
    
    return diff;
}

// Използване:
int main() {
    int arr[5] = {10, 20, 15, 30, 25};
    int size;
    int* diff = consecutiveDifference(arr, 5, size);
    
    for (int i = 0; i < size; i++) {
        std::cout << *(diff + i) << " ";
    }
    std::cout << std::endl;
    
    delete[] diff;
    return 0;
}
```

</details>

---

## **ЗАДАЧА 13: Sliding Window Maximum**

Напишете функция `slidingWindowMax`, която намира максималния елемент във всеки прозорец (window) с фиксиран размер `k`.

```cpp
// Вход:
int arr[8] = {1, 3, -1, -3, 5, 3, 6, 7};
int* result = slidingWindowMax(arr, 8, 3);

// Резултат:
[3, 3, 5, 5, 6, 7]
// Window [1, 3, -1] → max = 3
// Window [3, -1, -3] → max = 3
// Window [-1, -3, 5] → max = 5
// ...

// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
// Помощна функция за намиране на максимум в интервал
int findMaxInRange(int* arr, int start, int end) {
    int max = *(arr + start);
    for (int* ptr = arr + start + 1; ptr <= arr + end; ptr++) {
        if (*ptr > max) {
            max = *ptr;
        }
    }
    return max;
}

int* slidingWindowMax(int* arr, int size, int k, int& resultSize) {
    resultSize = size - k + 1;
    int* result = new int[resultSize];
    
    for (int i = 0; i < resultSize; i++) {
        *(result + i) = findMaxInRange(arr, i, i + k - 1);
    }
    
    return result;
}

// Използване:
int main() {
    int arr[8] = {1, 3, -1, -3, 5, 3, 6, 7};
    int size;
    int* result = slidingWindowMax(arr, 8, 3, size);
    
    for (int i = 0; i < size; i++) {
        std::cout << *(result + i) << " ";
    }
    std::cout << std::endl;
    
    delete[] result;
    return 0;
}
```

</details>

---

## **ЗАДАЧА 14: Двумерен масив с едномерна алокация**

Напишете функция, която създава "двумерен" масив (матрица) като едномерен динамичен блок и го запълва със стойности.

```cpp
// Искаме матрица 3x4:
// [1,  2,  3,  4]
// [5,  6,  7,  8]
// [9, 10, 11, 12]

int* matrix = createMatrix(3, 4);
// Достъп до елемент [i][j]: *(matrix + i * cols + j)
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* createMatrix(int rows, int cols) {
    int* matrix = new int[rows * cols];
    
    int value = 1;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            *(matrix + i * cols + j) = value;
            value++;
        }
    }
    
    return matrix;
}

// Отпечатване:
void printMatrix(int* matrix, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << *(matrix + i * cols + j) << " ";
        }
        std::cout << std::endl;
    }
}

// Използване:
int main() {
    int* matrix = createMatrix(3, 4);
    printMatrix(matrix, 3, 4);
    delete[] matrix;
    return 0;
}
```

**Обяснение на достъпа:**
```
За елемент [i][j]:
Позиция = i * cols + j

Пример за [1][2] в матрица 3x4:
Позиция = 1 * 4 + 2 = 6

Визуално:
Index:  0  1  2  3  4  5  [6] 7  8  9 10 11
Value:  1  2  3  4  5  6  [7] 8  9 10 11 12
```

</details>

---

## **ЗАДАЧА 15: Компресиране на масив (Run-Length Encoding)**

Напишете функция `compress`, която компресира масив от числа в двойки (стойност, брой повторения).

```cpp
// Вход:
int arr[10] = {1, 1, 1, 2, 2, 3, 3, 3, 3, 4};
int* compressed = compress(arr, 10, compressedSize);

// Резултат:
[1, 3, 2, 2, 3, 4, 4, 1]
// 1 се повтаря 3 пъти
// 2 се повтаря 2 пъти
// 3 се повтаря 4 пъти
// 4 се повтаря 1 път
// compressedSize = 8

// ⚠️ НЕ ЗАБРАВЯЙТЕ delete[]!
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* compress(int* arr, int size, int& compressedSize) {
    // В най-лошия случай всеки елемент е уникален
    int* temp = new int[size * 2];
    
    int writeIdx = 0;
    int* ptr = arr;
    int* end = arr + size;
    
    while (ptr < end) {
        int value = *ptr;
        int count = 1;
        
        // Броим последователни повторения
        while (ptr + count < end && *(ptr + count) == value) {
            count++;
        }
        
        // Записваме стойност и брой
        *(temp + writeIdx++) = value;
        *(temp + writeIdx++) = count;
        
        ptr += count;
    }
    
    // Копираме в точно толкова памет, колкото ни трябва
    compressedSize = writeIdx;
    int* compressed = new int[compressedSize];
    for (int i = 0; i < compressedSize; i++) {
        *(compressed + i) = *(temp + i);
    }
    
    delete[] temp;
    return compressed;
}
```

</details>

---

**Успех с домашните! 💪**

_Следваща стъпка: HardTasks.md за още предизвикателства_
