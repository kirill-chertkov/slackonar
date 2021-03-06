## Что это?
Утилита для сохранения сообщений из каналов в Слаке в маркдаун-файл. Создана для сохранения слаконаров, которые проводят наставники [Хекслета](https://ru.hexlet.io), но может быть использована для сохранения любых переписок.

Умеет разворачивать треды и сохранять изображения из соообщений. Для использования нужен только логин и пароль, тип пользователя может быть любым (Guest, Member, Admin, Owner). Двухфакторная аутентификация не поддерживается.

## Требования
- Docker
- Make (опционально)

## Установка
1. Склонируйте этот репозиторий
```
git clone https://github.com/kirill-chertkov/slackonar.git
```
2. Соберите образ
```
make build
```
3. Введите email и пароль, который используете для авторизации в Слаке, в файл `.env_example`. Переименуйте его в `.env`.

## Использование
Запускать сохранение удобнее из директории с репозиторием командой `make save`. В качестве аргументов она принимает ссылки на сообщение в Слаке.
- Если передать ссылку на одно сообщение, будет скачано это сообещение и все сообщения, которые появились в канале после него
```
make save first=https://hexlet-students.slack.com/archives/C01AR2DCTL0/p1604068859324600
```
- В качестве второго (необязательного) аргумента можно передать ссылку на сообщение, которое будет скачано последним. Сохранятся все сообщения от первого (`first`) до последнего (`last`) включительно
```
make save first=https://hexlet-students.slack.com/archives/C01AR2DCTL0/p1604068859324600 last=https://hexlet-students.slack.com/archives/C01AR2DCTL0/p1604227876342700
````
В результате в директории `slackonars`окажется файл в формате Markdown и сохраненные изображения в поддиректории `images`
```
slackonars
├── 04.11.2020-20:59:13.md
└── images
    ├── T019FJ8M43H-F01DK2J8EUE.png
    ├── T019FJ8M43H-F01DLT9TXMK.png
    ├── T019FJ8M43H-F01E2K4HTGR.png
    └── T019FJ8M43H-F01EFKE5HGQ.png
```

### Конвертация в PDF
Для конвертации в PDF рекомендую использовать `grip`, из всех опробованных вариантов он даёт самый читабельный и аккуратный результат. Это модуль Python, установить его можно так:
```
pip install grip
# альтернатива для Mac OS
brew install grip
```
Далее запускаете его в директории с маркдаун-файлом:
```
grip 04.11.2020-20:59:13.md -b --title='Тема и автор слаконара'
```
Запустится локальный веб-сервер и отрендерит маркдаун в HTML. Если использован флаг `-b`, автоматически откроется вкладка с документом в браузере. Останется только сохранить его как PDF: <kbd>Ctrl(⌘)+P</kbd> -> `Save as PDF`. Перед сохранением имеет смысл отключить опцию `Headers and footers`, чтобы на каждую страницу не добавлялся её номер и url.