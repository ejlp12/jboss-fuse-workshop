# LAB 02 – Menjalankan JBoss Fuse Server dengan Command Line

Buka Terminal window di mesin anda, kemudian `cd` ke direktori tempat instalasi JBoss Fuse (yaitu `~/Servers/jboss-fuse-6.1.0.redhat-379`). Dari direktori tersebut, jalankan perintah `bin/fuse` (atau `bin\fuse.bat` jika sistem operasi anda Windows ) untuk memulai menjalankan JBoss Fuse.

Secara default, JBoss Fuse akan mulai jalan dalam mode interaktif, artinya server akan dijalankan dan kemudian menampilkan command console (prompt).

Jika tidak ingin menjalankan server tanpa masuk ke Console, makan bisa kita jalanakan perintah dengan opsi seperti ini `bin/fuse server` dan untuk menjalankan Console di tempat lain yang akan terkoneksi ke server yang sudah dijalankan kita bisa menjalankan perintah `bin/fuse client`.

Berikut adalah tampilan saat Fuse server sudah jalan dengan mode interaktif.

```
Please wait while JBoss Fuse is loading...
100% [========================================================================]

      _ ____                  ______
     | |  _ \                |  ____|
     | | |_) | ___  ___ ___  | |__ _   _ ___  ___
 _   | |  _ < / _ \/ __/ __| |  __| | | / __|/ _ \
| |__| | |_) | (_) \__ \__ \ | |  | |_| \__ \  __/
 \____/|____/ \___/|___/___/ |_|   \__,_|___/\___|

  JBoss Fuse (6.1.0.redhat-379)
  http://www.redhat.com/products/jbossenterprisemiddleware/fuse/

Hit '<tab>' for a list of available commands
and '[cmd] --help' for help on a specific command.

Open a browser to http://localhost:8181 to access the management console

Create a new Fabric via 'fabric:create'
or join an existing Fabric via 'fabric:join [someUrls]'

Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown JBoss Fuse.

JBossFuse:karaf@root> 
```

Setelah muncul prompt `JBossFuse:karaf@root>` artinya JBoss Fuse sudah __up and running__.
Pada console tersebut kita bisa memberikan perintah untuk mengadministerasi JBoss Fuse server. Kita akan coba memberikan perintah untuk mematikan server dengan perintah berikut:

```
osgi:shutdown -f now
```

