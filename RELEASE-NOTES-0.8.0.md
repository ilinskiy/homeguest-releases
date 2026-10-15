# HomeGuest 0.8.0 — Multi-provider: Yandex + SmartLife / Tuya

HomeGuest 0.8.0 добавляет multi-provider архитектуру: существующий Yandex Smart Home продолжает работать как основной провайдер, а SmartLife / Tuya Cloud можно подключить как дополнительный источник устройств.

## Главное в 0.8.0

- одновременная работа Yandex Smart Home и SmartLife / Tuya Cloud;
- официальный Tuya Cloud API с HMAC-SHA256 авторизацией;
- отдельное безопасное хранение Tuya Client ID / Client Secret;
- импорт Tuya-устройств, online/state и поддерживаемых capabilities;
- управление Tuya on/off;
- управление яркостью с нормализацией в HomeGuest 1–100%;
- импорт SmartLife homes и rooms;
- объединение одинаковых комнат Yandex / Tuya по имени;
- отдельные provider badges в административном и гостевом интерфейсах;
- сохранение общей guest ACL модели HomeGuest независимо от провайдера.

## Tuya / SmartLife

Поддержаны подтверждённые реальные datapoints:

- `switch_1`;
- `switch_led`;
- `switch_led_1`;
- `bright_value_1`;
- `bright_value`;
- `bright_value_v2`;
- `brightness_min_1` / `brightness_max_1`.

Неизвестные vendor-specific datapoints по-прежнему игнорируются по deny-by-default. Управление цветом, scene/music и локальный Tuya LAN protocol в 0.8.0 не включены.

## Комнаты

HomeGuest получает SmartLife homes, rooms и состав устройств каждой комнаты через Tuya Home Management API.

Если название комнаты Tuya совпадает с существующей комнатой Yandex / HomeGuest после TrimSpace и сравнения без учёта регистра, используется существующая canonical room. Уникальные комнаты Tuya добавляются отдельно.

На реальной конфигурации перед выпуском были обнаружены 2 homes, 8 rooms и 30 Tuya-устройств, привязанных к комнатам.

## Администрирование

В разделе «Система» добавлена карточка SmartLife / Tuya:

- проверка новой пары Client ID / Client Secret без сохранения;
- сохранение credentials вне SQLite;
- повторная проверка сохранённого подключения;
- безопасный preview найденных устройств;
- управляемый restart HomeGuest после изменения настроек.

## Проверено на рабочем Keenetic

Перед выпуском подтверждено:

- `go test ./...`;
- запуск HomeGuest 0.8.0 на Keenetic MIPSLE softfloat;
- `/health` = ok;
- `/ready` = ready;
- Yandex provider healthy;
- SmartLife / Tuya Cloud discovery;
- Tuya on/off;
- Tuya brightness;
- импорт homes / rooms;
- объединение комнат Yandex / Tuya;
- provider badges;
- работа guest ACL после включения второго провайдера;
- только один production-экземпляр HomeGuest.

Проверенный commit исходного репозитория:

`d4f26cf2a28bd18f1f9b575df75f10297a38f26b`

Tag исходного репозитория:

`v0.8.0`

## Проверенные пакеты

Windows amd64:

`HomeGuest-0.8.0-windows-amd64.zip`

SHA256:

`E8047A0AA2768DD682CB25A06E65E437CA8E44D515D83AA61B772BFB6D5C3E01`

Linux amd64:

`HomeGuest-0.8.0-linux-amd64.tar.gz`

SHA256:

`191DC3E15F5E8F522D8864EADBED31B617185E349E7A32BE683E1F49E6172A5E`

Linux arm64:

`HomeGuest-0.8.0-linux-arm64.tar.gz`

SHA256:

`EC6013F22770FC01AD48550DA82EC67B03FB559742141FD4D23E261EA17C5490`

Keenetic full package:

`HomeGuest-0.8.0-keenetic-mipsle-softfloat.tar.gz`

SHA256:

`093F66E775A39EE625187417CE3908C10B2DBC211825472885166EE0E4E1734B`

Keenetic update package:

`HomeGuest-0.8.0-update-keenetic-mipsle.zip`

SHA256:

`76E3CBDB1ED125C891437F932385EA65A119D537F5263B42F178F4A437E1BA36`

## Public package

`HomeGuest-0.8.0-Keenetic-Tester-Kit.zip`

SHA256:

`DE1D6B883982966235E701AC79342FF73CF473E2A30EE09187D15766D9F304B3`

Комплект содержит:

- `HomeGuest-0.8.0-update-keenetic-mipsle.zip`;
- `HomeGuest-0.8.0-keenetic-mipsle-softfloat.tar.gz`;
- `README-FIRST.md`;
- `INSTALL-KEENETIC.md`;
- `TEST-CHECKLIST.md`;
- `REPORT-BUG.md`;
- `RELEASE-NOTES-0.8.0.md`;
- `SHA256SUMS.txt`.

## Безопасность публикации

Не публикуйте:

- Yandex OAuth token;
- Tuya Client ID / Client Secret;
- пароль администратора HomeGuest;
- содержимое `data/secrets`;
- cookies и данные сессии;
- рабочую SQLite-базу;
- необезличенные backup-файлы.

HomeGuest предназначен прежде всего для локальной домашней сети. Не публикуйте административную панель напрямую в Internet.
