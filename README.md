# SystemLog Config

Spring Cloud Config Server için merkezi yapılandırma deposu. **SystemLog** mikroservis mimarisindeki tüm servislerin konfigürasyon dosyalarını barındırır.

## Servisler

| Dosya | Servis | Port |
|---|---|---|
| `application.yml` | Ortak ayarlar (Eureka) | — |
| `api-gateway.yml` | API Gateway | 8080 |
| `ingestion-service.yml` | Log Toplama Servisi | 8081 |
| `processor-ai-service.yml` | AI İşleme Servisi (Gemini) | 8082 |
| `query-service.yml` | Sorgulama Servisi | 8083 |

## Mimari

```
Client ──▶ API Gateway (8080)
               ├──▶ Ingestion Service (8081) ──▶ Kafka
               └──▶ Query Service (8083) ──▶ PostgreSQL
                          
           Processor AI Service (8082)
               ├── Kafka Consumer
               ├── PostgreSQL
               └── Vertex AI (Gemini)
```

## Teknolojiler

- **Spring Cloud Config** — Merkezi konfigürasyon yönetimi
- **Spring Cloud Gateway** — API yönlendirme
- **Eureka** — Servis keşfi
- **Apache Kafka** — Mesaj kuyruğu
- **PostgreSQL** — Veritabanı
- **Google Vertex AI (Gemini)** — AI log analizi

## Kullanım

Bu repo, Spring Cloud Config Server tarafından uzak konfigürasyon kaynağı olarak kullanılır:

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/berkecftc/SystemLog-config.git
```

## Notlar

- `processor-ai-service.yml` içindeki veritabanı şifresi ve GCP proje bilgileri ortam değişkenleri ile yönetilmelidir.
- Hassas bilgiler için Spring Cloud Config encryption veya Vault entegrasyonu önerilir.
