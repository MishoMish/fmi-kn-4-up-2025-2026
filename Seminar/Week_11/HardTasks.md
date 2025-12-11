# **ТРУДНИ ЗАДАЧИ - СЕДМИЦА 11**

## **Адресна аритметика - Експертни предизвикателства**

---

## **ЗАДАЧА 1: QuickSort с адресна аритметика**

Имплементирайте QuickSort алгоритъм, използвайки **само указатели и адресна аритметика**. Не използвайте индекси `[]`.

```cpp
void quickSort(int* left, int* right);
```

**Пример:**
```cpp
int arr[8] = {64, 34, 25, 12, 22, 11, 90, 88};
quickSort(arr, arr + 7);  // right е последният елемент

// След сортиране:
[11, 12, 22, 25, 34, 64, 88, 90]
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
// Помощна функция за разделяне (partition)
int* partition(int* left, int* right) {
    int pivot = *right;  // Избираме последния елемент за pivot
    int* i = left - 1;   // Индекс на по-малкия елемент
    
    for (int* j = left; j < right; j++) {
        if (*j <= pivot) {
            i++;
            // Размяна
            int temp = *i;
            *i = *j;
            *j = temp;
        }
    }
    
    // Поставяме pivot на правилната позиция
    i++;
    int temp = *i;
    *i = *right;
    *right = temp;
    
    return i;
}

void quickSort(int* left, int* right) {
    if (left < right) {
        int* pivot = partition(left, right);
        
        quickSort(left, pivot - 1);    // Сортираме лявата част
        quickSort(pivot + 1, right);   // Сортираме дясната част
    }
}
```

**Визуализация на partition:**
```
Масив: [64, 34, 25, 12, 22, 11, 90, 88]
Pivot: 88

i j
↓ ↓
[64, 34, 25, 12, 22, 11, 90, 88]

След обхождане:
i                              pivot
↓                                ↓
[64, 34, 25, 12, 22, 11, 88, 90]
```

</details>

---

## **ЗАДАЧА 2: Намиране на медиана без сортиране**

Напишете функция, която намира медианата на масив **без да го сортира изцяло**. Използвайте алгоритъма QuickSelect (вариант на QuickSort).

```cpp
double findMedian(int* arr, int size);
```

**Пример:**
```cpp
int arr[7] = {7, 10, 4, 3, 20, 15, 8};
double median = findMedian(arr, 7);

// Изход:
8  // При сортиране: [3, 4, 7, 8, 10, 15, 20], средният е 8
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* partition(int* left, int* right) {
    int pivot = *right;
    int* i = left - 1;
    
    for (int* j = left; j < right; j++) {
        if (*j <= pivot) {
            i++;
            int temp = *i;
            *i = *j;
            *j = temp;
        }
    }
    
    i++;
    int temp = *i;
    *i = *right;
    *right = temp;
    
    return i;
}

int* quickSelect(int* left, int* right, int k) {
    if (left == right) return left;
    
    int* pivotPtr = partition(left, right);
    int pivotIndex = pivotPtr - left;
    
    if (k == pivotIndex) {
        return pivotPtr;
    } else if (k < pivotIndex) {
        return quickSelect(left, pivotPtr - 1, k);
    } else {
        return quickSelect(pivotPtr + 1, right, k - pivotIndex - 1);
    }
}

double findMedian(int* arr, int size) {
    if (size % 2 == 1) {
        // Нечетен брой елементи
        int* medianPtr = quickSelect(arr, arr + size - 1, size / 2);
        return *medianPtr;
    } else {
        // Четен брой елементи
        int* lower = quickSelect(arr, arr + size - 1, size / 2 - 1);
        int* upper = quickSelect(arr, arr + size - 1, size / 2);
        return (*lower + *upper) / 2.0;
    }
}
```

**Обяснение:**
- QuickSelect намира k-тия най-малък елемент за O(n) средно време
- За медиана търсим средния елемент (size/2)

</details>

---

## **ЗАДАЧА 3: Интерсекция и обединение на два сортирани масива**

Напишете функции `intersection` и `unionSorted`, които намират съответно сечението и обединението на два сортирани масива.

```cpp
int* intersection(int* arr1, int size1, int* arr2, int size2, int& resultSize);
int* unionSorted(int* arr1, int size1, int* arr2, int size2, int& resultSize);
```

**Пример:**
```cpp
int arr1[5] = {1, 3, 5, 7, 9};
int arr2[6] = {3, 5, 7, 10, 12, 15};

int size1;
int* inter = intersection(arr1, 5, arr2, 6, size1);
// inter: [3, 5, 7], size1 = 3

int size2;
int* uni = unionSorted(arr1, 5, arr2, 6, size2);
// uni: [1, 3, 5, 7, 9, 10, 12, 15], size2 = 8
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* intersection(int* arr1, int size1, int* arr2, int size2, int& resultSize) {
    int maxSize = (size1 < size2) ? size1 : size2;
    int* temp = new int[maxSize];
    
    int* ptr1 = arr1;
    int* ptr2 = arr2;
    int* end1 = arr1 + size1;
    int* end2 = arr2 + size2;
    int count = 0;
    
    while (ptr1 < end1 && ptr2 < end2) {
        if (*ptr1 == *ptr2) {
            *(temp + count++) = *ptr1;
            ptr1++;
            ptr2++;
        } else if (*ptr1 < *ptr2) {
            ptr1++;
        } else {
            ptr2++;
        }
    }
    
    // Копираме в точния размер
    resultSize = count;
    int* result = new int[resultSize];
    for (int i = 0; i < resultSize; i++) {
        *(result + i) = *(temp + i);
    }
    
    delete[] temp;
    return result;
}

int* unionSorted(int* arr1, int size1, int* arr2, int size2, int& resultSize) {
    int maxSize = size1 + size2;
    int* temp = new int[maxSize];
    
    int* ptr1 = arr1;
    int* ptr2 = arr2;
    int* end1 = arr1 + size1;
    int* end2 = arr2 + size2;
    int count = 0;
    
    while (ptr1 < end1 && ptr2 < end2) {
        if (*ptr1 < *ptr2) {
            *(temp + count++) = *ptr1;
            ptr1++;
        } else if (*ptr1 > *ptr2) {
            *(temp + count++) = *ptr2;
            ptr2++;
        } else {
            // Равни - добавяме само веднъж
            *(temp + count++) = *ptr1;
            ptr1++;
            ptr2++;
        }
    }
    
    // Добавяме останалите от arr1
    while (ptr1 < end1) {
        *(temp + count++) = *ptr1;
        ptr1++;
    }
    
    // Добавяме останалите от arr2
    while (ptr2 < end2) {
        *(temp + count++) = *ptr2;
        ptr2++;
    }
    
    resultSize = count;
    int* result = new int[resultSize];
    for (int i = 0; i < resultSize; i++) {
        *(result + i) = *(temp + i);
    }
    
    delete[] temp;
    return result;
}
```

</details>

---

## **ЗАДАЧА 4: In-place Merge Sort с адресна аритметика**

Имплементирайте Merge Sort, който работи **на място** (без допълнителни големи буфери), използвайки само указатели.

```cpp
void mergeSortInPlace(int* left, int* right);
```

**Забележка:** In-place merge sort е сложен. Опростена версия може да използва малък временен буфер.

<details>
<summary><b>📝 Решение (с малък буфер)</b></summary>

```cpp
void merge(int* left, int* mid, int* right) {
    int leftSize = mid - left + 1;
    int rightSize = right - mid;
    
    // Временни масиви
    int* leftArr = new int[leftSize];
    int* rightArr = new int[rightSize];
    
    // Копираме данните
    for (int i = 0; i < leftSize; i++) {
        *(leftArr + i) = *(left + i);
    }
    for (int i = 0; i < rightSize; i++) {
        *(rightArr + i) = *(mid + 1 + i);
    }
    
    // Merge
    int* l = leftArr;
    int* r = rightArr;
    int* current = left;
    int* leftEnd = leftArr + leftSize;
    int* rightEnd = rightArr + rightSize;
    
    while (l < leftEnd && r < rightEnd) {
        if (*l <= *r) {
            *current = *l;
            l++;
        } else {
            *current = *r;
            r++;
        }
        current++;
    }
    
    while (l < leftEnd) {
        *current = *l;
        l++;
        current++;
    }
    
    while (r < rightEnd) {
        *current = *r;
        r++;
        current++;
    }
    
    delete[] leftArr;
    delete[] rightArr;
}

void mergeSortInPlace(int* left, int* right) {
    if (left < right) {
        int* mid = left + (right - left) / 2;
        
        mergeSortInPlace(left, mid);
        mergeSortInPlace(mid + 1, right);
        merge(left, mid, right);
    }
}
```

</details>

---

## **ЗАДАЧА 5: Разстояние на Hamming за масиви**

Напишете функция, която изчислява Hamming distance между два масива с еднакъв размер (броя на позициите, на които се различават).

```cpp
int hammingDistance(int* arr1, int* arr2, int size);
```

**Пример:**
```cpp
int arr1[5] = {1, 2, 3, 4, 5};
int arr2[5] = {1, 2, 7, 4, 9};
int dist = hammingDistance(arr1, arr2, 5);

// Изход:
2  // Различават се на позиции 2 и 4
```

**Бонус:** Изчислете Hamming distance на ниво битове (за int).

<details>
<summary><b>📝 Решение (основна версия)</b></summary>

```cpp
int hammingDistance(int* arr1, int* arr2, int size) {
    int distance = 0;
    int* end = arr1 + size;
    
    for (; arr1 < end; arr1++, arr2++) {
        if (*arr1 != *arr2) {
            distance++;
        }
    }
    
    return distance;
}
```

</details>

<details>
<summary><b>📝 Решение (битово ниво)</b></summary>

```cpp
int countSetBits(int n) {
    int count = 0;
    while (n) {
        count += n & 1;
        n >>= 1;
    }
    return count;
}

int hammingDistanceBitwise(int* arr1, int* arr2, int size) {
    int distance = 0;
    int* end = arr1 + size;
    
    for (; arr1 < end; arr1++, arr2++) {
        int xorResult = *arr1 ^ *arr2;
        distance += countSetBits(xorResult);
    }
    
    return distance;
}
```

**Обяснение:**
- `arr1[i] ^ arr2[i]` дава битовете, в които се различават
- Броим единиците в XOR резултата

</details>

---

## **ЗАДАЧА 6: Longest Increasing Subsequence (LIS)**

Напишете функция, която намира дължината на най-дългата нарастваща подредица в масив използвайки dynamic programming с указатели.

```cpp
int longestIncreasingSubsequence(int* arr, int size);
```

**Пример:**
```cpp
int arr[8] = {10, 9, 2, 5, 3, 7, 101, 18};
int length = longestIncreasingSubsequence(arr, 8);

// Изход:
4  // [2, 3, 7, 18] или [2, 3, 7, 101] и др.
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int longestIncreasingSubsequence(int* arr, int size) {
    if (size == 0) return 0;
    
    int* dp = new int[size];
    
    // Инициализация
    for (int i = 0; i < size; i++) {
        *(dp + i) = 1;  // Минимална дължина е 1
    }
    
    // Попълване на dp
    for (int i = 1; i < size; i++) {
        for (int j = 0; j < i; j++) {
            if (*(arr + i) > *(arr + j)) {
                if (*(dp + j) + 1 > *(dp + i)) {
                    *(dp + i) = *(dp + j) + 1;
                }
            }
        }
    }
    
    // Намираме максималната дължина
    int maxLength = *dp;
    for (int* ptr = dp + 1; ptr < dp + size; ptr++) {
        if (*ptr > maxLength) {
            maxLength = *ptr;
        }
    }
    
    delete[] dp;
    return maxLength;
}
```

**Обяснение:**
- `dp[i]` съхранява дължината на LIS завършващ на позиция i
- За всяка позиция проверяваме всички предишни и обновяваме

</details>

---

## **ЗАДАЧА 7: 3-Way Partitioning (Dutch National Flag)**

Имплементирайте алгоритъма на Dijkstra за разделяне на масив на три части: елементи < pivot, = pivot, > pivot.

```cpp
void threeWayPartition(int* arr, int size, int pivot);
```

**Пример:**
```cpp
int arr[10] = {4, 9, 4, 4, 1, 9, 4, 4, 9, 4};
threeWayPartition(arr, 10, 4);

// Резултат (възможна подредба):
[1, 4, 4, 4, 4, 4, 4, 9, 9, 9]
//   <4    =4      >4
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
void threeWayPartition(int* arr, int size, int pivot) {
    int* low = arr;              // Край на "< pivot" зоната
    int* mid = arr;              // Текущ елемент за проверка
    int* high = arr + size - 1;  // Начало на "> pivot" зоната
    
    while (mid <= high) {
        if (*mid < pivot) {
            // Размяна с low
            int temp = *low;
            *low = *mid;
            *mid = temp;
            low++;
            mid++;
        } else if (*mid > pivot) {
            // Размяна с high
            int temp = *high;
            *high = *mid;
            *mid = temp;
            high--;
        } else {
            // *mid == pivot
            mid++;
        }
    }
}
```

**Визуализация:**

```
Начало:
[4, 9, 4, 4, 1, 9, 4, 4, 9, 4]
 ↑              ↑              ↑
low           mid           high

След обработка:
[1 | 4, 4, 4, 4, 4, 4 | 9, 9, 9]
   <4      =4         >4
```

</details>

---

## **ЗАДАЧА 8: Namиране на елемент, който се среща > n/2 пъти (Boyer-Moore Voting)**

Напишете функция, която намира мажоритарен елемент (ако съществува) - елемент, който се среща повече от n/2 пъти.

```cpp
int findMajority(int* arr, int size);
```

**Пример:**
```cpp
int arr[7] = {2, 2, 1, 1, 1, 2, 2};
int majority = findMajority(arr, 7);

// Изход:
2  // 2 се среща 4 пъти (> 7/2)
```

**Забележка:** Ако няма мажоритарен елемент, върнете -1.

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int findMajority(int* arr, int size) {
    // Фаза 1: Намиране на кандидат
    int candidate = *arr;
    int count = 1;
    
    for (int* ptr = arr + 1; ptr < arr + size; ptr++) {
        if (count == 0) {
            candidate = *ptr;
            count = 1;
        } else if (*ptr == candidate) {
            count++;
        } else {
            count--;
        }
    }
    
    // Фаза 2: Потвърждаване на кандидата
    count = 0;
    for (int* ptr = arr; ptr < arr + size; ptr++) {
        if (*ptr == candidate) {
            count++;
        }
    }
    
    return (count > size / 2) ? candidate : -1;
}
```

**Обяснение на алгоритъма:**
- Boyer-Moore Voting работи с "гласуване"
- Всеки път като видим кандидата, увеличаваме count
- Всеки път като видим различен, намаляваме count
- Когато count стане 0, сменяме кандидата
- Накрая проверяваме дали кандидатът наистина е мажоритарен

</details>

---

## **ЗАДАЧА 9: Максимална сума на подмасив (Kadane's Algorithm)**

Имплементирайте Kadane's algorithm за намиране на максималната сума на непрекъснат подмасив.

```cpp
int maxSubarraySum(int* arr, int size);
```

**Пример:**
```cpp
int arr[9] = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
int maxSum = maxSubarraySum(arr, 9);

// Изход:
6  // Подмасив [4, -1, 2, 1] има сума 6
```

**Бонус:** Върнете и индексите на подмасива със максимална сума.

<details>
<summary><b>📝 Решение (основна версия)</b></summary>

```cpp
int maxSubarraySum(int* arr, int size) {
    int maxSoFar = *arr;
    int maxEndingHere = *arr;
    
    for (int* ptr = arr + 1; ptr < arr + size; ptr++) {
        maxEndingHere = (*ptr > maxEndingHere + *ptr) ? *ptr : maxEndingHere + *ptr;
        
        if (maxEndingHere > maxSoFar) {
            maxSoFar = maxEndingHere;
        }
    }
    
    return maxSoFar;
}
```

</details>

<details>
<summary><b>📝 Решение (с индекси)</b></summary>

```cpp
struct SubarrayInfo {
    int maxSum;
    int startIdx;
    int endIdx;
};

SubarrayInfo maxSubarraySumWithIndices(int* arr, int size) {
    int maxSoFar = *arr;
    int maxEndingHere = *arr;
    
    int start = 0;
    int end = 0;
    int tempStart = 0;
    
    for (int i = 1; i < size; i++) {
        if (*(arr + i) > maxEndingHere + *(arr + i)) {
            maxEndingHere = *(arr + i);
            tempStart = i;
        } else {
            maxEndingHere = maxEndingHere + *(arr + i);
        }
        
        if (maxEndingHere > maxSoFar) {
            maxSoFar = maxEndingHere;
            start = tempStart;
            end = i;
        }
    }
    
    return {maxSoFar, start, end};
}
```

</details>

---

## **ЗАДАЧА 10: Permutations Generation с указатели**

Напишете функция, която генерира всички пермутации на масив рекурсивно.

```cpp
void generatePermutations(int* arr, int* start, int* end);
```

**Пример:**
```cpp
int arr[3] = {1, 2, 3};
generatePermutations(arr, arr, arr + 2);

// Изход:
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 2, 1]
[3, 1, 2]
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
void printArray(int* arr, int size) {
    for (int i = 0; i < size; i++) {
        std::cout << *(arr + i) << " ";
    }
    std::cout << std::endl;
}

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void generatePermutations(int* arr, int* start, int* end) {
    if (start == end) {
        printArray(arr, end - arr + 1);
        return;
    }
    
    for (int* ptr = start; ptr <= end; ptr++) {
        swap(start, ptr);
        generatePermutations(arr, start + 1, end);
        swap(start, ptr);  // Backtrack
    }
}

// Използване:
int main() {
    int arr[3] = {1, 2, 3};
    generatePermutations(arr, arr, arr + 2);
    return 0;
}
```

**Обяснение:**
- Фиксираме първия елемент и генерираме пермутации на останалите
- Размяна (swap) и връщане назад (backtrack) за всяка позиция

</details>

---

## **ЗАДАЧА 11: Binary Search с адресна аритметика**

Имплементирайте binary search, който връща **указател** към намерения елемент (или nullptr ако не е намерен).

```cpp
int* binarySearch(int* arr, int size, int target);
```

**Пример:**
```cpp
int arr[7] = {1, 3, 5, 7, 9, 11, 13};
int* result = binarySearch(arr, 7, 7);

if (result != nullptr) {
    std::cout << "Found at index: " << (result - arr) << std::endl;
}
```

<details>
<summary><b>📝 Решение (итеративно)</b></summary>

```cpp
int* binarySearch(int* arr, int size, int target) {
    int* left = arr;
    int* right = arr + size - 1;
    
    while (left <= right) {
        int* mid = left + (right - left) / 2;
        
        if (*mid == target) {
            return mid;
        } else if (*mid < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return nullptr;  // Не е намерен
}
```

</details>

<details>
<summary><b>📝 Решение (рекурсивно)</b></summary>

```cpp
int* binarySearchRecursive(int* left, int* right, int target) {
    if (left > right) {
        return nullptr;
    }
    
    int* mid = left + (right - left) / 2;
    
    if (*mid == target) {
        return mid;
    } else if (*mid < target) {
        return binarySearchRecursive(mid + 1, right, target);
    } else {
        return binarySearchRecursive(left, mid - 1, target);
    }
}

int* binarySearch(int* arr, int size, int target) {
    return binarySearchRecursive(arr, arr + size - 1, target);
}
```

</details>

---

## **ЗАДАЧА 12: Matrix Transpose с 1D представяне**

Напишете функция, която транспонира матрица, представена като едномерен масив.

```cpp
int* transposeMatrix(int* matrix, int rows, int cols);
```

**Пример:**
```cpp
// Матрица 3x2:
// [1, 2]
// [3, 4]
// [5, 6]
int matrix[6] = {1, 2, 3, 4, 5, 6};

int* transposed = transposeMatrix(matrix, 3, 2);

// Резултат 2x3:
// [1, 3, 5]
// [2, 4, 6]
// transposed: [1, 3, 5, 2, 4, 6]
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int* transposeMatrix(int* matrix, int rows, int cols) {
    int* transposed = new int[rows * cols];
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            // Елемент [i][j] отива на позиция [j][i]
            // Позиция в оригинала: i * cols + j
            // Позиция в транспонирания: j * rows + i
            *(transposed + j * rows + i) = *(matrix + i * cols + j);
        }
    }
    
    return transposed;
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
```

</details>

---

## **ЗАДАЧА 13: Heap Operations с адресна аритметика**

Имплементирайте основни heap операции (heapify, insert, extractMax) използвайки само адресна аритметика.

```cpp
void heapify(int* arr, int size, int* node);
void heapInsert(int* arr, int& size, int value);
int heapExtractMax(int* arr, int& size);
```

**Пример:**
```cpp
int heap[100];
int size = 0;

heapInsert(heap, size, 10);
heapInsert(heap, size, 20);
heapInsert(heap, size, 5);

int max = heapExtractMax(heap, size);  // 20
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
// Индекси в heap:
// Родител на i: (i-1)/2
// Ляв син на i: 2*i + 1
// Десен син на i: 2*i + 2

void heapify(int* arr, int size, int idx) {
    int largest = idx;
    int left = 2 * idx + 1;
    int right = 2 * idx + 2;
    
    if (left < size && *(arr + left) > *(arr + largest)) {
        largest = left;
    }
    
    if (right < size && *(arr + right) > *(arr + largest)) {
        largest = right;
    }
    
    if (largest != idx) {
        // Размяна
        int temp = *(arr + idx);
        *(arr + idx) = *(arr + largest);
        *(arr + largest) = temp;
        
        heapify(arr, size, largest);
    }
}

void heapInsert(int* arr, int& size, int value) {
    // Добавяме в края
    *(arr + size) = value;
    int current = size;
    size++;
    
    // Bubble up
    while (current > 0) {
        int parent = (current - 1) / 2;
        
        if (*(arr + current) > *(arr + parent)) {
            int temp = *(arr + current);
            *(arr + current) = *(arr + parent);
            *(arr + parent) = temp;
            current = parent;
        } else {
            break;
        }
    }
}

int heapExtractMax(int* arr, int& size) {
    if (size == 0) return -1;  // Грешка
    
    int max = *arr;
    
    // Последният елемент става корен
    *arr = *(arr + size - 1);
    size--;
    
    // Heapify от корена
    heapify(arr, size, 0);
    
    return max;
}
```

</details>

---

## **ЗАДАЧА 14: String Matching (KMP Algorithm) с указатели**

Имплементирайте KMP (Knuth-Morris-Pratt) алгоритъм за търсене на подстринг в стринг.

```cpp
int* computeLPS(char* pattern, int patternLen);
int kmpSearch(char* text, char* pattern);
```

**Пример:**
```cpp
char text[] = "ABABDABACDABABCABAB";
char pattern[] = "ABABCABAB";

int pos = kmpSearch(text, pattern);
// pos = 10 (позиция, на която започва pattern в text)
```

<details>
<summary><b>📝 Решение</b></summary>

```cpp
int stringLength(char* str) {
    int len = 0;
    while (*(str + len) != '\0') {
        len++;
    }
    return len;
}

int* computeLPS(char* pattern, int patternLen) {
    int* lps = new int[patternLen];
    *lps = 0;  // lps[0] е винаги 0
    
    int len = 0;
    int i = 1;
    
    while (i < patternLen) {
        if (*(pattern + i) == *(pattern + len)) {
            len++;
            *(lps + i) = len;
            i++;
        } else {
            if (len != 0) {
                len = *(lps + len - 1);
            } else {
                *(lps + i) = 0;
                i++;
            }
        }
    }
    
    return lps;
}

int kmpSearch(char* text, char* pattern) {
    int textLen = stringLength(text);
    int patternLen = stringLength(pattern);
    
    int* lps = computeLPS(pattern, patternLen);
    
    int i = 0;  // Индекс за text
    int j = 0;  // Индекс за pattern
    
    while (i < textLen) {
        if (*(pattern + j) == *(text + i)) {
            i++;
            j++;
        }
        
        if (j == patternLen) {
            delete[] lps;
            return i - j;  // Намерен на позиция i-j
        } else if (i < textLen && *(pattern + j) != *(text + i)) {
            if (j != 0) {
                j = *(lps + j - 1);
            } else {
                i++;
            }
        }
    }
    
    delete[] lps;
    return -1;  // Не е намерен
}
```

</details>

---

## **ЗАДАЧА 15: Convex Hull (Graham Scan) с адресна аритметика**

Имплементирайте Graham Scan алгоритъм за намиране на convex hull на набор от точки в 2D.

```cpp
struct Point {
    int x, y;
};

Point* convexHull(Point* points, int size, int& hullSize);
```

**Пример:**
```cpp
Point points[5] = {{0, 0}, {1, 1}, {2, 2}, {1, 0}, {0, 1}};
int hullSize;
Point* hull = convexHull(points, 5, hullSize);

// hull съдържа точките на convex hull
```

<details>
<summary><b>📝 Решение (опростена версия)</b></summary>

```cpp
#include <algorithm>
#include <cmath>

struct Point {
    int x, y;
};

// Намиране на най-долната точка (или най-лявата при равенство)
Point* findLowest(Point* points, int size) {
    Point* lowest = points;
    
    for (Point* p = points + 1; p < points + size; p++) {
        if (p->y < lowest->y || (p->y == lowest->y && p->x < lowest->x)) {
            lowest = p;
        }
    }
    
    return lowest;
}

// Cross product за определяне на ориентация
int crossProduct(Point* O, Point* A, Point* B) {
    return (A->x - O->x) * (B->y - O->y) - (A->y - O->y) * (B->x - O->x);
}

// Polar angle sort (опростен)
// В реална имплементация използвайте std::sort с custom comparator

Point* convexHull(Point* points, int size, int& hullSize) {
    if (size < 3) return nullptr;
    
    // 1. Намери най-долната точка
    Point* lowest = findLowest(points, size);
    
    // Размени я с първата
    Point temp = *points;
    *points = *lowest;
    *lowest = temp;
    
    // 2. Сортирай по полярен ъгъл спрямо първата точка
    // (тук трябва да имплементирате сортиране - използвайте QuickSort от предишна задача)
    
    // 3. Graham Scan
    Point* hull = new Point[size];
    int top = 0;
    
    *(hull + top++) = *points;
    *(hull + top++) = *(points + 1);
    *(hull + top++) = *(points + 2);
    
    for (int i = 3; i < size; i++) {
        while (top > 1 && crossProduct(hull + top - 2, hull + top - 1, points + i) <= 0) {
            top--;
        }
        *(hull + top++) = *(points + i);
    }
    
    hullSize = top;
    return hull;
}
```

**Забележка:** Пълната имплементация изисква правилно сортиране по полярен ъгъл, което може да се направи с custom comparator и QuickSort.

</details>

---

**Поздравления! Справихте се с експертните задачи! 🏆**

_Следваща стъпка: Приложете знанията си в реални проекти с динамична памет и структури от данни_
