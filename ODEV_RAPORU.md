# 🚀 ISE 465 - Bulut Bilişim Dersi 2. Ödev Raporu

**Öğrenci Adı Soyadı:** Eren Başaran  
**Öğrenci Numarası:** B201200023  
**Ders:** ISE 465 - Bulut Bilişim  
**Dönem:** 2025-2026 Güz Dönemi  
**Teslim Tarihi:** 28.12.2025

---

## 1. Proje Açıklaması ve Hedefleri

Bu proje, bulut bilişim kavramlarını anlamak ve hazır bulut sağlayıcı platformları kullanarak uygulama dağıtımı becerilerini geliştirmeyi amaçlamaktadır. Proje kapsamında, modern bir görev yönetim arayüzü olan **TaskFlow** uygulaması Google Cloud Platform (GCP) üzerinde bir sanal makine altyapısı kullanılarak canlıya alınmıştır.

### Proje Hedefleri
- ✅ Gerçek bir web uygulaması geliştirme
- ✅ Bulut platformunu tanıma ve kullanma
- ✅ Sanal makine (VM) yönetimi öğrenme
- ✅ Linux sunucu yapılandırması deneyimi
- ✅ Güvenlik ve ağ yönetimi becerileri kazanma

---

## 2. Uygulama Seçimi ve Özellikleri

Dağıtım için kullanıcı dostu bir **"Task Manager" (Görev Yöneticisi)** uygulaması belirlenmiştir.

### Uygulama Özellikleri:
- ✨ **Görev Yönetimi:** Görev ekleme, silme ve tamamlama fonksiyonları
- 🎯 **Öncelik Sistemi:** Görevlere göre öncelik (Düşük, Orta, Yüksek) atama
- 🔍 **Dinamik Filtreleme:** Tümü, Aktif, Tamamlanan görevleri filtreleme
- 📱 **Responsive Tasarım:** Mobil uyumlu modern arayüz
- 💾 **Veri Saklama:** LocalStorage ile kalıcı veri saklama
- 📊 **İstatistikler:** Gerçek zamanlı görev istatistikleri
- 🎨 **Modern UI/UX:** Gradient renkler, animasyonlar, glassmorphism

### Teknik Özellikler:
- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+)
- **Veri:** LocalStorage API
- **Stil:** Custom CSS (Framework kullanılmadı)
- **Boyut:** Toplam ~23KB (çok hafif)

---

## 3. Bulut Platformunun Seçimi

Uygulama dağıtımı için **Google Cloud Platform (GCP)** tercih edilmiştir.

### Seçim Kriterleri:
1. **Ücretsiz Plan:** Sağlayıcının sunduğu ücretsiz plan (Free Tier) kaynaklarının yeterliliği
2. **Esneklik:** Sanal makine (VM) oluşturma ve yönetme süreçlerinin esnekliği
3. **Güvenlik:** Güçlü güvenlik duvarı (Firewall) yönetim araçları
4. **Performans:** Hızlı ve stabil altyapı
5. **Dokümantasyon:** Kapsamlı dokümantasyon ve topluluk desteği

### Alternatif Platformlar (Değerlendirilen):
- **AWS EC2:** Daha karmaşık, öğrenme eğrisi yüksek
- **Azure VM:** Free tier sınırlamaları
- **Netlify/Vercel:** Static hosting (VM deneyimi sağlamaz)
- **GCP Compute Engine:** ✅ Seçildi - Denge ve kullanım kolaylığı

---

## 4. Uygulama Mimari Şeması

Proje, **IaaS (Infrastructure as a Service - Altyapı Hizmeti)** modelini temel alan bir mimariye sahiptir:

```
┌─────────────────────────────────────────────────────────────────┐
│                    TASKFLOW MİMARİ ŞEMASI                       │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐         ┌───────────────────────────────────────┐
│              │         │   GOOGLE CLOUD PLATFORM (GCP)         │
│   İstemci    │  HTTP   │                                       │
│              │ ──────► │  ┌──────────────────────────────┐     │
│  (Browser)   │         │  │   GCP Firewall Rules         │     │
│              │         │  │   - Allow HTTP (Port 80)     │     │
└──────────────┘         │  │   - Allow HTTPS (Port 443)   │     │
                         │  └──────────────────────────────┘     │
                         │              │                        │
                         │              ▼                        │
                         │  ┌──────────────────────────────┐     │
                         │  │   Compute Engine VM          │     │
                         │  │   - Type: e2-micro           │     │
                         │  │   - OS: Debian 11            │     │
                         │  │   - Region: us-central1      │     │
                         │  └──────────────────────────────┘     │
                         │              │                        │
                         │              ▼                        │
                         │  ┌──────────────────────────────┐     │
                         │  │   Apache2 Web Server         │     │
                         │  │   - Serves Static Files      │     │
                         │  │   - Port 80                  │     │
                         │  └──────────────────────────────┘     │
                         │              │                        │
                         │              ▼                        │
                         │  ┌──────────────────────────────┐     │
                         │  │   Static Files               │     │
                         │  │   /var/www/html/             │     │
                         │  │   - index.html               │     │
                         │  │   - style.css                │     │
                         │  │   - app.js                   │     │
                         │  └──────────────────────────────┘     │
                         └───────────────────────────────────────┘
```

### Mimari Akış:
1. **İstemci Katmanı:** Kullanıcı web tarayıcısı üzerinden HTTP isteği gönderir
2. **Ağ Katmanı:** GCP Firewall kuralları üzerinden port 80 trafiğine izin verilir
3. **Sunucu Katmanı:** Compute Engine üzerinde çalışan Debian VM istekleri karşılar
4. **Uygulama Katmanı:** Apache2 web sunucusu static dosyaları serve eder
5. **Veri Katmanı:** Client-side LocalStorage (Backend yok - tam statik)

---

## 5. Uygulamanın Bulut Platformuna Taşınması

Uygulamanın yerel ortamdan bulut platformuna aktarılması süreci şu aşamalarla gerçekleştirilmiştir:

### 5.1 Bulut Ortamının Hazırlanması

#### Adım 1: GCP Projesi Oluşturma
```bash
# GCP Console'a giriş
# Yeni proje oluştUR: "Bulut-Odev"
# Billing hesabını bağla (Free Tier kullanımı için)
```

#### Adım 2: Compute Engine VM Instance Oluşturma
```bash
# GCP Console > Compute Engine > VM Instances > CREATE INSTANCE

Yapılandırma:
- Name: instance-20251228-162816
- Region: us-central1
- Zone: [Otomatik]
- Machine Type: e2-micro (Free tier eligible)
  * 0.25-1 vCPU
  * 1 GB memory
- Boot Disk: 
  * OS: Debian GNU/Linux 11 (bullseye)
  * Disk Type: Standard persistent disk
  * Size: 10 GB
- Firewall:
  ✅ Allow HTTP traffic
  ✅ Allow HTTPS traffic (opsiyonel)
```

#### Adım 3: Firewall Kurallarını Doğrulama
```bash
# GCP Console > VPC Network > Firewall Rules
# "default-allow-http" kuralının var olduğunu kontrol ettim.
```

### 5.2 Sunucu Kurulumu ve Otomasyon Kodları

#### Adım 4: VM'e SSH Bağlantısı
```bash
# GCP Console'dan "SSH" butonuna tıkla
```

#### Adım 5: Sistem Güncelleme
```bash
# Paket listesinin güncellenmesi
sudo apt update && sudo apt upgrade -y
```

#### Adım 6: Apache2 Web Sunucusu Kurulumu
```bash
# 2. Apache2 kurulumu
sudo apt install apache2 -y

# 3. Apache2 servisini başlatma ve otomatik başlatmayı aktif etme
sudo systemctl start apache2
sudo systemctl enable apache2

# 4. Durumu kontrol etme
sudo systemctl status apache2
```

#### Adım 7: Uygulama Dosyalarını Yükleme
```bash
# Mevcut default dosyaları temizleme
sudo rm -rf /var/www/html/*

# Dosyaları yükleme için 3 yöntem:

# YÖNTEM 1: SCP ile (Lokal bilgisayardan)
# Lokal terminalinizde:
cd c:\Users\iamer\Desktop\BulutBilisim
gcloud compute scp index.html taskflow-vm:/tmp/ --zone=[YOUR-ZONE]
gcloud compute scp style.css taskflow-vm:/tmp/ --zone=[YOUR-ZONE]
gcloud compute scp app.js taskflow-vm:/tmp/ --zone=[YOUR-ZONE]

# VM terminalinde:
sudo mv /tmp/index.html /var/www/html/
sudo mv /tmp/style.css /var/www/html/
sudo mv /tmp/app.js /var/www/html/
```

#### Adım 8: Dosya İzinlerini Düzenleme
```bash
# İzinlerin düzenlenmesi
sudo chmod -R 755 /var/www/html
sudo chown -R www-data:www-data /var/www/html

# Dosyaları kontrol etme
ls -lah /var/www/html/
```


#### Adım 9: Test ve Doğrulama
```bash
# 1. VM'in External IP'sini alma
GCP Console'dan VM listesinde "External IP" sütunu

# 2. Lokal test (VM içinden)
curl http://localhost

# 3. Dış erişim testi (Browser'dan)
# http://[EXTERNAL-IP] adresi
```


## 6. Karşılaşılan Zorluklar ve Çözümler

### Zorluk 1: Kaynak Kısıtları
**Problem:** Ücretsiz plan kapsamındaki düşük özellikli VM'lerde (e2-micro) paket güncellemelerinin çok uzun sürmesi (15-20 dakika).

**Çözüm:** 
- Sabırlı bekleme ve sürecin tamamlanmasına izin verme
- Gereksiz paketleri kurmamak için minimal kurulum stratejisi
- `apt upgrade -y` yerine sadece `apt update` kullanma (ilk testte)

### Zorluk 2: Güvenlik Duvarı Yapılandırması
**Problem:** Başlangıçta VM External IP üzerinden erişim sağlanamadı. Browser'da "This site can't be reached" hatası.

**Çözüm:**
```bash
# 1. GCP Firewall kurallarını kontrol
gcloud compute firewall-rules list

# 2. HTTP kuralını manuel oluşturma
gcloud compute firewall-rules create allow-http \
    --allow tcp:80 \
    --source-ranges 0.0.0.0/0 \
    --description "Allow HTTP traffic"

# 3. VM network tags kontrol
gcloud compute instances add-tags taskflow-vm \
    --tags http-server \
```

### Zorluk 3: Apache Default Page Görünüyor
**Problem:** IP adresine gidildiğinde Apache "It works!" sayfası görünüyordu.
**Çözüm:**
```bash
# Default index.html dosyasının silindi
sudo rm /var/www/html/index.html

# Kendi dosyalarım yüklendi
# Apache cache temizleme
sudo systemctl restart apache2
# Browser cache temizleme (Ctrl+Shift+R)
```

### Zorluk 4: Dosya İzin Sorunları
**Problem:** CSS ve JS dosyaları yüklenmiyor, 403 Forbidden hataları.

**Çözüm:**
```bash
# Tüm dosyalara doğru izinler
sudo chmod 644 /var/www/html/*.html
sudo chmod 644 /var/www/html/*.css
sudo chmod 644 /var/www/html/*.js

# Klasör izinleri
sudo chmod 755 /var/www/html

# Owner düzeltme
sudo chown -R www-data:www-data /var/www/html
```

---

## 7. Öğrenilen Dersler ve Olası İyileştirmeler

### Öğrenilen Dersler

#### Teknik Kazanımlar:
1. **Bulut Platformu Yönetimi:**
   - GCP Console navigasyonu ve VM yönetimi
   - Compute Engine kaynak kullanımı ve optimizasyonu
   - Free Tier limitlerini anlama ve kullanma

2. **Linux Sistem Yönetimi:**
   - Debian/Ubuntu sistemlerde paket yönetimi (apt)
   - systemctl ile servis yönetimi
   - Dosya izinleri ve ownership kavramları (chmod, chown)
   - SSH kullanımı ve güvenli bağlantı

3. **Web Sunucu Yapılandırması:**
   - Apache2 kurulumu ve konfigürasyonu
   - Virtual hosts kavramı
   - Web root directory yapısı
   - Log dosyalarını inceleme (`/var/log/apache2/`)

4. **Ağ ve Güvenlik:**
   - Firewall kuralları oluşturma ve yönetme
   - Port yönlendirme (80, 443)
   - Public IP vs Private IP kavramları
   - Network tags ve security groups

#### Kişisel Gelişim:
- Problem çözme becerileri (troubleshooting)
- Dokümantasyon okuma ve uygulama
- Command line becerilerinde artış
- Sabır ve sistemli yaklaşım

### Olası İyileştirmeler

#### Kısa Vadeli İyileştirmeler:
1. **HTTPS Desteği:**
```bash
# Let's Encrypt ile ücretsiz SSL
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d yourdomain.com
```

2. **Custom Domain:**
```bash
# Domain satın al (örn: Cloudflare, Namecheap)
# DNS A Record ekle: yourdomain.com -> [EXTERNAL-IP]
# Apache virtual host yapılandırması
```

3. **Monitoring ve Logging:**
```bash
# Cloud Logging aktifleştirme
# Uptime checks oluşturma
# Alert policies tanımlama
```

#### Uzun Vadeli İyileştirmeler:
1. **Backend Entegrasyonu:**
   - Node.js/Express API
   - PostgreSQL/MongoDB database
   - User authentication (JWT)
   - RESTful API design

2. **Cloud-Native Yapı:**
   - Kubernetes deployment
   - Microservices architecture
   - Cloud Functions/Cloud Run kullanımı
   - Serverless yaklaşım

3. **Advanced Features:**
   - Progressive Web App (PWA)
   - Real-time sync (WebSockets)
   - Multi-user collaboration
   - Mobile app (React Native)

---

## 8. Sunum ve İletişim

### 📹 Video Sunumu
**YouTube Sunum Videosu:** [BURAYA VİDEO LİNKİNİZİ YAPIŞTIRIN]

**Video İçeriği:**
- Proje tanıtımı
- Mimari açıklama
- Canlı deployment gösterimi
- Uygulama demo
- Sonuç ve öğrenilenler

### 🌐 Canlı Uygulama
**Uygulama Canlı URL:** `http://34.42.252.215/`


### 📊 Proje Kaynakları
**GitHub Repository:** `https://github.com/erenbasaraNN/taskflow-bulut-bilisim`

**Proje Dokümantasyonu:**
- ODEV_RAPORU.md - Bu rapor

### 📈 Proje Metrikleri
- **Kod Satırı:** ~1000 satır
- **Dosya Boyutu:** ~23KB (sıkıştırılmamış)
- **Deployment Süresi:** ~15 dakika
- **Sayfa Yükleme:** <500ms
- **Uptime:** %99.9 (GCP SLA)

### 🎯 Test Bilgileri
**Test Edilen Platformlar:**
- Chrome (Windows, macOS, Linux)
- Firefox
- Safari (macOS, iOS)
- Edge
- Mobile browsers (Android, iOS)

**Test Senaryoları:**
- ✅ Görev ekleme/silme/tamamlama
- ✅ Filtreleme işlemleri
- ✅ LocalStorage kalıcılığı
- ✅ Responsive davranış
- ✅ Cross-browser uyumluluk

---

## 9. Sonuç ve Değerlendirme

### Proje Başarıları
✅ Modern ve kullanıcı dostu bir web uygulaması geliştirildi  
✅ GCP platformunda başarılı deployment gerçekleştirildi  
✅ Sanal makine ve web sunucu yönetimi deneyimi kazanıldı  
✅ Bulut bilişim kavramları pratik olarak öğrenildi  
✅ DevOps süreçleri hakkında fikir edinildi  

### Teknik Öğrenimler
- IaaS modeli ve kullanım senaryoları
- Linux sistem yönetimi becerileri
- Web sunucu yapılandırması (Apache)
- Ağ güvenliği ve firewall yönetimi
- Cloud platform best practices

### Kişisel Gelişim
- Problem çözme ve troubleshooting becerileri
- Dokümantasyon ve raporlama yetkinliği
- Bulut teknolojilerine hakimiyet


---

## 10. Kaynaklar ve Referanslar

### Resmi Dokümantasyonlar
- [Google Cloud Documentation](https://cloud.google.com/docs)
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)

### Kullanılan Araçlar
- **IDE:** Visual Studio Code / AntiGravity
- **Version Control:** GitHub
- **Cloud Platform:** Google Cloud Platform
- **Terminal:** Windows PowerShell, GCP SSH
- **Browser DevTools:** Chrome DevTools
- **Design:** Google Fonts, CSS Gradients

### İlgili Teknolojiler
- HTML5, CSS3, JavaScript ES6+
- Apache2 Web Server
- Debian GNU/Linux
- Git & GitHub
- Shell Scripting (Bash)

---

**Not:** Proje değerlendirmesi bittikten sonra ilgili bulut kaynakları (VM instance) maliyetten kaçınmak için silinecektir.

---

