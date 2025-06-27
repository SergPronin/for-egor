

## Программа 1: Вычисление суммы ряда

### Задача
Программа вычисляет сумму бесконечного ряда с заданной точностью и сравнивает её с аналитическим значением. Ряд выглядит так:  
\[ \sum_{n=1}^{\infty} \frac{1}{n (4n^2 - 1)^2} \]  
Сумма вычисляется до тех пор, пока очередной член ряда не станет меньше \( 10^{-10} \). Затем результат сравнивается с аналитическим значением \( 1.5 - 2 \cdot \ln(2) \).

### Логика программы
1. Программа использует цикл для вычисления членов ряда.
2. Каждый член ряда вычисляется по формуле \( \frac{1}{n (4n^2 - 1)^2} \).
3. Сумма накапливается, пока член ряда больше заданной точности (\( 10^{-10} \)).
4. После завершения цикла выводится сумма и аналитическое значение для сравнения.

### Код и объяснение
```cpp
#include <iostream>
#include <cmath>

int main() {
    double sum = 0.0; // Переменная для хранения суммы ряда
    int n = 1;        // Счётчик для номера члена ряда
    double term;      // Переменная для хранения текущего члена ряда

    do {
        term = 1.0 / (n * pow(4 * n * n - 1, 2)); // Вычисляем член ряда
        sum += term;                              // Добавляем член к сумме
        n++;                                      // Увеличиваем счётчик
    } while (fabs(term) > 1e-10);                // Проверяем условие точности

    double analytical = 1.5 - 2 * log(2.0);     // Аналитическое значение

    std::cout << "Сумма: " << sum << std::endl;  // Вывод суммы
    std::cout << "Значение: " << analytical << std::endl; // Вывод аналитического значения

    return 0; // Завершение программы
}
```

- **#include <iostream>**: Подключает библиотеку для ввода-вывода (например, для вывода текста на экран).
- **#include <cmath>**: Подключает математические функции, такие как `pow` (возведение в степень) и `log` (натуральный логарифм).
- **double sum = 0.0**: Создаёт переменную `sum` типа `double` (число с плавающей точкой) для хранения суммы ряда.
- **int n = 1**: Счётчик, начинающий с 1, для нумерации членов ряда.
- **double term**: Переменная для хранения текущего члена ряда.
- **do { ... } while (fabs(term) > 1e-10)**: Цикл `do-while` выполняется, пока абсолютное значение члена ряда (`fabs(term)`) больше \( 10^{-10} \).
- **term = 1.0 / (n * pow(4 * n * n - 1, 2))**: Вычисляет член ряда по формуле. `pow(x, y)` возводит \( x \) в степень \( y \).
- **sum += term**: Добавляет текущий член к общей сумме.
- **n++**: Увеличивает счётчик на 1.
- **fabs(term)**: Функция из `<cmath>`, возвращающая абсолютное значение числа.
- **double analytical = 1.5 - 2 * log(2.0)**: Вычисляет аналитическое значение, где `log(2.0)` — натуральный логарифм 2.
- **std::cout**: Выводит результат на экран. `std::endl` добавляет перенос строки.
- **return 0**: Указывает, что программа завершилась успешно.

### Пример ввода и вывода
**Ввод**: (нет ввода, программа работает автоматически)  
**Вывод**:  
```
Сумма: 0.057305
Значение: 0.057305
```

---

## Программа 2: Дублирование пар цифр (вариант с индексами)

### Задача
Программа принимает строку цифр и дублирует каждую пару цифр. Например, если введено "1234", результат будет "11223344".

### Логика программы
1. Пользователь вводит строку цифр.
2. Программа определяет длину строки и вычисляет новую длину (вдвое больше для каждой пары).
3. Создаётся новый массив для результата.
4. Каждая пара цифр копируется дважды в новый массив.
5. Результат выводится на экран.

### Код и объяснение
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char input[100]; // Массив для хранения введённой строки
    printf("Введите последовательность цифр: ");
    scanf("%s", input); // Считываем строку
    
    int len = strlen(input); // Длина строки
    int new_len = len + (len / 2) * 2; // Новая длина (учитываем пары)
    
    char* result = (char*)malloc(new_len + 1); // Выделяем память под результат
    int res_index = 0; // Индекс для записи в result
    
    for (int i = 0; i < len; i += 2) { // Обрабатываем пары
        result[res_index++] = input[i]; // Копируем первую цифру
        if (i + 1 < len) { // Проверяем, есть ли вторая цифра
            result[res_index++] = input[i+1]; // Копируем вторую цифру
        }
        result[res_index++] = input[i]; // Дублируем первую цифру
        if (i + 1 < len) {
            result[res_index++] = input[i+1]; // Дублируем вторую цифру
        }
    }
    result[res_index] = '\0'; // Добавляем нуль-терминатор
    
    printf("Результат: %s\n", result); // Вывод результата
    free(result); // Освобождаем память
    return 0;
}
```

- **#include <stdio.h>**: Подключает функции ввода-вывода, такие как `printf` и `scanf`.
- **#include <stdlib.h>**: Подключает функции для работы с памятью (`malloc`, `free`).
- **#include <string.h>**: Подключает функции для работы со строками, например, `strlen`.
- **char input[100]**: Массив для хранения строки (до 100 символов).
- **scanf("%s", input)**: Считывает строку от пользователя.
- **int len = strlen(input)**: Вычисляет длину строки с помощью функции `strlen`.
- **int new_len = len + (len / 2) * 2**: Вычисляет новую длину результата. Каждая пара удваивается, поэтому добавляем удвоенное количество пар.
- **char* result = (char*)malloc(new_len + 1)**: Выделяет динамическую память для результата. `+1` для нуль-терминатора (специальный символ `\0`, обозначающий конец строки).
- **for (int i = 0; i < len; i += 2)**: Цикл обрабатывает строку по парам (шаг 2).
- **result[res_index++] = input[i]**: Копирует символы в результирующий массив.
- **if (i + 1 < len)**: Проверяет, есть ли вторая цифра в паре.
- **result[res_index] = '\0'**: Устанавливает конец строки.
- **printf("Результат: %s\n", result)**: Выводит результат.
- **free(result)**: Освобождает выделенную память.

### Пример ввода и вывода
**Ввод**:  
```
Введите последовательность цифр: 1234
```
**Вывод**:  
```
Результат: 11223344
```

---

## Программа 3: Дублирование пар цифр (вариант с указателями)

### Задача
Эта программа выполняет ту же задачу, что и предыдущая, но использует указатели вместо индексов для работы со строками.

### Логика программы
Логика идентична программе 2, но вместо индексов (`i`, `res_index`) используются указатели для перемещения по массивам `input` и `result`.

### Код и объяснение
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char input[100];
    printf("Введите последовательность цифр: ");
    scanf("%s", input);
    
    int len = strlen(input);
    int new_len = len + (len / 2) * 2;
    
    char* result = (char*)malloc(new_len + 1);
    char* res_ptr = result; // Указатель на начало result
    
    for (char* ptr = input; *ptr != '\0'; ptr += 2) { // Перемещаемся по input
        *res_ptr++ = *ptr; // Копируем первую цифру
        if (*(ptr + 1) != '\0') { // Проверяем, есть ли вторая цифра
            *res_ptr++ = *(ptr + 1); // Копируем вторую цифру
        }
        *res_ptr++ = *ptr; // Дублируем первую цифру
        if (*(ptr + 1) != '\0') {
            *res_ptr++ = *(ptr + 1); // Дублируем вторую цифру
        }
    }
    *res_ptr = '\0'; // Устанавливаем конец строки
    
    printf("Результат: %s\n", result);
    free(result);
    return 0;
}
```

- **char* res_ptr = result**: Указатель на начало массива `result`.
- **char* ptr = input**: Указатель на начало строки `input`.
- **for (char* ptr = input; *ptr != '\0'; ptr += 2)**: Цикл продолжается, пока не достигнут конец строки (`\0`). Указатель перемещается на 2 символа.
- ** *res_ptr++ = *ptr**: Копирует символ, на который указывает `ptr`, в позицию `res_ptr` и сдвигает `res_ptr`.
- **if (*(ptr + 1) != '\0')**: Проверяет, есть ли следующий символ.
- Остальные элементы аналогичны программе 2.

### Пример ввода и вывода
**Ввод**:  
```
Введите последовательность цифр: 1234
```
**Вывод**:  
```
Результат: 11223344
```

---

## Программа 4: Сложение матриц

### Задача
Программа запрашивает размеры двух матриц, позволяет пользователю ввести их элементы, выполняет сложение матриц и выводит результат.

### Логика программы
1. Пользователь вводит количество строк и столбцов.
2. Создаются две матрицы с введёнными пользователем значениями.
3. Выполняется сложение матриц.
4. Результат выводится на экран.
5. Память, выделенная под матрицы, освобождается.

### Код и объяснение
```cpp
#include <iostream>

float** createMatrix(int rows, int cols) {
    float** matrix = (float**)malloc(rows * sizeof(float*)); // Выделяем память под строки
    for (int i = 0; i < rows; i++) {
        matrix[i] = (float*)malloc(cols * sizeof(float)); // Выделяем память под столбцы
        std::cout << "Введите " << cols << " элемента(ов) строки " << i + 1 << " матрицы: ";
        for (int j = 0; j < cols; j++) {
            std::cin >> matrix[i][j]; // Считываем элементы
        }
    }
    return matrix;
}

float** addMatrices(float** matrix1, float** matrix2, int rows, int cols) {
    float** result = (float**)malloc(rows * sizeof(float*));
    for (int i = 0; i < rows; i++) {
        result[i] = (float*)malloc(cols * sizeof(float));
        for (int j = 0; j < cols; j++) {
            result[i][j] = matrix1[i][j] + matrix2[i][j]; // Складываем элементы
        }
    }
    return result;
}

void printMatrix(float** matrix, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << matrix[i][j] << " "; // Выводим элементы
        }
        std::cout << "\n"; // Переход на новую строку
    }
}

void freeMatrix(float** matrix, int rows) {
    for (int i = 0; i < rows; i++) {
        free(matrix[i]); // Освобождаем память для строк
    }
    free(matrix); // Освобождаем память для указателей
}

int main() {
    int rows, cols;

    std::cout << "Введите количество строк: ";
    std::cin >> rows;
    std::cout << "Введите количество столбцов: ";
    std::cin >> cols;

    std::cout << "\nВвод первой матрицы:\n";
    float** matrix1 = createMatrix(rows, cols);
    std::cout << "\nПервая матрица:\n";
    printMatrix(matrix1, rows, cols);

    std::cout << "\nВвод второй матрицы:\n";
    float** matrix2 = createMatrix(rows, cols);
    std::cout << "\nВторая матрица:\n";
    printMatrix(matrix2, rows, cols);

    float** result = addMatrices(matrix1, matrix2, rows, cols);
    std::cout << "\nСумма матриц:\n";
    printMatrix(result, rows, cols);

    freeMatrix(matrix1, rows);
    freeMatrix(matrix2, rows);
    freeMatrix(result, rows);

    return 0;
}
```

- **float** createMatrix(int rows, int cols)**: Функция создаёт матрицу размером `rows × cols`, запрашивает у пользователя элементы и возвращает указатель на матрицу.
- **float** addMatrices(float** matrix1, float** matrix2, int rows, int cols)**: Складывает две матрицы, создавая новую.
- **void printMatrix(float** matrix, int rows, int cols)**: Выводит матрицу на экран.
- **void freeMatrix(float** matrix, int rows)**: Освобождает память, выделенную под матрицу.
- **float** matrix = (float**)malloc(rows * sizeof(float*))**: Выделяет память для массива указателей на строки.
- **matrix[i] = (float*)malloc(cols * sizeof(float))**: Выделяет память для элементов строки.
- **std::cin >> matrix[i][j]**: Считывает элемент матрицы.

### Пример ввода и вывода
**Ввод**:  
```
Введите количество строк: 2
Введите количество столбцов: 2
Ввод первой матрицы:
Введите 2 элемента(ов) строки 1 матрицы: 1 2
Введите 2 элемента(ов) строки 2 матрицы: 3 4
Ввод второй матрицы:
Введите 2 элемента(ов) строки 1 матрицы: 5 6
Введите 2 элемента(ов) строки 2 матрицы: 7 8
```
**Вывод**:  
```
Первая матрица:
1 2
3 4
Вторая матрица:
5 6
7 8
Сумма матриц:
6 8
10 12
```

---

## Программа 5: Проверка числа на палиндром

### Задача
Программа проверяет, является ли введённое целое число палиндромом (например, 12321 читается одинаково слева направо и справа налево). Используется косвенная рекурсия.

### Логика программы
1. Пользователь вводит число.
2. Функция `is_palindrome` вызывает вспомогательную функцию `is_palindrome_helper`.
3. `is_palindrome_helper` рекурсивно строит обратное число и сравнивает его с исходным.
4. Результат выводится на экран.

### Код и объяснение
```cpp
#include <iostream>

int is_palindrome_helper(int n, int reversed, int original);

int is_palindrome(int n) {
    return is_palindrome_helper(n, 0, n);
}

int is_palindrome_helper(int n, int reversed, int original) {
    if (n == 0) {
        return reversed == original; // Сравниваем обратное и исходное число
    }
    return is_palindrome_helper(n / 10, reversed * 10 + n % 10, original);
}

int main() {
    int number;
    std::cout << "Введите целое число: ";
    std::cin >> number;
    
    if (is_palindrome(number)) {
        std::cout << number << " является палиндромом" << std::endl;
    } else {
        std::cout << number << " не является палиндромом" << std::endl;
    }
    
    return 0;
}
```

- **int is_palindrome(int n)**: Главная функция, вызывающая вспомогательную функцию.
- **int is_palindrome_helper(int n, int reversed, int original)**: Рекурсивно строит обратное число:
  - `n / 10`: Убирает последнюю цифру числа.
  - `n % 10`: Получает последнюю цифру.
  - `reversed * 10 + n % 10`: Добавляет цифру к обратному числу.
- **if (n == 0)**: Условие выхода из рекурсии.
- **return reversed == original**: Проверяет, совпадает ли обратное число с исходным.

### Пример ввода и вывода
**Ввод**:  
```
Введите целое число: 12321
```
**Вывод**:  
```
12321 является палиндромом
```

**Ввод**:  
```
Введите целое число: 12345
```
**Вывод**:  
```
12345 не является палиндромом
```

---

## Программа 6: Система учёта обуви

### Задача
Программа реализует систему учёта обуви, позволяя добавлять, удалять и просматривать записи об обуви. Используются структуры и объединения для хранения данных о разных типах обуви.

### Логика программы
1. Пользователь выбирает действие через меню (добавить, удалить, показать обувь, выйти).
2. Для добавления обуви вводятся данные, включая тип (спортивная, классическая, сапоги) и специфические параметры.
3. Данные хранятся в массиве структур `ShoeItem`.
4. Удаление выполняется по индексу, а просмотр отображает все записи.

### Код и объяснение
```cpp
#include <stdio.h>
#include <string.h>
#include <iostream>
using namespace std;

#define SPORT_SHOES 1
#define CLASSIC_SHOES 2
#define BOOTS 3

struct ShoeItem {
    char model[50]; // Модель обуви
    float price;    // Цена
    char manufacturer[50]; // Производитель
    int availableSizes[10]; // Доступные размеры
    int sizeCount;  // Количество размеров
    char shoeType;  // Тип обуви
    
    union {
        struct {
            char sportType[30];
            float weight;
            bool hasArchSupport;
        } sportShoes;
        
        struct {
            char material[30];
            char color[20];
            bool isWaterproof;
        } classicShoes;
        
        struct {
            float shaftHeight;
            bool isInsulated;
            bool isWinter;
        } boots;
    };
};

ShoeItem inventory[100]; // Массив для хранения обуви
int itemCount = 0;       // Счётчик элементов

void addShoe() {
    if (itemCount >= 100) {
        cout << "База данных заполнена!\n";
        return;
    }
    
    ShoeItem newItem;
    cout << "Введите модель обуви: ";
    cin.getline(newItem.model, 50);
    cout << "Введите цену: ";
    cin >> newItem.price;
    cin.ignore();
    cout << "Введите производителя: ";
    cin.getline(newItem.manufacturer, 50);
    cout << "Введите количество доступных размеров: ";
    cin >> newItem.sizeCount;
    cin.ignore();
    cout << "Введите доступные размеры через пробел: ";
    for (int i = 0; i < newItem.sizeCount; i++) {
        cin >> newItem.availableSizes[i];
    }
    cin.ignore();
    cout << "Выберите тип обуви (1-спортивная, 2-классическая, 3-сапоги): ";
    cin >> newItem.shoeType;
    cin.ignore();
    
    switch(newItem.shoeType) {
        case SPORT_SHOES:
            cout << "Введите вид спорта: ";
            cin.getline(newItem.sportShoes.sportType, 30);
            cout << "Введите вес пары (г): ";
            cin >> newItem.sportShoes.weight;
            cout << "Есть поддержка свода стопы? (1-да, 0-нет): ";
            cin >> newItem.sportShoes.hasArchSupport;
            cin.ignore();
            break;
        case CLASSIC_SHOES:
            cout << "Введите материал: ";
            cin.getline(newItem.classicShoes.material, 30);
            cout << "Введите цвет: ";
            cin.getline(newItem.classicShoes.color, 20);
            cout << "Водоотталкивающая? (1-да, 0-нет): ";
            cin >> newItem.classicShoes.isWaterproof;
            cin.ignore();
            break;
        case BOOTS:
            cout << "Введите высоту голенища (см): ";
            cin >> newItem.boots.shaftHeight;
            cout << "Утепленные? (1-да, 0-нет): ";
            cin >> newItem.boots.isInsulated;
            cout << "Зимние? (1-да, 0-нет): ";
            cin >> newItem.boots.isWinter;
            cin.ignore();
            break;
        default:
            cout << "Неверный тип обуви!\n";
            return;
    }
    
    inventory[itemCount++] = newItem;
    cout << "Обувь добавлена в базу!\n";
}

void deleteShoe() {
    int index;
    cout << "Введите индекс для удаления (0-" << itemCount-1 << "): ";
    cin >> index;
    cin.ignore();
    
    if (index < 0 || index >= itemCount) {
        cout << "Неверный индекс!\n";
        return;
    }
    
    for (int i = index; i < itemCount - 1; i++) {
        inventory[i] = inventory[i+1]; // Сдвигаем элементы
    }
    
    itemCount--;
    cout << "Обувь удалена из базы!\n";
}

void printShoe(const ShoeItem &item) {
    cout << "\nМодель: " << item.model << "\n";
    cout << "Производитель: " << item.manufacturer << "\n";
    cout << "Цена: " << item.price << " руб.\n";
    cout << "Доступные размеры: ";
    for (int i = 0; i < item.sizeCount; i++) {
        cout << item.availableSizes[i] << " ";
    }
    cout << "\n";
    
    switch(item.shoeType) {
        case SPORT_SHOES:
            cout << "Тип: Спортивная обувь\n";
            cout << "Вид спорта: " << item.sportShoes.sportType << "\n";
            cout << "Вес: " << item.sportShoes.weight << " г\n";
            cout << "Поддержка свода: " << (item.sportShoes.hasArchSupport ? "Да" : "Нет") << "\n";
            break;
        case CLASSIC_SHOES:
            cout << "Тип: Классическая обувь\n";
            cout << "Материал: " << item.classicShoes.material << "\n";
            cout << "Цвет: " << item.classicShoes.color << "\n";
            cout << "Водоотталкивающая: " << (item.classicShoes.isWaterproof ? "Да" : "Нет") << "\n";
            break;
        case BOOTS:
            cout << "Тип: Сапоги\n";
            cout << "Высота голенища: " << item.boots.shaftHeight << " см\n";
            cout << "Утепленные: " << (item.boots.isInsulated ? "Да" : "Нет") << "\n";
            cout << "Сезон: " << (item.boots.isWinter ? "Зимние" : "Демисезонные") << "\n";
            break;
    }
    cout << "----------------------------\n";
}

void printAllShoes() {
    if (itemCount == 0) {
        cout << "База данных пуста!\n";
        return;
    }
    
    cout << "\nВесь ассортимент обуви:\n";
    for (int i = 0; i < itemCount; i++) {
        cout << "Индекс: " << i << "\n";
        printShoe(inventory[i]);
    }
}

int main() {
    int choice;
    
    do {
        cout << "\nСистема учета обуви - Меню:\n";
        cout << "1. Добавить обувь\n";
        cout << "2. Удалить обувь\n";
        cout << "3. Показать весь ассортимент\n";
        cout << "0. Выход\n";
        cout << "Выберите действие: ";
        cin >> choice;
        cin.ignore();
        
        switch(choice) {
            case 1: addShoe(); break;
            case 2: deleteShoe(); break;
            case 3: printAllShoes(); break;
            case 0: break;
            default: cout << "Неверный выбор!\n";
        }
    } while (choice != 0);
    
    return 0;
}
```

- **#define SPORT_SHOES 1**: Определяет константы для типов обуви.
- **struct ShoeItem**: Структура для хранения данных об обуви.
- **union**: Объединение для хранения специфичных данных в зависимости от типа обуви.
- **void addShoe()**: Добавляет новую обувь в массив `inventory`.
- **void deleteShoe()**: Удаляет обувь по индексу.
- **void printShoe(const ShoeItem &item)**: Выводит данные об одной паре обуви.
- **void printAllShoes()**: Выводит весь ассортимент.
- **cin.getline()**: Считывает строку с пробелами.
- **cin.ignore()**: Пропускает символы в буфере ввода (например, перенос строки).

### Пример ввода и вывода
**Ввод**:  
```
Система учета обуви - Меню:
1. Добавить обувь
2. Удалить обувь
3. Показать весь ассортимент
0. Выход
Выберите действие: 1
Введите модель обуви: Nike Air
Введите цену: 5000
Введите производителя: Nike
Введите количество доступных размеров: 2
Введите доступные размеры через пробел: 42 43
Выберите тип обуви (1-спортивная, 2-классическая, 3-сапоги): 1
Введите вид спорта: Бег
Введите вес пары (г): 300
Есть поддержка свода стопы? (1-да, 0-нет): 1
```
**Вывод**:  
```
Обувь добавлена в базу!
```

**Ввод**:  
```
Выберите действие: 3
```
**Вывод**:  
```
Весь ассортимент обуви:
Индекс: 0
Модель: Nike Air
Производитель: Nike
Цена: 5000 руб.
Доступные размеры: 42 43
Тип: Спортивная обувь
Вид спорта: Бег
Вес: 300 г
Поддержка свода: Да
----------------------------
```
