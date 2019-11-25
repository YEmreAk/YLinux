---
description: Linux üzerinde can sıkan kernel güncelleme olayı
---

# 💎 Kernel Güncelleme

## 🗽 Açıklama

Temel olarak 3 farklı yöntem ile kernel güncelleyebilirsin. Alttakilerden **sadece birini** kullanman yeterlidir.

* Grafik arayüzle basit kurulum için **🛠 Ubuntu Kernel Update Utility ile Kernel Güncelleme** aşamasına bakmalısın
* Detayları merak etmiyorsan **🤸‍ Komutlarla Hızlı Kurulum** alanındaki yapman yeterlidir
* Detayları merak ediyorsan **⤵ Güncel Kernel Dosyasının İndirilmesi** alanından başlamalısın

> Bu yazı bir alıntı \(türkçeleştirme\) yazısıdır, orjinal halini görmek için [buraya](https://www.cyberciti.biz/tips/compiling-linux-kernel-26.html) tıklayabilirsin.

## 🌞 Ubuntu Kernel Update Utility ile Kernel Güncelleme

```bash
sudo apt-add-repository ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install ukuu
sudo ukuu-gtk
```

## 🤸‍ Komutlarla Hızlı Kurulum

Detayları merak etmeyenler için hızlı kurulum 🏃‍

### ⚡ Kısa İşlemli Komutlar

Alttaki komutları direkt olarak kopyalayabilirsin.

```bash
VERSION=5.3.2
wget -O linux-VERSION.tar.xz https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-$VERSION.tar.xz
xz -d -v linux-VERSION.tar.xz
tar xvf linux-VERSION.tar
cd linux-VERSION
cp -v /boot/config-$(uname -r) .config
sudo apt-get install -y build-essential libncurses-dev bison flex libssl-dev libelf-dev
```

### ⏲ Uzun Süren Komutlar

Bu kısımdaki komutları satır satır kopyalamalısın.

> Yukarı komutları yazdığın dizinde olması lazım.

```bash
make -j $(nproc)
sudo make modules_install
sudo make install
sudo update-initramfs -c -k VERSION
sudo update-grub
reboot
```

## ⤵ Güncel Kernel Dosyasının İndirilmesi

[🐧 The Linux Kernel Archives](https://www.kernel.org/) sitesi üzerinden en güncel kernel sürümünü indirin veya alttaki komut ile indirmeyi 🖤 terminal üzerinden yapın:

> ❗ Hızlı kurulumu yaptıysan alttaki işlemlerin hiçbirini yapmana gerek yoktur.

```bash
wget -O linux-5.3.2.tar.xz https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.3.2.tar.xz
```

## 📦 Kernel Kurulumu

### 🗃 Arşivden Çıkarma

İndirdiğiniz kernel dosyasının bulunduğu dizine girin. \(Örn `cd ~/Downloads`\)

> **Terminal üzerinden indirme yaptıysanız** zaten o dizinde olacağınızdan geçiş yapmanıza **gerek yoktur**.

**Ubuntu, Debian**:

```bash
xz -d -v linux-5.3.2.tar.xz
tar xvf linux-5.3.2.tar
```

**Diğer**:

```bash
unzx -v linux-5.3.2.tar.xz
tar xvf linux-5.3.2.tar
```

### ⚙ Yapılandırma Ayarlarını Aktarma

```bash
cd linux-5.3.2
cp -v /boot/config-$(uname -r) .config
```

**Örnek Çıktı**:

```bash
'/boot/config-5.0.0-23-generic' -> '.config'
```

### 🧰 Geliştirici Araçlarının Kurulumu

```bash
sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev
```

### ⚒ Kernel'i Derleme

Sıkıştırılmış kernel imajını derlemek için alttaki komutu yazın:

```bash
make -j $(nproc)
```

> `-j $(nproc)` komutu ile birden fazla işlemci çekirdeği kullanılır

### 🔆 Kernel Modüllerini Yükleme

```bash
sudo make modules_install
```

### ⏬ Kernel Yükleme

Alttaki komut ile aşağıdaki dosyaları `/boot` dizinine yükleyeceğiz

* initramfs-5.3.2.img
* System.map-5.3.2
* vmlinuz-5.3.2

```bash
sudo make install
```

## 👨‍🔧 Grub Yapılandırmasını Güncelleme

Grub2 yükleyicisinin yapılandırma ayarlarını yapmamız gerekmekte.

> Bu komutlar isteğe bağlıdır. make install işlemi bu işlemleri zaten yapmış olacaktır

```bash
sudo update-initramfs -c -k 5.3.2
sudo update-grub
```

## 🚀 İşlemleri Sonlandırma

* `reboot` ile sistemi yeniden başlatıyoruz
* Ardından `uname -mrs` ile linux kernel versiyonunu kontrol ediyoruz

**Örnek Çıktı**:

```bash
Linux 5.3.2 x86_64
```

