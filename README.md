# OneScriptForStorage

Создание хранилищ по технологии разветвленной разработки фирмы 1С на OneScript

## Шаги и сложности

1. Начал разработку на MacOS (на винде точно проще будет, но пока так попробуем)
2. При начале работы столкнулся с установкой и дополнил основной репозиторий OneScript небольшими советами - https://github.com/EvilBeaver/OneScript/pull/1460
3. Попробовал напрямую вставить использование библиотеки - не получилось
4. Попробовал на маке запустить отладку - не получилось
5. Поменял текст на просто Сообщить("Привет мир"), чтобы проверить как работает - получилось запустить через командную строку типа oscript main.os, как в питоне - сработало
6. Начал разбираться как устанавливать библиотеки. Прочитал про opm, но не знал стоит он у меня или нет. Нашел статью про библиотеки - https://infostart.ru/1c/articles/699642/

В тексте выискал текст команды  opm install yadisk - подумал, вдруг сработает и написать у себя opm install v8storage - Сработало. Удивительно даже :)

7. Попробовал использовать код из библиотеки - работает.
8. Там дальше нужно будет использовать уже конкретную 1Ску, возможно придется переходить на виндовс, но я еще поковыряюсь тут

9. Переключился на Windows.
10. OneScript устанавливается действительно легко и играючи
11. Попытка установить библиотеку работы с хранилищем выдает ошибку:
"{Модуль C:\Program Files\OneScript\lib\opm\src\core\Классы\МенеджерУстановкиПакетов.os / Ошибка в строке: 136 / {Модуль C:\Program Files\OneScript\lib\opm\src\core\Классы\УстановкаПакета.os / Ошибка в строке: 108 / Внешнее исключение (System.UnauthorizedAccessException): Отказано в доступе по пути "C:\Program Files\OneScript\lib\v8storage".}}"

12. Попытался скачать исходники и скопировать их вручную в папку с библиотеками - удалось. Но onescript все равно не видит эту библиотеку. Видимо, нужно где-то ее отдельно прописать - где, непонятно.
13. Пробуем поменять путь установки библиотеки, как описано в opm
set OSCRIPTBIN=c:\temp\ 
opm update -all

В результате ничего не получилось.

14. Выдал права на папку OneScript - Находим в проводнике эту папку. На правую кнопку. Свойства. Закладка Безопасность. Выдаем права. Перезапускаем терминал и пробуем снова.
15. Удалось установить библиотеку.
16. Попытка запуска выдает сообщение - Не задан путь к платформе 1С. Тоже самое было на маке - теперь нужно понять, а как задать этот путь к платформе.
17. Открываем v8runner на ГитХаб и пытаемся найти про путь к платформе. Ничего не найдено.
18. Вот в этом issue - https://github.com/oscript-library/deployka/issues/14 прочитал, что такое может быть если куча разных платформ с тонким клиентом установлено. У меня как раз такой случай, когда используются тонкие клиенты более поздних версий. Значит нужно как-то указать явно нашу версию. Читаем дальше и пытаемся сделать это
19. Начал смотреть код v8runner.os (так как там указываются версии платформы, вернее ищутся) и нашел, что там тоже есть работа с хранилищем. Возможно, библиотека для хранилищ отдельно не нужна и можно использовать встроенную библиотеку из v8runner
20. Код в v8runner ссылается на v8find.os - именно там мы указываем платформу. Идем туда, чтобы посмотреть, где он пытается найти платформу. Сейчас отладчик пустой путь к платформе возвращает
21. 