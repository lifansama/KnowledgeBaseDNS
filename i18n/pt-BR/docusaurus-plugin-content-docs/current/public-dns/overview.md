---
title: Visão Geral
sidebar_position: 1
---

## O que é AdGuard DNS?

O AdGuard DNS é um resolvedor de DNS gratuito e orientado à privacidade que fornece conexão segura e também pode bloquear rastreamento, anúncios, phishing e conteúdo adulto (opcionalmente). O AdGuard DNS não requer a instalação de nenhum aplicativo. É fácil de usar e pode ser configurado sem esforço em qualquer dispositivo (smartphones, desktops, roteadores, consoles de jogos, etc.).

## Servidores públicos do AdGuard DNS

AdGuard DNS has three different types of public servers. O servidor "padrão" é para bloquear anúncios, rastreadores, malware e sites de phishing. A "proteção familiar" faz o mesmo, mas também bloqueia sites com conteúdo adulto e impõe a opção "Pesquisa segura" nos navegadores que a fornecem. "Sem filtragem" fornece uma conexão segura e confiável, mas não bloqueia nada. You can find detailed instructions on setting up AdGuard DNS on any device on [our website](https://adguard-dns.io/public-dns.html). Cada servidor suporta diferentes protocolos seguros: DNSCrypt, DNS-over-HTTPS (DoH), DNS-over-TLS (DoT) e DNS-over-QUIC (DoQ).

## Protocolos AdGuard DNS

Além do DNS simples (IPv4 e IPv6), o AdGuard DNS suporta vários protocolos criptografados, para que você possa escolher o que melhor se adapta a você.

### DNSCrypt

O AdGuard DNS permite que você use um protocolo criptografado específico — DNSCrypt. Graças a ele, todas as solicitações de DNS estão sendo criptografadas, o que protege você de possíveis interceptações de solicitações e posterior espionagem e/ou alteração. Mas comparado aos protocolos DoH, DoT e DoQ, o DNSCrypt é considerado obsoleto e, se possível, recomendamos o uso desses protocolos.

### DNS-over-HTTPS (DoH) e DNS-over-TLS (DoT)

DoH e DoT são protocolos DNS seguros modernos que ganham cada vez mais popularidade e se tornarão os padrões da indústria no futuro próximo. Ambos são mais confiáveis que o DNSCrypt e ambos são suportados pelo AdGuard DNS.

#### JSON API for DNS

AdGuard DNS also provides a JSON API for DNS. It is possible to get a DNS response in JSON by typing:

```text
curl 'https://dns.adguard-dns.com/resolve?name=www.example.com'
```

For detailed documentation, refer to [Google's guide to JSON API for DNS-over-HTTPS](https://developers.google.com/speed/public-dns/docs/doh/json). Getting a DNS response in JSON works the same way with AdGuard DNS.

:::note

Unlike with Google DNS, AdGuard DNS doesn't support `edns_client_subnet` and `Comment` values in response JSONs.

:::

### DNS-over-QUIC (DoQ)

[DNS-over-QUIC is a new DNS encryption protocol](https://adguard-dns.io/en/blog/dns-over-quic.html) and AdGuard DNS is the first public resolver that supports it. Ao contrário do DoH e do DoT, ele usa o QUIC como protocolo de transporte e finalmente traz o DNS de volta às suas raízes - trabalhando sobre UDP. Ele traz todas as coisas boas que o QUIC tem a oferecer — criptografia pronta para uso, tempos de conexão reduzidos, melhor desempenho quando os pacotes de dados são perdidos. Além disso, o QUIC é suposto ser um protocolo de nível de transporte e não há riscos de vazamentos de metadados que poderiam acontecer com o DoH.

### Rate limit

DNS rate limiting is a technique used to regulate the amount of traffic a DNS server can handle within a specific time period. We offer the option to increase the default limit for Team and Enterprise plans of Private AdGuard DNS. For more information, please [read the related article](/private-dns/server-and-settings/rate-limit.md).
