---
date: 2026-01-09
---
# Transformers

GPT - generative pre-trained transformer, также называют "context-based token predictor" [1].
Алгоритм работы трансформера:

1. принимает на вход prompt
1. выполняет forward pass
1. по итогам forward pass определяет следующий токен
1. добавляет этот токен к prompt и снова переходит к п.1

## Tokenization

Текст на входе разбивается на токены. И дальше в модель передается уже индексы из словаря, которые соответсвуют токенам.

## Embedding

Embedding - это вектор(массив) из N чисел, который является представлением токена в N-мерном пространстве. Сам по себе вектор ничего не значит, а значение имеет взаимное расположение токенов в N-мерном пространстве. При вычислении embedding к статической составляющей каждого токена добавляется контекс для конкретного текста. В итоге получается *contextualized word embeddings*...represent **a word with a different token based on its context**[2].

> We see that running this process for all the tokens in the input sequence produces a matrix of size T x C. The T stands for time, i.e., you can think of tokens later in the sequence as later in time. The C stands for channel, but is also referred to as "feature" or "dimension" or "embedding size". This length, C, is one of the several "hyperparameters" of the model, and is chosen by the designer to in a **tradeoff between model size and performance**.[1]

## Transformer block

> These form the bulk of any GPT model and are repeated a number of times, with the output of one block feeding into the next, continuing the residual pathway. As is common in deep learning, it's hard to say exactly what each of these layers is doing, but we have some general ideas: the earlier layers tend to focus on learning lower-level features and patterns, while the later layers learn to recognize and understand higher-level abstractions and relationships. In the context of natural language processing, the lower layers might learn grammar, syntax, and simple word associations, while the higher layers might capture more complex semantic relationships, discourse structures, and context-dependent meaning.[1]

В GPT-3 данные последовательно проходят 96 transormer blocks.[6]

### Normalization

Призвана повысить эффективность обучения модели. Значения матрицы по итогам вычислений могут получаться достаточно большими. Нормализация заключается к приведению значений матрицы в диапазон [-1, 1]. Нормализация выполняется перед self attention и mlp.

### Residual connection(pathway)

Призвана повысить эффективность обучения модели. Заключается в сложении матрицы, получившейся после слоя с матрицей, которая была до слоя. Название pathway описывает получаемую архитектуру: если представить, что обработка данных идет по слоям сверху вниз, то свеху вниз будет идти стрелка или "шина" из которого данные подаются на вход каждого слоя и потом туда же вливаются(суммирование), и дальше подаются на следующий слой.

См. [[Residual connections]]

### Self-attention

> The self-attention layer is perhaps the heart of the Transformer and of GPT. It's the phase where the columns in our input embedding matrix "talk" to each other. Up until now, and in all other phases, the columns can be regarded independently. ...the main goal of self-attention is that each column wants to find relevant information from other columns and extract their values, and does so by comparing its query vector to the keys of those other columns. With the added restriction that it can only look in the past. [1]

Последнее ограничение про взгляд только в прошлое называется *causal* self-attention. Также такой подход подчеркивает *autoregressive* природу трансформера, в отличие от BERT(B - biderectional).

Слой self attention представлен несколькими *head*, каждая из которых производит вычисления параллельно, чтобы:

> This increases the model’s capacity to model complex patterns in the input sequence that require **paying attention to different patterns at once**.

Например в GPT-3 в каждом Transformer block 96 голов внимания.

#### Relevance scoring

На этом шаге для данного токена определяется насколько каждый из предыдущих токенов *attend to*(оказывает влияние на смысл) данного.

> The relevance scoring step of attention is conducted by multiplying the **query vector** of the current position with the **keys matrix** [2]

#### Combining information

На предыдущем шаге для данного токена определили степень влияния на него всех предыдущих токенов. Теперь надо определить как именно надо изменить embedding данного токена, чтобы он "впитал" в себя дополнительную информацию из других токенов. И сделать это с учетом важности каждого предыдущего токена, полученной на предыдущем шаге. Это делается с использованием V матрицы.

Для увеличения производительности результаты вычисления K, V векторов могут кэшироваться, потому что при продвижении по тексту можно переиспользовать векторы для тех словы, которые уже обрабатывались на предыдущих шагах.

Другой оптимизацией является переиспользование K,V,Q матриц:

1. multi head - это исходный вариант с полностью независимыми heads, каждая из которых имеет свой набор K,V,Q матриц
1. multi query - K, V одни на все heads, а Q в каждой head своя
1. grouped query - K, V переиспользуются группой heads, Q для каждой head своя

#### Проблема размерности attention matrix

Механизм внимания оценивает связь между словами в обрабатываемом тексте, длина этого текста - это *context length*. Следовательно размерность матрицы внимания равна квадрату длины контекста. Поэтому заложеная в трансформере размерность матрицы внимания ограничивает длину контекста, которую модель "может удержать во внимании". Для решения этой проблемы есть разные подходы, например *sparse attention*.

### Feed-forward neural network(FFN)

Реализована в виде multilevel perceptron (mlp).

Именно в этих составляющих блоков трансформера сосредоточено большинство параметров(весов). Согласно [6] на этом этапе вектор, который изначально соответствал токену обогащается фактами. Например на входе трансформера была фраза "Каким спортом занимался Майкл Джордан?". Сначала механизм снимания дополнит эмбединг токена Джордан знаниями, что имя Майкл. А FFN добавит к этому вектору знания, что Майкл Джордан - это баскетболист.

## LLM head

На ее входи подается результат работы блоков, ее задача - определить следующий токен. **Для каждого токена из словаря** head определяет вероятность того, что именно он должен быть следующим.

> The LM head is a simple neural network layer itself. It is one of multiple possible “heads” to attach to a stack of Transformer blocks to build different kinds of systems.[2]

### Softmax

Это функция, которая применяется к итоговой матрице.

> converts a vector of real numbers (logits) into a probability distribution, ensuring **all outputs are between 0 and 1 and sum up to 1**, allowing them to represent probabilities for different classes, with the largest input getting the highest probability[3]

![Softmax function](img/Softmax%20function.png) [4]

### Temperature

Определяет логику выбора токена, исходя из вычисленной веротяности.

> We can also control the "smoothness" of the distribution by using a temperature parameter. A higher temperature will make the distribution more uniform, and a lower temperature will make it more concentrated on the highest probability tokens.[1]

---

## Источники

1. [LLM Visualization](https://bbycroft.net/llm)
2. [[Hands-On Large Language Models Language Understanding and Generation book]]
3. [Softmax function](https://en.wikipedia.org/wiki/Softmax_function)
4. [LLM и GPT - как работают большие языковые модели? Визуальное введение в трансформеры](https://youtu.be/wjZofJX0v4M?si=QbLJpugSLvsQe76N)
5. [Визуализация внимания, сердце трансформера](https://youtu.be/eMlx5fFNoYc?si=nKmeUQ_6Bi5q_eL-&t=762)
6. [Как LLM могут хранить факты](https://youtu.be/9-Jl0dxWQs8?si=GTxBQSJr3ABXkj2H)

## Ссылки

1. [[Attention]]
1. [[Perceptron]]
