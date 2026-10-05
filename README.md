# CipherMarket

**CipherMarket**, homomorfik şifrelemenin gerçek bir uygulama akışında nasıl kullanılabileceğini inceleyen bir bitirme projesi prototipidir. Projenin odağı, gizlilik odaklı şifrelemenin uygulanabilirliğini göstermek ve kuantum çağında veri güvenliği üzerine farkındalık oluşturmaktır.

Uygulama, satış verilerinin sayısal alanlarını veritabanına kaydetmeden önce **CKKS** ile şifreler; seçili toplama ve ortalama işlemlerini ciphertext üzerinde **TenSEAL** kullanarak gerçekleştirir. Proje, kuantum sonrası güvenlik sertifikası veya üretime hazır bir gizlilik çözümü iddiasında değildir.

## Neler gösteriyor?

- Her kurum için ayrı CKKS context'i ve anahtar çifti oluşturulmasını.
- Sayısal satış alanlarının şifrelenerek veritabanında saklanmasını.
- Şifreli değerler üzerinde toplama ve ortalama hesaplanmasını.
- Şifreleme akışının uygulama ve API katmanlarına nasıl bağlanabileceğini.

## Şifreleme modeli ve güven sınırları

- Satış verisi uygulama sunucusuna istek sırasında açık biçimde ulaşır. Backend sayısal alanları CKKS ile şifreledikten sonra veritabanına yazar.
- Ürün adı, kategori, tarih, şehir/bölge, kurum kimliği ve paylaşım bilgisi gibi metadata şifrelenmez.
- Kuruma ait secret context sunucuda, `SECRET_KEY`'den türetilen anahtarla ayrıca şifrelenerek saklanır. Sunucu veya yapılandırmasını kontrol eden taraf veriyi çözebilir; bu uçtan uca şifreleme değildir.
- Kurum içi seçili toplamlar ciphertext üzerinde hesaplanır ve sonuçlar backend tarafından çözülerek kullanıcıya döndürülür.
- Rakip sıralaması sunucu tarafında, paylaşılmış kayıtlar çözülerek hesaplanır. Bu özellik homomorfik sıralama değildir; tutarları sunucu görür ve sonuç sıralama bilgisi açığa çıkarabilir.
- CKKS yaklaşık sayısal aritmetik kullanır. Ufak sonuç farkları oluşabileceğinden muhasebe veya kesin finansal kayıtlar için uygun değildir.
- CKKS, kafes tabanlı kriptografiye dayanır ve kuantum sonrası kriptografi araştırmalarında ele alınan yaklaşımlarla ilişkilidir. Bu proje, kullanılan parametrelerin belirli bir güvenlik seviyesi sağladığını veya uygulamanın kuantum saldırılarına karşı güvenli olduğunu kanıtlamaz.

> **Uyarı:** Eğitim ve araştırma amaçlı prototiptir. Gerçek işletme verilerini kullanmayın ve uygulamayı herkese açık internete açmayın.

## Kullanılan teknolojiler

| Katman | Teknolojiler |
|---|---|
| Homomorfik şifreleme | CKKS, TenSEAL 0.3.18 |
| Backend | Python, FastAPI, SQLAlchemy |
| Veritabanı | PostgreSQL |
| Frontend | React 18, Vite |
| Servis ve web sunucusu | Docker Compose, Nginx |

## Yerelde çalıştırma

Gereksinimler: Docker Desktop ve Docker Compose.

```powershell
Copy-Item .env.example .env
docker compose up --build -d
```

Uygulama: <http://localhost:3000>  
API ve etkileşimli dokümantasyon: <http://localhost:8000/docs>

Servisleri durdurmak için:

```powershell
docker compose down
```

`docker compose down -v` veritabanı volume'unu ve içindeki tüm verileri siler. Yalnızca yerel demo verilerini sıfırlamak istediğinizden eminseniz kullanın.

`.env.example` yerel demo içindir. İnternete açık bir dağıtım için hazır değildir. Veritabanı portu ana makineye açılmaz; web ve API portları varsayılan olarak yalnızca `127.0.0.1` üzerinden erişilir.

## CSV ile veri yükleme

Gerekli sütunlar:

```text
product_name,category,quantity_sold,unit_price,sale_date
```

`region` sütunu isteğe bağlıdır. Tarih biçimi `YYYY-MM-DD`; yükleme sınırı 2 MB ve 5.000 satırdır.

## Proje yapısı

```text
backend/app/api/        API uç noktaları
backend/app/core/       Ayarlar, veritabanı ve kimlik doğrulama
backend/app/models/     Veritabanı modelleri
backend/app/services/   CKKS işlemleri ve demo veri üretimi
frontend/src/pages/     React sayfaları
docker-compose.yml      Yerel servis yapılandırması
```
Çalıştırma

Gereksinimler: Docker ve Docker Compose (lokal geliştirmek istersen Python 3.11+ ve Node.js 18+)

Depoyu klonla:

git clone https://github.com/kullanici-adi/homomorphic-market.git
cd homomorphic-market


Ortam değişkenlerini hazırla:

cp .env.example .env


Docker ile ayağa kaldır:

docker-compose up --build -d


Servisler açıldıktan sonra:

Frontend Arayüzü: http://localhost:3000

Backend Swagger Dokümantasyonu: http://localhost:8000/docs

Lisans

Bu proje MIT lisansı altındadır.
