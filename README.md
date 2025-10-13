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

### Результат выполнениея

В качестве результата пришлите:
1. Публичный и приватный ключ (`public.key` и `private.key`)
1. Passphrase (строкой)
1. Зашифрованный файл (`cypher.txt`)

---

## Решение.

<details>
<summary>Детали выполнения.</summary>
  
Проверка версии OpenSSL.

![openssl version](image.png)

Создание ключа.

![ssh key](image-1.png)


</details>