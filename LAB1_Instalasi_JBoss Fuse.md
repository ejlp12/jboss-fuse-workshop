# LAB 01 – INSTALLASI JBOSS FUSE


Instalasi JBoss Fuse cukup mudah. Yang diperlukan hanya melakukan unzip dari file installer ke sebuah target direktori.
File installer JBoss Fuse dapat diperoleh dari http://www.jboss.org/products/fuse.

Asumsi anda login di mesin Linux dengan user `jboss`, dan file installer ada di direktori `~/Downloads/`,
kita akan install JBoss Fuse di direktori `/home/jboss/Servers`.


```
[jboss@machine ~]# mkdir Servers
[jboss@machine ~]# cd Servers
[jboss@machine Servers]# unzip ~/Downloads/jboss-fuse-full-6.1.0.redhat-379.zip
```

Perintah tersebut diatas akan membuat subdirektori dengan mana  `jboss-fuse-6.1.0.redhat-379`.

Setelah selesai melakukan instalasi, anda akan perlu mengedit sedikit file konfigurasi untuk men-setup username untuk 
administator dan password-nya.

Edit `<JBoss Fuse Home>/etc/users.properties`, lalu uncomment (hapus karakter #) pada barus terakhir, serta ubah password-nya.
Format konfigurasi adalah `<username>=<password>,<role>`. Pada latihan ini kita akan ganti password-nya dari `admin` menjadi
`Passw0rd!`

```
admin=Passw0rd!,admin
```

Selesai sudah instalasi JBoss Fuse. Sangan cepat kan..

