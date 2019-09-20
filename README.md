# MSI-Z77A-G45-Hackintosh
[GUIDE] Installing macOS Mojave (10.14.x) on MSI Z77A-G45 using Clover UEFI

Установка MacOS Mojave 10.14.5 (UEFI)
1) BIOS настроить (см. рис. /Install Tools/BIOS MSI Z77A-G45.png)
2) Загрузить флешку до появления boot меню
3) config.plist установлен по умолчанию. В Option переключить на config(Z77A-G45).plist если у вас плата MSI Z77A-G45. Если вы хотите переключить на другую плату, то кинь файл config.plist в EFI/CLOVER.
4) Устанавливаем MacOS Mojave. Выбираем раздел apfs.
5) Как MacOS установлен запускаем MultiBeast 11.3.0. В Quick Start выбрать UEFI Boot Mode.
6) Build and Reboot.
(Примечание: если в clover bootloader список дисков не отображается, то ставь обновленный clover bootloader. Во время установки убрать все галочки, только в списке указать UEFI Drivers-FileSystemDrivers-VBoxHfs.)
