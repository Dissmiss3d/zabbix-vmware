# zabbix-vmware
VMWare ESX-VM izleme

##Güncelleme;

datastore ve os katmanında free space triggerları eklendi

-os katmanında %10Average-%20High free space

![image](https://user-images.githubusercontent.com/85514498/217327146-ed955158-43fd-4fc7-8767-12323237bd46.png)


-datastore için %5-%10 

-ek olarak [totalspace][remainingspace][%100dolacağısüre]

![image](https://user-images.githubusercontent.com/85514498/217328354-c774ed9f-197e-4d9c-815a-c965b9e3f804.png)


![image](https://user-images.githubusercontent.com/85514498/217327339-5c4c021d-eae9-4f7a-b9e0-a4b0c7d8f8f4.png)


vmware cluster status [yellow-red]


hypervisor için memory-cpu-cpu ready yakında eklenecek.
Hedef: Sanallaştırma ortamında bulunan sunucuları OS katmanında otomatik izlemeye almak. Hypervisor sorgularını daha kapsamlı yapmak ve takip etmek. Datastore doluluğunu takip etmek.

Aşağıdaki komut ile zabbix_server.conf dosyamızı düzenleme modunda açıyoruz.

nano /etc/zabbix/zabbix_server.conf

Burada aşağıdaki satırı bulup kaç adet Vmhost eklemeyi düşünüyorsanız buraya o sayıyı giriyoruz ben 1 adet gireceğim için bu değeri 1 yapıyorum.

StartVMwareCollectors=1

Düzenlemeleri yaptıktan sonra "zabbix-server" servisini restart etmeliyiz.

yaml formatında olan templateleri import ettikten sonra hostumuzu oluşturuyoruz;

(http://YOUR-SERVER/zabbix)
Navigate: Configuration -> Hosts -> Create host
![image](https://user-images.githubusercontent.com/85514498/212404126-94135dad-3fc5-4fd6-8cd2-99409e97df2e.png)

Aşağıdaki bilgileri giriyoruz;

Hostname  :Bir isim giriniz

Groups    :Hypervisors

Agent interface : Vmhost ip addres

Template : Vmware,VMware Guest,VMware Hypervisor


Makro Tanımlarının yapılması gerekiyor burada Host için gerekli tanımlamayı yaptık şimdi Macro sekmesine kullandığımız vmhost bilgilerini gireceğiz;

![image](https://user-images.githubusercontent.com/85514498/212404784-9f3d898e-1c56-452c-8817-5f97d73db1bf.png)


{$USERNAME}	root

{$PASSWORD}	rootpassword

{$URL}          https://ESX-IP-ADDRESS-OR-NAME/sdk

Bütün adımlar tamamlandığında otomatik olarak vcenter üzerinde bulunan VM ve Datastore keşiflerini yapacaktır.

![image](https://user-images.githubusercontent.com/85514498/212411037-4962bea4-19ad-4025-864d-f491a6574cca.png)




(VM'leri ve Hypervisorleri otomatik host olarak eklemektedir.
Datastore item olarak oluşmaktadır)


Yakın zamanda yapılacak güncellemeler;

Oluşturulan vm'lere otomatik windows-linux template eklemek,katman olarak ayırmak,servis izlemelerini başlatmak.

#Grafana kullanılan yapılarda telegraf'a ihtiyaç duymadan Zabbix üzerindeki VMWare-ESX metriklerini görselleştirme avantajı doğacak.
