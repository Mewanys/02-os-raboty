1. `systemd-analyze` - это команда, которая показывает общее время загрузки системы и разбивает его по этапам, сколько времени ушло на ядро и сколько на пользовательское пространство.
```
Startup finished in 11.938s (kernel) + 9.500s (userspace) = 21.438s 
graphical.target reached after 9.486s in userspace.
```
Объяснение вывода:
- `Startup finished in 11.938s (kernel) + 9.500s (userspace) = 21.438s` - общая загрузка заняла 21.438 секунды
- `11.938s (kernel)` - время, затраченное на инициализацию ядра
- `9.500s (userspace)` - время, затраченное на запуск пользовательских сервисов
- `graphical.target reached after 9.486s in userspace` — графический интерфейс стал доступен через 9.486 секунды после начала пользовательского пространства.

2. `systemd-analyze blame | head -10` - команда показывает список системных служб по времени их запуска, от самых медленных до самых быстрых. Это очень удобно для поиска узких мест при старте системы. Чем медленнее сервис, тем больше вероятность, что именно он задерживает загрузку.
```
4.386s dev-sda1.device
2.822s systemd-udev-trigger.service
2.504s systemd-journald.service
1.689s systemd-logind.service
1.627s apparmor.service
1.562s modprobe@drm.service
1.191s networking.service
1.073s grub-common.service
 947ms dbus.service
 669ms dev-hugepages.mount
```
Объяснение вывода:
- `dev-sda1.device` — устройство `/dev/sda1` стартовало дольше всего и заняло 4.386s
- `systemd-udev-trigger.service` — инициализация устройств и правила udev заняла 2.822s
- `systemd-journald.service` — сервис журналирования стартовал за 2.504s
- `systemd-logind.service` — управление логином и сессиями заняло 1.689s
- `apparmor.service` — политика AppArmor стартовала за 1.627s
- `modprobe@drm.service` — загрузка драйверов графики потребовала 1.562s
- `grub-common.service` и `dbus.service` также замедляют загрузку, но меньше остальных.

3. `systemd-analyze critical-chain` - команда показывает цепочку зависимостей между системными юнитами и определяет, какой именно сервис является узким местом для загрузки. Это позволяет понять, какой элемент запускает всю цепочку и задерживает остальные службы.

```
The time when unit became active or started is printed after the "@" character.
The time the unit took to start is printed after the "+" character.

graphical.target @9.486s
└─multi-user.target @9.485s
  └─systemd-logind.service @7.795s +1.689s
    └─basic.target @7.468s
      └─sockets.target @7.428s
        └─systemd-hostnamed.socket @7.428s
          └─sysinit.target @7.413s
            └─apparmor.service @5.784s +1.627s
              └─local-fs.target @5.765s
                └─tmp.mount @5.712s +49ms
                  └─swap.target @5.700s
                    └─dev-disk-by\x2duuid-1d7bfddf\x2da296\x2d481e\x2d8ffd\x2db46c3bff3d2c.swap @5.614s +75ms
                      └─dev-disk-by\x2duuid-1d7bfddf\x2da296\x2d481e\x2d8ffd\x2db46c3bff3d2c.device @5.577s

```
Объяснение вывода:
- `@` показывает момент, когда сервис или цель стали активными
- `+` показывает, сколько времени данная служба занимала на старте
- цепочка показывает, что загрузка графического интерфейса зависит от `multi-user.target`, затем от `systemd-logind.service`, а дальше от служб инициализации устройств и локальных файловых систем
это значит, что задержка в одном из начальных сервисов может замедлить весь процесс загрузки.

Итоговый вывод:
- три самые медленные службы: `dev-sda1.device` (4.386s), `systemd-udev-trigger.service` (2.822s), `systemd-journald.service` (2.504s)
- Самая медленная служба `dev-sda1.device` обычно задерживается из-за ожидания ответа блока диска или инициализации раздела, что часто происходит при монтировании файловой системы.