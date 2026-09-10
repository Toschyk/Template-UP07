# Template UP07
Открываем Microsoft SQL Management (Далее MSL. Можно найти через пуск в папке Microsoft SQL Server Tools 2022) и подключаемся к серверу, ничего вручную вводить не надо. Вы нажимаете подключиться или connection. Создаёте БД как указано в скриншоте. Задаём имя и нажимаем Ок

<img width="421" height="303" alt="image" src="https://github.com/user-attachments/assets/5d5cf86f-26e4-443a-9884-1022bdc7d070" />

После настройки БД создаём проект Приложение WPF (.NET Framework). Пока создаётся проект переходите в свойства, это вам пригодится потом.

<img width="332" height="448" alt="image" src="https://github.com/user-attachments/assets/cb3abb56-aa1a-4f73-8d41-5407276d57a7" />

После создания проекта переходим в средства и выбираем Подключиться к базе данных. 

<img width="472" height="534" alt="image" src="https://github.com/user-attachments/assets/ad78d0b6-695b-4012-aa99-f69ac6b304ad" />

Если у вас всё на английском ищите Test. Справа будут Средства. В качестве источника данных оставляем Microsoft SQL Server. В MSL копируем имя сервера (у меня это Toschyk-PC) и вставляем в имя сервера. Также не забудьте указать сертификат сервера, иначе у вас появится ошибка

<img width="555" height="715" alt="image" src="https://github.com/user-attachments/assets/8f16f01d-ed0d-42aa-b1d2-341cec052548" />

