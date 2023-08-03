# VinuChain

> Uzun bir seriven başlasın

> Arkadaşlar acelemiz yok adım adım gidin, ufak bir hatada çıkmaz bir yola girmiş olunabiliyor acele yok.

```
sudo apt-get update && sudo apt-get upgrade -y

sudo apt-get install ufw

sudo ufw enable

sudo ufw allow 22
sudo ufw allow 5050
sudo ufw allow 4000
sudo ufw allow 3000
```

```
# rues yazan yere bir kullanıcı ismi verin.
USER=rues

sudo mkdir -p /home/$USER/.ssh
sudo touch /home/$USER/.ssh/authorized_keys
sudo useradd -d /home/$USER $USER
sudo usermod -aG sudo $USER
sudo chown -R $USER:$USER /home/$USER/
sudo chmod 700 /home/$USER/.ssh
sudo chmod 644 /home/$USER/.ssh/authorized_keys
```

```
# şire isteyecek, belirleyin ve bu şifreleri unutmayın kaydedin
sudo nano /etc/sudoers

# Fotoğrafta gösterdiğim gibi ekleyiniz.
kullanıcıAdınız ALL=(ALL:ALL) ALL
# CTRL + X + Y + ENTER ile kaydedip çıkıyoruz.
```

![image](https://github.com/ruesandora/VinuChain/assets/101149671/0119282a-d7bb-4b43-bae7-824ee5c2be11)

```
# bu paketi yükleyelim:
sudo apt-get install -y build-essential

# go'yu yükleyelim:
wget https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
sudo tar -xvf go1.19.3.linux-amd64.tar.gz
sudo mv go /usr/local
```

```
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

```
# Opera'yı ve VinuChain'i yükleyelim:
git clone https://github.com/VinuChain/VinuChain
cd VinuChain/
make

# version kontrol; 1.1.2-rc.3
./build/opera version
```

```
cd
cd VinuChain
cd build/

wget https://tinyurl.com/genesis-testnet

# Değişmesi gereken bir şey yok.
nohup ./opera --port 3000 --nat any --genesis genesis-testnet --genesis.allowExperimental --bootnodes enode://3c4da2358ce3c3e117b03e4c87dff1d8d767a684e3c94f5eb29a4e88f549ba2f5a458eab60df637417411bb59b52f94542cf7d22f0dd1a10e45d5ae71c66e334@54.203.151.219:3000 > opera.log &

# yukarıda ki komuttan sonra "nohup: ignoring input.." çıktısı verecek, enter diyoruz.
tail -f opera.log
# bu komutla loglar akacak daha sonrasında.
# sync olmak 4-5 dk sürer bekleyin, explorerdan bakarsınız blok sayısına.
```

```
# İki key oluşturuyoruz, biri public adres diğeri public key.
# İsmen aynı olabilir lakin farklılıklarını anlatacağım.

# Burada oluşturulan cüzdan biligisi ve secret key file'ı kaydedelim.
# Bu public adresdir.
./opera account new
# account new ile oluşan cüzdana discorddan test tokeni isteyelim veya faucet. 100k token.

# Bu çıktıda ki bilgileri de kaydedelim.
# Bu public keydir.
./opera validator new

# bu çıktılarda ki bilgileri ve şifreyi kaybederseniz çözümü yok asla.
```

```
# şimdi bir javascript konsolu oluşturacağız
./opera attach
```

> Açılan > konsoluna [buradaki](https://github.com/ruesandora/VinuChain/blob/main/SFC_JSON.parse) değerleri girip enterlayın.

```
# " " arasında public adresimizi girelim:
sfcc = web3.ftm.contract(abi).at("0xfc00face00000000000000000000000000000000")
```

```
# bu komutta sıfır dışında bir çıktı almalısınız (aktif validatör sayısı)
sfcc.lastValidatorID() 






