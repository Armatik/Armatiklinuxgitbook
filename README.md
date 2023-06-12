---
description: Монитирование разделов с файловой системой NTFS в режиме RW через консоль.
---

# Монтирование разделов с NTFS

{% hint style="info" %}
По умлочанию в ALT linux все разделы с файловой системой NTFS монитируются в режиме RO[^1] что не позволяет записывать на них какую либо информацию. Исправим это смонтировав эти диски в режиме RW[^2].
{% endhint %}

## Решение:

Устанавливаем пакеты `fuse` и `ntfs-3g`.

```
epm install fuse ntfs-3g
```

Создаём директорию в которую будем монтировать наш диск.

```
sudo mkdir /mnt/ntfsdrive
```

> Вместо `ntfsdrive` можете указать любое удобное вам название директории.

С помощью `lsblk` просматриваем список доступных разделов и копируем название того который собираемся смонтировать. В моём случае это `nvme0n1p3`

```
lsblk
```

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Вводим команду для монтирования диска:

```
sudo mount -t ntfs-3g /dev/nvme0n1p3 /mnt/ntfsdrive
```

{% hint style="info" %}
Если вам выдало подобное сообщение с ошибкой при попытке монтирования диска воспользуйтесь утилитой `ntfsfix`, она поставляется вместе с ntfs-3g.
{% endhint %}

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption><p>Пример сообщения с ошибкой</p></figcaption></figure>

```
sudo ntfsfix /dev/nvme0n1p3
```

Теперь повторите попытку монтирования командой:

```
sudo mount -t ntfs-3g /dev/nvme0n1p3 /mnt/ntfsdrive
```

{% hint style="info" %}
Если команда выше вам не помогла то необхадимо загрузится в windows, и нажать на кнопку завершения работы с зажатой клавишей `shift`

Это происходит из за режима быстрой загрузки windows, отключить его можно с помощью команды `powercfg /h off` в терминале windows.&#x20;
{% endhint %}

Чтобы размонтировать раздел введите команду:

```
sudo umount /dev/nvme0n1p3
```

[^1]: Read-only режим работы только на чтение данных.

[^2]: Read-write режим работы на чтение и запись данных.
