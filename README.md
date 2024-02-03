# VOI Swarm Node Kurulumu

> **Uyarı:**
> Eğer daha önceki node'unuzu swarm kurulum ile değiştirecekseniz 2. aşamadan başlayınız.



## 1. Yeni Node Kurulumu
Aşağıdaki kodu çalıştırın ve adımları takip edin.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/VoiNetwork/voi-swarm/main/install.sh)"
```
Aşağıdaki gibi telemetry name soracak istediğiniz bir ismi yazıp enter tuşuna basın
```
Telemetry name:
```
Cüzdan için bir şifre belirlemenizi isteyecek oluşturmak istediğiniz şifreyi girin ve enter'a basın.
```
Please choose a password for wallet 'voi':
```

Çıkan soruya y'ye basarak onaylayın ve backup keylerini bir yere kaydedin.
```
Would you like to see it now? (Y/n):
```

Daha sonra şu şekilde bir çıktı ile cüzdanınızın public key'ini verecek. Bu cüzdana voi discorddan 1 VOI göndermeniz gerekiyor.
```
****************************************************************************************************************
*    To join the Voi network, do one of these:
*
*    a) Send at least 1 Voi to your account ORGQ2LUZH3LEQ5FN7ZLMB3PU3KP6K3IRP6J3ZZ4B4GEJJSEUONXXYPK6FY from another account
*
*    OR
*
*    b) Get 1 Voi for free:
*       - Go to the Voi Network Discord - https://discord.com/invite/vnFbrJrHeW
*       - Open the #voiager-chat channel
*       - Type /voi-testnet-faucet ORGQ2LUZH3LEQ5FN7ZLMB3PU3KP6K3IRP6J3ZZ4B4GEJJSEUONXXYPK6FY
*
* After you've done this, type 'completed' to go on
****************************************************************************************************************
```

#voiager-chat kanalına girerek şu komut ile 1 VOI isteyin
```
/voi-testnet-faucet <pulic keyiniz>
```

1 VOI geldikten sonra 'completed' yazarak devam edin. Network ile sync etmek için bir süre burada bekliyoruz.
Sonrasında size participation key verecek bunu da bir yere kaydedin bu key belli bir süre için geçerli süresi dolduktan sonra yenilemeniz gerekiyor.


## 2. Eski Node'u Swarm ile Değiştirme
Öncelikle eski node'u offline yapalım.
```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account changeonlinestatus -a $addr -o=0 &&\
sleep 1 &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'
```
'You are offline.' çıktısını alıyorsanız tamamdır.

Sonrasında servisi durdurup disable edelim.
```
sudo systemctl stop voi
sudo systemctl disable voi
```

Daha sonra aşağıdaki komutları çalıştıralım.
```
export VOINETWORK_SKIP_WALLET_SETUP=1
export VOINETWORK_IMPORT_ACCOUNT=1
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/VoiNetwork/voi-swarm/main/install.sh)"
```
