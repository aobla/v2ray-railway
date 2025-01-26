
# V2Ray с WebSocket и TLS/SSL

  

Этот проект позволяет развернуть сервер V2Ray с использованием протокола WebSocket через TLS/SSL. Для примера будем использовать платформу Railway.app. Проект уже готов на GitHub, и вам нужно просто настроить переменные окружения и деплой.

  

## Шаги по настройке

### 0. **Fork**
- Зарегистрируйтесь или войдите в аккаунт на GitHub
- Сделайте Fork этого репозитория

### 1. **Создание проекта на Railway.app**

  

- Зарегистрируйтесь или войдите в аккаунт на [Railway.app](https://railway.app) (можно через аккаунт GitHub).

- Создайте новый проект, выбрав **Deploy from GitHub**.

- Выберите ваш репозиторий (этот), содержащий проект с готовым Dockerfile и конфигурацией V2Ray.

  

### 2. **Настройка переменных окружения**

  

На платформе Railway.app вам нужно задать несколько переменных окружения, которые будут использоваться в вашем проекте. Для этого откройте настройки вашего проекта и добавьте:

  

- `UUID`: уникальный идентификатор клиента (например, сгенерированный через `uuidgen` или онлайн генератор).

- `PORT`: порт для работы сервера (обычно 443).

- `HOST`: ваш домен или хост (например, `yourdomain.com`).

- `PATH`: путь для WebSocket соединения (например, `/vmess`).

  

### 3. **Конфигурация сервера и клиента**

  

Так как проект уже содержит готовые файлы конфигурации, вам нужно лишь убедиться, что они используют переменные окружения. В файле конфигурации V2Ray (например, `config.json.template` или подобном) должны быть следующие строки:

  


	"wsSettings": {
		"path": "${PATH}",
		"headers": {
			"Host": "${HOST}"
		}
	}

  

### 4. **Подготовка Dockerfile**

  

Проект уже должен включать корректный `Dockerfile`. Убедитесь, что:

  

- Dockerfile копирует все необходимые файлы конфигурации в контейнер.

- Используется скрипт или команда для генерации финального файла конфигурации (`config.json`) на основе шаблона с переменными окружения.

- В секции `ENTRYPOINT` указано выполнение скрипта для генерации конфигурации, а также запуск V2Ray.

  

Если возникают ошибки, проверьте, что в Dockerfile правильно указаны пути и команды, а также добавлены необходимые зависимости.

  

---

  

### 5. **Запуск и деплой**

  

1. Загрузите проект в Railway, выбрав **Deploy from GitHub**.

2. В настройках проекта добавьте переменные окружения (`UUID`, `PORT`, `HOST`, `PATH`)

3. После завершения деплоя проверьте логи контейнера на платформе Railway, чтобы убедиться в отсутствии ошибок (в Deploy Logs должно быть сообщение `V2Ray 5.24.0 started`).

4. Используйте ваш клиент V2Ray для подключения, настроив его на:

- Протокол: VMess или VLESS (такой же как в `PATH`).

- Адрес: `wss://<ваш_домен><ваш_путь>` или просто `<ваш_домен>` (например, `wss://yourdomain.com/vmess`).

- UUID: используйте значение, указанное в переменной окружения `UUID`.

  

Если вы видите ошибки при подключении, проверьте корректность настроек клиента, а также логи сервера на Railway.

 

Проект настроен для работы с WebSocket через TLS/SSL. Все необходимые конфигурации уже находятся в репозитории на GitHub, вам нужно только настроить переменные окружения и развернуть приложение.

Пример конфигурации клиента `config_client.json` экспортирован из рабочего приложения.
