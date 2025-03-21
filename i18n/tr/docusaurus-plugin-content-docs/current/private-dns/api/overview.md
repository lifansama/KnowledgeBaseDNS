---
title: Genel Bakış
sidebar_position: 1
toc_min_heading_level: 2
toc_max_heading_level: 3
---

<!--
    API info is from here:
    https://api.adguard-dns.io/static/api/API.md
-->

AdGuard DNS, uygulamalarınızı entegre etmek için kullanabileceğiniz bir REST API sağlar.

## Kimlik Doğrulama

### Erişim belirteci oluştur

Make a POST request for the following URL with the given params to generate the `access_token`:

`https://api.adguard-dns.io/oapi/v1/oauth_token`

| Parametre         | Açıklama                                                                       |
|:----------------- |:------------------------------------------------------------------------------ |
| **kullanıcı adı** | Hesap e-postası                                                                |
| **parola**        | Hesap parolası                                                                 |
| mfa_token         | İki Faktörlü kimlik doğrulama belirteci (hesap ayarlarında etkinleştirilmişse) |

Yanıt olarak hem `access_token` hem de `refresh_token` alırsınız.

- `access_token` süresi belirli bir saniye sonra dolar (yanıttaki `expires_in` parametresiyle temsil edilir). `refresh_token` kullanarak yeni bir `access_token` oluşturabilirsiniz (Bakınız: `Yenileme Belirtecinden Erişim Belirteci Oluşturma`).

- `refresh_token` kalıcıdır. To revoke a `refresh_token`, refer: `Revoking a Refresh Token`.

#### Örnek istek

```bash
$ curl 'https://api.adguard-dns.io/oapi/v1/oauth_token' -i -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'username=user%40adguard.com' \
    -d 'password=********' \
    -d 'mfa_token=727810'
```

#### Örnek yanıt

```json
{
  "access_token": "jTFho_aymtN20pZR5RRSQAzd81I",
  "token_type": "bearer",
  "refresh_token": "H3SW6YFJ-tOPe0FQCM1Jd6VnMiA",
  "expires_in": 2620978
}
```

### Yenileme Belirtecinden Erişim Belirteci Oluşturma

Erişim belirteçlerinin geçerliliği sınırlıdır. Süresi dolduğunda, uygulamanız yeni bir `refresh token` talep etmek için `access token` kullanması gerekecektir.

Make the following POST request with the given params to get a new access token:

`https://api.adguard-dns.io/oapi/v1/oauth_token`

| Parametre         | Açıklama                                                                 |
|:----------------- |:------------------------------------------------------------------------ |
| **refresh_token** | `REFRESH TOKEN` kullanılarak yeni bir erişim belirteci oluşturulmalıdır. |

#### Örnek istek

```bash
$ curl 'https://api.adguard-dns.io/oapi/v1/oauth_token' -i -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'refresh_token=H3SW6YFJ-tOPe0FQCM1Jd6VnMiA'
```

#### Örnek yanıt

```json
{
  "access_token": "xQnT7GYT6Ag--3oY_EcOOdXe-I0",
  "token_type": "bearer",
  "refresh_token": "H3SW6YFJ-tOPe0FQCM1Jd6VnMiA",
  "expires_in": 2627999
}
```

### Revoking a Refresh Token

To revoke a refresh token, make the following POST request with the given params:

`https://api.adguard-dns.io/oapi/v1/revoke_token`

#### İstek Örneği

```bash
$ curl 'https://api.adguard-dns.io/oapi/v1/revoke_token' -i -X POST \
    -d 'token=H3SW6YFJ-tOPe0FQCM1Jd6VnMiA'
```

| Parametre         | Açıklama                               |
|:----------------- |:-------------------------------------- |
| **refresh_token** | `REFRESH TOKEN` which is to be revoked |

### Yetkilendirme uç noktası

> Bu uç noktaya erişmek için **devteam@adguard.com** adresinden bizimle iletişime geçmeniz gerekir. Lütfen bu uç noktanın sebebini, kullanım durumlarını açıklayın ve yönlendirme URI'sini sağlayın. Onaylandıktan sonra, **client_id** parametresi için kullanılması gereken benzersiz bir i̇stemci tanımlayıcısı alırsınız.

**oapi/v1/oauth_authorize** uç noktası, kaynak sahibiyle etkileşime geçmek ve korunan kaynağa erişim yetkisi almak için kullanılır.

Hizmet, kimlik doğrulaması için sizi AdGuard'a yönlendirir (henüz giriş yapmadıysanız) ve ardından uygulamanıza geri döner.

**oapi/v1/oauth_authorize** uç noktasının istek parametreleri şunlardır:

| Parametre         | Açıklama                                                                                                                                                  |
|:----------------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **response_type** | Yetkilendirme sunucusuna hangi iznin yürütüleceğini söyler                                                                                                |
| **client_id**     | Yetkilendirme isteyen OAuth istemcisinin kimliği                                                                                                          |
| **redirect_uri**  | Bir URL içerir. Bu uç noktadan gelen başarılı bir yanıt, bu URL'ye yönlendirme yapar                                                                      |
| **state**         | Güvenlik amacıyla kullanılan opak bir değer. Bu istek parametresi istekte ayarlanırsa, **redirect_uri** öğesinin bir parçası olarak uygulamaya döndürülür |
| **aid**           | Ortaklık tanımlayıcısı                                                                                                                                    |

Örneğin:

```http request
https://api.adguard-dns.io/oapi/v1/oauth_authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&state=1jbmuc0m9WTr1T6dOO82
```

Yetkilendirme sunucusuna hangi izn türünün kullanılacağını bildirmek için **response_type** istek parametresi aşağıdaki gibi kullanılır:

- Örtülü izin için, bir erişim belirteci eklemek üzere **Response_type=token** kullanın.

Başarılı bir yanıt **302 Found** olur ve bu **redirect_uri** (bir istek parametresidir) adresine bir yönlendirmeyi tetikler. Yanıt parametreleri, **redirect_uri** parametresinin **Location** başlığının parça bileşenine (yani `#` işaretinden sonraki kısım) gömülüdür.

Örneğin:

```http request
HTTP/1.1 302 Found
Location: REDIRECT_URI#access_token=...&token_type=Bearer&expires_in=3600&state=1jbmuc0m9WTr1T6dOO82
```

### API'ye erişim

Erişim ve yenileme belirteçleri oluşturulduktan sonra, başlıktaki erişim belirtecini geçirilerek API çağrıları yapılabilir.

- Başlık adı `Authorization` olmalıdır
- Başlık değeri `Bearer {access_token}` olmalıdır

## API

### Referans

Lütfen referans yöntemlerine [buradan](reference.md) bakın.

### OpenAPI özellikleri

OpenAPI specification is available at [https://api.adguard-dns.io/swagger/openapi.json][openapi].

Kullanılabilir API yöntemlerinin listesini görüntülemek için farklı araçlar kullanabilirsiniz. Örneğin, bu dosyayı [https://editor.swagger.io/][swagger] adresinde açabilirsiniz.

### Değişiklik günlüğü

AdGuard DNS API değişiklik günlüğünün tamamı [bu sayfada](private-dns/api/changelog.md) mevcuttur.

## Geri bildirim

Bu API'nin yeni yöntemlerle genişletilmesini istiyorsanız, lütfen `devteam@adguard.com` adresine e-posta gönderin ve nelerin eklenmesini istediğinizi bize bildirin.

[openapi]: https://api.adguard-dns.io/swagger/openapi.json
[swagger]: https://editor.swagger.io/
