# LAB 01 – INSTALLASI JBOSS FUSE


Sebelum mengikuti tutorial ini sebaiknya ada sudah tau tentang produk Red Hat JBoss Fuse, konsep termasuk komponen-komponen pendukung dan fungsinya (seperti Karaf, Camel, A-MQ, Fabric, Maven, dan lain-lain) serta beberapa alternatif atau opsi arsitektur deployment produk ini. 

Referensi yang mungkin sebaiknya dibaca dulu adalah dokumentasi resmi dari Red Hat JBoss Fuse yang bisa diakses disini:

[JBoss Fuse Documentation](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/)

Selain itu sebaiknya anda cek dulu kebutuhan sistem:

[JBoss Fuse Supported Configuration](https://access.redhat.com/articles/310603)

Dalam tutorial ini akan dijelaskan mengenai cara instalasi dengan arsitektur tertentu yang dianggap cukup mumpuni untuk PRODUCTION environment dengan mempertimbangkan skalabilitas dan high availibility dari sistem.

## Asumsi

1.  Asumsi anda menggunakan Linux operating system dengan versi yang disyaratkan. 
    Saya sendiri waktu membuat tutorial ini menggunakan sistem operasi OS-X Yosemite dengan menggunakan terminal dengan perintah-perintah yang kompatibel dengan Linux.
2.  Installasi JDK atau JRE saya anggap sudah dilakukan dengan versi seperti yang disyaratkan.
3.  Kita akan menginstall dan mengkonfigurasi **JBoss Fuse versi 6.2.0**

## Arsitektur yang akan dibangun

Kita akan membuat konfigurasi Fuse dengan arsitektur sebagai berikut:

* Satu Fabric Container Server yang diberi nama `root1`
* Satu Fabric Container yang akan berfungsi sebagai ESB (Camel routing atau service orchestration)

## Instalasi & Konfigurasi JBoss Fuse

1.  Login sebagai `root` di komputer anda lalu buat sebuah direktori `/Server1`

	```
	mkdir `/Server1`
	```

2.  Buat dua user pada operating system dengan nama `fuseadm`. User tersebut adalah user yang akan digunakan untuk menjalankan JBoss Fuse.

    Jalankan perintah berikut:

	```
	adduser fuseadm
  passwd fuseadm
	```

3. Download file installer JBoss Fuse 6.2 yaitu berupa file dengan nama `jboss-fuse-full-6.2.0.redhat-133.zip`

   Jika anda memiliki Subscription, anda bisa download dari [https://access.redhat.com](https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?product=jboss.fuse&downloadType=distributions)
   
   jika tidak, anda bisa download dari link berikut:
   
   [http://www.jboss.org/download-manager/file/jboss-fuse-6.2.0.GA-full_zip.zip](http://www.jboss.org/download-manager/file/jboss-fuse-6.2.0.GA-full_zip.zip)
   
   anda perlu register (gratis) sebelum bisa mendownload file tersebut.
   

### Konfigurasi di Mesin-1

1.  Ekstrak file zip tersebut di sebuah direktori, misalnya di `/Server1/`, lalu set direktori tersebut

	```
	unzip /downlod_dir/jboss-fuse-full-6.2.0.redhat-133.zip -d /Server1/
	chown -R fuse1 /Server1/jboss
	export $FUSE_INSTALL_DIR1="/Server1/jboss-fuse-6.2.0.redhat-133"
	```

	Selesai proses instalasi Fuse.

	>> Ya.. pada dasarnya installasi Fuse hanya ekstrak sebuah file zip!

2. 	Edit file `$FUSE_INSTALL_DIR/etc/users.properties` untuk menambahkan user yang dapat mengakses Karaf engine console dari Fuse.
	Hapus tanda komentar (#) pada baris paling bawah sehingga menjadi seperti ini

    ```
    admin=admin,admin,manager,viewer,Operator, Maintainer, Deployer, Auditor, Administrator, SuperUser
    ```

    Anda bisa menambahkan user lain dibawah baris tersebut
    Save file tersebut setelah edit selesai.
	
	Format file konfigurasi tersebut adalah sebagai berikut "username=password,role1,role2,role3,..." jadi kita akan memiliki user `admin` dengan password `admin`


3. Edit file `$FUSE_INSTALL_DIR/etc/system.properties` dan ubah nama dari Karaf instance dari `root` menjadi `root1`

   > Jika kita akan mengkonfigurasi Fuse cluster akan diperluan minimal 3 root (Fabric Server) jadi perlu dibedakan namanya. Untuk saat ini kita cukup perlu tahu saja bagaimana cara mengganti nama Karaf instance tapi kita tidak akan membuat konfigurasi cluster dulu. 


	```
	karaf.name = root1
	```

	Save file tersebut.

4. Start Karaf engine dengan mode service (running di background) dengan menggunakan user `fuseadm`, selain mode ini anda juga bisa menjalankan dengan mode interaktif.


	```
	su - fuseadm
	cd $FUSE_INSTALL_DIR/bin
	./start
	```

	Perintah tersebut akan menjalankan Karaf engine yaitu sebuah proses Java.

5.  Cek proses yang dijalankan dari perintah tersebut dengan perintah `ps`


	```
	$ ps auxw |grep karaf
	fuseadm          36386  68.7  5.0  4598316 422700 s000  S     4:05PM   0:42.53 /Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/bin/java -server -Xms512M -Xmx512M -XX:+UnlockDiagnosticVMOptions -XX:+UnsyncloadClass -XX:PermSize=128M -XX:MaxPermSize=256M -Dcom.sun.management.jmxremote -Djava.endorsed.dirs=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/jre/lib/endorsed:/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/lib/endorsed:/Server1/jboss-fuse-6.2.0.redhat-133/lib/endorsed -Djava.ext.dirs=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/jre/lib/ext:/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/lib/ext:/Server1/jboss-fuse-6.2.0.redhat-133/lib/ext -Dkaraf.instances=/Server1/jboss-fuse-6.2.0.redhat-133/instances -Dkaraf.home=/Server1/jboss-fuse-6.2.0.redhat-133 -Dkaraf.base=/Server1/jboss-fuse-6.2.0.redhat-133 -Dkaraf.data=/Server1/jboss-fuse-6.2.0.redhat-133/data -Dkaraf.etc=/Server1/jboss-fuse-6.2.0.redhat-133/etc -Djava.io.tmpdir=/Server1/jboss-fuse-6.2.0.redhat-133/data/tmp -Djava.util.logging.config.file=/Server1/jboss-fuse-6.2.0.redhat-133/etc/java.util.logging.properties -Djavax.management.builder.initial=org.apache.karaf.management.boot.KarafMBeanServerBuilder -Dkaraf.startLocalConsole=false -Dkaraf.startRemoteShell=true -classpath /Server1/jboss-fuse-6.2.0.redhat-133/lib/karaf-jaas-boot.jar:/Server1/jboss-fuse-6.2.0.redhat-133/lib/karaf-jmx-boot.jar:/Server1/jboss-fuse-6.2.0.redhat-133/lib/karaf.jar org.apache.karaf.main.Main
	```

	Cek juga port berapa saja yang dibuka oleh proses tersebut dengan perintah `lsof`

	```
	lsof -i TCP -sTCP:LISTEN -P | grep java
	java    36386 fuse1   27u  IPv6 0x46c7edd5b38f7f1d      0t0  TCP *:63102 (LISTEN)
	java    36386 fuse1  307u  IPv6 0x46c7edd5b38a781d      0t0  TCP localhost:63117 (LISTEN)
	java    36386 fuse1  310u  IPv6 0x46c7edd5b3f6881d      0t0  TCP *:1099 (LISTEN)
	java    36386 fuse1  311u  IPv6 0x46c7edd5afd1031d      0t0  TCP *:44444 (LISTEN)
	java    36386 fuse1  316u  IPv6 0x46c7edd58f117a1d      0t0  TCP *:8181 (LISTEN)
	java    36386 fuse1  329u  IPv6 0x46c7edd5b367c51d      0t0  TCP *:8101 (LISTEN)
	java    36386 fuse1  409u  IPv6 0x46c7edd5afd0d11d      0t0  TCP localhost:61616 (LISTEN)
	```

	berikut adalah penjelasan port yang digunakan

		63102 - Random port (Fabric)
		63117 - Random port (Fabric)
		1099  - RMI registry (JMX)
		44444 - RMI server
		8181  - Port web management console (Hawtio)
		8101  - Port console (command line, SSH connection)
		61616 - Port A-MQ yang secara default dijalankan

6.  Coba akses ke Fuse Management Console (Hawtio) dengan mengakses lewat browser ke URL berikut

	[http://localhost:8181](http://localhost:8181)

	Login dengan menggunakan user `admin` dan password `admin`
	
	![image](https://cloud.githubusercontent.com/assets/3068071/12263708/0284213a-b964-11e5-8808-00e878572885.png)

	Setelah login, tampilannya akan seperti ini:
	
    ![image](https://cloud.githubusercontent.com/assets/3068071/12263721/16876d72-b964-11e5-9f83-282b13415736.png)


7. Anda bisa coba juga akses ke Karaf command line dengan perintah berikut

	```
	./bin/client -r 3 -d 10 -u admin -p admin -a 8101
	```

	atau cukup dengan

	```
	./bin/client -u admin -p admin
	```

	Keterangan option pada perintah tersebut ssb:

		 -u [user]     specify the user name
		 -r [attempts] retry connection establishment (up to attempts times)
		 -d [delay]    intra-retry delay (defaults to 2 seconds)
		 -a [port]	   port


    Hasilnya anda akan mendapatkan **Karaf command line console** seperti ini:

	```
	Logging in as admin
	1822 [sshd-SshClient[5b0abc94]-nio2-thread-4] WARN org.apache.sshd.client.keyverifier.AcceptAllServerKeyVerifier - Server at [/0.0.0.0:8101, DSA, 9b:41:6d:3c:d9:2c:85:69:51:11:b0:79:cc:13:9b:4c] presented unverified {} key: {}
	      _ ____                  ______
	     | |  _ \                |  ____|
	     | | |_) | ___  ___ ___  | |__ _   _ ___  ___
	 _   | |  _ < / _ \/ __/ __| |  __| | | / __|/ _ \
	| |__| | |_) | (_) \__ \__ \ | |  | |_| \__ \  __/
	 \____/|____/ \___/|___/___/ |_|   \__,_|___/\___|

	  JBoss Fuse (6.2.0.redhat-133)
	  http://www.redhat.com/products/jbossenterprisemiddleware/fuse/

	Hit '<tab>' for a list of available commands
	and '[cmd] --help' for help on a specific command.

	Open a browser to http://localhost:8181 to access the management console

	Create a new Fabric via 'fabric:create'
	or join an existing Fabric via 'fabric:join [someUrls]'

	Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown JBoss Fuse.

	JBossFuse:admin@root>
	```


8.  Buat sebuah Fabric Container dengan perintah `fabric:create`

	```
	JBossFuse:admin@root> fabric:create --new-user admin --new-user-password admin --new-user-role admin --zookeeper-password admin --wait-for-provisioning --clean
	Waiting for container: root
	Waiting for container root to provision.
	```

	```
	lsof -i TCP -sTCP:LISTEN -P | grep java
	java    36386 fuse1   27u  IPv6 0x46c7edd5b38f7f1d      0t0  TCP *:63102 (LISTEN)
	java    36386 fuse1  248u  IPv6 0x46c7edd5b411371d      0t0  TCP *:64128 (LISTEN)
	java    36386 fuse1  261u  IPv6 0x46c7edd5b415d21d      0t0  TCP *:8181 (LISTEN)
	java    36386 fuse1  307u  IPv6 0x46c7edd5b38a781d      0t0  TCP localhost:63117 (LISTEN)
	java    36386 fuse1  310u  IPv6 0x46c7edd5b3f6881d      0t0  TCP *:1099 (LISTEN)
	java    36386 fuse1  311u  IPv6 0x46c7edd5afd1031d      0t0  TCP *:44444 (LISTEN)
	java    36386 fuse1  312u  IPv6 0x46c7edd5b3285b1d      0t0  TCP *:8101 (LISTEN)
	java    36386 fuse1  321u  IPv6 0x46c7edd5b41e6f1d      0t0  TCP *:61616 (LISTEN)
	java    36386 fuse1  411u  IPv6 0x46c7edd5afd0f91d      0t0  TCP *:2181 (LISTEN)
	```

	Ada 2 port tambahan yaitu 64128 (Fabric) dan 2181 (Fabric/Zookeeper Registry Service)


9.  Kita lihat daftar container yang terdaftar pada Fuse dengan perintah `fabric:container-list`

    Untuk mempermudah, Karaf command line console mendukung auto completion jadi kita tidak perlu hafal semua command, kita dapat mengetik sebagian kemudian menekan tombol "TAB" maka akan muncul daftar perintah yang sesuai. Misalnya kita ketik `karaf:` lalu TAB hasilnya seperti ini

    ````
    JBossFuse:admin@root> fabric:
	fabric:archetype-generate               fabric:archetype-info                   fabric:archetype-list                   fabric:archetype-workspace
	fabric:autoscale-status                 fabric:cluster-list                     fabric:container-add-profile            fabric:container-change-profile
	fabric:container-connect                fabric:container-create-child           fabric:container-create-ssh             fabric:container-default-jvm-options
	fabric:container-delete                 fabric:container-edit-jvm-options       fabric:container-info                   fabric:container-jmx-domains
	fabric:container-list                   fabric:container-remove-profile         fabric:container-resolver-list          fabric:container-resolver-set
	fabric:container-rollback               fabric:container-start                  fabric:container-stop                   fabric:container-upgrade
	fabric:create                           fabric:crypt-algorithm-get              fabric:crypt-algorithm-set              fabric:crypt-password-get
	fabric:crypt-password-set               fabric:encrypt-message                  fabric:ensemble-add                     fabric:ensemble-list
	fabric:ensemble-password                fabric:ensemble-remove                  fabric:info                             fabric:join
	fabric:mq-create                        fabric:patch-apply                      fabric:profile-change-parents           fabric:profile-copy
	fabric:profile-create                   fabric:profile-delete                   fabric:profile-display                  fabric:profile-download-artifacts
	fabric:profile-edit                     fabric:profile-export                   fabric:profile-import                   fabric:profile-list
	fabric:profile-refresh                  fabric:profile-rename                   fabric:profile-scale                    fabric:require-profile-delete
	fabric:require-profile-list             fabric:require-profile-set              fabric:requirements-export              fabric:requirements-import
	fabric:requirements-list                fabric:status                           fabric:version-create                   fabric:version-delete
	fabric:version-info                     fabric:version-list                     fabric:version-set-default              fabric:wait-for-provisioning
	fabric:watch                            fabric:welcome
	```


	```
	JBossFuse:admin@root> fabric:container-list
	[id]     [version]  [type]  [connected]  [profiles]              [provision status]
	root1*   1.0        karaf   yes          fabric                  success
	                                         fabric-ensemble-0000-1
	                                         jboss-fuse-full
	```

  Kita bisa lihat proses pembuatan (provision) container `root1` tersebut sudah selesai dengan **success** dan kita lihat juga `root1` memiliki 3 profiles dan proses pembuatan (provision) container tersebut sudah selesai dengan **success**. Setiap profiles memiliki fitur-fitur tersendiri.
  
  Kita bisa lihat semua profile yang tersedia dan depedency antar profile di Fuse yang sudah kita install dengan perintah `fabic:profile-list`
  
  ```
JBossFuse:admin@root> fabric:profile-list
	[id]                                      [# containers]  [parents]
	autoscale                                                 default
	default
	example-camel-autotest                                    feature-camel
	example-camel-cluster-cluster.client                      feature-camel
	example-camel-cluster-cluster.server                      feature-camel
	example-camel-cxf                                         feature-camel
	example-camel-loanbroker-mq.bank1                         feature-camel
	example-camel-loanbroker-mq.bank2                         feature-camel
	example-camel-loanbroker-mq.bank3                         feature-camel
	example-camel-loanbroker-mq.loanBroker                    feature-camel
	example-camel-mq                                          feature-camel
	example-camel-mq.bundle                                   feature-camel
	example-camel-twitter                                     feature-camel
	example-cxf-cxf.client                                    fabric
	example-cxf-cxf.server                                    feature-cxf
	example-dosgi-camel.consumer                              feature-camel
	example-dosgi-camel.provider                              feature-dosgi
	example-mq                                                example-mq-producer example-mq-consumer
	example-mq-base                                           karaf
	example-mq-consumer                                       example-mq-base
	example-mq-producer                                       example-mq-base
	fabric                                    1               karaf hawtio
	feature-camel                                             karaf
	feature-camel-jms                                         feature-camel mq-client
	feature-cxf                                               karaf
	feature-dosgi                                             karaf
	feature-fabric-web                                        karaf
	gateway-http                                              karaf
	gateway-mq                                                karaf
	hawtio                                                    karaf
	insight-camel                                             feature-camel insight-core
	insight-console                                           insight-core
	insight-core                                              default insight-metrics.base
	insight-elasticsearch.datastore                           insight-elasticsearch.node
	insight-elasticsearch.node                                insight-core
	insight-logs.elasticsearch                                default insight-metrics.base insight-elasticsearch.node
	insight-metrics.base
	insight-metrics.elasticsearch                             default insight-metrics.base insight-elasticsearch.node
	jboss-fuse-full                           1               fabric feature-camel mq-amq feature-cxf
	jboss-fuse-minimal                                        fabric feature-camel
	karaf                                                     default
	mq-amq                                                    mq-base
	mq-base                                                   karaf
	mq-client                                                 default
	mq-client-base                                            default
	mq-client-default                                         mq-client-base
	mq-client-local                                           mq-client-base
	mq-default                                                mq-base
	mq-replicated                                             mq-base
	openshift                                                 default
	org.jboss.quickstarts.fuse-camel-sap                      feature-camel
	quickstarts-beginner-camel.cbr                            feature-camel
	quickstarts-beginner-camel.eips                           feature-camel
	quickstarts-beginner-camel.errorhandler                   feature-camel
	quickstarts-beginner-camel.log                            feature-camel
	quickstarts-beginner-camel.log.wiki                       feature-camel
	quickstarts-camel.amq                                     feature-camel
	quickstarts-cxf-camel.cxf.code.first                      feature-camel feature-cxf
	quickstarts-cxf-camel.cxf.contract.first                  feature-camel feature-cxf
	quickstarts-cxf-rest                                      feature-cxf
	quickstarts-cxf-secure.rest                               feature-cxf
	quickstarts-cxf-secure.soap                               feature-cxf
	quickstarts-cxf-soap                                      feature-cxf
	quickstarts-fuse-camel.box                                feature-camel
	quickstarts-fuse-camel.linkedin                           feature-camel
	quickstarts-fuse-camel.odata                              feature-camel
	quickstarts-fuse-camel.salesforce                         feature-camel
	support-base                                              karaf
	unmanaged  
  ```
  
  Perhatikan bahwa profile `jboss-fuse-full` memiliki parent beberapa profile lain yaitu `fabric feature-camel mq-amq feature-cxf`
  

9. Sebelum kita lanjut, sebaiknya anda juga mengenal perintah `fabric:status` untuk melihat status dari suatu container

   Sebaiknya selalu pastikan status health dari suatu container atau profile dalam keadaan baik (100%) setelah kita membuat container tersebut dan mulai bekerja pada container tersebut:
   
   ```
   JBossFuse:admin@root> fabric:status
   [profile]                                [instances]    [health]
    fabric                                   1              100%
    fabric-ensemble-0000-1                   1              100%
    ```

10. Kita akan menghapus profile `jboss-fule-full` dari container bernama `root` karena `root` hanya akan berfungsi sebagai **Fabric Server** yang merupakan bagian dari **Fabric Ensemble** jadi root bisa  kita sebut sebagai __manager__ tapi bukan __pekerja__

	```
	fabric:container-remove-profile root1 jboss-fuse-full
	```
	
	Lihat kembali konfigurasi profile dari container `root1` dengan perintahh `fabric:container-list1
	

	Kemudian tambahkan zookeeper command ke profile default karena kita akan membuat arsitektur Fuse yang high-available dan distributed 

	```
	fabric:profile-edit --features fabric-zookeeper-commands default
	```	

	>> Jika mendapatkan error seperti ini, coba ulangi lagi perintah diatas
	>> `Error executing command: Push rejected: [RemoteRefUpdate[remoteName=refs/heads/master, REJECTED_NONFASTFORWARD, (null)...a1317a07ef0bc226e76fd9ce39a1d386a544a709, srcRef=refs/heads/master, message=null]]`

11. Buat profile anda sendiri, diberinama misalnya `ejlp12-fuse-full` (ganti `ejlp12` dengan nama anda).
    Profile ini yang akan kita gunakan sebagai ESB dimana proses integrasi, trasformasi, pemanggilan service ke back-end terjadi, jadi pada profile ini kita membutuhkan fitur camel untuk menjalankan camel routing, cfx untuk operasi web services, dan mq client untuk berinteraksi dengan A-MQ server. Penambahan fitur bisa dilakukan dengan mengganti menambahkan beberapa parent profile untuk `ejlp12-fuse-full`

	Buat profile baru dengan perintah berikut:

	```
	fabric:profile-copy jboss-fuse-full  ejlp12-fuse-full
	fabric:profile-change-parents ejlp12-fuse-full fabric feature-camel feature-cxf mq-client
	```

	Anda bisa cek profile `ejlp12-fuse-full` sudah dibikin dan terdaftar, cek dengan perintah `fabric:profile-list`

12. Tambahkan user administrator lain, misalnya `fuseadmin`

	```
	JBossFuse:admin@root> jaas:realms
	Index Realm                Module Class
	    1 karaf                io.fabric8.jaas.ZookeeperLoginModule
	JBossFuse:admin@root> jaas:manage --index 1
	JBossFuse:admin@root> jaas:useradd fuseadmin fuseadmin
	JBossFuse:admin@root> jaas:roleadd fuseadmin admin
	JBossFuse:admin@root> jaas:update	
	```



