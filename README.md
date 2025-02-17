# Че почем

## Как поднять
Для запуска потребуется тачка с докером.

Рекомендуется взять тачку с юниксом, взять макбук или поднять убунту на виртуалке.
Под виндой я не запускал, поэтому работоспособность не гарантируется.

Клонируем репу к себе и заходим в папку:
```
git clone https://github.com/BorisKaryshev/oracle_db_on_linux.git
cd oracle_db_on_linux

mkdir data
chmod a+rw data

docker compose up --build --force-recreate
```

## Дефолтные логины и пароли
Пароль для админа (SYS) задается через переменную окружения в docker-compose.yaml.
Если не менять установленный, то будет - root.

Так же через переменные окружения создается юзер (root с паролем root).

## Как подключиться

В качестве смотрелки используется `CloudBeaver`.

Конектнутся к смотрелке можно в любом бразуере по адресу:

    https://localhost:8978

Или, если запустили на ВМ, а открываем на хосте, то вместо `localhost` указываем *ip* ВМ.

(ВМ - виртуальная машина)

Регаемся в смотрелке (введите любой логин, пароль, без них не запустится).
![Сюда регаться](images/1.jpg)

Дальше прокликиваем.

Теперь заходим в аккаунт смотрелки. Шестеренка в правом верхнем углу и Login.

Жмакаем на плюсик в квадрате в левом верхнем углу, и на *New connection*.

Заполняем:

|Название|Значение|
|--|--|
|Host|sql_server.g|
|Database|FREEPDB1|
|Service type|Service|
|Role|SYSDBA|
|User name|SYS|
|User password|root|

так чтобы выглядело так же:
![Так подключаться](images/2.jpg)


Если хотите подключиться не как админ (SYS), то Role выберете не SYSDBA, а Normal.
И логин/пароль соответственно другие введите.

## Доп инфа

Скрипты в папке example_tables при запуске контейнера с Oracle создают несколько таблиц, которые можно использовать для отладки своих скриптов.

    Но доступны они, пока что, только если заходить из под админа (SYS)
    и в качестве сервиса нужно указать FREE, а не FREEPDB1

Создаются следующие таблицы.

Про космос:
- metadata
- stars
- planets

Про сотрудников:
- departments
- employees

Про географию:
- regions
- countries
- cities
- currencies
- currencies_countries

---
По умолчанию, при перезапуске контейнера все данные теряются.
Чтобы это не происходило, они сохраняться в папке data.

Так что если хотите переподнять БД с нуля, то сотрите эту папку и создайте заново через:
```
sudo rm -r data
mkdir data
chmod a+rw data
```

---
Поставить докер на убунту можно следующими командами:

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
