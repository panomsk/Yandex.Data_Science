# Защита персональных данных клиентов
Вам нужно защитить данные клиентов страховой компании «Хоть потоп». Разработайте такой метод преобразования данных, чтобы по ним было сложно восстановить персональную информацию. Обоснуйте корректность его работы.

Нужно защитить данные, чтобы при преобразовании качество моделей машинного обучения не ухудшилось. Подбирать наилучшую модель не требуется.

**Описание данных**
Набор данных находится в файле `/datasets/insurance.csv`  
•	***Признаки:*** пол, возраст и зарплата застрахованного, количество членов его семьи.  
•	***Целевой признак:*** количество страховых выплат клиенту за последние 5 лет.  

## Общий вывод по проекту
**При выполнении проекта:** 

**В разделе 1 проекта:**  
- Данные успешно загружены из файла: `/datasets/insurance.csv`.
- Датасет содержит 5000 строк и 5 колонок. Пропусков в исходных данных нет.
- Присутствует 153 дубликата, но поскольку признаков у нас не так много и отсутствует id клиента, то возможно это данные разных клиентов. Соответственно удалять их не будем.  
- Целевой признак положительно коррелирует с возрастом клиентов, больше корреляций между признаками не наблюдается.  

**В разделе 2 проекта:**  
- Получено выражение связывающее веса исходной и модифицированной модели (полученной при умножении матрицы признаков исходной модели на обратимую матрицу).  
- Доказано с помощью аппарата линейной алгебры, что вектор предсказаний исходной и модифицированной модели совпадают.  
- Можно сделать вывод, что качество линейной регрессии не изменится (т.к. вектор предсказаний исходной и модифицированной модели идентичные, то и значения метрик качества также будут идентичными).

**В разделе 3 проекта:**  
- Описан алгоритм обучения и применения модели линейной регрессии для предсказания количества страховых выплат клиенту, работающий с зашифрованными данными.

**В разделе 4 проекта:**  
- Разработана модель линейной регрессии
- Проверена работа модели с зашифрованными данными, проведено сравнение 2 видов моделей (разработанной и из пакета sklearn) при работе с зашифрованными и исходными данными, показана идентичность метрики r2_score для всех вариантов обучения.

**По итогу проекта Заказчику предлагается использовать следующий алгоритм, позволяющий защитить конфиденциальные данные о клиентах:**  
- Генерируется случайная обратимая матрица $P$ размера $[k,k]$, где $k$ - количество признаков в датасете. При случайной генерации вероятность получения необратимой матрицы крайне мала (это возможно только при линейной зависимости строк или столбцов) 
- Матрица признаков домножается на полученную матрицу $P$, таким образом осуществляется шифрование исходных данных. Восстановить данные, не зная матрицу P не получится.  
- На зашифрованных признаках обучается модель линейной регрессии, получается вектор весов.
- Далее для использования модели необходимо выборку с признаками по новым клиентам (новые объекты для использования модели) умножать на матрицу P и использовать модифицированную модель. 