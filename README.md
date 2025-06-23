# Калькулятор матриц



- `MatrixCalculatorUI.java`: Реализует графический интерфейс с таблицами для ввода матриц, кнопками операций и областью вывода.
- `att1.java`: Содержит методы для вычисления определителя, решения систем уравнений методом Крамера и приведения матрицы к верхнетреугольной форме.
- `att2.java`: Реализует метод Гаусса для решения систем уравнений.
- `att3.java`: Реализует нахождение обратной матрицы (2x2 и произвольного размера) и умножение матриц.


## Использование программы

1. **Ввод матриц**:
   - Введите числа в таблицы "Матрица A" и "Матрица B".
   - Пустые ячейки интерпретируются как 0.
   - Используйте кнопки "Добавить строку", "Удалить строку", "Добавить столбец", "Удалить столбец" для изменения размеров матриц.

2. **Операции**:
   - Нажмите одну из кнопок операций:
     - **Вычислить определитель**: Для матрицы A (должна быть квадратной).
     - **Решить методом Крамера**: Решает систему \( Ax = B \) (A — квадратная, B — вектор-столбец).
     - **Привести к треугольному виду**: Для матрицы A (квадратная).
     - **Решить методом Гаусса**: Решает систему \( Ax = B \).
     - **Найти обратную матрицу 2x2**: Для матрицы A размером 2x2.
     - **Умножить матрицы**: Умножает матрицы A и B, если число столбцов A равно числу строк B.
     - **Найти обратную матрицу**: Для матрицы A любого размера (квадратной).
   - Результат или сообщение об ошибке появится в области вывода.

## Описание методов

### Класс `att1`

#### 1. `calculateDeterminant(double[][] matrix)`
- **Назначение**: Вычисляет определитель квадратной матрицы и возвращает его в виде строки.
- **Математическая суть**: Определитель — скаляр, связанный с квадратной матрицей, используемый для решения систем уравнений и проверки обратимости.
- **Как работает**:
  1. Проверяет, что матрица квадратная (`matrix.length == matrix[0].length`).
  2. Вызывает приватный метод `computeDeterminant`.
  3. Форматирует результат как строку.
- **Код**:
  ```java
  public String calculateDeterminant(double[][] matrix) {
      if (matrix.length != matrix[0].length) {
          throw new IllegalArgumentException("Матрица должна быть квадратной для вычисления определителя");
      }
      return "Определитель: " + computeDeterminant(matrix);
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вычисление: \( det = 1 \cdot 4 - 2 \cdot 3 = -2 \)
  - Вывод: `Определитель: -2.0`

#### 2. `computeDeterminant(double[][] matrix)` (приватный)
- **Назначение**: Рекурсивно вычисляет определитель матрицы.
- **Математическая суть**: Для \( 1 \times 1 \): возвращает элемент. Для \( 2 \times 2 \): \( a \cdot d - b \cdot c \). Для \( n \times n \) (\( n > 2 \)): разложение по первой строке с кофакторами.
- **Как работает**:
  1. Если \( n = 1 \), возвращает `matrix[0][0]`.
  2. Если \( n = 2 \), вычисляет \( a \cdot d - b \cdot c \).
  3. Для \( n > 2 \): суммирует \( matrix[0][j] \cdot computeCofactor(0, j) \).
- **Код**:
  ```java
  private double computeDeterminant(double[][] matrix) {
      int n = matrix.length;
      if (n == 1) return matrix[0][0];
      if (n == 2) return matrix[0][0] * matrix[1][1] - matrix[0][1] * matrix[1][0];
      double det = 0;
      for (int j = 0; j < n; j++) {
          det += matrix[0][j] * computeCofactor(matrix, 0, j);
      }
      return det;
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]`
  - Вычисление: \( det = 1 \cdot (5 \cdot 9 - 6 \cdot 8) - 2 \cdot (4 \cdot 9 - 6 \cdot 7) + 3 \cdot (4 \cdot 8 - 5 \cdot 7) = 0 \)
  - Вывод: `0.0`

#### 3. `computeCofactor(double[][] matrix, int row, int col)` (приватный)
- **Назначение**: Вычисляет кофактор элемента матрицы.
- **Математическая суть**: Кофактор = \( (-1)^{row+col} \cdot det(minor) \).
- **Как работает**:
  1. Вызывает `getMinor` для получения минора.
  2. Вычисляет определитель минора через `computeDeterminant`.
  3. Умножает на \( (-1)^{row+col} \).
- **Код**:
  ```java
  private double computeCofactor(double[][] matrix, int row, int col) {
      return Math.pow(-1, row + col) * computeDeterminant(getMinor(matrix, row, col));
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`, `row = 0`, `col = 0`
  - Минор: `[[4]]`
  - Кофактор: \( (-1)^{0+0} \cdot 4 = 4 \)
  - Вывод: `4.0`

#### 4. `getMinor(double[][] matrix, int row, int col)` (приватный)
- **Назначение**: Возвращает минор матрицы (без строки `row` и столбца `col`).
- **Математическая суть**: Удаляет строку и столбец, создавая матрицу \( (n-1) \times (n-1) \).
- **Как работает**:
  1. Создаёт матрицу \( (n-1) \times (n-1) \).
  2. Копирует элементы, пропуская `row` и `col`.
- **Код**:
  ```java
  private double[][] getMinor(double[][] matrix, int row, int col) {
      int n = matrix.length;
      double[][] minor = new double[n - 1][n - 1];
      for (int i = 0, r = 0; i < n; i++) {
          if (i == row) continue;
          for (int j = 0, c = 0; j < n; j++) {
              if (j == col) continue;
              minor[r][c++] = matrix[i][j];
          }
          r++;
      }
      return minor;
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]`, `row = 0`, `col = 0`
  - Вывод: `[[5, 6], [8, 9]]`

#### 5. `solveByCramer(double[][] A, double[][] B)`
- **Назначение**: Решает систему уравнений \( Ax = B \) методом Крамера.
- **Математическая суть**: \( x_i = det(A_i) / det(A) \), где \( A_i \) — матрица A с i-м столбцом, заменённым на B.
- **Как работает**:
  1. Проверяет, что A квадратная, B — вектор-столбец.
  2. Вычисляет \( det(A) \).
  3. Если \( |det(A)| < EPSILON \), выбрасывает исключение.
  4. Для каждого \( x_i \):
     - Копирует A в `temp`.
     - Заменяет i-й столбец на B.
     - Вычисляет \( x_i = det(temp) / det(A) \).
     - Восстанавливает `temp` для следующей итерации.
- **Код**:
  ```java
  public String solveByCramer(double[][] A, double[][] B) {
      if (A.length != A[0].length || A.length != B.length || B[0].length != 1) {
          throw new IllegalArgumentException("Матрица A должна быть квадратной, B - вектор-столбец");
      }
      double detA = computeDeterminant(A);
      if (Math.abs(detA) < EPSILON) {
          throw new IllegalArgumentException("Определитель равен нулю, метод Крамера неприменим");
      }
      StringBuilder result = new StringBuilder("Решение методом Крамера:\n");
      double[][] temp = copyMatrix(A);
      for (int i = 0; i < A.length; i++) {
          for (int r = 0; r < A.length; r++) {
              temp[r][i] = B[r][0];
          }
          result.append("x").append(i + 1).append(" = ").append(computeDeterminant(temp) / detA).append("\n");
          for (int r = 0; r < A.length; r++) {
              temp[r][i] = A[r][i];
          }
      }
      return result.toString();
  }
  ```
- **Пример**:
  - Ввод:
    - \( A = [[2, 1], [1, 3]] \)
    - \( B = [[5], [4]] \)
  - Вычисление:
    - \( det(A) = 2 \cdot 3 - 1 \cdot 1 = 5 \)
    - \( A_1 = [[5, 1], [4, 3]] \), \( det(A_1) = 11 \), \( x_1 = 11/5 = 2.2 \)
    - \( A_2 = [[2, 5], [1, 4]] \), \( det(A_2) = 3 \), \( x_2 = 3/5 = 0.6 \)
  - Вывод:
    ```
    Решение методом Крамера:
    x1 = 2.2
    x2 = 0.6
    ```

#### 6. `toTriangularForm(double[][] matrix)`
- **Назначение**: Приводит матрицу к верхнетреугольной форме и возвращает её с определителем.
- **Математическая суть**: Использует метод Гаусса для приведения к треугольной форме; определитель = произведение диагональных элементов с учётом знака перестановок.
- **Как работает**:
  1. Проверяет, что матрица квадратная.
  2. Копирует матрицу.
  3. Вызывает `makeUpperTriangular` для приведения к треугольной форме.
  4. Вычисляет определитель как произведение диагональных элементов, умноженное на знак.
  5. Форматирует матрицу и определитель в строку.
- **Код**:
  ```java
  public String toTriangularForm(double[][] matrix) {
      if (matrix.length != matrix[0].length) {
          throw new IllegalArgumentException("Матрица должна быть квадратной");
      }
      double[][] upper = copyMatrix(matrix);
      double detSign = makeUpperTriangular(upper);
      double det = detSign;
      StringBuilder result = new StringBuilder("Верхнетреугольная матрица:\n");
      for (double[] row : upper) {
          for (double val : row) {
              result.append(String.format("%10.2f", val));
          }
          result.append("\n");
      }
      for (int i = 0; i < upper.length; i++) {
          det *= upper[i][i];
      }
      result.append("Определитель: ").append(det);
      return result.toString();
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вычисление:
    - Верхнетреугольная форма: `[[1, 2], [0, -2]]`
    - Знак: \( detSign = 1 \)
    - Определитель: \( 1 \cdot (-2) = -2 \)
  - Вывод:
    ```
    Верхнетреугольная матрица:
         1.00      2.00
         0.00     -2.00
    Определитель: -2.0
    ```

#### 7. `makeUpperTriangular(double[][] matrix)` (приватный)
- **Назначение**: Приводит матрицу к верхнетреугольной форме и возвращает знак определителя.
- **Математическая суть**: Обнуляет элементы под диагональю, меняя строки при необходимости.
- **Как работает**:
  1. Для каждого столбца \( k \):
     - Если диагональный элемент мал, меняет строки (`swapRows`), обновляя знак (\( detSign *= -1 \)).
     - Вычитает строку \( k \), умноженную на коэффициент, из нижних строк.
  2. Возвращает знак определителя.
- **Код**:
  ```java
  private double makeUpperTriangular(double[][] matrix) {
      int n = matrix.length;
      double detSign = 1;
      for (int k = 0; k < n - 1; k++) {
          if (Math.abs(matrix[k][k]) < EPSILON) {
              for (int i = k + 1; i < n; i++) {
                  if (Math.abs(matrix[i][k]) > EPSILON) {
                      swapRows(matrix, k, i);
                      detSign *= -1;
                      break;
                  }
              }
          }
          if (Math.abs(matrix[k][k]) < EPSILON) continue;
          for (int i = k + 1; i < n; i++) {
              double factor = matrix[i][k] / matrix[k][k];
              for (int j = k; j < n; j++) {
                  matrix[i][j] -= factor * matrix[k][j];
              }
          }
      }
      return detSign;
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вывод: `[[1, 2], [0, -2]]`, `detSign = 1`

#### 8. `swapRows(double[][] matrix, int i, int j)` (приватный)
- **Назначение**: Меняет местами строки \( i \) и \( j \).
- **Математическая суть**: Вспомогательный метод для перестановки строк.
- **Как работает**:
  1. Сохраняет строку \( i \).
  2. Заменяет строки.
- **Кod**:
  ```java
  private void swapRows(double[][] matrix, int i, int j) {
      double[] temp = matrix[i];
      matrix[i] = matrix[j];
      matrix[j] = temp;
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`, `i = 0`, `j = 1`
  - Вывод: `[[3, 4], [1, 2]]`

#### 9. `copyMatrix(double[][] matrix)` (приватный)
- **Назначение**: Создаёт копию матрицы.
- **Математическая суть**: Предотвращает изменение исходной матрицы.
- **Как работает**:
  1. Создаёт новую матрицу того же размера.
  2. Копирует элементы с помощью `System.arraycopy`.
- **Код**:
  ```java
  private double[][] copyMatrix(double[][] matrix) {
      double[][] copy = new double[matrix.length][matrix[0].length];
      for (int i = 0; i < matrix.length; i++) {
          System.arraycopy(matrix[i], 0, copy[i], 0, matrix[i].length);
      }
      return copy;
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вывод: `[[1, 2], [3, 4]]`

### Класс `att2`

#### 10. `solveByGauss(double[][] A, double[][] B)`
- **Назначение**: Решает систему уравнений \( Ax = B \) методом Гаусса.
- **Математическая суть**: Приводит расширенную матрицу \( [A|B] \) к ступенчатому виду, анализирует ранги и находит решение.
- **Как работает**:
  1. Проверяет, что B — вектор-столбец, и размеры совместимы.
  2. Создаёт расширенную матрицу \( [A|B] \).
  3. Вызывает `performGaussElimination`.
- **Код**:
  ```java
  public String solveByGauss(double[][] A, double[][] B) {
      if (A.length != B.length || B[0].length != 1) {
          throw new IllegalArgumentException("Матрица B должна быть вектором-столбцом с числом строк, равным числу строк матрицы A");
      }
      double[][] augmented = new double[A.length][A[0].length + 1];
      for (int i = 0; i < A.length; i++) {
          System.arraycopy(A[i], 0, augmented[i], 0, A[0].length);
          augmented[i][A[0].length] = B[i][0];
      }
      return performGaussElimination(augmented);
  }
  ```
- **Пример**:
  - Ввод:
    - \( A = [[2, 1], [1, 3]] \)
    - \( B = [[5], [4]] \)
  - Вывод:
    ```
    Решение методом Гаусса:
    x1 = 2.2000
    x2 = 0.6000
    ```

#### 11. `performGaussElimination(double[][] matrix)` (приватный)
- **Назначение**: Выполняет метод Гаусса для решения системы.
- **Математическая суть**: Приводит матрицу к ступенчатому виду, определяет ранги A и \( [A|B] \), решает систему.
- **Как работает**:
  1. **Прямой ход**:
     - Ищет опорный элемент, меняет строки.
     - Нормирует опорную строку.
     - Обнуляет элементы в столбце.
  2. **Ранги**:
     - Считает ранг A и \( [A|B] \).
     - Если \( augRank > rank \): нет решений.
     - Если \( rank < cols-1 \): бесконечное число решений.
  3. **Обратный ход**:
     - Вычисляет \( x_i \) от последней строки к первой.
- **Код**:
  ```java
  private String performGaussElimination(double[][] matrix) {
      // ... (см. исходный код)
  }
  ```
- **Пример**:
  - Ввод:
    - \( [A|B] = [[1, 1, 3], [2, 2, 6]] \)
  - Вывод: `Система имеет бесконечное число решений`

#### 12. `swapRows(double[][] matrix, int i, int j)` (приватный)
- **Назначение**: Меняет местами строки \( i \) и \( j \).
- **Математическая суть**: Аналогично методу в `att1`.
- **Код**:
  ```java
  private void swapRows(double[][] matrix, int i, int j) {
      double[] temp = matrix[i];
      matrix[i] = matrix[j];
      matrix[j] = temp;
  }
  ```

### Класс `att3`

#### 13. `calculateInverse2x2(double[][] matrix)`
- **Назначение**: Вычисляет обратную матрицу \( 2 \times 2 \).
- **Математическая суть**: \( A^{-1} = \frac{1}{det} \cdot [[d, -b], [-c, a]] \).
- **Как работает**:
  1. Проверяет размер \( 2 \times 2 \).
  2. Вычисляет определитель.
  3. Если \( |det| < EPSILON \), выбрасывает исключение.
  4. Формирует обратную матрицу.
  5. Проверяет \( A \cdot A^{-1} \).
- **Код**:
  ```java
  public String calculateInverse2x2(double[][] matrix) {
      // ... (см. исходный код)
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вывод:
    ```
    Обратная матрица 2x2:
    [-2.0000, 1.0000]
    [1.5000, -0.5000]
    Проверка (A * A^-1):
    [1.0000, 0.0000]
    [0.0000, 1.0000]
    ```

#### 14. `multiplyMatricesToString(double[][] A, double[][] B)`
- **Назначение**: Умножает матрицы и возвращает результат в виде строки.
- **Математическая суть**: Для \( A \) (\( m \times n \)) и \( B \) (\( n \times p \)), результат — \( m \times p \).
- **Как работает**:
  1. Проверяет совместимость и непустоту матриц.
  2. Вызывает `multiplyMatrices`.
  3. Форматирует результат с выравниванием.
- **Код**:
  ```java
  public String multiplyMatricesToString(double[][] A, double[][] B) {
      if (A.length == 0 || B.length == 0 || A[0].length != B.length) {
          throw new IllegalArgumentException("Число столбцов матрицы A должно быть равно числу строк матрицы B");
      }
      double[][] result = multiplyMatrices(A, B);
      StringBuilder sb = new StringBuilder("Результат умножения матриц:\n");
      for (double[] row : result) {
          for (double val : row) {
              sb.append(String.format("%10.2f", val));
          }
          sb.append("\n");
      }
      return sb.toString();
  }
  ```
- **Пример**:
  - Ввод:
    - \( A = [[1, 2, 3], [4, 5, 6]] \)
    - \( B = [[7, 8], [9, 10], [11, 12]] \)
  - Вывод:
    ```
    Результат умножения матриц:
        58.00     64.00
       139.00    154.00
    ```

#### 15. `calculateInverseAnySize(double[][] matrix)`
- **Назначение**: Вычисляет обратную матрицу произвольного размера методом Гаусса-Жордана.
- **Математическая суть**: Приводит расширенную матрицу \( [A|I] \) к виду \( [I|A^{-1}] \).
- **Как работает**:
  1. Проверяет, что матрица квадратная.
  2. Создаёт расширенную матрицу \( [A|I] \).
  3. Приводит к ступенчатому виду, проверяет определитель.
  4. Извлекает \( A^{-1} \).
  5. Проверяет \( A \cdot A^{-1} \).
- **Код**:
  ```java
  public String calculateInverseAnySize(double[][] matrix) {
      // ... (см. исходный код)
  }
  ```
- **Пример**:
  - Ввод: `matrix = [[1, 2], [3, 4]]`
  - Вывод:
    ```
    Обратная матрица:
       -2.0000    1.0000
        1.5000   -0.5000
    Проверка (A * A^-1):
        1.0000    0.0000
        0.0000    1.0000
    ```

#### 16. `multiplyMatrices(double[][] A, double[][] B)` (приватный)
- **Назначение**: Выполняет умножение матриц.
- **Математическая суть**: \( result[i][j] = \sum_{k} A[i][k] \cdot B[k][j] \).
- **Как работает**:
  1. Создаёт матрицу \( A.length \times B[0].length \).
  2. Вычисляет элементы через тройной цикл.
- **Кod**:
  ```java
  private double[][] multiplyMatrices(double[][] A, double[][] B) {
      double[][] result = new double[A.length][B[0].length];
      for (int i = 0; i < A.length; i++) {
          for (int j = 0; j < B[0].length; j++) {
              for (int k = 0; k < A[0].length; k++) {
                  result[i][j] += A[i][k] * B[k][j];
              }
          }
      }
      return result;
  }
  ```
- **Пример**:
  - Ввод:
    - \( A = [[1, 2, 3], [4, 5, 6]] \)
    - \( B = [[7, 8], [9, 10], [11, 12]] \)
  - Вывод: `[[58, 64], [139, 154]]`

#### 17. `swapRows(double[][] matrix, int i, int j)` (приватный)
- **Назначение**: Меняет местами строки \( i \) и \( j \).
- **Математическая суть**: Аналогично методу в `att1`.
- **Кod**:
  ```java
  private void swapRows(double[][] matrix, int i, int j) {
      double[] temp = matrix[i];
      matrix[i] = matrix[j];
      matrix[j] = temp;
  }
  ```

### Класс `MatrixCalculatorUI`

#### 18. `MatrixCalculatorUI()`
- **Назначение**: Конструктор, инициализирует графический интерфейс.
- **Как работает**:
  1. Устанавливает заголовок, размер, фон (тёмная тема).
  2. Вызывает `initializeUI`.
- **Кod**:
  ```java
  public MatrixCalculatorUI() {
      setTitle("Калькулятор матриц");
      setSize(1200, 800);
      setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      setLayout(new BorderLayout(10, 10));
      getContentPane().setBackground(new Color(30, 30, 30));
      initializeUI();
  }
  ```

#### 19. `initializeUI()` (приватный)
- **Назначение**: Настраивает компоненты интерфейса.
- **Как работает**:
  1. Создаёт панель с таблицами A и B.
  2. Добавляет панель операций с 7 кнопками.
  3. Настраивает текстовую область вывода.
- **Кod**:
  ```java
  private void initializeUI() {
      // ... (см. исходный код)
  }
  ```

#### 20. `createMatrixPanel(String title, DefaultTableModel model, JTable table)` (приватный)
- **Назначение**: Создаёт панель с таблицей для ввода матрицы.
- **Как работает**:
  1. Настраивает таблицу (фон, сетка, заголовок).
  2. Добавляет кнопки для изменения размеров.
- **Кod**:
  ```java
  private JPanel createMatrixPanel(String title, DefaultTableModel model, JTable table) {
      // ... (см. исходный код)
  }
  ```

#### 21. `createButton(String text, ActionListener listener)` (приватный)
- **Назначение**: Создаёт кнопку с заданным текстом и обработчиком.
- **Как работает**:
  1. Настраивает стиль кнопки (тёмная тема).
  2. Добавляет обработчик событий.
- **Кod**:
  ```java
  private JButton createButton(String text, java.awt.event.ActionListener listener) {
      // ... (см. исходный код)
  }
  ```

#### 22. `getMatrixFromTable(JTable table, DefaultTableModel model)` (приватный)
- **Назначение**: Извлекает матрицу из таблицы.
- **Как работает**:
  1. Читает данные из таблицы.
  2. Преобразует строки в числа, обрабатывает пустые ячейки.
- **Кod**:
  ```java
  private double[][] getMatrixFromTable(JTable table, DefaultTableModel model) {
      // ... (см. исходный код)
  }
  ```

#### 23–29. Методы обработки операций
- **Назначение**: Вызывают соответствующие методы из `att1`, `att2`, `att3` и выводят результаты.
- **Методы**:
  - `computeDeterminant()`: Вызывает `att1.calculateDeterminant`.
  - `solveByCramer()`: Вызывает `att1.solveByCramer`.
  - `toTriangularForm()`: Вызывает `att1.toTriangularForm`.
  - `solveByGauss()`: Вызывает `att2.solveByGauss`.
  - `computeInverse2x2()`: Вызывает `att3.calculateInverse2x2`.
  - `multiplyMatrices()`: Вызывает `att3.multiplyMatricesToString`.
  - `computeInverseAny()`: Вызывает `att3.calculateInverseAnySize`.

## Примеры ввода и вывода

- **Определитель**:
  - Матрица A: `[[1, 2], [3, 4]]`
  - Кнопка: "Вычислить определитель"
  - Вывод: `Определитель: -2.0`

- **Метод Крамера**:
  - Матрица A: `[[2, 1], [1, 3]]`
  - Матрица B: `[[5], [4]]`
  - Кнопка: "Решить методом Крамера"
  - Вывод:
    ```
    Результат:
    Решение методом Крамера:
    x1 = 2.2
    x2 = 0.6
    ```

- **Умножение матриц**:
  - Матрица A: `[[1, 2, 3], [4, 5, 6]]`
  - Матрица B: `[[7, 8], [9, 10], [11, 12]]`
  - Кнопка: "Умножить матрицы"
  - Вывод:
    ```
    Результат:
    Результат умножения матриц:
        58.00     64.00
       139.00    154.00
    ```
