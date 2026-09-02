# Defense-Grade Zero-Trust Network Architecture (Savunma Sanayi Ağ Mimarisi)



## 📌 Proje Özeti (Overview)
Bu proje, yüksek güvenlik gereksinimleri olan savunma ve taktik saha operasyonları için **Cisco Packet Tracer** üzerinde tasarlanmış uçtan uca bir ağ mimarisidir. Sistem; Merkez Karargah, Taktik Saha Üssü ve Otonom Sınır Karakolu olmak üzere 3 izole bölgeden oluşmaktadır. 

Mimari, yanal hareketleri (lateral movement) engellemek amacıyla **Sıfır Güven (Zero-Trust)** prensiplerine göre inşa edilmiş ve dinamik yönlendirme (OSPF), VLAN bölütleme, ve otonom IoT uç noktaları entegrasyonu ile desteklenmiştir.

## 🚀 Temel Teknolojiler ve Protokoller (Tech Stack)
* **Yönlendirme (Routing):** OSPF (Open Shortest Path First) Area 0
* **Güvenlik (Security):** Cisco ASA 5506-X Firewall, Extended Access Control Lists (ACLs)
* **Ağ Bölütleme (Segmentation):** IEEE 802.1Q (VLAN), Router-on-a-Stick (RoaS)
* **Kablosuz Uç (Wireless/Edge):** WLC (Wireless LAN Controller), WPA2-PSK Kimlik Doğrulaması, LAP-PT
* **Alt Ağ Planlaması (Subnetting):** Transit/Omurga bağlantılar için Güvenli `/30` VLSM maskeleme

## 🏗️ Ağ Topolojisi ve Bölgeler (Network Zones)

### 1. Merkez Karargah (HQ / Core Data Center)
* **Bileşenler:** Cisco 2911 Router, Multilayer Switch, ASA 5506-X Firewall, Merkezi Sunucular (IoT, Syslog, AAA).
* **İşlev:** Dış tehditlere karşı ASA Güvenlik Duvarı ile izole edilmiş ana veri merkezi. Ağın yönetim ve depolama kalbidir (`192.168.10.0/24`). Dış ağ bağlantısı (Outside) için `10.99.99.0/30` transit ağı kullanılmıştır.

### 2. Taktik Saha Üssü (Tactical Field Base)
* **Bileşenler:** Cisco 2911 Router, 2x 2960 Switch, IP Telefonlar ve Operasyonel Bilgisayarlar.
* **İşlev:** Personel ve görev açısından kritik donanımların bulunduğu aktif bölge. 
* **Zero-Trust Uygulaması:** Radar Birimi (VLAN 20) ve Personel Birimi (VLAN 30) olarak ikiye ayrılmıştır. Genişletilmiş ACL kullanılarak Personel ağından Radar ağına giden her türlü trafik (ICMP dahil) tamamen bloklanmış, yanal sızma testlerine karşı izole edilmiştir.

### 3. Otonom Sınır Karakolu (Border IoT Outpost)
* **Bileşenler:** WLC, LAP-PT (Access Points), SBC (Single Board Computers), Akıllı Sensörler (Hareket Dedektörü, İleri Hat Web Kameraları) ve Saha Tableti.
* **İşlev:** MAVLink telemetrisi veya ileri hat sensör verilerinin toplanabileceği otonom sınır ağı (`10.20.40.0/24`). WLC üzerinden yayınlanan şifreli (WPA2) `Siber_Sinir_Agi` SSID'si ile IoT cihazları ağa entegre edilmiş ve DHCP havuzu ile dinamik IP yönetimi sağlanmıştır.

## ⚙️ Kurulum ve Simülasyon Senaryoları (How to Run & Test)

Projeyi kendi ortamınızda test etmek için:
1. `sinirkontrolagi.pkt` (Packet Tracer) dosyasını Cisco Packet Tracer (v8.x+) ile açın.
2. Tüm omurga hatlarının (OSPF Convergance) yeşil duruma geçmesi için `Fast Forward Time` (Alt+D) butonuna birkaç kez basın.
3. **Güvenlik (ACL) Testi:** Taktik sahadaki bir Personel PC'sinden (`172.16.30.x`), Radar PC'sine (`172.16.20.x`) `ping` atın. Trafiğin güvenlik duvarı kuralı (SECURE_RADAR) tarafından düşürüldüğünü (Destination Host Unreachable) doğrulayın.
4. **Uçtan Uca İletişim Testi:** Sınır hattındaki otonom bir cihazdan (Örn: Tablet PC), Merkez Karargah'taki `192.168.10.x` IP'li IoT sunucusuna ulaşılabildiğini test edin.
5. **WLC İncelemesi:** Sınır ağına bağlı herhangi bir cihazın web tarayıcısından `10.20.40.2` arayüzüne giderek WLC kablosuz kontrolcü ayarlarını inceleyebilirsiniz.

## 🛡️ Proje Kazanımları
Bu proje, endüstri standartlarında savunma/siber güvenlik altyapısı tasarlama, IP çakışmalarını yönetme, cihaz yapılandırma hatalarını (troubleshooting) CLI üzerinden çözme ve güvenli IoT-Edge entegrasyonu sağlama yetkinliklerini kanıtlamak amacıyla geliştirilmiştir.