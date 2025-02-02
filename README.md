# Genre-Classification

### ROC-AUC - целевая метрика. ROC-AUC для валидационной - 0.8339

Использование методов МО для классификации жанров
[validation+Spotify+Y.Music songs](https://disk.yandex.ru/d/z_QRcU0mWkp87Q)

Задача:
Классифицировать жанры, используя классические методы машинного обучения. Будем использовать библиотеку `librosa` для Feature Engineering.
### Содержание проекта:
* [librosa_preview.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/librosa_preview.ipynb) - тетрадь про библиотеку librosa и различные методы librosa.feature. Анализ спектрограмм и их визуализация

Пример визуализации:

<image src="pics/chroma.png" alt="chroma">

* [parsing.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/parsing.ipynb) - тетрадь, содержащая парсинг Yandex.Music и Spotify(API).
* [Dataset.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/Dataset.ipynb) - сборка датасета с данных парсинга.(Обработка музыки через librosa, создание датафрейма)
* [spotify.csv](https://github.com/TimRicMus/Genre-Classification/blob/main/spotify.csv) - датасет из песен со Spotify. 
* yandex.csv - датасет из песен с Yandex Music
* final.csv - объединенный датасет(Spotify+Y.Music)
* valid.csv - датасет для теста
* [Yandex_dataset.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/Yandex_dataset.ipynb) - тетрадь, содержащая сборку датасета yandex.csv
* [hyperparameters.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/hyperparameters.ipynb) - подбор гиперпараметров
* [Project_models.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/Project_models.ipynb) - тетрадь Feature Engineering
* [Dataset_valid.ipynb](https://github.com/TimRicMus/Genre-Classification/blob/main/Dataset_valid.ipynb) - тетрадь со сборкой валидационной выборки
### Feature Engineering:
В качестве анализа данных были рассмотрены, например, доверительные интервалы

<image src="pics/means.png" alt="means">
  
* ANOVA - статистический метод, используемый для анализа различий между средними значениями двух или более групп данных.

Предпосылки: нормальность, равенство дисперсий, независимость. Такие предпосылки крайне сложно учесть.

Пример нарушения предпосылки:

<image src="pics/qqplot.png" alt="qqplot">

  Попытка запустить метод и проанализировать различия не увенчалась успехом, так как ни одна предпосылка не была выполнена для наших    данных. Однако результат, который мы получили(каждый признак влияет на результат), соотносится с реальностью. Методы `librosa.feature` позволяют отразить определенную особенную характеристику звука(подробнее в librosa_preview). Но таким результатам доверять мы не будем
*Генерация признаков
  
Генерировали полиномиальные комбинации 3 степени. Для отбора признаков используем тест Краскела - Уоллиса. Также сгенерировали экспоненты и корни от признаков.

* Квантизация признаков

<image src="pics/umap.png" alt="umap">

Кластеризуем объекты нашей матрицы, затем создадим категориальный признак, который будет отвечать принадлежность к кластеру. На тестовых объектах мы будем считать расстояния до всех центров кластеров, а затем относить объекты к тем кластерам, что находятся к ним ближе всего.
* Кластерный анализ - метод, позволяющий группировать объекты на основе их сходства в пределах одной группы и различия между группами.

Использовали `UMAP` для снижения размерности нашей матрицы, чтобы впоследствии отобразить их на графике.

* Дисбаланс классов

  Применили `SMOTE`, чтобы избавиться от дисбаланса классов в наших данных.




