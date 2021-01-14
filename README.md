﻿# Пример виджета для модуля ОАИ КПА (_oai_kpa_stm_) 
_______
# Installation
pip install https://github.com/a-styuf/oai_kpa_stm/releases/download/v.0.1.0/oai_kpa_stm-0.1.0-py3-none-any.whl

or

pip install  https://github.com/a-styuf/oai_kpa_stm/releases/download/v.0.1.0/oai_kpa_stm-0.1.0.tar.gz

# Update
pip uninstall oai_modbus
and do Installation
_______
Данный виджет служит двум целям: 
- либо использование отдельно в качестве инженерного окна для отладки и работы с модулем.
- либо в составе большей программы как инженерное окно 

# Материал для предварительного ознакомления
Перед началом работы с данным примером необходимо ознакомиться со следующими материалами:
- простой туториал по началу работы с pyQt - [тут](https://pythonworld.ru/gui/pyqt5-firstprograms.html)
- использование QtDesigner через наследование класса виджета/окна (важно, используется именно такой подход) - [тут](https://tproger.ru/translations/python-gui-pyqt/)
- [тут](https://cucumbler.ru/blog/articles/nastrojka-pycharm-dlja-raboty-s-bibliotekoj-pyqt5.html) и [тут](https://pythonpyqt.com/how-to-install-pyqt5-in-pycharm/) описан подход использования QtDesigner из под pyCharm

При разработке ~~можно~~ **нужно** обращаться к следующим ресурсам
- длинный [туториал](https://python-scripts.com/pyqt5) по pyQt
- полное [описание](https://doc.qt.io/qt-5/index.html) всех классов Qt5 но все примеры на c++
- [вики](https://wiki.python.org/moin/PyQt/Tutorials) по pyQt
- любой другой ресурс, который вам понравится

# Что такое [Qt](https://ru.wikipedia.org/wiki/Qt)
Кроссплатформенный IDE для разработки программного обеспечения на языке программирования C++. 
Есть также «привязки» ко многим другим языкам программирования: 
- Python — PyQt, PySide;
- Ruby — QtRuby;
- Java — Qt Jambi;
- PHP — PHP-Qt и другие.

Хорошо развит, широко поддерживается, имеет кучу обучающих материалов и примеров. Замечательно документирован. Кроссплатформенный, бесплатный (лицензия GPL3).

# Что такое [pyQt](https://ru.wikipedia.org/wiki/PyQt)
набор расширений (биндингов) графического фреймворка Qt для языка программирования Python, выполненный в виде расширения Python.

# Почему pyQt
pyQt реализует практически весь функционал Qt (т.е. обладает всеми его приимуществами). Имеет встроенное средство создания окон ([QtDesigner](https://ru.wikipedia.org/wiki/Qt_Designer)), что особенно важно при быстром создании инженерных окон.
Не имеет собственно средства рисования графиков, но легко поддерживает [matplotlib](https://pythonspot.com/pyqt5-matplotlib/) (прошлый выбора автора, не очень подходит для онлайн отрисовки) и [pyqtgraph](http://www.pyqtgraph.org/) (выбор автора, отлично подходит для онлайн отрисовки).

# Сохранение параметров модуля
Каждый модуль должен иметь возможность сохранения своих параметров в файл в папке ".\cfg\<unic_name>.json".
В нем есть два словаря: "core" и "user"
Список обязательных параметров - "core" (изменение списка параметров по согласованию с автором):
- серийный номер: serial_num (по умолчанию: на ваш выбор (например "20713699424D"))
- тип окна: widget (по умолчанию: _True_) 
    - используется для задания режима работы: 
        - _True_ - работа в режиме виджета
        - _False_ - работа в режиме отдельного окна
Возможные параметры для сохранения - "user" (составляете на свое усмотрение):
- любые параметры, позволяющие избежать в модуле зависимости от используемой аппаратуры(например: названия стм-параметров (для модуля ОАИ КПА СТМ), границы определения состояний, т.д.)

Для сохранения и вызова настроек используется модуль json ([описание подхода](https://python-scripts.com/json).

У настроек есть приоритеты (от высшего к низшему): параметры __init()__ -> cfg-file -> default.
В случае отсутствия параметров при создании объекта используется параметры из файла конфигурации; в случае отсутствия файла - используются параметры по умолчанию.

# Рекомендумая структура проекта (на примере платы ОАИ КПА СТМ)
* oai_kpa_stm (моудль с виджетом)
	* oai_kpa_stm_data (модуль, обеспечивающий надстройку над регистрами, позволяющий рабоать с платой ничего не зная о нижнем уровне общения)
		* oai_modbus/OAI_Modbus ([модуль](https://github.com/CrinitusFeles/OAI_Modbus), организующий доступ к записи/чтению регистров платы через протокол модбас)
		* oai_kpa_stm_widget_qt (модуль, автоматически сгенеренный из файла, созданного в QtDesigner)

# Naming convention (TBD)
1. Основной модуль - виджет:
    oai_kpa_**xxx** - где xxx сокращение от имени платы или назначения (oai_kpa_stm для модуля сигнальной телеметрии СТМ)
    Существующие сокращения: 
    - mku - матричные команд управления, МКУ
    - power - плата питания
    - mko - MKO
    - etc
2. Модуль обработки данных (TBD):
    oai_kpa_xxx_**data**
3. Модуль для QtDesigner 
    oai_kpa_xxx_**yyy_qt**:
      - yyy - назначение модуля: widget, window, etc (oai_kpa_stm_widget_qt для данного примера)
      - qt - обозначение источника файла - не написанный руками, а сгенеренный QtDesigner

# Предподготовка
_Обязательно_
1. Pycharm Community Edition (Последняя версия)
2. Python 3.7 или 3.8 (Рекомендуется)
3. pyserial, puQt5, pyqtgraph, matplotlib
_Работа с примером_
4. Скачайте архив с библиотекой или клонируйте [репозиторий](https://github.com/a-styuf/oai_kpa_stm) к себе на локальный диск: `git clone https://github.com/a-styuf/oai_kpa_stm)`.
5. Переименуйте все необходимые файлы.

# Сохранение в лог-файл
Обязательным функционалом является сохранение данных в логи. Отмечу, что это не логи работы программы, а лог работы платы.
Лог модуля должен храниться по следующему адресу: _/log/(%Y_%m_%d_<uniq_name>)/"%Y_%m_%d %H-%M-%S <uniq_name> .csv_
 Пример: _log/2020_12_22 oai_kpa_stm/2020_12_22 13-18-35 oai_kpa_stm .csv_
 Формат данных - _.csv_ с разделителем ";".
 Пример:
    * как в csv 
    Time, s;АЦП 0 К 0;АЦП 0 К 1;АЦП 0 К 2;АЦП 0 К 3;АЦП 0 К 4;АЦП 0 К 5;АЦП 0 К 6;АЦП 0 К 7;АЦП 0 К 8;АЦП 0 К 9;АЦП 0 К 10;АЦП 0 К 11;АЦП 0 К 12;АЦП 0 К 13;АЦП 0 К 14;АЦП 0 К 15;АЦП 1 К 0;АЦП 1 К 1;АЦП 1 К 2;АЦП 1 К 3;АЦП 1 К 4;АЦП 1 К 5;АЦП 1 К 6;АЦП 1 К 7;АЦП 1 К 8;АЦП 1 К 9;АЦП 1 К 10;АЦП 1 К 11;АЦП 1 К 12;АЦП 1 К 13;АЦП 1 К 14;АЦП 1 К 15
1.392;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000
2.364;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000;0.000
    * как в excel
|Time, s|АЦП 0 К 0|АЦП 0 К 1|АЦП 0 К 2|АЦП 0 К 3|АЦП 0 К 4|АЦП 0 К 5|АЦП 0 К 6|АЦП 0 К 7|АЦП 0 К 8|АЦП 0 К 9|АЦП 0 К 10|АЦП 0 К 11|АЦП 0 К 12|АЦП 0 К 13|АЦП 0 К 14|АЦП 0 К 15|АЦП 1 К 0|АЦП 1 К 1|АЦП 1 К 2|АЦП 1 К 3|АЦП 1 К 4|АЦП 1 К 5|АЦП 1 К 6|АЦП 1 К 7|АЦП 1 К 8|АЦП 1 К 9|АЦП 1 К 10|АЦП 1 К 11|АЦП 1 К 12|АЦП 1 К 13|АЦП 1 К 14|АЦП 1 К 15|
|-------|---------|---------|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|----------|----------|----------|---------|---------|---------|---------|---------|---------|---------|---------|---------|---------|----------|----------|----------|----------|----------|----------|
|1.392  |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000     |0.000     |0.000     |0.000     |0.000     |0.000     |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000     |0.000     |0.000     |0.000     |0.000     |0.000     |
|2.364  |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000     |0.000     |0.000     |0.000     |0.000     |0.000     |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000    |0.000     |0.000     |0.000     |0.000     |0.000     |0.000     |

Первый столбец _Time, s_ обязательный. Остальные данные по желанию разработчика.

Существует два подхода к заполнению лог файлов:
* периодический (как в данном примере): сохранение среза данных один разв в определенное период
* событийный: сохранение при возникновении определенного события (например при нажатии кнопки на экране происходит запись нового состояния)
* смешанный: и периодическое сохранение, и событийные записи

Первый способ подходит для систем сбора данных (например: телеметрия, питание). Вторая - для событийных модулей (например: uart, команды урпавления).


# Обязательный функционал
1. Блок подключения (виден толькоо при работе в режиме отдельного окна, иначе подключается через общую программу):
    - кнопка подключения
    - строка ввода serial number
    - строка вывода состояния
2. Параметр уникального имени в объекте: нужно для корректного хранения настроек и логов.
3. Сохранение параметров (например серийного номера) в файл с настройками.
4. Не блокирующая работа: ни одна из функций не должна быть блокирующая. В случае необходимости использования подобных функций используются потоки. Функции по работе с общими данными для потоков защищаются замками.
    - [пример 1](https://habr.com/ru/post/149420/)
    - [пример 2](https://python-scripts.com/threading)
5. Вывод актуального состояния в строку состояния.
6. Сохранение в лог-файлы

# Шаблон виджета в QtDesigner
 На данный момент окно делиться условно на две области:
 - userFrame (область для изменения) - область для размещения собственного функционала модуля
 - coreGLayout (желательно не править) - область с базовым функционалом (кнопка подключения, строка для ввода серийного номера и статусная строка)

Обязательно используйте читаемые название для элементов GUI. Сейчас автор использует следующее правило:
Верблюжьим стилем, сначала функциональное назначение, затем название элемента. 
Пример: для таблицы с СТМ-параметрами: _stmTableWidget_, где:
* _stm_ - функциональное назначение
* _TableWidget_ - название виджета

Для создания питон модуля для использования в вашем ПО используется скрипт create_py_from.bat:
```
pyuic5 oai_kpa_stm_widget_qt.ui -o oai_kpa_stm_widget_qt.py
pause
```
Его задача - запустить скрипт для преобразования файла из .ui в .py. 
Так как в pyCharm всегда доступен терминал в нижнем поле (там же, где показывается консоль вашей программы), его удобно запускать оттуда.
Для этого необходимо перейти в закладку "Terminal", а там вбить название bat-ника (create_py_from.bat), и запустить его. В терминале доступно автодополнение по кнопке Tab, что позволяет ввести только первые несколько букв, а затем нажать Tab.

# Проблемы и ошибки
В случае проблем, предложений и вопросов прошу создавать тему через github-овские [issues](https://github.com/a-styuf/oai_kpa_stm/issues)
и обращаться ко мне через whatsapp и почту  
