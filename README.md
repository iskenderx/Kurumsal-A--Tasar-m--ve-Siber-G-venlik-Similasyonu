# 🌐 Cisco Packet Tracer ile Kurumsal Kampüs Ağı Tasarımı ve Ağ Güvenliği Projesi

Bu proje, orta ölçekli bir kurumsal şirketin ağ altyapısının sıfırdan tasarlanması, ağ servislerinin (DHCP/DNS/HTTP) merkezileştirilmesi ve **Katman 2 ile Katman 3 düzeyinde siber güvenlik politikalarının (ACL)** uygulanmasını içeren uçtan uca bir ağ simülasyonudur.

## 🛠️ Proje Mimarisi ve Teknik Özellikler

Proje, şirketin iş akışını güvenceye almak ve departmanlar arası yetkisiz erişimleri engellemek adına 3 ana departman ve 1 DMZ/Sunucu alanı olarak segmentlere ayrılmıştır:

*   **VLAN 10 (Yonetim):** `192.168.10.0/24` - Şirket yönetim birimi bilgisayarları.
*   **VLAN 20 (Muhasebe):** `192.168.20.0/24` - Finansal verilerin işlendiği hassas departman.
*   **VLAN 30 (Bilgi-Islem & Sunucu):** `192.168.30.0/24` - BT personeli ve şirket içi servisler (Web/DNS).

## 🚀 Uygulanan Teknolojiler ve Konfigürasyonlar

1. **Katman 2 Güvenliği ve Segmentasyon (Switching):**
   * `802.1Q` protokolü kullanılarak **VLAN Trunking** mimarisi kuruldu.
   * **Katman 2 Güvenliği (Hardening):** Siber güvenlik standartları gereği, switch üzerinde kullanılmayan tüm boş portlar (`Fa0/7 - 24`) `shutdown` edilerek fiziksel sızma girişimleri engellendi. Sunucu bağlantısı ve BT bilgisayarları güvenli portlara izole edildi.

2. **Gelişmiş Yönlendirme (Routing):**
   * Tek bir fiziksel hat üzerinden çoklu VLAN trafiğini yönetmek amacıyla Cisco Router üzerinde **Router-on-a-Stick** (Sub-interfaces) yapılandırması tamamlandı.

3. **Merkezi Ağ Servisleri (Network Services):**
   * **Static & DHCP Sunucu Yönetimi:** Şirket içi Web ve DNS sunucusu `192.168.30.100` IP adresiyle static olarak yapılandırıldı. Router üzerinde her VLAN için dinamik IP havuzları tanımlandı ve sunucu IP'si `excluded-address` ile çakışmalara karşı korundu.
   * **DNS & HTTP Servisleri:** Sunucu üzerinde HTTP ve DNS servisleri aktif edilerek yerel ağda çalışanlar için web portalı simüle edildi.

4. **Siber Güvenlik Politikaları: Genişletilmiş ACL (Extended Access Control List):**
   * **Zero Trust (Sıfır Güven) Yaklaşımı:** Hassas veri güvenliğini sağlamak amacıyla Router üzerinde `Extended ACL` yazıldı. 
   * **Kısıtlama:** Muhasebe departmanının (`VLAN 20`), Yönetim departmanına (`VLAN 10`) erişimi (Ping, ICMP, IP trafiği) ağ seviyesinde tamamen bloklandı. Diğer departmanların iş süreçleri için internet ve sunucu erişimleri açık bırakıldı.

## 🧪 Test ve Doğrulama
* `show vlan brief` ile VLAN atamaları doğrulandı.
* Bilgisayarların DHCP üzerinden başarılı şekilde IP aldığı gözlemlendi.
* Muhasebe departmanından Yönetim departmanına giden isteklerin ACL kuralları tarafından engellendiği (`Destination host unreachable`) doğrulandı.
* BT bilgisayarları üzerinden Web sunucusuna sorunsuz erişim sağlandı.
