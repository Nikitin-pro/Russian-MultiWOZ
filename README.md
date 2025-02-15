# RussianMultiWOZ
Russian MultiWOZ -  аннотированная коллекция письменных диалогов на русском языке, охватывающая несколько областей человеческой жизни. При размере в 10 тыс. диалогов, это первый набор текстовых данных на русском языке для создания диалоговых моделей, ориентированных на конкретные задачи.

# Формат

RussianMultiWOZ повторяет формат данных, предложенный в (Budzianowski et al. 2018, EMNLP). Ключевая часть диалогов и разметки содержится в json.

Датасет содержит 3 406 однодоменных диалогов, включающих бронирование, если домен позволяет это сделать, и 7 032 многодоменных диалога, состоящих как минимум из 2-5 доменов. Для обеспечения воспроизводимости результатов, корпус был случайным образом разделен на обучающий (train), тестовый (test) и валидационный (dev). Тестовый и валидационный содержат по 1 тыс. примеров. Несмотря на то, что все диалоги являются связными, некоторые из них не были закончены с точки зрения описания задачи. Поэтому наборы для проверки и тестирования содержат только полностью успешные диалоги, что позволяет справедливо сравнивать модели. В валидационном и тестовом наборах нет диалогов из доменов "больница" и "полиция".

Каждый диалог состоит из цели (goal), нескольких высказываний пользователя и системы, а также состояний (belief state). Кроме того, добавлено описание задачи на естественном языке, представленное туркерам, работающим со стороны посетителя. Диалоги с MUL в названии относятся к многодоменным диалогам. Диалоги с SNG относятся к однодоменным диалогам.


# :thought_balloon: References
Если вы используете какой-либо исходный код или наборы данных из этой работы, просим ссылаться на нас. Бибтексы перечислены ниже:
```

[Nikitin et al. 2022]
@article{nikitin2022multiwoz,
  title={Russian MultiWOZ 2.1: Multi-Domain Dialogue State Corrections and State Tracking Baselines},
  author={Nikitin, Ilya and Kopachinskaya, Anastasia},
  journal={arXiv preprint arXiv:1907.00000},
  year={2022}
}
```


# Бейзлайн

### Установка требований
Python 3 with `pip`, `pytorch==0.4.1`

### Предобработка
Чтобы загрузить и предобработать данные, запустите:

```python create_delex_data.py```

### Обучение нейросети
Чтобы тренировать модель, запустите:

```python train.py [--args=value]```

Некоторые аргументы (гиперпараметры):

```
// гиперпараметры для обучения модели
--max_epochs        : numbers of epochs
--batch_size        : numbers of turns per batch
--lr_rate           : initial learning rate
--clip              : size of clipping
--l2_norm           : l2-regularization weight
--dropout           : dropout rate
--optim             : optimization method

// структура нейронной сети
--emb_size          : word vectors emedding size
--use_attn          : whether to use attention
--hid_size_enc      : size of RNN hidden cell
--hid_size_pol      : size of policy hidden output
--hid_size_dec      : size of RNN hidden cell
--cell_type         : specify RNN type
```

## Тестирование (оценка) модели
Для оценки модели запустите:

```python test.py [--args=value]```

## Результаты

Мы используем небольшой поиск по сетке (grid search) по набору гиперпараметров и выбираем лучшую модель для тестового набора.
We ran a small grid search over various hyperparameter settings and reported the performance of the best model on the test set.

Критерий отбора - это формула 0.5*match + 0.5*success+100*BLEU, оцененная на валидационном наборе данных.

Итоговый набор гиперпараметров:

```
// гиперпараметры для обучения модели
--max_epochs        : 20
--batch_size        : 64
--lr_rate           : 0.005
--clip              : 5.0
--l2_norm           : 0.00001
--dropout           : 0.0
--optim             : Adam

// структура нейронной сети
--emb_size          : 50
--use_attn          : True
--hid_size_enc      : 150
--hid_size_pol      : 150
--hid_size_dec      : 150
--cell_type         : lstm
```


# Лицензия
Russian MultiWOZ - это набор инструментов с открытым исходным кодом для построения сквозных обучаемых диалоговых моделей на русском языке, ориентированных на конкретные задачи. Он создан Анастасией Копачинской и Ильей Никитиным из Национального исследовательского университета "Высшая школа экономики" (Москва) под лицензией MIT.


# Контакты с разработчиками
Если вы нашли какие-либо ошибки в коде, пожалуйста, свяжитесь с нами: ianikitin93 at gmail dot com
