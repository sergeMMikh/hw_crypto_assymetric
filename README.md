# Домашнее задание к занятию «3.2. Ассиметричные криптосистемы»
## Михалёв Сергей.

## Задача LibreSSL & OpenSSL

В этой задаче мы научимся шифровать данные с помощью публичного ключа и расшифровывать с помощью приватного.

Первое, с чего мы должны начать - это сгенерировать ключ. Для этого нужно выбрать алгоритм, размер ключа и passphrase (кодовую фразу).

<details>
<summary>Шаги выполнения задания.</summary>

#### Шаг 1. Создайте закрытый ключ с длиной 2048:

```shell script
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -aes256 -out private.key
```

Где [`genpkey`](https://www.openssl.org/docs/manmaster/man1/openssl-genpkey.html) - подкоманда, отвечающая за генерацию ключей.

Введите кодовую фразу два раза (обратите внимание, символы не будут отображаться в целях безопасности).

Откройте файл `private.key` в любом текстовом редакторе (можете просмотреть в терминале с помощью команды `cat private.key`), удостоверьтесь, что он выглядит примерно так:

```text
-----BEGIN ENCRYPTED PRIVATE KEY-----
...
МНОГО БУКВ
...
-----END ENCRYPTED PRIVATE KEY-----
```

#### Шаг 2. Сгенерируйте открытый ключ на базе закрытого: 

```shell script
openssl pkey -pubout -in private.key -out public.key
```

Где [`pkey`](https://www.openssl.org/docs/man1.0.2/man1/pkey.html) - подкоманда, отвечающая за обработку ключей.

Откройте файл `public.key` (можете просмотреть в терминале с помощью команды `cat public.key`) в любом текстовом редакторе, удостоверьтесь, что он выглядит примерно так:

```text
-----BEGIN PUBLIC KEY-----
...
МНОГО БУКВ
...
-----END PUBLIC KEY-----
```

#### Шаг 3. Создайте файл message.txt со своей фамилией

#### Шаг 4. Зашифруйте сообщение с помощью публичного ключа: 

```shell script
openssl pkeyutl -encrypt -pubin -inkey public.key -in message.txt -out cypher.txt
```

Где [`pkeyutl`](https://www.openssl.org/docs/manmaster/man1/openssl-pkeyutl.html) - подкоманда, отвечающая за низкоуровневые операции с публичными ключами (включая шифрование и расшифрование).

Откройте файл `cypher.txt` в любом текстовом редакторе, удостоверьтесь, что он не читабелен.

#### Шаг 5. Убедитесь, что файл `cyper.txt` расшифровывается:

```shell script
openssl pkeyutl -decrypt -inkey private.key -in cypher.txt -out decrypted.txt
```
</details>

#### Ожидаемый Результат выполнениея

В качестве результата пришлите:
1. Публичный и приватный ключ (`public.key` и `private.key`)
1. Passphrase (строкой)
1. Зашифрованный файл (`cypher.txt`)

---

## Решение.

<details>
<summary>Детали выполнения.</summary>
  
Проверка версии OpenSSL.

![openssl version](images/image.png)

Создание приватного и публичного ключа.

![ssh keys](images/image-2.png)

При шифровке текстового файла возникла ошибка: *размер входных данных больше, чем допускает RSA-ключ с выбранной схемой паддинга.* Пришлось сократить текст.

![text](images/image-1.png)

Расшифровка файла.

![decrypted](images/image-3.png)


</details>

Результат:
 * [`public.key`](Task1/public.key);
 * [`private.key`](Task1/private.key);
 * Passphrase содержится в коментарии к решению;
 * Зашифрованный файл [`cypher.txt`](Task1/cypher.txt)

-----

## Задача GnuPG: подпись сообщения

В этом ДЗ мы научимся подписывать и проверять подпись документов с помощью [GnuPG](https://gnupg.org) - популярной реализации [OpenPGP](https://www.ietf.org/rfc/rfc4880.txt). В частности, она используется в большом количестве Open Source проектов для подписи пакетов с ПО.

![](images/gnupg.png)

С Российской криптографией мы научимся работать чуть позже (поскольку тестовый период для Российских продуктов ограничен во времени, а они будут нам с вами нужны ещё на нескольких курсах).

#### Ожидаемый Результат выполнениея

В качестве результата пришлите:
1. Публичный ключ (с шага 5)
1. Файл сообщения и подпись к нему (с шага 3)

--- 

## Решение.

<details>
<summary>Детали выполнения.</summary>
  
Проверка версии GnuPG

![GnuPG version](images/image-4.png)

Шаг 1. Генерация keypair.

```bash
$ gpg --full-generate-key
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keybox '/home/smm/.gnupg/pubring.kbx' created
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 2048
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Sergey
Email address: s.m.mih@mail.ru
Comment:
You selected this USER-ID:
    "Sergey <s.m.mih@mail.ru>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /home/smm/.gnupg/trustdb.gpg: trustdb created
gpg: directory '/home/smm/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/smm/.gnupg/openpgp-revocs.d/4AF7C541F48FD15EEC57712B6C115D056BA1FCF3.rev'
public and secret key created and signed.

pub   rsa2048 2025-10-14 [SC]
      4AF7C541F48FD15EEC57712B6C115D056BA1FCF3
uid                      Sergey <s.m.mih@mail.ru>
sub   rsa2048 2025-10-14 [E]
```

Шаг 2. Просмотр ключей

![keys view](images/image-5.png)

Шаг 3. Подпись сообщения

![message](images/image-6.png)

Шаг 4. Проверка подписи (ложное сообщение)

![bad message](images/image-7.png)

Шаг 5. Экспорт публичного ключа

![key export](images/image-8.png)

</details>

Результат:
* [Публичный ключ](Task_2/public.gpg) (с шага 5)
* [Файл сообщения](Task_2/message.txt) и [подпись к нему](Task_2/message.sig) (с шага 3).