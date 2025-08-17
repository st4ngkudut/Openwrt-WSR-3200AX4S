# Panduan Install OpenWrt pada Buffalo WSR-3200AX4S

---

## 1. Boot Pertama via TFTP (Recovery Mode)

1. Ubah nama file berikut:
   ```
   openwrt-24.10.0-mediatek-mt7622-buffalo_wsr-3200ax4s-initramfs-kernel.bin
   ```
   menjadi:
   ```
   linux.trx-recovery
   ```

2. Nyalakan router dengan menekan tombol **AOSS** agar masuk ke mode **TFTP recovery**.  
   File `linux.trx-recovery` akan dimuat melalui TFTP.

---

## 2. Login ke OpenWrt via SSH

- Setelah berhasil boot, router akan memiliki alamat IP default **192.168.1.1**.  
- Login dengan SSH:
  ```bash
  ssh root@192.168.1.1
  ```

---

## 3. Nonaktifkan Firmware Check (Wajib Dilakukan Sekali Saja)

> Penting: Jika langkah ini dilewati, hasilnya tidak bisa dipastikan.  
> Hanya perlu dilakukan **satu kali**. Untuk update berikutnya **tidak perlu diulang**.

Jalankan perintah berikut di SSH:

```bash
echo "/dev/mtd3 0x0 0x1000 0x20000" > /etc/fw_env.config
fw_setenv boot2 "run boot_rd_img;bootm"
```

---

## 4. Install Firmware

1. Salin file hasil build berikut ke router (pakai `scp`):
   ```
   openwrt-24.10.0-mediatek-mt7622-buffalo_wsr-3200ax4s-squashfs-factory-uboot.bin
   ```

2. Jalankan perintah `sysupgrade` dengan opsi **-F**:
   ```bash
   sysupgrade -F openwrt-24.10.0-mediatek-mt7622-buffalo_wsr-3200ax4s-squashfs-factory-uboot.bin
   ```

3. Tunggu hingga proses selesai dan router reboot.

---

## 5. Upgrade Versi Berikutnya

Untuk versi OpenWrt berikutnya, cukup gunakan file build:  
```
...squashfs-sysupgrade.bin
```

Lalu upgrade seperti biasa melalui:
- Perintah `sysupgrade`
- Atau melalui **LuCI (Web UI)**

Tidak perlu lagi melakukan langkah **firmware check**.

---

## Selesai
