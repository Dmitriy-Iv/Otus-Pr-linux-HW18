# **Введение**

В данном домашнем задании нам необходимо получить практический начальный опыт работы с утилитой бэкапов Borgbackup.

---

Для выполнения этого ДЗ выполняем запуск Vagrantfile, в котором создаются две Vm: client-borg и server-borg. С помощью ansible настраиваем создание хранилища для хранения бэкапов на server-borg в директории /var/backup. Далее инициализируем borg на backup сервере с client сервера (выполняется при помощи скрипта create_repo.sh). После чего создаётся сервис borg-backup.service который в автоматическом режиме бэкапит директорию /etc.

- Для запуска выполняем следующее:
```
git clone https://github.com/Dmitriy-Iv/Otus-Pr-linux-HW18.git
cd Otus-Pr-linux-HW18/Ansible/
vagrant up
```

- Спустя пять-шесть минут заходим на client-borg и проверяем, появился ли у нас бэкап.
```
vagrant ssh client-borg
sudo -i
borg list borg@192.168.50.20:/var/backup/
(ввод пароля 'Otus1234')
```

- Что попало в архив можно посмотреть, открыв лог на client-borg `/var/log/borg.log`. Настроено через rsyslog (добавлены соотвествующие строки в borg-backup.service, создан файл конфигурации `/etc/rsyslog.d/borg_backup.conf`).
```
[root@client-borg ~]# cat /var/log/borg.log
Jun 16 15:34:46 client-borg borg_backup: ------------------------------------------------------------------------------
Jun 16 15:34:46 client-borg borg_backup: Archive name: etc-2022-06-16_15:34:43
Jun 16 15:34:46 client-borg borg_backup: Archive fingerprint: 8a0e520f9a69005b12c31c1cfb02bdf481dfab58fc640baad4780cdd6658c06e
Jun 16 15:34:46 client-borg borg_backup: Time (start): Thu, 2022-06-16 15:34:44
Jun 16 15:34:46 client-borg borg_backup: Time (end):   Thu, 2022-06-16 15:34:46
Jun 16 15:34:46 client-borg borg_backup: Duration: 1.69 seconds
Jun 16 15:34:46 client-borg borg_backup: Number of files: 1703
Jun 16 15:34:46 client-borg borg_backup: Utilization of max. archive size: 0%
Jun 16 15:34:46 client-borg borg_backup: ------------------------------------------------------------------------------
Jun 16 15:34:46 client-borg borg_backup: Original size      Compressed size    Deduplicated size
Jun 16 15:34:46 client-borg borg_backup: This archive:               28.44 MB             13.50 MB             11.85 MB
Jun 16 15:34:46 client-borg borg_backup: All archives:               28.44 MB             13.50 MB             11.85 MB
Jun 16 15:34:46 client-borg borg_backup: Unique chunks         Total chunks
Jun 16 15:34:46 client-borg borg_backup: Chunk index:                    1286                 1702

```