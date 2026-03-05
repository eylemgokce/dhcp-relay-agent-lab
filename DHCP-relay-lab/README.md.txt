# DHCP Relay Agent Yapılandırması (Cisco Packet Tracer)

Bu proje **Cisco Packet Tracer ortamında DHCP Relay Agent yapılandırmasının nasıl çalıştığını göstermek amacıyla hazırlanmıştır.**
Farklı ağlarda bulunan istemcilerin başka bir ağda bulunan DHCP sunucusundan IP alabilmesi için **DHCP Relay Agent (ip helper-address)** kullanılmıştır.

 

![DHCP Relay Topoloji](topology.png)

  

## 📌 Topoloji

Projede iki farklı ağ bulunmaktadır:

* **192.168.1.0/24 → DHCP Server'ın bulunduğu ağ**
* **192.168.2.0/24 → DHCP istemcilerinin bulunduğu ağ**

Router, iki ağ arasında yönlendirme yaparken DHCP isteklerini **DHCP sunucuya ileten relay agent** olarak çalışır.

## 🖥️ Ağ Diyagramı

* DHCP Server → `192.168.1.100`
* Router G0/0 → `192.168.1.1`
* Router G0/1 → `192.168.2.1`
* PC0, PC1 → DHCP Server ile aynı ağda
* PC2, PC3 → Farklı ağda (DHCP Relay kullanarak IP alır)

## ⚙️ Kullanılan Cihazlar

* 1 × Cisco 1941 Router
* 2 × Cisco 2960 Switch
* 1 × Server (DHCP Server)
* 4 × PC (Client)

## 🧩 DHCP Server Yapılandırması

Server üzerinde iki farklı DHCP havuzu oluşturulmuştur.

### Pool 1

```
Network: 192.168.1.0
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

### Pool 2

```
Network: 192.168.2.0
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

## 🔧 Router Yapılandırması

Router üzerinde iki interface yapılandırılmıştır.

### Interface Yapılandırması

```
enable
configure terminal

interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface g0/1
ip address 192.168.2.1 255.255.255.0
ip helper-address 192.168.1.100
no shutdown
```

### DHCP Relay Komutu

```
ip helper-address 192.168.1.100
```

Bu komut sayesinde **192.168.2.0 ağındaki DHCP broadcast istekleri unicast olarak DHCP sunucuya iletilir.**

## 🔍 Test

Test işlemleri:

1. PC2 ve PC3 cihazlarında IP Configuration → **DHCP seçilir**
2. İstemcilerin **192.168.2.x** ağından IP aldığı gözlemlenir
3. `ping 192.168.1.100` komutu ile DHCP server erişimi test edilir

## 📚 Öğrenilen Konular

* DHCP çalışma mantığı
* Broadcast ve Unicast kavramı
* DHCP Relay Agent
* `ip helper-address` kullanımı
* Router üzerinden DHCP yönlendirme

## 📂 Kullanılan Araç

* Cisco Packet Tracer

## 👨‍💻 Amaç

Bu proje aşağıdaki konuları öğrenmek için hazırlanmıştır:

* Farklı ağlar arasında DHCP dağıtımı
* Router üzerinden DHCP isteklerini yönlendirme
* Gerçek ağ senaryolarına benzer lab ortamı oluşturma

---



