Daftar isi
1. Persiapan dan prasyarat
2. Instalasi WSL2 dan Ubuntu
3. Pemeriksaan dan instalasi JDK 17
4. Download dan ekstraksi Hadoop 3.5.0
5. Konfigurasi variabel lingkungan
6. Instalasi dan konfigurasi SSH
7. Konfigurasi file Hadoop XML
8. Pembuatan direktori dan format NameNode
9. Menjalankan HDFS dan memverifikasi proses
10. Pengujian upload dan baca file HDFS
11. Membuka dashboard web Hadoop
12. Menghentikan dan menjalankan kembali Hadoop
13. Troubleshooting dan checklist akhir

1. Persiapan dan Prasyarat
Sebelum menginstal Apache Hadoop 3.5.0, pastikan komputer memenuhi kebutuhan dasar dan perangkat lunak pendukung telah disiapkan. Panduan ini menggunakan Windows 10/11 dengan Ubuntu melalui WSL2 agar Hadoop dapat dijalankan dalam lingkungan Linux.
Hadoop 3.5.0 akan dikonfigurasi dalam mode single-node, yaitu seluruh layanan berjalan pada satu komputer. Konfigurasi ini cocok untuk pembelajaran dan praktikum, bukan untuk cluster produksi. 
1.1	Perangkat keras dan perangkat lunak
Komponen	Persyaratan
Sistem operasi	Windows 10/11 yang mendukung WSL2
Lingkungan Linux	Ubuntu melalui WSL2
Java	JDK 17 untuk menjalankan server Hadoop 3.5.0
Hadoop	Apache Hadoop 3.5.0
SSH	OpenSSH client dan server
Koneksi internet	Dibutuhkan untuk mengunduh paket
Tidak ada batas minimum RAM atau kapasitas disk yang ditetapkan secara khusus dalam panduan ini. Pastikan komputer memiliki sumber daya yang memadai dan ruang disk yang cukup untuk instalasi serta data praktikum.
1.2 Perangkat lunak yang perlu disiapkan
Ada empat komponen utama yang perlu dipahami sebelum instalasi.
 
1. Windows dan WSL2
WSL2 memungkinkan pengguna menjalankan lingkungan Linux langsung di Windows tanpa harus memasang Linux sebagai sistem operasi utama.
 
2. Ubuntu
Ubuntu menjadi lingkungan tempat Hadoop dipasang dan dijalankan. Semua perintah Linux dalam tutorial ini diketik di terminal Ubuntu, bukan di Command Prompt Windows.
 
3. JDK 17
Java dibutuhkan karena Hadoop dikembangkan menggunakan Java. Hadoop 3.5.0 memerlukan JDK 17 pada sisi server. Jika komputer sudah menggunakan versi Java lain, JDK 17 dapat dipasang berdampingan tanpa harus menghapus Java yang lama. 
 
4. Apache Hadoop 3.5.0
Hadoop menyediakan HDFS untuk penyimpanan data dan komponen pemrosesan data. Dalam panduan awal ini, fokusnya adalah mengaktifkan HDFS dan dashboard NameNode.
1.3 Memahami alur instalasi
Sebelum memulai, pahami urutan pengerjaan berikut agar konfigurasi tidak dilakukan secara terbalik.
 
1.4 Pemeriksaan awal komputer
Sebelum memasang perangkat lunak, lakukan pemeriksaan berikut.
A. Periksa versi Windows
Tekan Windows + R, ketik:
winver
Tekan Enter. Jendela yang muncul menampilkan versi Windows yang digunakan.

B. Periksa apakah WSL sudah tersedia
Buka PowerShell, kemudian jalankan:
wsl --list --verbose
Jika WSL sudah terpasang, perintah tersebut menampilkan distribusi Linux yang tersedia beserta versi WSL-nya. Jika Ubuntu tercantum dengan VERSION 2, kamu dapat melanjutkan ke tahap berikutnya.
Jika WSL belum terpasang, lanjutkan ke bagian 2 untuk melakukan instalasi.

C. Periksa Java jika sebelumnya sudah terpasang
Buka terminal Ubuntu, lalu jalankan:
java -version
javac -version
ls /usr/lib/jvm/
Jika Java belum ditemukan, atau versi yang tersedia bukan JDK 17, jangan khawatir. Instalasi JDK 17 akan dibahas pada bagian berikutnya.
2. Instalasi WSL2 dan Ubuntu
2.1 Tujuan instalasi
Pada tahap ini, kita akan menyiapkan lingkungan Linux di Windows menggunakan Windows Subsystem for Linux (WSL) 2 dan menginstal distribusi Ubuntu. Lingkungan ini akan digunakan untuk menjalankan Hadoop 3.5.0 sehingga pengguna tidak perlu memasang sistem operasi Linux secara terpisah atau membuat virtual machine secara manual.
WSL2 memungkinkan Windows menjalankan lingkungan Linux dengan dukungan kernel Linux sungguhan. Dalam tutorial ini, Windows digunakan sebagai sistem operasi utama, sedangkan Ubuntu menjadi lingkungan untuk instalasi Java, Hadoop, konfigurasi HDFS, dan eksekusi perintah Linux.
Alur instalasinya adalah:
 
2.2 Memeriksa persyaratan sistem
Sebelum menginstal, pastikan komputer memenuhi persyaratan berikut.
•	Menggunakan Windows 10 versi yang mendukung WSL2 atau Windows 11.
•	Memiliki koneksi internet untuk mengunduh komponen WSL dan Ubuntu.
•	Memiliki ruang penyimpanan yang cukup untuk Ubuntu, Java, Hadoop, dan data latihan.
•	Memiliki akses administrator Windows untuk mengaktifkan fitur yang diperlukan.
•	Menggunakan virtualisasi perangkat keras yang aktif apabila dibutuhkan oleh konfigurasi komputer.
Memeriksa versi Windows
1.	Tekan Windows + R.
2.	Ketik perintah berikut.
winver
3.	Tekan Enter.
4.	Periksa edisi dan versi Windows yang muncul.
Untuk instalasi baru, Windows 11 merupakan pilihan yang praktis. Jika menggunakan Windows 10, pastikan versinya mendukung WSL2 dan memenuhi persyaratan Microsoft.
2.3 Memeriksa apakah WSL sudah terpasang
Buka PowerShell atau Windows Terminal. Untuk pemeriksaan awal, hak administrator belum tentu diperlukan.
Jalankan:
wsl --status
Kemudian periksa distribusi Linux yang sudah terpasang:
wsl --list --verbose
Perintah kedua dapat disingkat menjadi:
wsl -l -v
Cara membaca hasilnya:
Hasil pemeriksaan	Tindakan
WSL belum tersedia atau belum terpasang	Lanjutkan ke instalasi WSL.
Ubuntu sudah terpasang dengan versi 2	Langsung lanjut ke konfigurasi awal Ubuntu.
Ubuntu sudah terpasang dengan versi 1	Pertimbangkan mengubahnya ke WSL2.
Ubuntu belum terpasang, tetapi WSL tersedia	Instal distribusi Ubuntu.

Jangan menghapus atau memasang ulang Ubuntu apabila distribusi tersebut sudah berisi data penting.
2.4 Instalasi WSL2 dan Ubuntu dengan cara utama
Metode yang paling sederhana adalah menggunakan perintah wsl --install melalui PowerShell. Perintah ini mengaktifkan komponen yang diperlukan, memasang kernel Linux, dan memasang Ubuntu sebagai distribusi bawaan pada instalasi baru yang didukung. 
Langkah 1. Buka PowerShell sebagai administrator
1.	Buka menu Start Windows.
2.	Ketik PowerShell atau Windows Terminal.
3.	Klik kanan aplikasi, lalu pilih Run as administrator.
4.	Jika muncul jendela User Account Control (UAC), pilih Yes.
Langkah 2. Jalankan perintah instalasi
Ketik perintah berikut:
wsl --install
Tekan Enter dan tunggu sampai proses selesai. Pastikan komputer tetap terhubung ke internet.
Jika Windows meminta komputer untuk dimulai ulang (restart), simpan pekerjaan yang sedang terbuka lalu lakukan restart.
Langkah 3. Buka Ubuntu
Setelah komputer menyala kembali:
1.	Buka menu Start.
2.	Cari aplikasi Ubuntu.
3.	Klik aplikasi tersebut.
4.	Tunggu sampai proses inisialisasi dan ekstraksi berkas selesai.
Pada peluncuran pertama, Ubuntu akan meminta pengguna membuat akun Linux.
Langkah 4. Buat username dan password Linux
Ubuntu akan menampilkan prompt seperti berikut:
Enter new UNIX username:
Masukkan username yang diinginkan (bebas; nafilahanumoioi) atau misalnya:
meimei
Kemudian buat password dan masukkan kembali ketika diminta.
Perlu diperhatikan:
•	Username Linux tidak harus sama dengan username Windows.
•	Gunakan username sederhana, sebaiknya huruf kecil tanpa spasi.
•	Ketika mengetik password di terminal Linux, karakter yang diketik biasanya tidak ditampilkan. Ini normal.
•	Password Linux ini digunakan untuk akun Ubuntu dan autentikasi perintah administratif melalui sudo, bukan otomatis sama dengan password Windows.
Setelah proses selesai, terminal akan menampilkan prompt yang kurang lebih seperti berikut:
meimei@computername:~$
Tanda $ menunjukkan bahwa terminal sedang menggunakan akun pengguna biasa.
2.5 Alternatif instalasi apabila cara utama gagal
Jika wsl --install tidak berhasil atau Ubuntu tidak ikut terpasang, gunakan alternatif berikut.
Alternatif A. Memilih distribusi Ubuntu secara eksplisit
Buka PowerShell sebagai administrator, kemudian lihat distribusi yang tersedia:
wsl --list --online
Pilih nama Ubuntu yang tersedia pada hasil tersebut. Contohnya, apabila ingin menggunakan Ubuntu 24.04 LTS:
wsl --install -d Ubuntu-24.04
Nama distribusi harus sesuai dengan daftar yang ditampilkan di komputer. Jangan menganggap semua nama versi Ubuntu pasti tersedia di setiap lingkungan.
Alternatif B. Mengunduh melalui sumber web
Apabila proses instalasi mengalami masalah dengan Microsoft Store, dokumentasi Microsoft juga menyediakan opsi --web-download pada versi WSL yang mendukungnya.
wsl --install --web-download -d Ubuntu
Jika opsi tersebut tidak dikenali, gunakan cara utama atau ikuti petunjuk instalasi manual dari Microsoft. 
Panduan resmi: Instalasi WSL dari Microsoft Learn.

Alternatif C. WSL sudah terpasang, tetapi Ubuntu belum tersedia
Jika WSL sudah tersedia, periksa daftar distribusi:
wsl --list --online
Kemudian instal Ubuntu:
wsl --install -d Ubuntu
Jika Ubuntu sudah terpasang, jangan menjalankan instalasi ulang hanya untuk mengikuti tutorial ini. Lanjutkan ke pemeriksaan versi.
2.6 Memastikan Ubuntu menggunakan WSL2
Setelah Ubuntu terpasang, kembali ke PowerShell dan jalankan:
wsl --list --verbose
Contoh hasil yang diharapkan:
  NAME               STATE        VERSION
* Ubuntu-24.04      Running          2
Nama distribusi dapat berbeda. Hal yang paling penting adalah kolom VERSION menunjukkan angka 2. Jika hasilnya menunjukkan versi 1, ubah distribusi tersebut menjadi WSL2 dengan perintah berikut:
wsl --set-version Ubuntu-24.04 2
Ganti Ubuntu-24.04 sesuai nama distribusi pada komputer. Proses konversi dapat memerlukan waktu; cadangkan data penting sebelum melakukannya. 
Jika ingin menjadikan WSL2 sebagai versi default untuk instalasi distribusi Linux baru, jalankan:
wsl --set-default-version 2
Perintah tersebut hanya mengatur versi default untuk instalasi baru. Untuk mengubah distribusi yang sudah ada, gunakan wsl --set-version.
2.7 Memperbarui paket Ubuntu
Buka aplikasi Ubuntu, lalu jalankan:
sudo apt update
Masukkan password Ubuntu jika diminta. Perintah ini memperbarui daftar informasi paket yang tersedia, bukan langsung memperbarui seluruh aplikasi. Untuk memperbarui paket yang sudah terpasang, jalankan:
sudo apt upgrade
Jika diminta konfirmasi, ketik Y lalu tekan Enter. Tunggu sampai proses selesai.
2.8 Mengenal perbedaan terminal Windows dan Ubuntu
Selama instalasi Hadoop, ada dua lingkungan terminal yang akan digunakan.
Terminal	Contoh prompt	Kegunaan
PowerShell	PS C:\Users\...>	Memasang WSL, memeriksa versi WSL, dan mengelola distribusi Linux.
Ubuntu	meimei@computername:~$	Memasang Java, mengunduh Hadoop, mengedit konfigurasi, dan menjalankan perintah HDFS.

Perhatikan bahwa perintah untuk kedua terminal tidak selalu dapat dipertukarkan. Sebagai contoh, wsl --list --verbose dijalankan dari PowerShell, sedangkan sudo apt update dijalankan dari Ubuntu.



3. Pemeriksaan dan Instalasi JDK 17
3.1 Tujuan instalasi
Java Development Kit (JDK) merupakan komponen yang dibutuhkan untuk menjalankan Apache Hadoop. Pada tahap ini, kita akan memeriksa Java yang sudah tersedia di komputer, menentukan apakah perlu memasang JDK 17, kemudian mengatur variabel lingkungan agar Hadoop dapat menemukan Java dengan benar.

Catatan penting: Tutorial ini menggunakan Hadoop 3.5.0. Untuk menjalankan komponen server Hadoop 3.5.0, gunakan JDK 17. Jika komputer sudah memiliki Java versi lain, Java tersebut tidak harus dihapus. JDK 17 dapat dipasang berdampingan, lalu dipilih secara khusus di lingkungan Ubuntu untuk Hadoop. 

Apache Hadoop 3.5.0
+1




Pada tutorial ini, seluruh perintah Java dijalankan di terminal Ubuntu WSL, bukan di PowerShell Windows. Instalasi Java di Windows tidak otomatis berarti Java tersebut sudah terpasang di Ubuntu.
3.2 Memeriksa Java yang sudah terpasang
Buka aplikasi Ubuntu melalui menu Start Windows.
Langkah 1. Periksa versi Java
Jalankan:
java -version


Perintah ini menampilkan versi Java Runtime yang ditemukan melalui konfigurasi lingkungan saat ini.
Langkah 2. Periksa compiler Java
Jalankan:
javac -version


Perintah ini memeriksa apakah compiler Java tersedia. Compiler termasuk dalam JDK.
Langkah 3. Periksa direktori Java
Jalankan:
ls /usr/lib/jvm/


Direktori tersebut biasanya berisi instalasi Java yang dikelola melalui paket Ubuntu.
Langkah 4. Periksa lokasi Java yang digunakan
Jalankan:
which java
readlink -f "$(which java)"


Perintah pertama menunjukkan lokasi perintah Java yang ditemukan. Perintah kedua membantu menampilkan lokasi berkas Java sebenarnya setelah mengikuti tautan simbolis.
3.3 Menentukan apakah JDK 17 perlu diinstal
Berdasarkan hasil pemeriksaan pada subbab sebelumnya, tentukan langkah yang sesuai.
Kondisi	Tindakan
java -version menunjukkan Java 17 dan javac -version tersedia	Tidak perlu instal ulang. Lanjutkan ke konfigurasi JAVA_HOME.
Java 8, 11, 21, atau versi lain yang terpasang	Pasang JDK 17 untuk Hadoop 3.5.0.
java tidak ditemukan	Pasang JDK 17.
java tersedia, tetapi javac tidak ditemukan	Kemungkinan yang terpasang hanya JRE atau konfigurasi JDK belum benar. Pasang paket JDK 17.
3.4 Menginstal JDK 17 di Ubuntu
Ubuntu menyediakan paket OpenJDK melalui pengelola paket apt. Paket yang digunakan adalah openjdk-17-jdk. 

Ubuntu for Developers
+1



Langkah 1. Memperbarui daftar paket
Jalankan di terminal Ubuntu:
sudo apt update


Langkah 2. Memasang JDK 17
Jalankan:
sudo apt install openjdk-17-jdk


Jika muncul pertanyaan konfirmasi, ketik Y lalu tekan Enter.

Tunggu hingga proses instalasi selesai. Durasi instalasi bergantung pada koneksi internet dan kondisi komputer.
Langkah 3. Memeriksa hasil instalasi
Jalankan kedua perintah berikut:
java -version


javac -version


Hasil yang diharapkan menunjukkan versi utama Java 17. Nomor pembaruan dan detail versi dapat berbeda, misalnya 17.0.x.

Contoh:
openjdk version "17.0.x"


Dan:
javac 17.0.x


Jika kedua perintah berhasil dan menunjukkan Java 17, lanjutkan ke konfigurasi variabel lingkungan.
3.5 Menentukan lokasi JDK 17
Hadoop membutuhkan variabel JAVA_HOME yang menunjuk ke direktori instalasi Java. Direktori ini bukan lokasi berkas java secara langsung, melainkan direktori induk instalasi JDK.

Jalankan:
ls /usr/lib/jvm/


Pada Ubuntu, lokasi JDK 17 biasanya seperti berikut:
/usr/lib/jvm/java-17-openjdk-amd64


Nama direktori bisa berbeda sesuai versi Ubuntu dan arsitektur perangkat. Karena itu, periksa hasil perintah terlebih dahulu.

Untuk mendapatkan lokasi Java 17 secara otomatis dari sistem alternatif Java, jalankan:
readlink -f "$(which java)"


Contoh hasil:
/usr/lib/jvm/java-17-openjdk-amd64/bin/java


Dari hasil tersebut, nilai JAVA_HOME adalah:
/usr/lib/jvm/java-17-openjdk-amd64


Bagian /bin/java tidak dimasukkan ke dalam nilai JAVA_HOME.
3.6 Mengatur JAVA_HOME dan PATH
Pada tahap ini, kita mengatur variabel lingkungan di Ubuntu agar perintah Java dapat ditemukan dengan benar dan konfigurasi tetap tersedia ketika terminal dibuka kembali.
Langkah 1. Membuka berkas konfigurasi shell
Jalankan:
nano ~/.bashrc


Berkas .bashrc menyimpan konfigurasi shell untuk akun pengguna saat menggunakan Bash.
Langkah 2. Menambahkan konfigurasi Java
Gulir ke bagian paling bawah, lalu tambahkan:
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH="$JAVA_HOME/bin:$PATH"


Penting: Jika lokasi JDK pada komputer berbeda, ganti nilai JAVA_HOME sesuai direktori yang ditemukan pada langkah sebelumnya.
Langkah 3. Menyimpan berkas
Di editor Nano:
1. Tekan Ctrl + O untuk menyimpan.
2. Tekan Enter untuk mengonfirmasi nama berkas.
3. Tekan Ctrl + X untuk keluar.
Langkah 4. Memuat konfigurasi
Jalankan:
source ~/.bashrc


Perintah tersebut menerapkan konfigurasi baru ke terminal yang sedang digunakan.
Langkah 5. Memeriksa JAVA_HOME
Jalankan:
echo "$JAVA_HOME"


Hasil yang diharapkan:
/usr/lib/jvm/java-17-openjdk-amd64


Selanjutnya, periksa Java dan compiler:
java -version
javac -version


Untuk memastikan perintah Java berasal dari JDK yang dipilih:
which java


Jika semua hasil sesuai, konfigurasi Java pada akun Ubuntu sudah siap.
3.7 Bagaimana jika terdapat beberapa versi Java?
Komputer dapat memiliki beberapa versi Java secara bersamaan. Dalam kondisi tersebut, perintah java yang dijalankan belum tentu menggunakan versi yang diinginkan Hadoop.

Untuk memeriksa alternatif Java yang terdaftar, jalankan:
sudo update-alternatives --config java


Jika tersedia beberapa pilihan, sistem akan menampilkan daftar lokasi Java. Pilih Java 17 jika ingin menjadikannya versi default untuk perintah java.

Untuk compiler, periksa dengan:
sudo update-alternatives --config javac


Perlu diperhatikan bahwa update-alternatives mengatur pilihan default sistem, sedangkan JAVA_HOME menunjuk direktori yang digunakan aplikasi tertentu. Keduanya sebaiknya mengarah ke versi Java yang konsisten.

Jika tidak ingin mengubah Java default untuk aplikasi lain, cukup atur JAVA_HOME dan PATH sesuai kebutuhan Hadoop.
3.8 Mengatasi masalah umum
Masalah	Penyebab atau solusi
java: command not found	Pastikan JDK terpasang dan direktori bin masuk ke PATH.
javac: command not found	Pastikan paket openjdk-17-jdk sudah terpasang, bukan hanya JRE.
JAVA_HOME is not set	Periksa JAVA_HOME pada konfigurasi pengguna dan konfigurasi Hadoop.
Versi Java tetap berbeda setelah mengubah .bashrc	Jalankan source ~/.bashrc, kemudian periksa which java dan echo "$JAVA_HOME".
Java 17 sudah terpasang di Windows, tetapi tidak terdeteksi di Ubuntu	Instal Java di Ubuntu. Instalasi Windows dan Ubuntu merupakan lingkungan yang berbeda.
Satu hal yang perlu diingat untuk tahap berikutnya: konfigurasi JAVA_HOME pada .bashrc belum tentu otomatis digunakan oleh proses Hadoop yang dijalankan melalui SSH. Karena itu, pada tahap konfigurasi Hadoop, kita juga akan memeriksa berkas hadoop-env.sh dan memastikan JAVA_HOME ditetapkan di sana.
