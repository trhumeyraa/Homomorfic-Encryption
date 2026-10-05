Homomorphic Market

Homomorphic Market, şirketlerin ve pazar aktörlerinin finansal ve operasyonel verilerini açık etmeden sektör genelinde kıyaslama ve rekabet analizi yapabilmesi için geliştirdiğim gizlilik odaklı bir analitik platformudur.

Bu projede temel problem şuydu: Normalde sektör analizi veya pazar payı kıyaslaması yapmak isteyen şirketler, hassas verilerini üçüncü taraf bir analitik aracına veya merkezi bir sunucuya yüklemek zorunda kalıyor. Bu da ticari sırların sızması ya da veri gizliliği ihlalleri riskini doğuruyor.

Ben bu problemi çözmek için platformu Homomorfik Şifreleme (Homomorphic Encryption) üzerine kurdum. Sistemde pazar verileri sunucuya gelmeden önce istemci tarafında şifreleniyor. Sunucu, verinin şifresini hiçbir zaman çözmeden (plaintext görmeden) şifreli metinler (ciphertext) üzerinde toplama, ortalama alma ve kıyaslama hesaplamaları yapabiliyor.

Temel Özellikler

İşlem Sırasında Veri Güvenliği (Data-in-Use): Klasik şifrelemede veri hesaplanırken bellekte çözülmek zorundadır. Burada veri hesaplama anında bile şifreli kalır; sunucuya veya veritabanına doğrudan sızılsa bile ham veriye ulaşılamaz.

Kuantum Direnci (Post-Quantum Security): Kullandığım homomorfik şifreleme motoru, klasik şifreleme algoritmalarını (RSA, ECC vb.) Shor Algoritması ile dakikalar içinde kırabilen kuantum bilgisayarlara karşı korumalıdır. RLWE (Ring-Learning With Errors) gibi kafes tabanlı (lattice-based) matematiksel problemlere dayandığı için geleceğin kuantum tehditlerine karşı doğuştan dayanıklıdır.

Gizlilik Koruyarak Kıyaslama: Katılımcılar pazar ortalamalarını ve rekabet grafiklerini görebilir ancak hiçbir katılımcı diğerinin ya da platformun ham rakamlarını göremez.

Kuantum ve Kriptografi Tarafı

Geleneksel açık anahtarlı şifreleme yöntemleri kuantum bilgisayarlar geliştikçe risk altına giriyor. Bu projede kullandığım şema doğrudan Kuantum Sonrası Kriptografi (PQC) standartlarıyla örtüşen kafes tabanlı kriptografiye dayanır:

Şifrelenmiş pazar verisi API'ye iletildiğinde, arka planda sadece şifreli tensörler üzerinde homomorfik toplama ve skalar çarpma işlemleri yürütülür.

Gizli anahtar (private key) sadece veriyi giren istemcide bulunur, sunucuya kesinlikle gitmez.

Kuantum bilgisayarlar da dahil olmak üzere hiçbir aktör şifreli verinin içindeki gerçek satış, maliyet ve stok rakamlarını tersine mühendislikle elde edemez.

Teknoloji Yığını

Backend

FastAPI (Python 3.11+)

Homomorphic Encryption Engine (Kafes tabanlı ciphertext operasyonları)

PostgreSQL & SQLAlchemy

JWT ve hash tabanlı kimlik doğrulama

Frontend

React 18+ (Vite)

Tailwind CSS

Recharts / Chart.js

Axios

Altyapı

Docker & Docker Compose

Nginx (Reverse Proxy)

Dizin Yapısı

homomorphic-market/
├── backend/
│   ├── app/
│   │   ├── api/             # Analytics, Competitive, Auth ve Data Entry endpointleri
│   │   ├── core/            # Ayarlar, veritabanı bağlantısı ve güvenlik modülleri
│   │   ├── models/          # Veritabanı modelleri
│   │   ├── schemas/         # Pydantic şemaları
│   │   └── services/        # Homomorfik hesaplama ve veri üretim servisleri
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── pages/           # Dashboard, Competitive, DataEntry ve Auth sayfaları
│   │   ├── hooks/           # useAuth ve state yönetimi
│   │   └── utils/           # API servisleri ve yardımcı araçlar
│   └── Dockerfile
├── docker-compose.yml
└── README.md


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
