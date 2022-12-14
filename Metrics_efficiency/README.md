### Оценка эффективности моделей бинарной классификации.

#### Цель работы

Научиться оценивать эффективность моделей классификации.

#### Содержание работы

1. Загрузите данные о вероятности развития сердечного приступа. 
Описание датасета смотрите здесь: https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction.
2. Разделите данные на обучающую и тестовую выборки в соотношении 80% / 20% без перемешивания.
3. Постройте следующие модели классификации:
    1. случайное предсказание (подбрасывание кубика)
    2. логистическая регрессия (LogisticRegression);
    3. метод опорных векторов с линейным ядром (SVC(kernel='linear'));
    4. метод линейного дискриминантного анализа (LinearDiscriminantAnalysis)
    5. метод k ближайших соседей (KNeighborsClassifier с двумя классами).
4. На основе метрики  accuracy  сделайте вывод о недообученности-переобученности модели логистической регрессии. Для этого сделайте предсказание на тренировочном датасете и посчитайте accuracy на нём, затем на тестовом датасете и тоже посчитайте accuracy. Сравните эти значения, они должны быть примерно одинаковыми. Если точность на обучающей выборке оказывается сильно выше - это означает, что модель переобучилась.
5. Сделайте вывод о применимости каждой модели на основе метрики  F1-score (чем выше её значение, тем модель эффективнее).
6. На одном графике постройте ROC-кривые для каждой модели. Визуально определите самую эффективную и наименее пригодную модели. Чем ROC-кривая выше, тем модель более эффективна.
7. Выведите соответствующие значения AUC (площадь под ROC - кривой) и расположите модели в порядке пригодности.


#### Методические указания

Загрузим данные в датафрейм `data`.
Целевым значением будет значение `"output"` - случится приступ или нет. Вынесем этот столбец в отдельную переменную `y`, а в переменную `x` запишем все столбцы из `data`, кроме столбца `target`:

```py
target = "output"
y = data[target]
x = data.drop(target, axis=1)
```

Импортируем нужные библиотеки для вычисления метрик:
```py
from sklearn.metrics import accuracy_score, roc_curve, auc, f1_score

```
Вычислим метрику accuracy на тренировочном датасете:
```py
y_pred_train = model.predict(x_train)
accuracy_score(y_train, y_pred_train)
```

Вычислим метрику accuracy на тестовом датасете:
```py
y_pred = model.predict(x_test)
accuracy_score(y_test, y_pred)
```

Вычислим метрику F1-score:
```py
f1 = f1_score(y_valid, y_pred)
```

Алгоритм логистической регрессии позволяет получить не только предсказанную метку класса, но и вероятность принадлежности этому классу. Эти вероятности можно получить с помощью метода .predict_proba:

```py
y_pred = model.predict(x_test)
y_pred_proba = model.predict_proba((x_test)
```

Рассчитываем значения, необходимые для построения ROC-кривой (receiver operating characteristics curve):

```py
probs = y_pred_proba[:, 1]
fpr, tpr, treshold = roc_curve(y_test, probs)
```

Построить график ROC-кривой можно следующим образом:

```py
plt.plot(fpr, tpr)
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC-кривая')
plt.show()
```
Вычислим значение площади под кривой (AUC-area under curve). Чем лучше классификатор разделяет два класса, тем больше эта площадь и ближе к 1:

```py
roc_auc = auc(fpr, tpr)
```

#### Задания для самостоятельного выполнения

1. Изобразите precision-recall кривую.
2. Оцените эффективность моделей при помощи показателя average precision.
1. Постройте другую классификационную модель и сравните ее с описанными по следующим метрикам: accuracy, precision, recall, ROC-AUC, F1. Сделайте вывод о том, какая модель наиболее удачная для данной задачи.

#### Контрольные вопросы

1. Что обозначают элементы confusion matrix?
2. По какой формуле рассчитывается метрика accuracy? Что она показывает?
3. Почему метрика accuracy непригодна при дисбалансе классов?
4. По какой формуле рассчитывается метрика precision? Что она показывает?
5. По какой формуле рассчитывается метрика recall? Что она показывает?
6. Какая метрика объединяет в себе precision и recall? По какой формуле она рассчитывается?
7. Какой показатель часто используется для сравнительного анализа нескольких моделей классификации?
8. Зависимость каких величин (при изменении порога) показывает ROC-кривая?