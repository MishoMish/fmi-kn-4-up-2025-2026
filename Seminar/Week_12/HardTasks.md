# **ТРУДНИ ЗАДАЧИ - СЕДМИЦА 12**

## **Структури, Класове и Enums - Напреднало ниво**

---

## **⚠️ ВАЖНО**

Тези задачи комбинират структури, класове и enums с:

- Динамична памет
- Алгоритми за сортиране и търсене
- Сложни структури от данни
- Обектно-ориентирани принципи

---

## **ЗАДАЧА 1: Клас DynamicArray (Generic Stack/Queue)**

Създайте клас `DynamicIntArray` който:

1. Динамично заделя памет за масив от integers
2. Автоматично се разширява при достигане на капацитета
3. Имплементира методи: `add()`, `remove()`, `insert()`, `get()`, `set()`
4. Има copy constructor и destructor
5. Поддържа операции като стек И опашка

**Допълнителни методи:**

- `push()` / `pop()` - стек операции
- `enqueue()` / `dequeue()` - опашка операции
- `clear()` - изчиства масива
- `resize()` - променя капацитета

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

class DynamicIntArray {
private:
    int* data;
    int capacity;
    int count;

    void resize(int newCapacity) {
        int* newData = new int[newCapacity];

        int elementsToCopy = (count < newCapacity) ? count : newCapacity;
        for (int i = 0; i < elementsToCopy; i++) {
            newData[i] = data[i];
        }

        delete[] data;
        data = newData;
        capacity = newCapacity;

        if (count > newCapacity) {
            count = newCapacity;
        }
    }

public:
    DynamicIntArray(int initialCapacity = 10) {
        capacity = initialCapacity;
        count = 0;
        data = new int[capacity];
    }

    // Copy constructor
    DynamicIntArray(const DynamicIntArray& other) {
        capacity = other.capacity;
        count = other.count;
        data = new int[capacity];

        for (int i = 0; i < count; i++) {
            data[i] = other.data[i];
        }
    }

    ~DynamicIntArray() {
        delete[] data;
    }

    void add(int value) {
        if (count >= capacity) {
            resize(capacity * 2);
        }
        data[count] = value;
        count++;
    }

    bool remove(int index) {
        if (index < 0 || index >= count) {
            return false;
        }

        for (int i = index; i < count - 1; i++) {
            data[i] = data[i + 1];
        }
        count--;

        return true;
    }

    bool insert(int index, int value) {
        if (index < 0 || index > count) {
            return false;
        }

        if (count >= capacity) {
            resize(capacity * 2);
        }

        for (int i = count; i > index; i--) {
            data[i] = data[i - 1];
        }

        data[index] = value;
        count++;

        return true;
    }

    int get(int index) const {
        if (index >= 0 && index < count) {
            return data[index];
        }
        return 0;
    }

    bool set(int index, int value) {
        if (index < 0 || index >= count) {
            return false;
        }
        data[index] = value;
        return true;
    }

    // Stack operations
    void push(int value) {
        add(value);
    }

    bool pop(int& value) {
        if (count == 0) {
            return false;
        }
        value = data[count - 1];
        count--;
        return true;
    }

    // Queue operations
    void enqueue(int value) {
        add(value);
    }

    bool dequeue(int& value) {
        if (count == 0) {
            return false;
        }
        value = data[0];
        remove(0);
        return true;
    }

    void clear() {
        count = 0;
    }

    int size() const {
        return count;
    }

    int getCapacity() const {
        return capacity;
    }

    void display() const {
        std::cout << "[";
        for (int i = 0; i < count; i++) {
            std::cout << data[i];
            if (i < count - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
        std::cout << "Size: " << count << ", Capacity: " << capacity << std::endl;
    }
};

int main() {
    DynamicIntArray arr(3);

    std::cout << "Adding elements:" << std::endl;
    arr.add(10);
    arr.add(20);
    arr.add(30);
    arr.display();

    std::cout << "\nAdding more (will resize):" << std::endl;
    arr.add(40);
    arr.add(50);
    arr.display();

    std::cout << "\nInserting 25 at index 2:" << std::endl;
    arr.insert(2, 25);
    arr.display();

    std::cout << "\nUsing as stack (push/pop):" << std::endl;
    DynamicIntArray stack;
    stack.push(1);
    stack.push(2);
    stack.push(3);
    stack.display();

    int value;
    stack.pop(value);
    std::cout << "Popped: " << value << std::endl;
    stack.display();

    std::cout << "\nUsing as queue (enqueue/dequeue):" << std::endl;
    DynamicIntArray queue;
    queue.enqueue(100);
    queue.enqueue(200);
    queue.enqueue(300);
    queue.display();

    queue.dequeue(value);
    std::cout << "Dequeued: " << value << std::endl;
    queue.display();

    return 0;
}
```

</details>

---

## **ЗАДАЧА 2: Клас StudentDatabase**

Създайте система за управление на студенти с:

**Структура Student:**

- id, name, faculty, specialty, year, grades[]

**Клас StudentDatabase:**

- Динамичен масив от Student
- Методи за добавяне/премахване/търсене
- Сортиране по различни критерии
- Изчисляване на средна оценка
- Филтриране по факултет/специалност

**Функционалности:**

1. `addStudent()` - добавя студент
2. `removeStudent(int id)` - премахва по ID
3. `findStudent(int id)` - намира студент
4. `sortByName()` / `sortByGrade()` / `sortById()`
5. `filterByFaculty(const char* faculty)`
6. `getTopStudents(int n)` - връща топ N студенти

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

struct Student {
    int id;
    char name[100];
    char faculty[50];
    char specialty[50];
    int year;
    double grades[10];
    int gradeCount;

    double getAverageGrade() const {
        if (gradeCount == 0) return 0;

        double sum = 0;
        for (int i = 0; i < gradeCount; i++) {
            sum += grades[i];
        }
        return sum / gradeCount;
    }

    void display() const {
        std::cout << "ID: " << id << std::endl;
        std::cout << "Name: " << name << std::endl;
        std::cout << "Faculty: " << faculty << std::endl;
        std::cout << "Specialty: " << specialty << std::endl;
        std::cout << "Year: " << year << std::endl;
        std::cout << "Average Grade: " << getAverageGrade() << std::endl;
    }
};

class StudentDatabase {
private:
    Student* students;
    int capacity;
    int count;

    void resize() {
        capacity *= 2;
        Student* newStudents = new Student[capacity];

        for (int i = 0; i < count; i++) {
            newStudents[i] = students[i];
        }

        delete[] students;
        students = newStudents;
    }

public:
    StudentDatabase(int initialCapacity = 10) {
        capacity = initialCapacity;
        count = 0;
        students = new Student[capacity];
    }

    ~StudentDatabase() {
        delete[] students;
    }

    void addStudent(const Student& student) {
        if (count >= capacity) {
            resize();
        }
        students[count] = student;
        count++;
    }

    bool removeStudent(int id) {
        int index = -1;
        for (int i = 0; i < count; i++) {
            if (students[i].id == id) {
                index = i;
                break;
            }
        }

        if (index == -1) {
            return false;
        }

        for (int i = index; i < count - 1; i++) {
            students[i] = students[i + 1];
        }
        count--;

        return true;
    }

    Student* findStudent(int id) {
        for (int i = 0; i < count; i++) {
            if (students[i].id == id) {
                return &students[i];
            }
        }
        return nullptr;
    }

    void sortByName() {
        for (int i = 0; i < count - 1; i++) {
            for (int j = 0; j < count - i - 1; j++) {
                if (strcmp(students[j].name, students[j + 1].name) > 0) {
                    Student temp = students[j];
                    students[j] = students[j + 1];
                    students[j + 1] = temp;
                }
            }
        }
    }

    void sortByGrade() {
        for (int i = 0; i < count - 1; i++) {
            for (int j = 0; j < count - i - 1; j++) {
                if (students[j].getAverageGrade() < students[j + 1].getAverageGrade()) {
                    Student temp = students[j];
                    students[j] = students[j + 1];
                    students[j + 1] = temp;
                }
            }
        }
    }

    void sortById() {
        for (int i = 0; i < count - 1; i++) {
            for (int j = 0; j < count - i - 1; j++) {
                if (students[j].id > students[j + 1].id) {
                    Student temp = students[j];
                    students[j] = students[j + 1];
                    students[j + 1] = temp;
                }
            }
        }
    }

    void filterByFaculty(const char* faculty) const {
        std::cout << "Students from " << faculty << ":" << std::endl;
        bool found = false;

        for (int i = 0; i < count; i++) {
            if (strcmp(students[i].faculty, faculty) == 0) {
                students[i].display();
                std::cout << std::endl;
                found = true;
            }
        }

        if (!found) {
            std::cout << "No students found." << std::endl;
        }
    }

    void getTopStudents(int n) const {
        if (n > count) n = count;

        // Копираме и сортираме
        Student* temp = new Student[count];
        for (int i = 0; i < count; i++) {
            temp[i] = students[i];
        }

        // Сортираме по оценка (descending)
        for (int i = 0; i < count - 1; i++) {
            for (int j = 0; j < count - i - 1; j++) {
                if (temp[j].getAverageGrade() < temp[j + 1].getAverageGrade()) {
                    Student swap = temp[j];
                    temp[j] = temp[j + 1];
                    temp[j + 1] = swap;
                }
            }
        }

        std::cout << "Top " << n << " students:" << std::endl;
        for (int i = 0; i < n; i++) {
            temp[i].display();
            std::cout << std::endl;
        }

        delete[] temp;
    }

    void displayAll() const {
        if (count == 0) {
            std::cout << "No students in database." << std::endl;
            return;
        }

        for (int i = 0; i < count; i++) {
            students[i].display();
            std::cout << std::endl;
        }
    }

    int getCount() const {
        return count;
    }
};

int main() {
    StudentDatabase db;

    Student s1 = {101, "Alice Johnson", "FMI", "Computer Science", 2, {5.5, 6.0, 5.8}, 3};
    Student s2 = {102, "Bob Smith", "FMI", "Software Engineering", 3, {5.0, 5.5, 5.2}, 3};
    Student s3 = {103, "Charlie Brown", "FKSU", "Mathematics", 1, {6.0, 5.8, 5.9}, 3};
    Student s4 = {104, "Diana Prince", "FMI", "Informatics", 2, {5.7, 5.9, 6.0}, 3};

    db.addStudent(s1);
    db.addStudent(s2);
    db.addStudent(s3);
    db.addStudent(s4);

    std::cout << "All students:" << std::endl;
    db.displayAll();

    std::cout << "\n--- Sorted by grade ---" << std::endl;
    db.sortByGrade();
    db.displayAll();

    std::cout << "\n--- Top 2 students ---" << std::endl;
    db.getTopStudents(2);

    std::cout << "\n--- Filter by FMI ---" << std::endl;
    db.filterByFaculty("FMI");

    return 0;
}
```

</details>

---

## **ЗАДАЧА 3: Enum State Machine (Светофар + Таймер)**

Създайте система за светофар с автоматична смяна на състоянията:

**Enum TrafficLightState:**

- Red, Yellow, Green, RedYellow (преход)

**Клас TrafficLight:**

- Текущо състояние
- Таймер (колко време остава в текущото състояние)
- Правила за преход между състояния:
  - Red (30s) → RedYellow (3s) → Green (25s) → Yellow (3s) → Red

**Методи:**

- `tick()` - симулира 1 секунда
- `getState()` - връща текущото състояние
- `getTimeRemaining()` - времe до смяна
- `canCross()` - може ли пешеходец да мине

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

enum class TrafficLightState {
    Red,
    RedYellow,
    Green,
    Yellow
};

class TrafficLight {
private:
    TrafficLightState state;
    int timeRemaining;

    void setState(TrafficLightState newState) {
        state = newState;

        switch (state) {
            case TrafficLightState::Red:
                timeRemaining = 30;
                break;
            case TrafficLightState::RedYellow:
                timeRemaining = 3;
                break;
            case TrafficLightState::Green:
                timeRemaining = 25;
                break;
            case TrafficLightState::Yellow:
                timeRemaining = 3;
                break;
        }
    }

    void transition() {
        switch (state) {
            case TrafficLightState::Red:
                setState(TrafficLightState::RedYellow);
                break;
            case TrafficLightState::RedYellow:
                setState(TrafficLightState::Green);
                break;
            case TrafficLightState::Green:
                setState(TrafficLightState::Yellow);
                break;
            case TrafficLightState::Yellow:
                setState(TrafficLightState::Red);
                break;
        }
    }

public:
    TrafficLight() {
        setState(TrafficLightState::Red);
    }

    void tick() {
        timeRemaining--;

        if (timeRemaining <= 0) {
            transition();
        }
    }

    TrafficLightState getState() const {
        return state;
    }

    int getTimeRemaining() const {
        return timeRemaining;
    }

    bool canCross() const {
        return state == TrafficLightState::Red;
    }

    const char* getStateName() const {
        switch (state) {
            case TrafficLightState::Red: return "RED";
            case TrafficLightState::RedYellow: return "RED+YELLOW";
            case TrafficLightState::Green: return "GREEN";
            case TrafficLightState::Yellow: return "YELLOW";
            default: return "UNKNOWN";
        }
    }

    void display() const {
        std::cout << "[" << getStateName() << "] ";
        std::cout << "Time: " << timeRemaining << "s ";
        std::cout << "(Pedestrians: " << (canCross() ? "CROSS" : "WAIT") << ")";
        std::cout << std::endl;
    }
};

int main() {
    TrafficLight light;

    std::cout << "Traffic Light Simulation (90 seconds)" << std::endl;
    std::cout << "======================================" << std::endl;

    for (int time = 0; time < 90; time++) {
        if (time % 5 == 0) {  // Показваме на всеки 5 секунди
            std::cout << "Time " << time << "s: ";
            light.display();
        }

        light.tick();
    }

    std::cout << "\nFull cycle demonstration:" << std::endl;
    TrafficLight demo;

    int totalTime = 0;
    TrafficLightState initialState = demo.getState();

    do {
        std::cout << "T=" << totalTime << "s: ";
        demo.display();
        demo.tick();
        totalTime++;
    } while (demo.getState() != initialState || totalTime <= 1);

    std::cout << "\nFull cycle completed in " << totalTime << " seconds." << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 4: Клас LinkedList (Свързан списък)**

Създайте клас за едносвързан списък от integers:

**Структура Node:**

```cpp
struct Node {
    int data;
    Node* next;
};
```

**Клас LinkedList:**

- private: Node\* head
- Методи:
  - `addFront()` / `addBack()` / `addAt(index)`
  - `removeFront()` / `removeBack()` / `removeAt(index)`
  - `find(value)` - връща индекса
  - `reverse()` - обръща списъка
  - `sort()` - сортира списъка
  - `removeDuplicates()` - премахва дубликати

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

struct Node {
    int data;
    Node* next;

    Node(int value) : data(value), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;
    int count;

public:
    LinkedList() : head(nullptr), count(0) {}

    ~LinkedList() {
        clear();
    }

    void clear() {
        while (head != nullptr) {
            Node* temp = head;
            head = head->next;
            delete temp;
        }
        count = 0;
    }

    void addFront(int value) {
        Node* newNode = new Node(value);
        newNode->next = head;
        head = newNode;
        count++;
    }

    void addBack(int value) {
        Node* newNode = new Node(value);

        if (head == nullptr) {
            head = newNode;
        } else {
            Node* current = head;
            while (current->next != nullptr) {
                current = current->next;
            }
            current->next = newNode;
        }
        count++;
    }

    bool addAt(int index, int value) {
        if (index < 0 || index > count) {
            return false;
        }

        if (index == 0) {
            addFront(value);
            return true;
        }

        Node* newNode = new Node(value);
        Node* current = head;

        for (int i = 0; i < index - 1; i++) {
            current = current->next;
        }

        newNode->next = current->next;
        current->next = newNode;
        count++;

        return true;
    }

    bool removeFront() {
        if (head == nullptr) {
            return false;
        }

        Node* temp = head;
        head = head->next;
        delete temp;
        count--;

        return true;
    }

    bool removeBack() {
        if (head == nullptr) {
            return false;
        }

        if (head->next == nullptr) {
            delete head;
            head = nullptr;
            count--;
            return true;
        }

        Node* current = head;
        while (current->next->next != nullptr) {
            current = current->next;
        }

        delete current->next;
        current->next = nullptr;
        count--;

        return true;
    }

    bool removeAt(int index) {
        if (index < 0 || index >= count || head == nullptr) {
            return false;
        }

        if (index == 0) {
            return removeFront();
        }

        Node* current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current->next;
        }

        Node* temp = current->next;
        current->next = temp->next;
        delete temp;
        count--;

        return true;
    }

    int find(int value) const {
        Node* current = head;
        int index = 0;

        while (current != nullptr) {
            if (current->data == value) {
                return index;
            }
            current = current->next;
            index++;
        }

        return -1;
    }

    void reverse() {
        Node* prev = nullptr;
        Node* current = head;
        Node* next = nullptr;

        while (current != nullptr) {
            next = current->next;
            current->next = prev;
            prev = current;
            current = next;
        }

        head = prev;
    }

    void sort() {
        if (head == nullptr || head->next == nullptr) {
            return;
        }

        // Bubble sort
        for (int i = 0; i < count - 1; i++) {
            Node* current = head;

            for (int j = 0; j < count - i - 1; j++) {
                if (current->data > current->next->data) {
                    int temp = current->data;
                    current->data = current->next->data;
                    current->next->data = temp;
                }
                current = current->next;
            }
        }
    }

    void removeDuplicates() {
        if (head == nullptr) {
            return;
        }

        Node* current = head;

        while (current != nullptr && current->next != nullptr) {
            Node* runner = current;

            while (runner->next != nullptr) {
                if (runner->next->data == current->data) {
                    Node* temp = runner->next;
                    runner->next = runner->next->next;
                    delete temp;
                    count--;
                } else {
                    runner = runner->next;
                }
            }

            current = current->next;
        }
    }

    int size() const {
        return count;
    }

    void display() const {
        if (head == nullptr) {
            std::cout << "List is empty" << std::endl;
            return;
        }

        Node* current = head;
        while (current != nullptr) {
            std::cout << current->data;
            if (current->next != nullptr) {
                std::cout << " -> ";
            }
            current = current->next;
        }
        std::cout << std::endl;
    }
};

int main() {
    LinkedList list;

    std::cout << "Adding elements to back: 10, 20, 30" << std::endl;
    list.addBack(10);
    list.addBack(20);
    list.addBack(30);
    list.display();

    std::cout << "\nAdding 5 to front:" << std::endl;
    list.addFront(5);
    list.display();

    std::cout << "\nAdding 15 at index 2:" << std::endl;
    list.addAt(2, 15);
    list.display();

    std::cout << "\nFinding 20: index = " << list.find(20) << std::endl;

    std::cout << "\nReversing list:" << std::endl;
    list.reverse();
    list.display();

    std::cout << "\nSorting list:" << std::endl;
    list.sort();
    list.display();

    std::cout << "\nAdding duplicates (10, 20):" << std::endl;
    list.addBack(10);
    list.addBack(20);
    list.display();

    std::cout << "\nRemoving duplicates:" << std::endl;
    list.removeDuplicates();
    list.display();

    return 0;
}
```

</details>

---

## **ЗАДАЧА 5: Клас Polynomial (Пълна имплементация)**

Създайте пълна имплементация на клас за полиноми с динамична памет:

**Клас Polynomial:**

- Динамичен масив за коефициентите
- Степен на полинома
- Конструктори, copy constructor, destructor
- Операции: събиране, изваждане, умножение
- Производна и интеграл
- Оценка при дадено x
- Намиране на корени (опционално)

**Формули:**

- Производна: $(x^n)' = n \cdot x^{n-1}$
- Интеграл: $\int x^n dx = \frac{x^{n+1}}{n+1} + C$

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cmath>

class Polynomial {
private:
    double* coefficients;
    int degree;

public:
    Polynomial(int deg = 0) {
        degree = (deg >= 0) ? deg : 0;
        coefficients = new double[degree + 1];

        for (int i = 0; i <= degree; i++) {
            coefficients[i] = 0;
        }
    }

    Polynomial(const Polynomial& other) {
        degree = other.degree;
        coefficients = new double[degree + 1];

        for (int i = 0; i <= degree; i++) {
            coefficients[i] = other.coefficients[i];
        }
    }

    ~Polynomial() {
        delete[] coefficients;
    }

    void setCoefficient(int power, double value) {
        if (power >= 0 && power <= degree) {
            coefficients[power] = value;
        }
    }

    double getCoefficient(int power) const {
        if (power >= 0 && power <= degree) {
            return coefficients[power];
        }
        return 0;
    }

    double evaluate(double x) const {
        double result = 0;
        for (int i = 0; i <= degree; i++) {
            result += coefficients[i] * pow(x, i);
        }
        return result;
    }

    Polynomial add(const Polynomial& other) const {
        int maxDeg = (degree > other.degree) ? degree : other.degree;
        Polynomial result(maxDeg);

        for (int i = 0; i <= maxDeg; i++) {
            double c1 = (i <= degree) ? coefficients[i] : 0;
            double c2 = (i <= other.degree) ? other.coefficients[i] : 0;
            result.coefficients[i] = c1 + c2;
        }

        return result;
    }

    Polynomial subtract(const Polynomial& other) const {
        int maxDeg = (degree > other.degree) ? degree : other.degree;
        Polynomial result(maxDeg);

        for (int i = 0; i <= maxDeg; i++) {
            double c1 = (i <= degree) ? coefficients[i] : 0;
            double c2 = (i <= other.degree) ? other.coefficients[i] : 0;
            result.coefficients[i] = c1 - c2;
        }

        return result;
    }

    Polynomial multiply(const Polynomial& other) const {
        int resultDegree = degree + other.degree;
        Polynomial result(resultDegree);

        for (int i = 0; i <= degree; i++) {
            for (int j = 0; j <= other.degree; j++) {
                result.coefficients[i + j] += coefficients[i] * other.coefficients[j];
            }
        }

        return result;
    }

    Polynomial derivative() const {
        if (degree == 0) {
            return Polynomial(0);
        }

        Polynomial result(degree - 1);

        for (int i = 1; i <= degree; i++) {
            result.coefficients[i - 1] = coefficients[i] * i;
        }

        return result;
    }

    Polynomial integral() const {
        Polynomial result(degree + 1);

        for (int i = 0; i <= degree; i++) {
            result.coefficients[i + 1] = coefficients[i] / (i + 1);
        }
        // coefficients[0] остава 0 (константата на интегриране)

        return result;
    }

    void display() const {
        bool first = true;

        for (int i = degree; i >= 0; i--) {
            if (coefficients[i] != 0) {
                if (!first && coefficients[i] > 0) {
                    std::cout << " + ";
                } else if (coefficients[i] < 0) {
                    std::cout << " - ";
                }

                double absCoef = fabs(coefficients[i]);

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
};

int main() {
    // P1(x) = 3x^2 + 2x - 5
    Polynomial p1(2);
    p1.setCoefficient(0, -5);
    p1.setCoefficient(1, 2);
    p1.setCoefficient(2, 3);

    // P2(x) = x^2 - 1
    Polynomial p2(2);
    p2.setCoefficient(0, -1);
    p2.setCoefficient(2, 1);

    std::cout << "P1(x) = ";
    p1.display();

    std::cout << "P2(x) = ";
    p2.display();

    std::cout << "\nP1(2) = " << p1.evaluate(2) << std::endl;

    Polynomial sum = p1.add(p2);
    std::cout << "\nP1 + P2 = ";
    sum.display();

    Polynomial diff = p1.subtract(p2);
    std::cout << "P1 - P2 = ";
    diff.display();

    Polynomial prod = p1.multiply(p2);
    std::cout << "P1 * P2 = ";
    prod.display();

    Polynomial deriv = p1.derivative();
    std::cout << "\nP1'(x) = ";
    deriv.display();

    Polynomial integ = p1.integral();
    std::cout << "∫P1(x)dx = ";
    integ.display();
    std::cout << " + C" << std::endl;

    return 0;
}
```

</details>

---

## **ЗАДАЧА 6: Game Character System**

Създайте система за герои в игра:

**Enum CharacterClass:**

- Warrior, Mage, Archer, Rogue

**Enum Stat:**

- Health, Mana, Strength, Intelligence, Agility

**Клас Character:**

- name, level, characterClass
- stats (масив или структура)
- inventory (масив от items)
- Методи:
  - `levelUp()` - увеличава ниво и статистики
  - `takeDamage()` / `heal()`
  - `useSkill()` - зависи от класа
  - `addItem()` / `removeItem()`

**Различни бонуси според класа:**

- Warrior: +10 Health, +5 Strength per level
- Mage: +5 Mana, +7 Intelligence per level
- Archer: +7 Health, +6 Agility per level
- Rogue: +6 Health, +4 Agility, +2 Strength per level

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>
#include <cstring>

enum class CharacterClass {
    Warrior,
    Mage,
    Archer,
    Rogue
};

struct Stats {
    int health;
    int maxHealth;
    int mana;
    int maxMana;
    int strength;
    int intelligence;
    int agility;
};

struct Item {
    char name[50];
    int value;
};

class Character {
private:
    char name[100];
    int level;
    CharacterClass charClass;
    Stats stats;
    Item inventory[20];
    int inventoryCount;

    void initializeStats() {
        switch (charClass) {
            case CharacterClass::Warrior:
                stats.maxHealth = 100;
                stats.maxMana = 20;
                stats.strength = 15;
                stats.intelligence = 5;
                stats.agility = 8;
                break;
            case CharacterClass::Mage:
                stats.maxHealth = 60;
                stats.maxMana = 100;
                stats.strength = 5;
                stats.intelligence = 20;
                stats.agility = 7;
                break;
            case CharacterClass::Archer:
                stats.maxHealth = 75;
                stats.maxMana = 40;
                stats.strength = 10;
                stats.intelligence = 8;
                stats.agility = 18;
                break;
            case CharacterClass::Rogue:
                stats.maxHealth = 70;
                stats.maxMana = 50;
                stats.strength = 12;
                stats.intelligence = 10;
                stats.agility = 16;
                break;
        }

        stats.health = stats.maxHealth;
        stats.mana = stats.maxMana;
    }

public:
    Character(const char* n, CharacterClass cc) {
        strcpy(name, n);
        charClass = cc;
        level = 1;
        inventoryCount = 0;
        initializeStats();
    }

    void levelUp() {
        level++;

        switch (charClass) {
            case CharacterClass::Warrior:
                stats.maxHealth += 10;
                stats.strength += 5;
                stats.agility += 2;
                break;
            case CharacterClass::Mage:
                stats.maxMana += 15;
                stats.intelligence += 7;
                stats.maxHealth += 3;
                break;
            case CharacterClass::Archer:
                stats.maxHealth += 7;
                stats.agility += 6;
                stats.strength += 3;
                break;
            case CharacterClass::Rogue:
                stats.maxHealth += 6;
                stats.agility += 4;
                stats.strength += 2;
                stats.intelligence += 1;
                break;
        }

        stats.health = stats.maxHealth;
        stats.mana = stats.maxMana;

        std::cout << name << " leveled up to level " << level << "!" << std::endl;
    }

    void takeDamage(int damage) {
        // Agility влияе на dodge chance
        int dodgeChance = stats.agility / 2;
        if (rand() % 100 < dodgeChance) {
            std::cout << name << " dodged the attack!" << std::endl;
            return;
        }

        stats.health -= damage;
        if (stats.health < 0) {
            stats.health = 0;
        }

        std::cout << name << " took " << damage << " damage. ";
        std::cout << "HP: " << stats.health << "/" << stats.maxHealth << std::endl;
    }

    void heal(int amount) {
        stats.health += amount;
        if (stats.health > stats.maxHealth) {
            stats.health = stats.maxHealth;
        }

        std::cout << name << " healed " << amount << " HP. ";
        std::cout << "HP: " << stats.health << "/" << stats.maxHealth << std::endl;
    }

    void useSkill() {
        switch (charClass) {
            case CharacterClass::Warrior:
                if (stats.mana >= 10) {
                    stats.mana -= 10;
                    int damage = stats.strength * 2;
                    std::cout << name << " uses Power Strike! Deals " << damage << " damage." << std::endl;
                } else {
                    std::cout << "Not enough mana!" << std::endl;
                }
                break;

            case CharacterClass::Mage:
                if (stats.mana >= 20) {
                    stats.mana -= 20;
                    int damage = stats.intelligence * 3;
                    std::cout << name << " casts Fireball! Deals " << damage << " magic damage." << std::endl;
                } else {
                    std::cout << "Not enough mana!" << std::endl;
                }
                break;

            case CharacterClass::Archer:
                if (stats.mana >= 15) {
                    stats.mana -= 15;
                    int damage = stats.agility * 2;
                    std::cout << name << " shoots Arrow Storm! Deals " << damage << " damage." << std::endl;
                } else {
                    std::cout << "Not enough mana!" << std::endl;
                }
                break;

            case CharacterClass::Rogue:
                if (stats.mana >= 12) {
                    stats.mana -= 12;
                    int damage = (stats.agility + stats.strength) * 2;
                    std::cout << name << " performs Backstab! Deals " << damage << " critical damage." << std::endl;
                } else {
                    std::cout << "Not enough mana!" << std::endl;
                }
                break;
        }
    }

    bool addItem(const Item& item) {
        if (inventoryCount >= 20) {
            std::cout << "Inventory is full!" << std::endl;
            return false;
        }

        inventory[inventoryCount] = item;
        inventoryCount++;
        std::cout << "Added " << item.name << " to inventory." << std::endl;

        return true;
    }

    void displayStats() const {
        std::cout << "\n=== " << name << " ===" << std::endl;
        std::cout << "Class: " << getClassName() << std::endl;
        std::cout << "Level: " << level << std::endl;
        std::cout << "HP: " << stats.health << "/" << stats.maxHealth << std::endl;
        std::cout << "Mana: " << stats.mana << "/" << stats.maxMana << std::endl;
        std::cout << "Strength: " << stats.strength << std::endl;
        std::cout << "Intelligence: " << stats.intelligence << std::endl;
        std::cout << "Agility: " << stats.agility << std::endl;
        std::cout << "Inventory: " << inventoryCount << "/20 items" << std::endl;
    }

    const char* getClassName() const {
        switch (charClass) {
            case CharacterClass::Warrior: return "Warrior";
            case CharacterClass::Mage: return "Mage";
            case CharacterClass::Archer: return "Archer";
            case CharacterClass::Rogue: return "Rogue";
            default: return "Unknown";
        }
    }
};

int main() {
    Character warrior("Conan", CharacterClass::Warrior);
    Character mage("Gandalf", CharacterClass::Mage);

    warrior.displayStats();
    mage.displayStats();

    std::cout << "\n--- Level Up ---" << std::endl;
    warrior.levelUp();
    warrior.levelUp();

    std::cout << "\n--- Combat ---" << std::endl;
    warrior.useSkill();
    mage.useSkill();

    warrior.takeDamage(30);
    warrior.heal(15);

    Item sword = {"Iron Sword", 100};
    Item potion = {"Health Potion", 50};

    warrior.addItem(sword);
    warrior.addItem(potion);

    warrior.displayStats();

    return 0;
}
```

</details>

---

## **ЗАДАЧА 7: Binary Tree (Двоично дърво за търсене)**

Имплементирайте двоично дърво за търсене (BST):

**Структура TreeNode:**

```cpp
struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;
};
```

**Клас BinarySearchTree:**

- `insert(value)` - добавя елемент
- `remove(value)` - премахва елемент
- `search(value)` - търси елемент
- `inOrder()` / `preOrder()` / `postOrder()` - обхождания
- `findMin()` / `findMax()`
- `height()` - височина на дървото

<details>
<summary><b>📝 Решение</b></summary>

```cpp
#include <iostream>

struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int value) : data(value), left(nullptr), right(nullptr) {}
};

class BinarySearchTree {
private:
    TreeNode* root;

    TreeNode* insertRec(TreeNode* node, int value) {
        if (node == nullptr) {
            return new TreeNode(value);
        }

        if (value < node->data) {
            node->left = insertRec(node->left, value);
        } else if (value > node->data) {
            node->right = insertRec(node->right, value);
        }

        return node;
    }

    TreeNode* findMin(TreeNode* node) const {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    TreeNode* removeRec(TreeNode* node, int value) {
        if (node == nullptr) {
            return nullptr;
        }

        if (value < node->data) {
            node->left = removeRec(node->left, value);
        } else if (value > node->data) {
            node->right = removeRec(node->right, value);
        } else {
            // Намерихме го
            if (node->left == nullptr) {
                TreeNode* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                TreeNode* temp = node->left;
                delete node;
                return temp;
            }

            // Две деца
            TreeNode* temp = findMin(node->right);
            node->data = temp->data;
            node->right = removeRec(node->right, temp->data);
        }

        return node;
    }

    bool searchRec(TreeNode* node, int value) const {
        if (node == nullptr) {
            return false;
        }

        if (value == node->data) {
            return true;
        } else if (value < node->data) {
            return searchRec(node->left, value);
        } else {
            return searchRec(node->right, value);
        }
    }

    void inOrderRec(TreeNode* node) const {
        if (node != nullptr) {
            inOrderRec(node->left);
            std::cout << node->data << " ";
            inOrderRec(node->right);
        }
    }

    void preOrderRec(TreeNode* node) const {
        if (node != nullptr) {
            std::cout << node->data << " ";
            preOrderRec(node->left);
            preOrderRec(node->right);
        }
    }

    void postOrderRec(TreeNode* node) const {
        if (node != nullptr) {
            postOrderRec(node->left);
            postOrderRec(node->right);
            std::cout << node->data << " ";
        }
    }

    int heightRec(TreeNode* node) const {
        if (node == nullptr) {
            return 0;
        }

        int leftHeight = heightRec(node->left);
        int rightHeight = heightRec(node->right);

        return 1 + ((leftHeight > rightHeight) ? leftHeight : rightHeight);
    }

    void destroyTree(TreeNode* node) {
        if (node != nullptr) {
            destroyTree(node->left);
            destroyTree(node->right);
            delete node;
        }
    }

public:
    BinarySearchTree() : root(nullptr) {}

    ~BinarySearchTree() {
        destroyTree(root);
    }

    void insert(int value) {
        root = insertRec(root, value);
    }

    void remove(int value) {
        root = removeRec(root, value);
    }

    bool search(int value) const {
        return searchRec(root, value);
    }

    void inOrder() const {
        std::cout << "In-order: ";
        inOrderRec(root);
        std::cout << std::endl;
    }

    void preOrder() const {
        std::cout << "Pre-order: ";
        preOrderRec(root);
        std::cout << std::endl;
    }

    void postOrder() const {
        std::cout << "Post-order: ";
        postOrderRec(root);
        std::cout << std::endl;
    }

    int findMinValue() const {
        if (root == nullptr) {
            std::cout << "Tree is empty!" << std::endl;
            return -1;
        }
        TreeNode* minNode = findMin(root);
        return minNode->data;
    }

    int findMaxValue() const {
        if (root == nullptr) {
            std::cout << "Tree is empty!" << std::endl;
            return -1;
        }

        TreeNode* current = root;
        while (current->right != nullptr) {
            current = current->right;
        }
        return current->data;
    }

    int height() const {
        return heightRec(root);
    }
};

int main() {
    BinarySearchTree bst;

    std::cout << "Inserting: 50, 30, 70, 20, 40, 60, 80" << std::endl;
    bst.insert(50);
    bst.insert(30);
    bst.insert(70);
    bst.insert(20);
    bst.insert(40);
    bst.insert(60);
    bst.insert(80);

    bst.inOrder();
    bst.preOrder();
    bst.postOrder();

    std::cout << "\nHeight: " << bst.height() << std::endl;
    std::cout << "Min: " << bst.findMinValue() << std::endl;
    std::cout << "Max: " << bst.findMaxValue() << std::endl;

    std::cout << "\nSearching for 40: " << (bst.search(40) ? "Found" : "Not found") << std::endl;
    std::cout << "Searching for 90: " << (bst.search(90) ? "Found" : "Not found") << std::endl;

    std::cout << "\nRemoving 20" << std::endl;
    bst.remove(20);
    bst.inOrder();

    std::cout << "\nRemoving 30" << std::endl;
    bst.remove(30);
    bst.inOrder();

    std::cout << "\nRemoving 50" << std::endl;
    bst.remove(50);
    bst.inOrder();

    return 0;
}
```

</details>

---

## **🏆 Поздравления!**

Ако сте решили всички задачи, вече владеете:

- ✅ Структури и класове
- ✅ Динамична памет
- ✅ Енумерации (enum class)
- ✅ Обектно-ориентирано програмиране
- ✅ Структури от данни (масиви, списъци, дървета)
- ✅ Алгоритми (сортиране, търсене, обхождане)

**Следваща стъпка:** Наследяване, полиморфизъм, шаблони!
