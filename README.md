# Final-Project-Keamanan-Jaringan-Komputer
Kelompok 5

## Anggota Kelompok 5 :

| Nama Lengkap              | NRP        |
| --------------------      | ---------- |
| M. Januar Eko Wicaksono   | 5027221006 |
| Iki Adfi Nur Mohamad      | 5027221033 |
| Rahmad Aji Wicaksono      | 5027221034 |
| Ilhan Ahmad Syafa         | 5027221040 |
| Muhammad Arsy Athallah    | 5027221048 |
| Awang Fraditya            | 5027221055 |

## Daftar isi

- [Soal](#soal)
- [Topology](#topology)
- [Configuration](#configuration)
- [Solution](#solution)

## Soal

A. Studi Kasus / Problem Based Learning
Teknologi Informasi memiliki sebuah jaringan komputer dengan detail sebagai berikut:
- 1 buah server di lantai 6 gedung perpustakaan. dalam server ini memiliki web service yang bisa diakses dari internal network.
- di lantai 7, memiliki 4 ruang kelas dengan fasilitas wifi, dengan kapasitas masing 40 mahasiswa.
- di lantai 9, memiliki 2 ruang lab, yang terdiri dari 25 komputer (lab 1) dan 25 komputer (lab 2) dengan koneksi ethernet. selain itu juga memiliki konektivitas wifi.
- semua jaringan terhubung oleh router utama (backbone) yang diletakkan di gedung riset center di DPTSI.

B. Tugas
1. Coba simulasikan jaringan tersebut menggunakan GNS3.
- pastikan routing sudah benar dan masing masing instance dapat terhubung sebagaimana mestinya
2. Anda diminta untuk melakukan analisis risiko yang mungkin terjadi pada jaringan komputer Teknologi informasi, berdasarkan standar ISO 27001:2022.
3. Berdasarkan analisis risiko yang dibuat, coba implementasikan teknik keamanan Jaringan Komputer, coba implementasikan teknik keamanan jaringan pada simulasi GNS3 yang telah dibuat.
4. simulasikan bentuk bentuk serangan kemanan siber untuk menguji apakah teknik keamanan yang diimplementasikan sudah benar.

## Topology

![alt text](https://github.com/mrvlvenom/FP-KJK/blob/main/img/Topologi.png)

### Prefix IP Kelompok 5 : 192.232

## Configuration

- DPTSI (DHCP Relay/Router (Backbone))

```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
  address 192.232.1.1
  netmask 255.255.255.0

auto eth2
iface eth2 inet static
  address 192.232.2.1
  netmask 255.255.255.0

auto eth3
iface eth3 inet static
  address 192.232.3.1
  netmask 255.255.255.0

up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.232.0.0/16
```

- Web Server (Lt.6 Perpustakaan)

```bash
auto eth0
iface eth0 inet static
  address 192.232.1.2
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Penyerang (Lt.6 Perpustakaan)

```bash
auto eth0
iface eth0 inet static
  address 192.232.1.3
  netmask 255.255.255.0
  gateway 192.232.1.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Kelas 702 (40 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.2.5
  netmask 255.255.255.0
  gateway 192.232.2.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Kelas 703 (40 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.2.4
  netmask 255.255.255.0
  gateway 192.232.2.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Kelas 704 (40 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.2.3	
  netmask 255.255.255.0
  gateway 192.232.2.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf 
```

- Kelas 705 (40 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.2.2
  netmask 255.255.255.0
  gateway 192.232.2.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Lab 1 (25 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.3.2
  netmask 255.255.255.0
  gateway 192.232.3.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Lab 2 (25 host)

```bash
auto eth0
iface eth0 inet static
  address 192.232.3.3
  netmask 255.255.255.0
  gateway 192.232.3.1

up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

## Solution
Dibawah ini masih analisis sementara kami untuk menyelesaikan case soal diatas.

### 1. Simulasi Jaringan di GNS3
- Langkah-langkah:
  1. **Membangun Topologi**:
     - Tambahkan router utama (DPTSI) sebagai backbone.
     - Hubungkan router dengan masing-masing switch yang mewakili lantai (Lantai7, Lantai9, Lt.6Perpustakaan).
     - Hubungkan perangkat seperti server, kelas, dan lab sesuai topologi. (sesuai dengan topology diatas)
  2. **Konfigurasi Routing**:
     - Gunakan **Static Routing** atau **Dynamic Routing Protocol** seperti OSPF di router utama untuk menghubungkan semua jaringan.
     - Pastikan semua subnet memiliki IP Address yang unik.
     - Subnet IP:
       - Kelas (Lantai7): 192.168.10.0/24
       - Lab1 (Lantai9): 192.168.20.0/24
       - Lab2 (Lantai9): 192.168.30.0/24
       - Web Server (Lt.6): 192.168.40.0/24
  3. **Pengujian Konektivitas**:
     - Gunakan perintah `ping` antar perangkat untuk memastikan semua terhubung.

     ![alt text](https://github.com/mrvlvenom/FP-KJK/blob/main/img/ping1.png)

### 2. Analisis Risiko Berdasarkan ISO 27001:2022
- **Identifikasi Risiko**:
  - **Kegagalan Perangkat Keras**:
    - Router atau switch mengalami gangguan.
  - **Serangan Siber**:
    - Denial of Service (DoS) pada web server.
    - Serangan MITM (Man-In-The-Middle) pada jaringan WiFi.
  - **Kerentanan Konfigurasi**:
    - Salah konfigurasi routing atau firewall.
- **Evaluasi Risiko**:
  - Beri prioritas risiko berdasarkan dampak dan kemungkinan.
- **Rencana Penanganan**:
  - Implementasikan kontrol teknis dan prosedural (lihat langkah 3).

### 3. Implementasi Teknik Keamanan
- **Langkah-langkah Keamanan Jaringan**:
  1. **Firewall dan ACL (Access Control List)**:
     - Konfigurasikan firewall pada router untuk membatasi akses ke subnet tertentu.
     - Gunakan ACL untuk mengizinkan hanya lalu lintas tertentu ke server web.
  2. **Keamanan Server**:
     - Gunakan IDS/IPS (Intrusion Detection/Prevention System) pada server.
  3. **Backup dan Recovery**:
     - Siapkan backup konfigurasi router dan switch.

### 4. Simulasi Serangan Siber dan Pengujian Keamanan
- **Serangan yang Disimulasikan**:
  - **Denial of Service (DoS)**:
    - Gunakan alat seperti scapy untuk mengirim lalu lintas palsu ke server.
  - **MITM Attack**:
    - Gunakan tool seperti arp spoofing.
- **Pengujian Mitigasi**:
  - Periksa apakah firewall mampu menahan serangan DoS.
- **Evaluasi Hasil**:
  - Bandingkan log sebelum dan sesudah teknik keamanan diterapkan untuk memastikan mitigasi berhasil.

### 5. Implementasi Serangan ARP Spoofing dan Firewall dari Penyerang ke Web Server

### *Setup* pada *Web Server*
Lakukan instalasi berikut pada setiap *server*:
- Install Python: `sudo apt install python3 python3-pip python3-venv -y`
- Install pip: `sudo apt install python3-pip -y`
- Install scapy: `pip3 install scapy`

### *Setup* Pada *Client Penyerang*
Lakukan instalasi berikut pada device penyerang:
- Install dsniff: `sudo apt install dsniff`

## Identification
Skrip deteksi memantau lalu lintas jaringan secara real-time untuk mengidentifikasi anomali ARP Spoofing, seperti perubahan alamat MAC yang tidak sesuai. Ketika anomali terdeteksi, insiden dicatat secara detail, termasuk alamat IP, MAC Address, waktu kejadian.

### Setup Program Python pada Web Server
Kemudian buatlah suatu *file* python pada Web Server yang berisi program [berikut](https://github.com/mrvlvenom/FP-KJK/blob/main/arp.py)

Kemudian jalankan command berikut:
`python3 [nama_program_python]`

Kemudian mendapatkan output sebagai berikut:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/1.png)

### Setup Client Penyerang
Kemudian jalankan command berikut:
`arpspoof -i [interface] -t [IP target] -r [IP host/router]`

Dan Hasilnya seperti berikut:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/2.png)

### Hasil Pemantauan oleh Web Server
Pantau *server* yang menjadi target penyerangan, apabila muncul *alert* seperti gambar di bawah ini maka program detektor berjalan dengan baik. 

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/3.png)

Skrip secara otomatis memblokir alamat IP penyerang untuk mencegah penyebaran serangan lebih lanjut ke perangkat lain dalam jaringan.

Kemudian jalankan command berikut:
`iptables -L -v`

kemudian menghasilkan sebagai berikut:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/4.png)

Setelah itu penyerang tidak bisa mengirimkan packet ke Web Server, seperti hasil dibawah ini:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/5.png)

Di lain sisi, client yang tidak menyerang bisa mengirimkan packet ke Web Server:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/6.png)

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/8.png)

Kemudian bersihkan jaringan dari paket ARP berbahaya. Skrip memastikan tidak ada cache ARP yang terinfeksi pada perangkat, menggunakan alat seperti perintah “ARP Flush” untuk membersihkan cache pada setiap node.
Kemudian jalankan command berikut:
`iptables -F`

untuk membersihkan penyerang yang sudah terdeteksi oleh web server. Kemudian bisa mengirimkan packet dengan normal:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/9.png)

Dari program python tadi, hasil dari programnya akan dimasukkan ke dalam log.txt, seperti pada gambar:

![image](https://github.com/mrvlvenom/FP-KJK/blob/main/img/7.png)


### Tools yang Dibutuhkan
- **GNS3**: Untuk simulasi jaringan.
- **Scapy** dan **ARP Spoof**: Untuk simulasi serangan.

