# LinkedIn & Indeed Job Application Automation

Bu proje, LinkedIn ve Indeed üzerinden toplanan iş ilanlarını **otomatik filtreleyen**, **özelleştirilmiş CV ve Cover Letter üreten** ve tüm sonuçları **Google Sheets’e kaydeden** bir otomasyon workflow’udur.  

<img width="1365" height="676" alt="Screenshot 2025-09-20 232658" src="https://github.com/user-attachments/assets/483f8325-6534-43c6-b783-5ef8aaf766ef" />

## 🚀 Özellikler

- **Job Scraping**  
  - LinkedIn (Apify) ve Indeed (Apify) üzerinden iş ilanları çekilir.  
  - Her iş ilanı detaylı şekilde alınır: şirket adı, iş tanımı, ilan linki, applyUrl, lokasyon, seniorityLevel.  

- **Relevance Filtering**  
  - GPT modeli, iş ilanlarını kullanıcının CV’siyle karşılaştırır.  
  - İstanbul’a öncelik verilir.  
  - “Engelli/Disability” vb. ilanlar otomatik elenir.  
  - Her ilana uygunluk skoru (`fitScore`) verilir.  

- **HR Email Enrichment**  
  - Hunter.io API ile şirketin HR veya genel iletişim e-mail adresi bulunur.  
  - En yüksek güven puanına (confidence score) sahip e-mail seçilir.  

- **Document Generation**  
  - Kullanıcının CV’si iş ilanına göre GPT tarafından optimize edilir.  
  - İşe özel Cover Letter üretilir.  
  - Tüm belgeler Google Docs üzerinde saklanır.  

- **Google Sheets Logging**  
  - Tüm iş ilanları ve başvuru detayları tek bir tabloda tutulur:  
    - Şirket adı  
    - İş unvanı  
    - Lokasyon / ülke  
    - Job link / apply URL  
    - HR email adresi  
    - Cover Letter linki  
    - Resume linki  
    - Fit Score  
    - Gönderilecek mail içeriği  

## 🛠 Kullanılan Servisler & API’ler

- **Apify** → LinkedIn & Indeed scraping  
- **OpenAI GPT** → CV optimizasyonu, Cover Letter & mail içeriği  
- **Hunter.io** → HR e-mail adresi bulma  
- **Google Docs API** → CV & Cover Letter saklama  
- **Google Sheets API** → Başvuruları loglama  

## 📊 Workflow Mantığı

1. **İş ilanları toplanır** (Apify → LinkedIn & Indeed).  
2. **GPT ile relevance check** → İlan adayın CV’siyle eşleştirilir.  
   - Uygun değilse → elenir.  
   - Uygunsa → işlenmeye devam edilir.  
3. **HR e-mail adresi bulunur** (Hunter.io).  
4. **CV & Cover Letter hazırlanır** (Google Docs).  
5. **Sonuçlar Google Sheets’e yazılır** → tüm süreç tek tablo üzerinden takip edilebilir.  

## 📂 Örnek Google Sheet Kolonları

| Index | Country | Company | Job Title | Fit Score | HR Email | Apply URL | CV Link | Cover Letter Link | Message |
|-------|---------|---------|-----------|-----------|----------|-----------|---------|------------------|---------|

## ⚙️ Kurulum

1. **n8n Kurulumu**  
   - Projeyi n8n içerisine import edin (`.json` workflow dosyası).  

2. **Google Cloud Console**  
   - Sheets API + Docs API + Drive API enable edin.  
   - OAuth2 credentials oluşturun.  
   - n8n’de Google Sheets & Google Docs için credential tanımlayın.  

3. **Apify**  
   - Apify hesabınızdan token alın.  
   - Workflow’ta ilgili node’un içine ekleyin.  

4. **Hunter.io**  
   - API key alın.  
   - Workflow’ta “Find HR Email” node’una ekleyin.  

5. **OpenAI**  
   - API key alın.  
   - n8n’de GPT node’larına ekleyin.  

## 📌 Notlar

- HR email bulunamazsa “Not Found” olarak kaydedilir.  
- Aynı applyUrl’e sahip duplicate iş ilanları workflow tarafından elenir.  
- Workflow, otomatik mail göndermez — sadece **tüm başvuru verilerini Sheet’e yazar.**  
