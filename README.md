<h1 align="center"> VinuChain </h1>

> VinuChain ile bir süredir ilgileniyorum, bir kaç gündür de validatörünü çalıştırıyorum.

> Ödül konusuna aldığım cevap; `Validatörünüz ne kadar çalışırsa o kadar testnet ödülü`, siz yinede sorun isterseniz.

> Ne kadar sürecek sorusuna aldığım cevap `1.5 ay`, tahmini maliyetim: `Hetznerde $6.`

> `Lütfen kod bloklarında ve dışında yazdığım her şeyi okuyunuz, nedenini çok iyi anlayacaksınız.`

> Topluluk kanalları: [Duyurular](https://t.me/RuesAnnouncement) - [Sorularınız İçin](https://t.me/RuesChat) - [VinuChain Discord](https://discord.gg/vinu) - [Explorer](https://vinuscan.com/)

<h1 align="center"> Donanım </h1>

> Sunucu olarak [Hetzner Kullanıyorum](https://github.com/ruesandora/Hetzner/blob/main/README.md)

```
# Benim kullandığım: 
2 CPU - 4 RAM - 80 SSD

# Dökümasyon önerisi:
4 CPU - 16 RAM - 500 SSD
```

> Benim sunucumda kullanım görselde ki gibi:

![image](https://github.com/ruesandora/VinuChain/assets/101149671/4ccb5329-e04a-4552-ae54-b391703d2f6b)


<h1 align="center"> Sistem güncellemelerimiz </h1>

```sh
# Sistem güncellemeleri
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install ufw

# Yes diyebilirsiz portlarınız açık değilse
sudo ufw enable

sudo ufw allow 22
sudo ufw allow 5050
sudo ufw allow 4000
sudo ufw allow 3000
```

<h1 align="center"> Değişkenler </h1>

```sh
# rues yazan yere bir kullanıcı ismi verin
USER=rues

sudo mkdir -p /home/$USER/.ssh
sudo touch /home/$USER/.ssh/authorized_keys
sudo useradd -d /home/$USER $USER
sudo usermod -aG sudo $USER
sudo chown -R $USER:$USER /home/$USER/
sudo chmod 700 /home/$USER/.ssh
sudo chmod 644 /home/$USER/.ssh/authorized_keys

# Giriş yapınız bu klasöre:
sudo nano /etc/sudoers

# Fotoğrafta gösterdiğim gibi ekleyiniz. (Kullanıcı adını unutmayın)
kullanıcıİsminiz ALL=(ALL:ALL) ALL
# CTRL + X + Y + ENTER ile kaydedip çıkıyoruz.
```

![image](https://github.com/ruesandora/VinuChain/assets/101149671/de311c2d-6ab1-4b03-8075-df4861a3a0c2)

<h1 align="center"> Go ve bazı kütüphaneler </h1>

```sh
# bu paketi yükleyelim:
sudo apt-get install -y build-essential

# go'yu yükleyelim:
wget https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
sudo tar -xvf go1.19.3.linux-amd64.tar.gz
sudo mv go /usr/local

# nano ile dosyanın içersine girelim:
nano ~/.bash_aliases

# Bu değerleri dosyanın içine yapıştırın ve kaydedip çıkın.
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

# source edelim ve go version
source ~/.bash_aliases
go version
```

<h1 align="center"> VinuChain ve Opera </h1>

```sh
# Opera'yı ve VinuChain'i yükleyelim:
git clone https://github.com/VinuChain/VinuChain
cd VinuChain/
make

# version kontrol; 1.1.2-rc.3
./build/opera version

# doğru dizine gidelim:
cd
cd VinuChain
cd build/

# Genesisi yükleyelim
wget https://tinyurl.com/genesis-testnet

# Burada opera log ile node'u başlatıyoruz, değişmesi gereken bir yer yok.
nohup ./opera --port 3000 --nat any --genesis genesis-testnet --genesis.allowExperimental --bootnodes enode://3c4da2358ce3c3e117b03e4c87dff1d8d767a684e3c94f5eb29a4e88f549ba2f5a458eab60df637417411bb59b52f94542cf7d22f0dd1a10e45d5ae71c66e334@54.203.151.219:3000 > opera.log &

# yukarıda ki komuttan sonra "nohup: ignoring input.." çıktısı verecek, enter diyoruz.
tail -f opera.log
# Bu komuttan sonra loglar hızlıca akacak ve 2-3 dkya sync olacaksınız, explorerdan bakarsınız blok sayısına.

# Explorer gibi bir çok temel linkler vs. reponun sonunda veya başında bulundururum.
```

<h1 align="center"> Dikkat ediilmesi gereken noktalar </h1>

> DİKKAT: Sırada ki işlemlerde, `iki key` oluşturuyoruz, biri `public adres` diğeri `public key`.

> DİKKAT: İsmen aynı olabilir lakin farklılıklarını anlatacağım. Zaten birisi `kısa` birisi `uzun` olacak.

> DİKKAT: Bu komutlar sonrasında winscp ile `/root/.opera/keystore/` içersinde ki `UTC` dosyasını ve `validatör` klasörünü saklayın!

```sh
# Bu public adresdir. Kısa olandır.
./opera account new
# account new ile oluşan cüzdana discorddan test tokeni isteyelim. 100k token. 1 tanede fee için olsun. 100.001

# Bu çıktıda ki bilgileri de kaydedelim.
./opera validator new
# Bu public keydir. Uzun olandır.

# bu çıktılarda ki bilgileri ve şifreyi kaybederseniz çözümü yok asla, yeni validator kurmanız gerekir.
```

<h1 align="center"> İşlemlerimize JavaScript ile devam edeceğiz </h1>

```sh
# şimdi bir javascript konsolu oluşturacağız
./opera attach
```

> DİKKAT: Sunucunuzda Açılan ">" konsoluna [buradaki](https://github.com/ruesandora/VinuChain/blob/main/SFC_JSON.parse) değişkenleri girip enterlayın.

> Daha basit bir ifade ile; [buradaki](https://github.com/ruesandora/VinuChain/blob/main/SFC_JSON.parse) linkten komutu kopyalayıp yapıştırın.

```sh
# Bir yeri düzenlemenize gerek yok, abi kontratını başlatalım:
sfcc = web3.ftm.contract(abi).at("0xfc00face00000000000000000000000000000000")

# bu komutta sıfır dışında bir çıktı almalısınız (aktif validatör sayısı)
sfcc.lastValidatorID() 

# Buradan validator ID'ınıze bakın:
sfcc.getValidatorID("KısaCüzdanAdresi")
# Çıktıda 0 çıkması gerekiyor çünkü henüz validatör oluşturmadık, 0 dışında bir şey varsa hata.

# validatör cüzdanımızı unlock ediyoruz:
personal.unlockAccount("KısaCüzdanAdresi", "Şifre", 300)
# Burada ki şifreyi zaten yukarıda belirtmiştik, bu komutun çıktısı TRUE olacaktır.

# Şimdi register edelim validatörümüzü:
tx = sfcc.createValidator("UzunCüzdanAdresi", {from:"kısaCüzdanAdresi", value: web3.toWei("100000.0", "ftm")})
# Eğer token yoksa çalışmaz, sorun yoksa yeşil yanan bir TX çıktısı alırsınız.

# TX'i kontrol edelim bu komutla, en sonda status: 0x1 çıktısı vermeli.
ftm.getTransactionReceipt("txAdresi")
# Veya explorerdan aratabilirsiniz tx'inizi.

# Yukarıda validator ID'ınıze bakmıştık 0'dı çünkü oluşturmamıştık, şimdi ise:
sfcc.getValidatorID("kısaCüzdanAdresi")
# Bir sayı çıktısı almalısınız , çıktı neyse mesela 15, 15. validatörsünüz ve bu ID'yi saklayın not edin!
```

<h1 align="center"> Node işlemleri tamam şimdi Validatörde </h1>

```sh
# Operaya kill çekelim bu olmak zorunda:
pkill opera

# Yukarıda ki şifremizi password.txt dosyasına girip kaydedelim.
nano password.txt
# Bu klasörün içine şifrenizi yazın ve CTRL X Y ENTER ile çıkın kaydedip.
```

```sh
# Son adım:
nohup ./opera --bootnodes enode://3c4da2358ce3c3e117b03e4c87dff1d8d767a684e3c94f5eb29a4e88f549ba2f5a458eab60df637417411bb59b52f94542cf7d22f0dd1a10e45d5ae71c66e334@54.203.151.219:3000 --validator.id <ValidatorIDniz> --validator.pubkey <UzunAdres> --validator.password password.txt > validator.log &

# Bu komutta iki şey değişiyoruz, ValidatorID ve UzunAdresi, lütfen içine yazıp < > işaretlerini kaldırın.

# Logu kontrol edelim validatör için:
tail -f validator.log
# Akıyorsa no problemo
```

> Validatorunuzu [buradan](https://vinuscan.com/staking) kontrol edin (ID'si ile) downtime 0 ise sorun yok, artıyorsa node'u kontrol etmek gerekir.

<h1 align="center"> Konu dışı </h1>

> VinuChain bir meme token değil merak etmeyin. Ahım şahım bir proje değil ama maliyeti düşük ve çalıştıracağım ben.

> Madem buraya kadar okudunuz, elimde 5 milyon VC (VinuChain) token var, rueschatten yazan `ilk 50 kişiye 100k` veriyorum.

>  Topluluk kanalları: [Duyurular](https://t.me/RuesAnnouncement) - [Sorularınız İçin](https://t.me/RuesChat) - [VinuChain Discord](https://discord.gg/vinu) - [Explorer](https://vinuscan.com/)

> Daha ekleyecek yorumlarım vb. şeyler olabilir onun için yukarıdan yıldızlayın takibinizde olsun, eksik görürseniz PR atabilirsin, si yu!









