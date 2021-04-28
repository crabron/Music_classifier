# Music_classifier

**Оглавление**

- [Вступление](#Вступление)
- [Цель проекта](#Цель%проекта)
- [Задачи](#Задачи)
- [Инструменты](#Инструменты)
- [Требования](#Требования)
- [Ссылки](#Ссылки)
- [Презентация](#Презентация)

Материалы для проекта по классификации музыкальных жанров треков, отобранных студентами Института биоинформатики 2020/21 года

__Студенты__:\
Григорий Гладков \
Алексей Зверев \
Анна Квач \
Маргарита Комарова \
Анна Шемякина

__Руководитель__: Александр Ильин (Институт биоинформатики)

## Вступление

Данный учебный проект призван ознакомить студентов с выделением признаков из аудиофайлов, жанровой кластеризации и классификации объектов на основке выделенных признаков, а также с работой с несбалансированными данными.

Датасет, составленный студентами Института Биоинформатики, состоит из 322 аудиофайлов в форматах mp3 и flac и сопровожден описанием в формате Google Sheets.

Лист, описывающий треки, содержит следующие колонки:
- имя студента, который предложил трек (student)
- название песни и имя автора (song)
- основной жанр (coarse_genre)
- опционально от одного до трех поджанров (genre1, genre2, genre3).

## Цель%проекта

Жанровая классификация индивидуального датасета из 322 треков с помощью Python.

## Задачи

1) Обработать собранный студентами Института биоинформатики датасет

2) Дополнить датасет для сбалансирования представленности жанров

3) Выделить признаки треков
- с использованием пакета Librosa в Python
- с использованием Spotify API (spotipy)

4) Провести кластеризацию треков с помощью алгоритма t-SNE

5) Обучить классификатор RandomForest (с использованием GridSearch)

## Классификаторы

Из-за несбалансированности исходного датасета для дальнейшей работы были отобраны аудиофайлы, относящиеся к самым представленным жанрам - 6 жанров для Spotify-based классификатора и __10__ жанров для Librosa-based классификатора. Также нами была исключена информация о поджанрах. 

В случае использования Spotify API требовалось только название песни и имя автора, однако выделение признаков было возможно только для треков, уже находящихся в коллекции spotipy. Таким образом от датасета студентов осталось 300 треков.
Вначале RandomForest был обучен на выборке из стандартных плейлистов Spotify (по 1000 треков на каждый из 6 жанров). В качестве тест выборок использовался датасет студентов из 300 треков, а также стандартная выборка Spotify для сравнения (в этом случае датасет был разбит на обучающую и тест выборки). Еще один классификатор был обучен по признакам Spotify на самом датасете студентов, в котором были оставлены только треки наиболее представленных 6 жанров.

Для выделения признаков с помощью Librosa мы использовали аудиофайлы, собранные студентами ИБ. Чтобы сбалансировать датасет, мы выбрали топ-10 самых частых жанров и добывили еще аудиофайлов в эти жанры, чтобы к каждому из них относилось как минимум 18 песен. Далее мы исключили из нашего датасета все песни, относящиеся к минорным жанрам (те, что не вошли в топ-10). Полученный датасет (307 треков) был разбит на обучающую и тест выборки, которые были использованы для обучения и проверки качества работы RandomForest.

Результаты сопровождаются визуализацией кластеризаций выборок по признакам с использованием алгоритма t-SNE.

## Результаты

Весь результаты представлены в Jupyter notebook облачного сервиса Google Colab.

- Spotify_features.ipynb - получение признаков, составление датасетов признаков через Spotify API
- based_on_Librosa_features.ipynb - получение признаков на основе анализа структуры музыкального трека при помощи библиотеки librosa и классификация полученных данных 
- MC_rf_and_plots.ipynb - классификация(Spotify API признаки) и визуализация полученных данных
- Additional_colab_notebook.ipynb - дополнительный код с предобработкой данных, не вошел в результаты


## Выводы

- Мы сделали два классификатора, используя комплексные метрики Spotify и базовые метрики Librosa.
- Протестировали Spotify-based классификатор на Test выборке из самого Spotify и на датасете студентов
- Обучили и протестировали Librosa-based классификатор на датасете студентов
- Наши классификаторы лучше всего научились вычленять песни жанра металл, что позволяет заключить, что металл - наиболее устойчивый и хорошо очерченный жанр
- Лучшие результаты по определению основных жанров были получены при обучении Spotify-based классификатора на данных Spotify и тестировании на датасете студентов
- Датасет студентов содержит специфические песни, что не позволят хорошо обучить классификатор

## Инструменты

Выделение признаков треков - Spotify Web API [[1]](#1), spotipy package [[2]](#2), Librosa package [[3]](#3).
Среда для разработки скрипта - Google Colab [[4]](#4).

## Требования

Совместимость гарантирвана для следующих версий пакетов и API Python:

    numpy=1.19.5
    pandas=1.1.5
    seaborn =0.11.1
    matplotlib=3.2.2
    sklearn=0.22.2
    pickle= 0.7.5
    librosa=0.8.0
    spotipy==2.18.0
    gspread==3.0.1
    google.colab=1.0.0 
    oauth2client.client==4.1.3


## Ссылки
<a id="1">[1]</a> 
Spotify Web API:
https://developer.spotify.com/documentation/web-api/

<a id="2">[2]</a> 
spotipy:
https://spotipy.readthedocs.io/en/2.18.0/

<a id="3">[3]</a> 
McFee, Brian, Colin Raffel, Dawen Liang, Daniel PW Ellis, Matt McVicar, Eric Battenberg, and Oriol Nieto. "librosa: Audio and music signal analysis in python." In Proceedings of the 14th python in science conference, pp. 18-25. 2015.

<a id="4">[4]</a> 
Bisong E. (2019) Google Colaboratory. In: Building Machine Learning and Deep Learning Models on Google Cloud Platform. Apress, Berkeley, CA. https://doi.org/10.1007/978-1-4842-4470-8_7

## Презентация

songs_classifiers.pdf
https://docs.google.com/presentation/d/1XLVwMDuXrFdom3C0PVwFac_0YjH7mhg_hUu-SEYEX8A/edit?usp=sharing
