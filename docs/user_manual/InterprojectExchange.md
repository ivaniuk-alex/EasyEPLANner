# Межпроектный обмен сигналами #

## Содержание ##

1. [Введение](#1-Введение)
2. [Включение редактирования обмена](#2-Как-включить-редактирование-межпроектного-обмена)
3. [Загрузка существующего обмена сигналами](#3-Загрузка-существующего-обмена-сигналами)
4. [Добавление проекта для обмена](#4-Добавление-проекта-для-обмена-сигналами)
5. [Удаление проекта из обмена](#5-Удаление-проекта-из-обмена-сигналами)
6. [Связывание сигналов](#6-Связывание-сигналов)
7. [Изменение связи сигналов](#7-Изменение-связи-сигналов)
8. [Удаление связи сигналов](#8-Удаление-связи-сигналов)
9. [Настройка PAC (контроллера)](#9-Настройка-PAC-контроллера)
10. [Поиск по сигналам и их фильтрация](#10-Сохранение-обмена-сигналами)
11. [Поиск по сигналам и их фильтрация](#11-Поиск-по-сигналам-и-их-фильтрация)
12. [Сообщения об ошибках, предупреждения, информация](#12-Сообщения-об-ошибках-предупреждения-информация)
13. [Ручная доработка старого обмена сигналами в новый](#13-Ручная-доработка-старого-обмена-сигналами-в-новый)
14. [Краткое описание доработки старого обмена сигналами в новый](#14-Краткое-описание-доработки-старого-обмена-сигналами-в-новый)

## 1 Введение ##

<p align="justify">Межпроектный обмен сигналами (<i>устройствами</i>) позволяет настроить обмен сигналами между проектами. После сохранения обмена, будут сгенерированы (<i>или изменены</i>) файлы проектов, между которыми происходит обмен (<i>в папке с каждым проектом, с которым происходит обмен</i>). Эти файлы будут готовый для загрузки в контроллер.</p>

## 2 Как включить редактирование межпроектного обмена ##

<p align="justify">Для того, чтобы начать работу с данным модулем, необходимо включить Eplan, выбрать проект и нажать на пункт "<i>Межконтроллерный обмен сигналами</i>" в меню "<i>EPlaner</i>". Подробнее, на рисунке ниже.</p>

<p align="center"><img src="images/InterProjectExchange/RunExchange.png" alt="Местонахождение меню с модулем"></p>

<p align="center"><b>Рисунок</b> - <em>Местонахождение меню для входа в модуль настройки обмена</em></p>

#### Техническая информация ####

<p align="justify">После нажатия этой кнопки, будут обновлены все устройства проекта (<i>обновятся файлы проекта</i>), и будет происходить считывание данных, которые нужны для обмена.</p>

Порядок считывания файлов проекта:
1. main.io.lua
2. main.devices.lua
3. shared.lua (_если есть_)

<p align="justify">Если есть <i>shared.lua</i>, будет считано описание обмена и для тех проектов, которые описаны в этом файле. Обмен считывается для того проекта, который открыт в Eplan. Следовательно, полностью файлы <i>shared.lua</i> для остальных проектов не считываются. Считывается только информация клиента и сервера для редактируемого проекта (<i>который открыт в Eplan</i>) Для остальных проектов, порядок считывания файлов будет такой же, как описан выше, только наличие <i>shared.lua</i> является обязательным.</p>

<p align="justify">Предполагается, что проекты находятся в одной папке, так как путь к папке с проектами считывается из файла конфигурации надстройки.</p>

## 3 Загрузка существующего обмена сигналами ##

<p align="justify">Для того, чтобы существующий обмен сигналов был успешно загружен, необходимо, чтобы все связанные проекты имели файл <i>shared.lua</i>. Модуль считыват весь файл <i>shared.lua</i> для текущего открытого (<i>главного</i>) проекта, и нужные куски из остальных (<i>альтернативных проектов</i>). На рисунке ниже показан пример, как может выглядеть обмен между двумя проектами.</p>

<p align="center"><img src="images/InterProjectExchange/MainWindow.png" alt="Главное окно межпроектного обмена"></p>

<p align="center"><b>Рисунок</b> - <em>Главное окно обмена сигналами между проектами</em></p>

Описание формы:

1. Список сигналов с их описанием, которые есть в главном проекте
2. Список уже связанных сигналов (_сопоставление_)
3. Список сигналов с их описанием, которые есть в выбранном альтернативном проекте
4. Название главного проекта
5. Строка поиска по сигналам главного проекта
6. Кнопка настройки фильтрации сигналов в главном проекте (_описано в следующих разделах_)
7. Селектор переключения режима обмена сигналами
8. Кнопка настройки контроллеров проектов. Настраиваются альтернативные проекты
9. Селектор переключения проекта, с которым настраивается обмен сигналами
10. Кнопка добавления связи с проектом
11. Кнопка удаления связи с проектом
12. Строка поиска по сигналам выбранного альтернативного проекта
13. Кнопка сохранения результатов редактирования обмена
14. Кнопка отмены редактирования (_сделанные изменения не сохраняются_)

#### Что такое режим, и как этим пользоваться ####
<p align="justify">Режим обмена (<i>цифра <b>7</b> на картинке</i>), позволяет переключить направление обмена сигналами. Режим определяется относительно главного проекта. Когда включен режим "<b>Источник >> Приемник</b>" - это значит, что главный проект (<i>PL-Тест-монитора, на рисунке</i>) является <b>источником (сервером)</b> сигналов, а альтернативный проект является <b>приемником (клиентом)</b> сигналов. Таким образом, сигналы главного проекта будут записаны в <b>shared_devices</b>, а сигналы альтернативного проекта в <b>remote_gateways</b> (<i>настройка PAC для remote_gateways описана в разделах ниже</i>).</p>

<p align="justify">Режим обмена "<b>Приемник >> Источник</b>" работает в обратном порядке. В этом режиме, главный проект является приемником сигналов из альтернативного проекта (<b>источника</b>). Сигналы главного проекта будут записаны в <b>remote_gateways</b>, а альтернативного в <b>shared_devices</b>.

#### Множественность проектов ####
<p align="justify">Модуль настройки обмена сигналами работает одновременно со всеми проектами, которые есть в списке связуемых проектов. Это позволяет настроить весь обмен между проектами за раз, но также можно настраивать и по одному проекту и сохранять.</p>

#### Примечание ####
Для загрузки контроллерного обмена, который был сделан руками (_до появления этой функциональности_), необходимо привести файл к виду, описааному в [главе 13](#13-Ручная-доработка-старого-обмена-сигналами-в-новый) и [главе 14](#14-Краткое-описание-доработки-старого-обмена-сигналами-в-новый) данного руководства.

## 4 Добавление проекта для обмена сигналами ##

Для добавления обмена сигналами с каким либо проектами, необходимо нажать кнопку со знаком "_Плюс_", на главной форме (_см. рисунок ниже_).

<p align="center"><img src="images/InterProjectExchange/AddProjectFirstStep.png" alt="Кнопка добавления проекта"></p>

<p align="center"><b>Рисунок</b> - <em>Кнопка добавления проекта для обмена</em></p>

Откроется диалог, который будет указывать на папку с проектами, и в этой папке необходимо выбрать папку с проектом, с которым нужно добавить обмен сигналами.

<p align="center"><img src="images/InterProjectExchange/AddProjectSecondStep.png" alt="Диалог выбра папки с проектом"></p>

<p align="center"><b>Рисунок</b> - <em>Диалог выбора папки с проектом</em></p>

После выбора папки и подтверждени выбора, система загрузит файлы проекта (_порядок описан в [главе 2](#2-Как-включить-редактирование-межпроектного-обмена)_), и для настройки сигналов будет выбран загруженный проект.

Если у добавляемого проекта есть <i>shared.lua</i> файл, то он будет прочитан.

#### Техническая информация ####
Файл <i>shared.lua</i> читается, что бы знать его структуру, так как у загружаемого проекта есть связи с другими проектами, то их нельзя нарушать. Предполагается, что в этом файле нет связи с главным проектом на момент загрузки (<i>с точки зрения обмена точка-точка это невозможно, т.к. был бы считан обмен в главном проекте</i>).

## 5 Удаление проекта из обмена сигналами ##

Для удаления связи с проектом, необходимо выбрать этот проект в списке проектов и нажать кнопку удаления со знаком "_Минус_", на главной форме (_см. рисунок ниже_).

<p align="center"><img src="images/InterProjectExchange/DeleteProjectFirstStep.png" alt="Кнопка удаления связи с проектом"></p>

<p align="center"><b>Рисунок</b> - <em>Кнопка удаления связи с проектом</em></p>

После этого, подтвердить или отменить действие

<p align="center"><img src="images/InterProjectExchange/DeleteProjectSecondStep.png" alt="Диалоговое окно подтверждения действия удаления"></p>

<p align="center"><b>Рисунок</b> - <em>Диалоговое окно подтверждения удаления проекта</em></p>

<p align="justify">Если действие будет отменено - ничего не изменится, а если подтверждено - то этот проект будет помечен "<i>На удаление</i>" и убран из списка проектов. Получить к нему доступ будет нельзя.</p>

<p align="justify">При сохранении результатов, помеченный на удаление проект будет удален из файлов <i>shared.lua</i>. Если у удаленного проекта нет никакого обмена сигналами с другими проектами, будет удален и сам файл <i>shared.lua</i></p>

#### Механизм восстановления удаленного проекта ####
<p align="justify">Модуль умеет восстанавливтаь удаленный из обмена проект, если он был удален в той же сессии, в которой и восстанавливается. Особенность заключается в том, что проект не удаляется из системы до момента сохранения результатов работы, это позволяет, в случае необходимости, вернуть обратно удаленный проект (<i>если удаление ведется в той же сессии</i>), просто добавив его обратно через механизм добавления проектов, и, система восстановит проект с настройками контроллера и сигналами, которые были на момент удаления обмена с проектом.</p>

## 6 Связывание сигналов ##

<p align="justify">Связывание сигналов между собой представляет механизм выбора нужных сигналов в списках, а система автоматически связывает их. Рассмотрим подробнее.</p>

<p align="justify">Перед связыванием сигналов, необходимо выбрать режим обмена и проект, с которым нужно обмениваться. От режима обмена зависит, какие сигналы, и куда будут записываться.</p>

Для связывания сигналов нужно сделать два простых действия (см. форму в [главе 3](#3-Загрузка-существующего-обмена-сигналами)):
1. Выбрать сигнал в списке сигналов главного проекта
2. Выбрать сигнал в списке сигналов альтернативного проекта

<p align="justify">После этого, система автоматически свяжет эти сигналы. Последовательность в списке выше не важна. Можно выбрать сигналы и в противоположном порядке.</p>

#### Примечание ####

<p align="justify">Главными типами сигналов являются сигналы главного проекта. При связывании <b>AI</b> сигнала главного проекта с <b>AO</b> сигналом альтернативного проекта, сигнал будет добавлен в группу <b>AI</b>.</p>

Система не всегда может определить тип сигнала, рассмотрим эти случаи:
1. Оба типа сигнала известны - все связывается автоматически
2. Известен тип сигнала альтернативного проекта - будет выбран противоположный ему тип (_например, выбран <b>AI</b>, будет добавлено в <b>AO</b>_)
3. Известен тип сигнала главного проекта - аналогично пункту <b>1</b>
4. Неизвестны оба типа сигналов - будет показано окно, где надо отметить тип сигнала для главного проекта (_см. рисунок ниже_)

<p align="center"><img src="images/InterProjectExchange/UnknownDeviceType.png" alt="Окно выбора типа сигнала"></p>

<p align="center"><b>Рисунок</b> - <em>Окно выбора типа сигнала</em></p>

Описание формы:
1. Кнопки выбора типа сигнала главного (_текущего_) проекта
2. Кнопка сохранения выбора (_произойдет связывание сигналов_)
3. Отмена выбора (_произойдет отмена связывания сигналов_)

<p align="justify">После связывания сигнала, все изменения сохраняются в текущей сессии и можно легко менять проект, добавлять, удалять и переключать режимы.</p>

#### Возможность поиска сигналов не пользуясь полем поиска ####

<p align="justify">Для каждого из списков с сигналами есть возможность поиска сигнала (<i>идет поиск не только по его ОУ, но и по описанию</i>). Для этого, нужно что бы фокус был на списке (<i>элементе управления</i>). Для этого, можно нажать на любой сигнал и начать что-нибудь вводить на клавиатуре. В поле поиска будут вноситься вводимые буквы и система будет искать совпадения в списке по вашему запросу. Клавиши <b>BackSpace</b> и <b>Delete</b>, соответственно, удаляют то, что вы ввели.</p>

## 7 Изменение связи сигналов ##

<p align="justify">Уже существующие связи сигналов можно редактировать. Модуль позволяет менять любой сигнал на любой другой. Для этого, необходимо выбрать сигналы (пару), которую нужно редактировать.</p>

<p align="center"><img src="images/InterProjectExchange/UpdateSignalsBind.png" alt="Режим редактирования пары сигналов"></p>

<p align="center"><b>Рисунок</b> - <em>Режим редактирования пары сигналов</em></p>

<p align="justify">На рисунке показана выбранная пара сигналов (<i>цифра 1</i>), а также подсвечены сигналы в списках сигналов по проектам (<i>цифра 2</i>). Соответственно, после этого, мы можем менять выбранный сигнал в главном и альтернативных проектах. Для этого просто кликаем на интересующие сигналы в списках и они будут заменены на новые.</p>

<p align="justify">Для того, что бы закончить редактирование, снять выделение с пары сигналов, можно нажать клавишу <b>Enter</b>, или <b>Esc</b>, или кликнуть на пустое место в этом списке (<i>или в любое другое пустое место в элементах формы</i>).</p>

Навигация по спискам (всем трем спискам) может осуществляться с помощью стрелочек вверх и вниз на клавиатуре.

#### ВНИМАНИЕ ####

<p align="justify">Сделано допущение, что можно поменять любой сигнал на любой другой, без изменения группы. Учитывайте это при изменении одного типа сигнала на другой.</p>

## 8 Удаление связи сигналов ##

<p align="justify">Для удаления связи сигналов между собой, необходимо выбрать связь в списке связанных сигналов и нажать клавишу <b>Delete</b>. Связь будет разорвана, сигналы будут удалены из списка связанных сигналов. (<i>см. рисунок ниже</i>)</p>

<p align="center"><img src="images/InterProjectExchange/DeleteSignalBind.png" alt="Состояние сигнала перед удалением"></p>

<p align="center"><b>Рисунок</b> - <em>Состояние окна с сигналами перед удалением сигнала</em></p>

<p align="justify">Для того, что бы отменить выбор с пары сигналов (<i>снять выделение</i>), можно нажать клавишу <b>Enter</b>, или <b>Esc</b>, или кликнуть на пустое место в этом списке (<i>или в любое другое пустое место в элементах формы</i>).</p>

Навигация по спискам (всем трем спискам) может осуществляться с помощью стрелочек вверх и вниз на клавиатуре.

## 9 Настройка PAC (контроллера) ##

Модуль позволяет настраивать параметры контроллера для межпроектного обмена (см. рисунок ниже). 

<p align="center"><img src="images/InterProjectExchange/PACSettings.png" alt="Состояние сигнала перед удалением"></p>

<p align="center"><b>Рисунок</b> - <em>Состояние окна с сигналами перед удалением сигнала</em></p>

Описание формы:
1. Режим работы главного проекта (_Источник (сервер), Приемник (клиент)_)
2. Список с загруженными для обмена проектами
3. Имя проекта, с которым настраивается контроллер
4. IP-Адрес контроллера (_будет меняться в зависимости от режима главного проекта_)
5. IP-Адрес эмулятора
6. Кнопки включения/отключения эмуляции
7. Время цикла, мс.
8. Таймаут, мс.
9. Порт
10. Кнопки включения/отключения шлюза
11. Номер станции Modbus (_номер клиента_)
12. Кнопка применения и сохранение изменений
13. Кнопка отмены изменений и закрытия формы

#### Как с этим работать ####

Обмен сигналами у нас идет от по принципу "<b>Источник - Приемник</b>" или "<b>Приемник - Источник</b>". 

<p align="justify">Когда селектор главного проекта стоит в положении "<b>Источник</b>", настраивается контроллер для альтернативного проекта. В поле "<b>IP-Адрес</b>" указывается адрес главного проекта (<i>автоматически</i>). Эта информация будет записана в <i>shared.lua</i> файл альтернативного проекта.</p>

<p align="justify">При переключении селектора главного проекта в положение "<b>Приемник</b>", настраивается контроллер для главного проекта. В поле "<b>IP-Адрес</b>" указывается адрес альтернативного проекта (<i>который выбран из списка</i>). Эта информация будет записана в <i>shared.lua</i> файл главного проекта.</p>

<p align="justify">Таким образом, когда селектор режима главного проекта стоит в положении "<b>Источник</b>", то настраиваются параметры чтения сигналов проекта альтернативными проектами, а когда селектор стоит в положении "<b>Приемник</b>", уже наоборот - настраиваются параметры чтения сигналов проекта главным проектом из альтернативных проектов.</p>

## 10 Сохранение обмена сигналами ##

<p align="justify">Для того, чтобы сохранить обмен сигналами, нужно нажать кнопку "<b>Сохранить</b>" в главном окне. Система запустит сохранение и закроет форму. В итоге, будут сгенерированы файлы <i>shared.lua</i> для всех проектов, которые участвовали в обмене сигналами. Файл <i>shared.lua</i> главного проекта будет перезаписан (<i>он всегда генерируется с нуля</i>), а файлы <i>shared.lua</i> альтернативных проектов будут изменены точечно.</p>

<p align="justify">Изменения альтернативных проектов работает так: если обмена с этим проектом не было, то запись будет сделана в начале определения переменной (<i>если это прием сигналов - в начале переменной <b>remote_gateways</b>, а если источник сигналов - в начале <b>shared_devices</b></i>), а если обмен с этим проектом уже был (<i>редактировали сигналы, настройки контроллера</i>), то изменения будут произведены в том месте, где редактировался обмен сигналами (<i>там, где будут найдены строки, отвечающие за обмен с конкретным проектом</i>).</p>

<p align="justify">Предполагается, что проекты находятся в одной папке, так как путь к папке с проектами считывается из файла конфигурации надстройки.</p>

## 11 Поиск по сигналам и их фильтрация ##

Модуль поддерживает поиск по сигналам и их фильтрацию. Для того, чтобы воспользоваться поиском, нужно ввести необходимую комбинацию в одну из строк поиска (см. [главу 3](#3-Загрузка-существующего-обмена-сигналами), главную форму, где находятся строки поиска). 

<p align="justify">Также, для каждого из списков с сигналами есть возможность поиска сигнала (<i>идет поиск не только по его ОУ, но и по описанию</i>). Для этого, нужно что бы фокус был на списке (<i>элементе управления</i>). Для этого, можно нажать на любой сигнал и начать что-нибудь вводить на клавиатуре. В поле поиска будут вноситься вводимые буквы и система будет искать совпадения в списке по вашему запросу. Клавиши <b>BackSpace</b> и <b>Delete</b>, соответственно, удаляют то, что вы ввели.</p>

Помимо поиска существует механизм фильтрации сигналов по их типам (см. рисунок ниже)

<p align="center"><img src="images/InterProjectExchange/FilterSettings.png" alt="Состояние сигнала перед удалением"></p>

<p align="center"><b>Рисунок</b> - <em>Окно настройки фильтрации</em></p>

Описание формы:
1. Кнопка очистки текущего фильтра
2. Список типов сигналов для главного проекта
3. Включение/отключение группировки сигналов по группам
4. Список типов сигналов для альтернативного проекта
5. Кнопка применения изменений
6. Кнопка отмены изменений

<p align="justify">Фильтр настраивается сколь угодно раз. Его конфигурация хранится в конфигурационном файле надстройки и загружается каждый раз, когда загружается окно фильтра, поэтому можно задать настройки один раз и они будут постоянно подгружаться. Для того, что бы настроить фильтр, нужно отметить нужные типы сигналов для отображения (<i>поставить галочки</i>). Те сигналы и пункты, которые не выбраны, будут деактивированы и скрыты.</p>

## 12 Сообщения об ошибках, предупреждения, информация ##

Здесь описаны различные сообщения об ошибках, которые могут возникать, и то, из-за чего они возникают.

Критические ошибки:
1. "Не найден файл __main.io.lua__ проекта <_название проекта_>" - при загрузке описания проекта, не был найден файл __main.io.lua__, из которого берется нужная информация по устройствам и контроллеру.
2. "Не найден файл __main.devices.lua__ <_название проекта_>" - при загрузке описания проекта, не был найден файл __main.devices.lua__, из которого берутся определения устройств, которые находятся в файле __shared.lua__.
3. "Не найден файл конфигурации для фильтра" - система не смогла найти файл, в котором сохранены настройки фильтрации.
4. "Ошибка сохранения промежуточных данных" - при настройке PAC (_контроллера_), не смогли сохраниться данные в одном из трех случаев:
    - изменение режима главного проекта
    - изменение выбранного альтернативного проекта
    - при сохранении всех результатов не смог сохраниться результат, по последнему редактируемому проекту.
5. "Ошибка изменения связи" - система не смогла отредактировать связь между сигналами (_изменить один из связанных сигналов_).
6. "Ошибка удаления связи" - система не смогла удалить связь между парой сигналов.
7. "Ошибка подсветки выделенных элементов в списках" - система не смогла подсветить выбранную пару сигналов в списках сигналов по проектам.
8. "Данные по проекту не загружены" - система не смогла загрузить данные по проекту во время добавления проекта к обмену сигналами.
9. "Ошибка при удалении связи с проектом" - система не смогла удалить связь с проектом (_пометить проект на удаление_).

Предупреждения:
1. "Устройства имеют одинаковый тип сигнала" - связываются устройства, с одинаковым типом сигнала (_например, **DI** и **DI**_).
2. "Разные типы сигналов (_попытка связать дискретные с аналоговыми или наоборот_)" - связываются сигналы с разными типами (_**AI** и **DO**, например_).
3. "По указанному пути не найдены файлы проекта" - система не обнаружила файлы проекта при добавлении проекта к обмену сигналами.
4. "Проверьте номера станций главного проекта в режиме "Источник", дублирование номеров" - дублируются номера станций главного проекта (_который открыт в EPLAN_).

Информационные:
1. "Проект <_название проекта_> уже обменивается с этим проектом сигналами" - такой проект уже обменивается сигналами с главным проектом.

## 13 Ручная доработка старого обмена сигналами в новый ##

<p align = "justify">В этом разделе расписано, что надо переделать в старом описании <i>shared.lua</i>, что бы его можно было редактировать через этот модуль. Пример основан на тестовых проектах, главный проект - <code>"PL-Тест-монитора"</code>, альтернативный проект - <code>"PL-Тест-монитора-новый"</code>. Обмен происходит в режиме <code>"Источник >> Приемник"</code>.</p>

<p align = "justify">Главный и альтернативный проект ничем не отличаются по своей сути, кроме того, что главный - всегда открыт в EPLAN, а альтернативный - обменивается с главным проектом. Любой открытый проект в EPLAN будет являться главным, а все проекты, с которыми он обменивается - альтернативными.</p>

Фрагмент кода в новом исполнении из главного проекта:

```LUA
shared_devices =
{
    [2] =
    {
        projectName = "PL-Тест-монитора-новый",
        DI =
        {
        __DI2,
        __DI1,
        __DI3,
        },
        DO =
        {
        DO1,
        DO2,
        DO3,
        },
        AI =
        {
        __AI2,
        __AI1,
        __AI3,
        },
        AO =
        {
        AO3,
        AO2,
        AO2,
        },
    },
}
```

Фрагмент кода в старом исполнении из главного проекта:

```LUA
shared_devices =
{
    -- PL-Тест-монитора-новый
    [2] =
    {
        DI =
        {
        __DI2,
        __DI1,
        __DI3,
        },
        DO =
        {
        DO1,
        DO2,
        DO3,
        },
        AI =
        {
        __AI2,
        __AI1,
        __AI3,
        },
        AO =
        {
        AO3,
        AO2,
        AO2,
        },
    },
}
```

Как видно из кода, мы добавили строку `projectName = "PL-Тест-монитора-новый"` в _shared_lua_ главного проекта (который открыт в Eplan). В этом коде, главный проект отправляет свои сигналы проекту, имя которого мы добавили.

Рассмотрим фрагменты кода, когда мы принимаем сигналы из главногое проекта в альтернативном (который не открыт в Eplan, но главный проект взаимодействует с этим проектом, как с альтернативным).

Фрагмент кода в новом исполнении для альтернативного проекта:

```LUA
remote_gateways =
{
    ['PL-Тест-монитора'] =
    {
        ip = '127.0.0.1',    -- адрес удаленного контроллера
        ipemulator = '127.0.0.1',    -- адрес удаленного контроллера при эмуляции на столе
        emulation = false,    -- включение эмуляции
        cycletime = 200,    -- время ожидания между опросами контроллера
        timeout = 300,    -- таймаут для modbus клиента
        port = 10502,    -- modbus - порт удаленного контроллера
        enabled = true,    -- включить/выключить шлюз
        station = 2,    -- номер станции modbus удаленного клиента
        DO =
        {
        DO2,
        DO1,
        DO3,
        },
        DI =
        {
        __DI2,
        __DI3,
        __LS2,
        },
        AO =
        {
        AO2,
        AO1,
        AO3,
        },
        AI =
        {
        __AI3,
        __AI2,
        __AI1,
        },
    },
}
```

Фрагмент кода в старом исполнении для альтернативного проекта:

```LUA
remote_gateways =
{
    ['PL-TEST-MONITOR'] =
    {
        ip = '127.0.0.1',    -- адрес удаленного контроллера
        ipemulator = '127.0.0.1',    -- адрес удаленного контроллера при эмуляции на столе
        emulation = false,    -- включение эмуляции
        cycletime = 200,    -- время ожидания между опросами контроллера
        timeout = 300,    -- таймаут для modbus клиента
        port = 10502,    -- modbus - порт удаленного контроллера
        enabled = true,    -- включить/выключить шлюз
        station = 2,    -- номер станции modbus удаленного клиента
        DO =
        {
        DO2,
        DO1,
        DO3,
        },
        DI =
        {
        __DI2,
        __DI3,
        __LS2,
        },
        AO =
        {
        AO2,
        AO1,
        AO3,
        },
        AI =
        {
        __AI3,
        __AI2,
        __AI1,
        },
    },
}
```

<p align="justify">Как видно из описанных выше кусков кода, мы всего-лишь заменили название проекта с <code>PL-TEST-MONITOR</code> на <code>PL-Тест-монитора</code>. Первое имя нигде и никогда не фигурировало и по нему нельзя узнать, из какого проекта данные нужно читать. Второе (<i>новое</i>) имя, позволит нам найти проект, из которого нам надо читать сигналы (<i>новое имя совпадает с названием проекта Eplan и с названием каталога с .lua файлами проекта</i>).</p>

#### Итог ####

Когда мы указываем проект в __shared_devices__, мы также должны указывать строку с названием проекта <code>projectName = "PL-Тест-монитора-новый"</code>, для того, что бы при считывании __shared_devices__ мы знали, какому проекту эти сигналы отправляются. В этом случае, мы отправляем сигналы проекту <code>PL-Тест-монитора-новый</code>.

Также, когда мы указываем настройки контроллера в __remote_gateways__, ключом таблицы является текущее название проекта <code>['PL-Тест-монитора']</code>, из которого читаются сигналы. Так мы можем знать, в каком проекте надо сигналы прочитать. В этом случае мы читаем сигналы из проекта - <code>PL-Тест-монитора</code>

Это все изменения, которые позволят читать межконтроллерный обмен, сделанный до появления этого функционала. В данном примере, эти проекты являются реальными тестовыми проектами и имеют каталоги с аналогичными названиями, где хранятся их файлы _.lua_.

## 14 Краткое описание доработки старого обмена сигналами в новый  ##

Контроллер одновременно является **сервером** и **клиентом** и работает сразу в этих двух режимах. Например, в проекте `T1-Проект` настраивается связь с каким-то проектом `T2-Проект`, режим в котором рассматривается перенастройка обмена - __Источник >> Приемник__.

### 14.1 Преобразование сервера (источника) ###
Когда контроллер в режиме сервера, то он может передать сигналы клиентам, которые к нему подключились. Идентификация клиента идет по номеру. Номером является индекс массива. Индексы клиентов, и их имена прописываются в **shared_devices**.

Номера клиентов на сервере **не** могут пересекаться, и система это контролирует. Внизу представлен фрагмент старого описания отправки трех сигналов с сервера клиенту с номером 2 (_номер станции_). Номер станции выступает одним из связующих элементов, который связывает клиент и сервер (_сервер отправляет данные клиенту под номером **Х**, а клиент подключается к серверу с обозначением, что это именно он клиент номер **Х**_).

```LUA
shared_devices =
{
    [2] = -- 2 это номер клиента (станции Modbus).
    {
        DI =
        {
        __DI2,
        __DI1,
        __DI3,
        },
    },
}
```

Для того, чтобы идентифицировать клиент (*для новой функциональности*) необходимо добавить строку с названием проекта (*контроллер которого является клиентом*): 

`projectName = 'Название-проекта,'` Запятая нужна, это особенность синтаксиса LUA.

В нашем случае, так как мы из проекта `T1-Проект` отдаем сигналы в `T2-Проект`, то и projectName будет равен `T2-Проект`. Это название проекта-клиента.
В итоге, новое описание должно выглядеть так:

```LUA
shared_devices =
{
    [2] =
    {
        projectName = 'T2-Проект',
        DI =
        {
        __DI2,
        __DI1,
        __DI3,
        },
    },
}
```

**Рекомендуется** брать название проекта для вставки из общего пула проектов (*папки с проектами*), где эти имена генерируются автоматически.

### 14.1 Преобразование клиента (приемника) ###
Контроллер в режиме клиента может подключаться к другому контроллеру, обращаться к его серверной части и получать сигналы. Клиенты прописываются в **remote_gateways**. Номера клиентов в клиентской части **могут** пересекаться, потому что сервер всегда разный.

Ниже приведен код старого описания:
```LUA
remote_gateways =
{
    ['PL_TEST_MONITOR'] = -- Название, которое необходимо изменить
    {
        ip = '127.0.0.1',    -- адрес удаленного контроллера
        ipemulator = '127.0.0.1',    -- адрес удаленного контроллера при эмуляции на столе
        emulation = false,    -- включение эмуляции
        cycletime = 200,    -- время ожидания между опросами контроллера
        timeout = 300,    -- таймаут для modbus клиента
        port = 10502,    -- modbus - порт удаленного контроллера
        enabled = true,    -- включить/выключить шлюз
        station = 2,    -- номер станции modbus удаленного клиента
        DO =
        {
        DO2,
        DO1,
        DO3,
        },
    },
}
```

В старом описании название проекта нигде не фигурировало и писалось латиницей. В новом же описании это название нужно изменить на корректное название проекта. Название проекта нужно брать также, как и при преобразовании сервера (*из папки с проектами*). Это название используется при идентификации клиентом сервера (*чтобы проект мог найти сервер, к которому ему надо подключиться*), следовательно, это название будет фигурировать в файлах, которые не являются файлами проекта с названием клиента. Например, настройка связи проекта `T1-Проект` с проектом `T2-Проект`. В файле **shared.lua** проекта `T2-Проект` будет указано имя сервера, а именно `T1-Проект`. Пример ниже.

**ВАЖНО** - для логики контроллера название сервера и клиента (*в __shared_devices__ и __remote_gateways__*) не важно. Это нужно для того, чтобы функционал межконтроллерного обмена мог считать данные по обмену.

```LUA
remote_gateways =
{
    ['T1-Проект'] = -- Новое название (клиент обращается к этому проекту).
    {
        ip = '127.0.0.1',    -- адрес удаленного контроллера
        ipemulator = '127.0.0.1',    -- адрес удаленного контроллера при эмуляции на столе
        emulation = false,    -- включение эмуляции
        cycletime = 200,    -- время ожидания между опросами контроллера
        timeout = 300,    -- таймаут для modbus клиента
        port = 10502,    -- modbus - порт удаленного контроллера
        enabled = true,    -- включить/выключить шлюз
        station = 2,    -- номер станции modbus удаленного клиента
        DO =
        {
        DO2,
        DO1,
        DO3,
        },
    },
}
```

В режиме __Источник >> Приемник__ для такой настройки меняется **shared_devices** в редактируемом проекте и **remote_gateways** в проектах-клиентах. В обратном режиме наоборот.