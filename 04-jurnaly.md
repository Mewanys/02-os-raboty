
# Задание 4. Чтение журналов

1. `journalctl -b -p err` - все ошибки текущей загрузки (более детально опиши команду)
```
сен 28 22:04:12 debian kernel: piix4_smbus 0000:00:07.3: SMBus Host Controller not enabled!
```
Сообщение `SMBus Host Controller not enabled!` означает, что контроллер SMBus не был включён. В данном случае это не приводит к критической ошибке системы.
**SMBus** — системная шина для обмена данными с некоторыми аппаратными устройствами.


2. `journalctl -u systemd-logind.service` - показывает записи системного журнала. Служба `systemd-logind.service` отвечает за управление пользовательскими сеансами, входом и выходом пользователей.
```
янв 28 20:53:21 debian systemd[1]: Starting systemd-logind.service - User Login Management...
янв 28 20:53:22 debian systemd-logind[656]: Watching system buttons on /dev/input/event4 (Power Button)
янв 28 20:53:22 debian systemd-logind[656]: Watching system buttons on /dev/input/event0 (AT Translated Se>
янв 28 20:53:23 debian systemd-logind[656]: New seat seat0.
янв 28 20:53:23 debian systemd[1]: Started systemd-logind.service - User Login Management.
янв 28 20:53:30 debian systemd-logind[656]: New session 1 of user user.
янв 28 20:53:30 debian systemd-logind[656]: New session 2 of user user.
-- Boot d97cde619c374ce3bb7706889884c434 --
янв 30 20:03:35 debian systemd[1]: Starting systemd-logind.service - User Login Management...
янв 30 20:03:35 debian systemd-logind[651]: New seat seat0.
янв 30 20:03:35 debian systemd-logind[651]: Watching system buttons on /dev/input/event4 (Power Button)
янв 30 20:03:35 debian systemd-logind[651]: Watching system buttons on /dev/input/event0 (AT Translated Se>
янв 30 20:03:35 debian systemd[1]: Started systemd-logind.service - User Login Management.
янв 30 20:04:00 debian systemd-logind[651]: New session 1 of user user.
янв 30 20:04:00 debian systemd-logind[651]: New session 2 of user user.
янв 30 20:48:21 debian systemd-logind[651]: Session 1 logged out. Waiting for processes to exit.
янв 30 20:48:21 debian systemd-logind[651]: Removed session 1.
янв 30 20:48:27 debian systemd-logind[651]: New session 4 of user user.
янв 30 20:50:29 debian systemd-logind[651]: Session 4 logged out. Waiting for processes to exit.
янв 30 20:50:29 debian systemd-logind[651]: Removed session 4.
янв 30 20:50:39 debian systemd-logind[651]: Removed session 2.
янв 30 20:50:52 debian systemd-logind[651]: New session 5 of user user.
янв 30 20:50:53 debian systemd-logind[651]: New session 6 of user user.
янв 30 20:52:59 debian systemd-logind[651]: The system will power off now!
янв 30 20:52:59 debian systemd-logind[651]: System is powering down.
янв 30 20:52:59 debian systemd-logind[651]: Session 5 logged out. Waiting for processes to exit.
янв 30 20:53:01 debian systemd-logind[651]: Removed session 5.
янв 30 20:53:01 debian systemd-logind[651]: Removed session 6.
янв 30 20:53:01 debian systemd[1]: Stopping systemd-logind.service - User Login Management...
янв 30 20:53:01 debian systemd[1]: systemd-logind.service: Deactivated successfully.
янв 30 20:53:01 debian systemd[1]: Stopped systemd-logind.service - User Login Management.
-- Boot b38fb3be53e344e18b25b815358e2af2 --
янв 30 20:53:41 debian systemd[1]: Starting systemd-logind.service - User Login Management...
янв 30 20:53:42 debian systemd-logind[633]: New seat seat0.
янв 30 20:53:42 debian systemd[1]: Started systemd-logind.service - User Login Management.
янв 30 20:53:43 debian systemd-logind[633]: Watching system buttons on /dev/input/event0 (AT Translated Se>
янв 30 20:53:43 debian systemd-logind[633]: Watching system buttons on /dev/input/event4 (Power Button)
янв 30 20:53:54 debian systemd-logind[633]: New session 1 of user user.
янв 30 20:53:54 debian systemd-logind[633]: New session 2 of user user.
янв 30 21:03:29 debian systemd-logind[633]: Session 1 logged out. Waiting for processes to exit.
янв 30 21:03:29 debian systemd-logind[633]: Removed session 1.
янв 30 21:03:39 debian systemd-logind[633]: New session 3 of user root.
янв 30 21:03:39 debian systemd-logind[633]: New session 4 of user root.
янв 30 21:03:39 debian systemd-logind[633]: Removed session 2.
янв 30 21:03:51 debian systemd-logind[633]: The system will power off now!
```
Вывод:
- `Started systemd-logind.service` - служба запущена.
- `New session 1 of user user` - пользователь вошёл в систему, создан сеанс №1.
- `New session 2 of user user` - создан ещё один сеанс пользователя.
- `Session 1 logged out` - пользователь вышел из сеанса.
- `Removed session 1` - сеанс удалён.
- `The system will power off now!` - система получила команду на выключение.

3. `dmesg | tail -30 ` - выводит сообщения ядра, полученные во время работы системы. а `tail` последние 30 строк вывода.

```
[   19.137009] cryptd: max_cpu_qlen set to 1000
[   19.226932] vmwgfx 0000:00:0f.0: vgaarb: deactivate vga console
[   19.227468] Console: switching to colour dummy device 80x25
[   19.249872] vmwgfx 0000:00:0f.0: [drm] FIFO at 0x00000000fe000000 size is 8192 KiB
[   19.249909] vmwgfx 0000:00:0f.0: [drm] VRAM at 0x00000000e8000000 size is 131072 KiB
[   19.249949] vmwgfx 0000:00:0f.0: [drm] Running on SVGA version 2.
[   19.249971] vmwgfx 0000:00:0f.0: [drm] Capabilities: rect copy, cursor, cursor bypass, cursor bypass 2, 8bit emulation, alpha cursor, extended fifo, multimon, pitchlock, irq mask, display topology, gmr, traces, gmr2, screen object 2, command buffers, command buffers 2, gbobject, dx, hp cmd queue, no bb restriction, cap2 register, 
[   19.249992] vmwgfx 0000:00:0f.0: [drm] Capabilities2: grow otable, intra surface copy, dx2, gb memsize 2, screendma reg, otable ptdepth2, non ms to ms stretchblt, cursor mob, mshint, cb max size 4mb, dx3, frame type, trace full fb, extra regs, lo staging, 
[   19.250845] vmwgfx 0000:00:0f.0: [drm] DMA map mode: Caching DMA mappings.
[   19.251046] vmwgfx 0000:00:0f.0: [drm] Legacy memory limits: VRAM = 4096 KiB, FIFO = 256 KiB, surface = 0 KiB
[   19.251054] vmwgfx 0000:00:0f.0: [drm] MOB limits: max mob size = 262144 KiB, max mob pages = 65536
[   19.251061] vmwgfx 0000:00:0f.0: [drm] Max GMR ids is 64
[   19.251065] vmwgfx 0000:00:0f.0: [drm] Max number of GMR pages is 65536
[   19.251069] vmwgfx 0000:00:0f.0: [drm] Maximum display memory size is 262144 KiB
[   19.335005] vmwgfx 0000:00:0f.0: [drm] Screen Target display unit initialized
[   19.339644] vmwgfx 0000:00:0f.0: [drm] Fifo max 0x00040000 min 0x00001000 cap 0x0000077f
[   19.340441] vmwgfx 0000:00:0f.0: [drm] Using command buffers with DMA pool.
[   19.358153] vmwgfx 0000:00:0f.0: [drm] Available shader model: Legacy.
[   19.365960] [drm] Initialized vmwgfx 2.20.0 for 0000:00:0f.0 on minor 0
[   19.398089] fbcon: vmwgfxdrmfb (fb0) is primary device
[   19.449547] Console: switching to colour frame buffer device 160x50
[   19.498370] vmwgfx 0000:00:0f.0: [drm] fb0: vmwgfxdrmfb frame buffer device
[   21.801206] 8021q: 802.1Q VLAN Support v1.8
[   23.224537] cfg80211: Loading compiled-in X.509 certificates for regulatory database
[   23.267566] Loaded X.509 cert 'benh@debian.org: 577e021cb980e0e820821ba7b54b4961b8b4fadf'
[   23.267902] Loaded X.509 cert 'romain.perier@gmail.com: 3abbc6ec146e09d1b6016ab9d6cf71dd233f0328'
[   23.268237] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
[   23.268850] Loaded X.509 cert 'wens: 61c038651aabdcf94bd0ac7ff06c7248db18c600'
[   23.380143] 8021q: adding VLAN 0 to HW filter on device ens33
[   23.381065] e1000: ens33 NIC Link is Up 1000 Mbps Full Duplex, Flow Control: None
```

В данном выводе критических ошибок нет. Большинство сообщений являются информационными и показывают успешную инициализацию виртуального оборудования и сети.


## Итоговый отчет
1. Какие ошибки или предупреждения нашлись в текущей загрузке (если ошибок нет —
так и напишите, это нормально)
> В результате поиска ошибок была найдена 1 ошибка. Она означает, что контроллер SMBus не был включён.
```
`piix4_smbus 0000:00:07.3: SMBus Host Controller not enabled!`
```

2. разберите одно любое сообщение: что за служба, что она сообщает;
> Сообщение относится к драйверу `piix4_smbus` ядра Linux. Он отвечает за работу SMBus. Система сообщает, что контроллер SMBus не был активирован. Для данной системы это некритично, особенно если SMBus не используется.

3. чем journalctl -b отличается от journalctl -b -1 и когда нужен второй вариант.
- `journalctl -b` показывает сообщения журнала текущей загрузки системы. 
- `journalctl -b -1` показывает сообщения предыдущей загрузки системы.
> Второй вариант нужен, если после перезагрузки возникла проблема и необходимо посмотреть, что происходило перед предыдущим выключением или перезагрузкой. Это позволяет найти ошибки, которые могли привести к сбою или случайному завершению работы.