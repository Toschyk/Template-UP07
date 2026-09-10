# Template UP07
Открываем Microsoft SQL Management (Далее MSL. Можно найти через пуск в папке Microsoft SQL Server Tools 2022) и подключаемся к серверу, ничего вручную вводить не надо. Вы нажимаете подключиться (connection). Создаёте БД как указано в скриншоте. Задаём имя и нажимаем Ок. Возле папки нажимаем на + чтобы папка раскрылась и видим вашу БД с тем именем, которое вы задали. У меня это Template. Далее создаёте таблицы, записи, связи. Я рекомендую использовать запросы (new query).

<img width="421" height="303" alt="image" src="https://github.com/user-attachments/assets/5d5cf86f-26e4-443a-9884-1022bdc7d070" />

После настройки БД создаём проект Приложение WPF (.NET Framework). Пока создаётся проект переходите в свойства, это вам пригодится потом.

<img width="332" height="448" alt="image" src="https://github.com/user-attachments/assets/cb3abb56-aa1a-4f73-8d41-5407276d57a7" />

После создания проекта переходим в средства и выбираем Подключиться к базе данных. 

<img width="472" height="534" alt="image" src="https://github.com/user-attachments/assets/ad78d0b6-695b-4012-aa99-f69ac6b304ad" />

Если у вас всё на английском ищите Test. Справа будут Средства. В качестве источника данных оставляем Microsoft SQL Server. В MSL копируем имя сервера (у меня это Toschyk-PC) и вставляем в имя сервера. Также не забудьте указать сертификат сервера, иначе у вас появится ошибка

<img width="555" height="715" alt="image" src="https://github.com/user-attachments/assets/8f16f01d-ed0d-42aa-b1d2-341cec052548" />

Теперь вам необходимо разработать страницы для проекта. Страницы могут быть разные в зависимости от проекта.

Как только вы сделали страницы нужно оформить переключение между ними и также страницу регистрации и авторизации.
Ниже будет мой пример запроса для таблицы юзеров

```SQL
CREATE TABLE Users (
    Id            INT IDENTITY(1,1) PRIMARY KEY,
    Username      NVARCHAR(50)  NOT NULL UNIQUE,
    Email         NVARCHAR(100) NOT NULL UNIQUE,
    PasswordHash  NVARCHAR(256) NOT NULL,   -- Хэш пароля (SHA-256 + соль)
    PasswordSalt  NVARCHAR(128) NOT NULL,   -- Соль для хэширования
    FullName      NVARCHAR(100) NULL,
    CreatedAt     DATETIME2     NOT NULL DEFAULT SYSDATETIME(),
    LastLoginAt   DATETIME2     NULL,
    IsActive      BIT           NOT NULL DEFAULT 1
);
GO


CREATE INDEX IX_Users_Username ON Users(Username);
CREATE INDEX IX_Users_Email ON Users(Email);
GO


CREATE PROCEDURE sp_RegisterUser
    @Username     NVARCHAR(50),
    @Email        NVARCHAR(100),
    @PasswordHash NVARCHAR(256),
    @PasswordSalt NVARCHAR(128),
    @FullName     NVARCHAR(100) = NULL,
    @NewId        INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;

    IF EXISTS (SELECT 1 FROM Users WHERE Username = @Username)
    BEGIN
        RAISERROR('Пользователь с таким логином уже существует.', 16, 1);
        RETURN;
    END

    IF EXISTS (SELECT 1 FROM Users WHERE Email = @Email)
    BEGIN
        RAISERROR('Пользователь с таким email уже зарегистрирован.', 16, 1);
        RETURN;
    END

    INSERT INTO Users (Username, Email, PasswordHash, PasswordSalt, FullName)
    VALUES (@Username, @Email, @PasswordHash, @PasswordSalt, @FullName);

    SET @NewId = SCOPE_IDENTITY();
END
GO


CREATE PROCEDURE sp_GetUserByUsername
    @Username NVARCHAR(50)
AS
BEGIN
    SET NOCOUNT ON;
    SELECT Id, Username, Email, PasswordHash, PasswordSalt, FullName, IsActive
    FROM Users
    WHERE Username = @Username AND IsActive = 1;
END
GO

-- Обновление времени последнего входа
CREATE PROCEDURE sp_UpdateLastLogin
    @UserId INT
AS
BEGIN
    UPDATE Users SET LastLoginAt = SYSDATETIME() WHERE Id = @UserId;
END
GO
```SQL
