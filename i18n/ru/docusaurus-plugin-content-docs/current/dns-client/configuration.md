---
title: Файл конфигурации
sidebar_position: 2
---

<!-- markdownlint-configure-file {"ul-indent":{"indent":4,"start_indent":2,"start_indented":true}} -->

Полный пример конфигурационного файла [YAML][yaml] с комментариями смотрите в файле [`config.dist.yml`][dist].

<!--
    TODO(a.garipov): Find ways to add IDs to individual list items.
-->

[dist]: https://github.com/AdguardTeam/AdGuardDNSClient/blob/master/config.dist.yaml
[yaml]: https://yaml.org/

## `dns` {#dns}

Объект `dns` настраивает поведение DNS-сервера. Он обладает следующими свойствами:

### `cache` {#dns-cache}

Объект `cache` настраивает кеширование результатов запросов к DNS. Он обладает следующими свойствами:

- `enabled`: следует ли кешировать результаты DNS.

  **Пример:** `true`

- `size`: максимальный размер кеша результатов DNS в виде удобочитаемых данных. Он должен быть больше нуля, если `enabled` равно `true`.

  **Пример:** `128 МБ`

- `client_size`: максимальный размер кеша результатов DNS для каждого настроенного адреса клиента или подсети в виде удобочитаемых данных. Он должен быть больше нуля, если `enabled` равно `true`.

  **Пример:** `4 МБ`

### `server` {#dns-server}

Объект `server` настраивает обработку входящих запросов. Он обладает следующими свойствами:

- `bind_retry`: The confguration of the retry mechanism for binding to the listen addresses. This is useful if the server is started before the network is ready and the addresses are not yet available, as on some editions of Windows when installed as a system service.

  :::note

  This object is available since **v0.0.3**.

  :::

  Он обладает следующими свойствами:

  - `enabled`: Whether bind retry is enabled or not.

    **Пример:** `true`

  - `interval`: The interval between retries as a human-readable duration.

    **Example:** `1s`

  - `count`: The maximum number of attempts after the first failure. That is, if `count` is `4`, the total number of attempts will be five.

    **Example:** `4`

- `listen_addresses`: набор адресов с портами для прослушивания.

  **Пример свойства:**

  ```yaml
  'listen_addresses':
      - address: '127.0.0.1:53'
      - address: '[::1]:53'
  ```

### `bootstrap` {#dns-bootstrap}

Объект `bootstrap` настраивает разрешение адресов серверов [upstream](#dns-upstream). Он обладает следующими свойствами:

- `серверы`: список серверов для разрешения имён хостов upstream-серверов.

  **Пример свойства:**

  ```yaml
  'servers':
      - address: '8.8.8.8:53'
      - address: '192.168.1.1:53'
  ```

- `timeout`: время ожидания для bootstrap DNS-запросов.

  **Пример:** `2 с`

### `upstream` {#dns-upstream}

Объект `upstream` настраивает фактическое разрешение запросов. Он обладает следующими свойствами:

- `groups`: набор upstream-серверов, связанных с именем группы. Он обладает следующими свойствами:

  - `address`: адрес upstream-сервера.

    **Пример:** `'8.8.8.8:53'`

  - `match`: список критериев для сопоставления запроса. Каждая запись может содержать следующие свойства:

    - `question_domain`: домен или суффикс домена, для разрешения которого должен использоваться набор upstream-серверов.

      **Пример:** `'mycompany.local'`

    - `client`: адрес клиента или подсеть адресов клиента, с которых набор upstream-серверов должен разрешать запросы. В нём не должно быть значащих битов за пределами маски подсети.

      **Пример:** `'192.0.2.0/24'`

    :::note

    Свойства, указанные в одной записи, объединяются с помощью логического AND. Записи объединяются логическим OR.

    :::

    **Пример свойства:**

    ```yaml
    'match':
        - question_domain: 'mycompany.local'
        client: '192.168.1.0/24'
        - question_domain: 'mycompany.external'
        - client: '1.2.3.4'
    ```

  :::info

  `groups` должно содержать как минимум одну запись с именем `default` и, опционально, одну запись с именем `private`, у обеих записей не должно быть свойства `match`.

  :::

  Группа `default` будет использоваться, если нет совпадений среди других групп. Группа `private` будет использоваться для разрешения PTR-запросов для частных IP-адресов. На такие запросы будет получен ответ `NXDOMAIN`, если не определена группа `private`.

- `timeout`: время ожидания для upstream DNS-запросов.

  **Пример:** `2 с`

### `fallback` {#dns-fallback}

Объект `fallback` настраивает поведение DNS-сервера в случае сбоя. Он обладает следующими свойствами:

- `servers`: список серверов, которые будут использоваться после того, как фактический [upstream](#dns-upstream) не смог ответить.

  **Пример свойства:**

  ```yaml
  'servers':
      - address: 'tls://94.140.14.140'
  ```

- `timeout`: время ожидания для fallback DNS-запросов.

  **Пример:** `2 с`

## `debug` {#debug}

Объект `debug` настраивает функции отладки. Он обладает следующими свойствами:

### `pprof` {#debug-pprof}

Объект `pprof` настраивает HTTP-обработчики [`pprof`][pkg-pprof]. Он обладает следующими свойствами:

- `port`: порт для прослушивания отладочных HTTP-запросов на localhost.

  **Пример:** `6060`

- `enabled`: включено или нет профилирование отладки.

  **Пример:** `true`

[pkg-pprof]: https://golang.org/pkg/net/http/pprof

## `log` {#log}

Объект `log` настраивает ведение журнала. Он обладает следующими свойствами:

- 'output': куда записываются логи.

  :::note

  Записываются в системный журнал в текстовом формате (см. ниже) и используют системную временную метку.

  :::

  Возможные значения:

  - `syslog` означает, что используется системный журнал, специфичный для конкретной платформы: syslog для Linux и Event Log для Windows.

  - `stdout` для стандартного потока вывода.

  - `stderr` для стандартного потока ошибок.

  - Абсолютный путь к файлу журнала.

  **Пример:** `/home/user/logs`

  **Пример:** `C:\Users\user\logs.txt`

  **Пример:** `syslog`

- `format`: определяет формат записей журнала.

  Возможные значения:

  - `default`: простой формат. Пример:

    ```none
    INFO service started prefix=program addr=127.0.0.1:53
    ```

  - 'json': структурированный формат JSON. Пример:

    ```json
    {"level":"INFO","msg":"service started","prefix":"program","addr":"127.0.0.1:53"}
    ```

  - `jsonhybrid`: то же, что `json`, но с ограниченным количеством полей. Пример:

    ```json
    {"level":"INFO","msg":"service started, attrs: prefix=program addr=127.0.0.1:53"}
    ```

  - `text`: структурированный текстовый формат. Пример:

    ```none
    level=INFO msg="service started" prefix=program addr=127.0.0.1:53
    ```

  **Пример:** `default`

- `timestamp`: указывает, включать ли временную метку в записях журнала.

  **Пример:** `false`

- `verbose`: указывает, должен ли журнал быть более информативным.

  **Пример:** `false`
