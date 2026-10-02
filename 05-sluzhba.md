# Задание 5. Собственная служба systemd
1. Создание скрипта

Был создан файл `/usr/local/bin/zagruzka-log.sh.`

Содержимое файла:
```
#!/bin/bash
echo "Система загружена: $(date)" >> /var/log/moya-zagruzka.log
```
Скрипт был сделан исполняемым командой:

`sudo chmod +x /usr/local/bin/zagruzka-log.sh`

Скрипт при запуске записывает в файл /var/log/moya-zagruzka.log строку с текущей датой и временем загрузки системы.

2. Создание файла службы

Был создан файл:

`/etc/systemd/system/zagruzka-log.service`

Содержимое файла:
```
[Unit]
Description=Запись информации о загрузке системы

[Service]
Type=oneshot
ExecStart=/usr/local/bin/zagruzka-log.sh

[Install]
WantedBy=multi-user.target
```
3. Включение автозапуска

Для применения настроек и включения службы использовались команды:
```
sudo systemctl daemon-reload
sudo systemctl enable zagruzka-log.service
sudo systemctl start zagruzka-log.service
```
Проверка состояния службы:

`systemctl status zagruzka-log.service`

Пример вывода:
```
○ zagruzka-log.service - Запись отметки о загрузке системы
     Loaded: loaded (/etc/systemd/system/zagruzka-log.service; enabled; preset: enabled)
     Active: inactive (dead) since Fri 2026-10-02 16:27:13 +07; 7min ago
 Invocation: 0f75dab203244ad39c4a0cdb1f3d0dd3
    Process: 740 ExecStart=/usr/local/bin/zagruzka-log.sh (code=exited, status=0/SUCCESS)
   Main PID: 740 (code=exited, status=0/SUCCESS)
   Mem peak: 1.7M
        CPU: 39ms
```
Статус inactive (dead) для этой службы нормально, потому что скрипт выполняется один раз и после успешного выполнения завершается.

4. Проверка после перезагрузки

После двух перезагрузок системы файл `/var/log/moya-zagruzka.log` содержит новые записи:
```
Система загружена: Пн 28 сен 2026 23:22:07 +07
Система загружена: Пт 02 окт 2026 15:18:55 +07
```
Каждая строка соответствует отдельному запуску службы после загрузки системы.

5. Значение параметров `Type=oneshot` и `WantedBy=multi-user.target`

- `Type=oneshot` означает, что служба выполняет указанную команду один раз и после её завершения останавливается. Такой тип подходит для скриптов, которые необходимо выполнить только один раз.

- `WantedBy=multi-user.target` означает, что служба будет запускаться при достижении системой стандартного многопользовательского режима. Благодаря этому служба автоматически запускается при загрузке Linux.