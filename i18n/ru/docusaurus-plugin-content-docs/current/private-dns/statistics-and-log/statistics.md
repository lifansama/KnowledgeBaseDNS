---
title: Статистика
sidebar_position: 2
---

## Общая статистика

На вкладке _Статистика_ отображается вся сводная статистика по DNS-запросам, сделанным устройствами, подключёнными к Личному AdGuard DNS. Она показывает общее количество и местоположение запросов, количество заблокированных запросов, список компаний, которым были адресованы запросы, типы запросов и наиболее часто запрашиваемые домены.

![Заблокированный сайт \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/overall_stats.png)

## Категории

### Типы запросов

- **Реклама**: рекламные и другие связанные с рекламой запросы, которые собирают и передают данные пользователей, анализируют их поведение, и таргетируют рекламу
- **Трекеры**: запросы с сайтов и от третьих сторон с целью отслеживания активности пользователей
- **Соцсети**: запросы к сайтам социальных сетей
- **CDN**: запросы, связанные с Content Delivery Network (CDN), глобальной сетью прокси-серверов, ускоряющей доставку контента конечным пользователям
- **Прочее**

![Типы запросов \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/request_types.png)

### Топ компаний

Здесь можно увидеть компании, отправившие наибольшее количество запросов.

![Топ компаний \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/top_companies.png)

### Топ направлений

Здесь показаны страны, в которые отправлено больше всего запросов.

В дополнение к названиям стран список содержит две более общие категории:

- **Не применимо**: Ответ не содержит IP-адреса
- **Неизвестное направление**: Страна не может быть определена по IP-адресу

![Топ направлений \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/top_destinations.png)

### Топ доменов

Содержит список доменов, к которым было отправлено наибольшее количество запросов.

![Топ доменов \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/top_domains.png)

### Зашифрованные запросы

Показывает общее количество запросов и процент зашифрованного и незашифрованного трафика.

![Зашифрованные запросы \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/statistics/encrypted_requests.png)

### Топ клиентов

Показывает количество запросов, сделанных клиентам. Чтобы увидеть IP-адреса клиентов, включите опцию _Записывать IP-адреса_ в _Настройках сервера_. [Подробнее о настройках сервера](/private-dns/server-and-settings/advanced.md) можно найти в соответствующем разделе.
