# Doğrulama ve Mevcut Sınırlar

## Doğrulanmış Teknik Kanıtlar

Private repository'nin güncel geliştirme dalında aşağıdaki bileşenler uygulanmıştır:

- Source SQL instance validation
- User database discovery ve system database filtering
- Tekil ve batch backup orchestration
- Manifest, ZIP package ve migration handoff üretimi
- SQL Server silent setup runner
- Installer job/step tracking ve fail-fast orchestration
- Post-install instance ve SQL authentication connectivity kontrolü
- WinForms source, installer ve restore ekranları
- Zip-Slip korumalı package extraction
- Strongly typed manifest reader
- Transfer code ve handoff validation
- Manifest/handoff JobId consistency kontrolü
- `RESTORE FILELISTONLY` tabanlı logical file discovery
- `WITH MOVE` ve çoklu data/log dosyası desteği
- Mevcut veritabanında kontrollü `REPLACE`
- Database bazlı restore sonuçları, cancellation ve genel summary

Commit geçmişi source pipeline'dan UI ve gerçek restore akışına kadar özelliklerin ayrı issue/task'lerle geliştirildiğini gösterir.

## Manuel ve Smoke Doğrulama

Kod geçmişinde source migration ve target installer akışlarının console/smoke senaryolarıyla çalıştırıldığı; SQL kurulum pipeline'ının test named instance üzerinde doğrulandığı kayıtlıdır.

Bu kanıtlar bir otomatik regresyon paketiyle eşdeğer değildir. Public vaka çalışması bu nedenle herhangi bir test sayısı, coverage yüzdesi veya production-ready garantisi sunmaz.

## Otomatik Test Durumu

Solution içinde xUnit unit ve integration test proje iskeletleri vardır; ancak güncel dalda gerçek test case dosyaları bulunmamaktadır.

Öncelikli test backlog'u:

- Domain state transition unit testleri
- Manifest/handoff validation testleri
- Zip-Slip ve extraction boundary testleri
- Source flow orchestration testleri
- Installer fail-fast senaryoları
- SQL command/path generation testleri
- Restore partial-success ve cancellation testleri
- Gerçek SQL Server ile integration testleri

## Güvenlik ve Operasyonel Sınırlar

| Alan | Mevcut durum | Güçlendirme ihtiyacı |
|---|---|---|
| Credential yönetimi | Config tabanlı | Secret store, kullanıcı girişi veya Windows korumalı saklama |
| Package integrity | Manifest/handoff tutarlılığı | Cryptographic hash ve/veya package signature |
| Transfer | Operatör kontrollü fiziksel aktarım | İsteğe bağlı güvenli transfer kanalı ve audit |
| Restore overwrite | Mevcut DB için `REPLACE` destekleniyor | Açık confirmation, dry-run ve pre-restore snapshot politikası |
| Multi-DB atomicity | DB bazlı sonuç ve kısmi başarı | Rollback/compensation runbook'u |
| SQL setup media | Önceden sağlanan local path | Onaylı media doğrulaması ve hash kontrolü |
| Release | MVP / private source | Versioned artifact, signed installer ve release checklist |
| Telemetry | Yerel structured logs | Sanitization, retention ve merkezi gözlemlenebilirlik |

## Ürün Sınırı

Mevcut MVP şu özellikleri varmış gibi sunmaz:

- otomatik bulut veya ağ üzerinden paket transferi,
- zero-touch müşteri migration'ı,
- dağıtık job scheduler,
- otomatik rollback,
- cryptographically signed migration packages,
- merkezi audit/telemetry platformu,
- tamamlanmış otomatik regresyon paketi,
- genel kullanıma açık production release.

Bu sınırlar teknik borcu saklamak için değil, uygulanan mühendisliği gelecek plandan ayırmak için görünür tutulmuştur.
