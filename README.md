# 1-Sunucuya-2-Node-Kurmak-Cosmos

### Cosmos ağında 2 adet validatörünüz var ve ikisini de aynı sunucu da çalıştırma istiyorsunuz... 
## Ne yapmalısınız?

Yapmanız gereken tek şey portların çakışmasını engellemek. 
Bunun için `config.toml`, `client.toml`, `app.toml` dosyalarının içindeki portların hepsini değiştirmeniz gerekiyor...

Fakat bunun daha kolay bir yolu var: 

### Terminale aşağıdaki kodları düzenleyerek yazın: 
** Aşağıdaki örnek kyve ye aittir. siz düzenlemek istediğiniz nodeun bulunduğu klasöre göre aşağıdaki kodlardaki `.kyve` yazılarını düzenleyin. **
Aşağıdaki kodları dikkatlice inceleyin ve mantığını anlamaya çalışın. 
ilk yazan port orjinal porttur, onu değiştirmeyeceğiz. bir sonraki ise değiştirmek istediğimiz porttur. 
yani 26658 orjinal 36358 değiştirmek istediğimiz. Bunu bununla değiştir diyoruz yani aslında aşağıdaki kodları kullanarak. 
Bir orjinal bir değiştir, bir orjinal bir değiştir. 

Yani sizin yapmanız gereken bir değiştirme bir değiştir. 
Kodları incelerseniz anlatmak istediğimi daha iyi anlayacaksınız. 

Şimdi gelelim işlemlere...

Aşağıdaki kodları sırasıyla yazalım:

```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:36358\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:36357\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6351\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:36356\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":36350\"%" $HOME/.kyve/config/config.toml
```

```
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9350\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9351\"%" $HOME/.kyve/config/app.toml
```

```
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:36357\"%" $HOME/.kyve/config/client.toml
```


Tüm işlemler bu kadar! yukarıdaki kodları yazarak 3 dosyanın içindeki tüm portları değiştirdiniz. 


### Özet şekilde anlatmak gerekirse değiştirmemiz gereken portlar şunlar: 

**`Config.toml` klasörü içerisinde:**

proxy_app portu orjinali `26658` dir.

laddr portu orjinali `26657` dir.

pprof_laddr portu orjinali `6060` dır.

Başka bir laddr portu  orjinali `26656` dır

ve prometheus listen portu orjinali `26660` dır.




**`App.toml` klasörü içerisinde:**

address portu orjinali `9090` dır.

ve diğer address portu orjinali `9091` dir.



**`Client.toml` içerisinde:**

Node portu orjinali `26657` dir. 

Yukarıdaki kodlarda bulunan orjinal portları değiştirmeyin! 

Diğer portları kendinize göre düzenleyin. 



*İpucu: Eğer sunucunuz çok güçlü ise sınırsız sayıda cosmos node unu tek sunucuda çalıştırabilirsiniz. Tek yapmanız gereken hiçbirinin portunun diğeriyle çakışmamasını sağlamak.*

*İpucu2: Her node için minimum 100gb disk alanı ayırmanızı öneririm. Diyelim ki 8cpu 16 ram 200gb bir sunucunuz var. 
bu sunucuya maksimum 2 adet node kurun. İlk kurulum sırasında node boyutu gözünüze düşük gelebilir. genellikte ilk kurulumda tüm nodelar maksimum 6-7gb alan kaplarlar. 
Fakat daha sonra senkronizasyon ve stres testleri sırasında sunucuzda mutlaka ekstra alan olmalı. blok sayısı arttıkça kullanılan disk alanının da hızla artacağını unutmayın.*

*İpucu3: Ekstra bir işlem gerektirmeyen ve özel gereksinim istemeyen standart cosmos nodelarına 4cpu 8 ram 150gb lık bir sunucu genellikle yeterlidir. 
Gözlemlerime dayanarak söyleyebileceğim şeyler şunlar: 

Cpu kullanımı: 1 node 2cpu harcıyor.

Ram kullanımı: 1 node 2.5 - 3 gb ram harcıyor. 

Disk kullanımı: 1 node ortalama 1 ay süren testlerde ortalama 70 80gb disk alanı harcıyor. 

tabi bunlar genel analizlerim. her zaman böyle olacak diye bir kural yok. ama ortalama sorunsuz bir cosmos doğrulayıcısının harcadığı gereksinimler bu şekilde... 

Yine de katıldığımız projelerin testnet olduğunu unutmayın. test sırasında hdd lerinize fazla yüklenilebilir. databaseleriniz şişebilir. 
örneğin logları görüntülemek için kod girdiğimizde akan yazıların hep kaydedilir. ve bu da db nin büyümesi sorununu yaratır. onun çözümü yok mu? elbette var:) 
bir sonraki yazımızda pruning (budama) işlemlerinden söz ederiz. şimdilik bu kadar yeter :) *

**Happy Validating!**
