# Jarkom-Modul-1-2025-K-01

| Nama                                  | NRP        |
| ------------------------------------- | ---------- | 
| Muhammad Fatihul Qolbi Ash Shiddiqi   | 5027241023 | 
| Hansen Chang                          | 5027241028 |        

### Soal 1
<img width="1114" height="504" alt="image" src="https://github.com/user-attachments/assets/8a664953-6377-42f6-9b87-5cce23e49386" />

### Soal 2
<img width="1019" height="321" alt="image" src="https://github.com/user-attachments/assets/dd5d33f2-4003-4ccc-b86b-30732e8caf65" />
- Eru
```
auto eth0
iface eth0 inet dhcp
```
Melakukan testing apakah sudah terhubung dengan internet dengan melakukan `ping google.com -c 5`
### Soal 3
- Eru
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.64.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.64.2.1
	netmask 255.255.255.0
```

- Melkor
```
auto eth0
iface eth0 inet static
	address 10.64.1.2
	netmask 255.255.255.0
	gateway 10.64.1.1
```
- Menwa
```
auto eth0
iface eth0 inet static
	address 10.64.1.3
	netmask 255.255.255.0
	gateway 10.64.1.1
```
- Varda
```
auto eth0
iface eth0 inet static
	address 10.64.1.3
	netmask 255.255.255.0
	gateway 10.64.1.1
```
- Ulmo
```
auto eth0
iface eth0 inet static
	address 10.64.2.3
	netmask 255.255.255.0
	gateway 10.64.2.1
```
mengecek ip dari masing-masing client dengan `ip a` lalu untuk mengecek apakah setiap client telah terhubung, gunakan `ping <IP Client>`
### Soal 4
Pada terminal Eru jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE` lalu tes dengan `ping`
<img width="995" height="317" alt="image" src="https://github.com/user-attachments/assets/2bbea819-edfe-4dd1-be8f-26cfcab137b0" />
lakukan `cat /etc/resolv.conf` untuk mendapatkan nameserver

Pada terminal Menwa, jalankan `echo nameserver 192.168.122.1 > /etc/resolv.conf` dan untuk mengecek apakah sudah terhubung dengan internet, tes dengan `ping google.com -c 5`
### Soal 5


### Soal 6
pada terminal Eru jalankan `apt update && apt install iptables -y` lalu `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.64.0.0/16
`

dilanjutkan pada terminal Menwa buka file Shell Script `nano skrip_susup.sh` lalu ubah agar bisa di execute dengan `chmod +x skrip_susup.sh` dan masukkan kode
```
#!/bin/bash

echo "Memulai pembuatan traffic jaringan selama 10 detik..."

cleanup() {
    echo ""
    echo "Menghentikan semua proses pembuat traffic..."
    kill $(jobs -p) > /dev/null 2>&1
    echo "Selesai."
}

trap cleanup EXIT

ping -i 0.1 8.8.8.8 > /dev/null 2>&1 &

wget -qO /dev/null http://speedtest.tele2.net/10MB.zip &
wget -qO /dev/null http://proof.ovh.net/files/10Mb.dat &

nmap -T4 -F scanme.nmap.org > /dev/null 2>&1 &

sleep 10

lakukan start capture pada link antara Menwa dan Switch1 di GNS3 dan wireshark terbuka

```

# BAGIAN WIRESHARK ( 14-20 )

### Soal 14

### Langkah Pengerjaan
Sebelum Mengeksplor WireShark, cari packets are recorded yang ada di .pcapng. 
Dengan masuk --> Statistics --> Capture File Properties

<img width="1187" height="652" alt="image" src="https://github.com/user-attachments/assets/33288602-d33d-4ea4-95ae-79cee32554b5" />

Didapat 500358 packages. 

A. Filtering seluruh Keyword di HTTP . Identifikasi masing masing dan ditemukan Beberapa username dan Password yang berbeda dengan Akhiran Invalid Credentials. 

<img width="534" height="328" alt="Screenshot 2025-10-01 222820" src="https://github.com/user-attachments/assets/5d0ef0c6-16c6-4bf6-af33-f720db6b387c" />

B. Saya mencoba Filter dengan keyword “LOGIN” , Karena saya berpikir Dengan kata kunci login. Bisa menemukan username dan password yang asli. Pakai filter http contains "Login". 

<img width="457" height="328" alt="image" src="https://github.com/user-attachments/assets/6ec0e34b-ccb5-49c9-bdba-21b78b760407" />

C. Ditemukan Username dan Password yang sebenarnya tinggal masukin ke NetCat

<img width="817" height="492" alt="image" src="https://github.com/user-attachments/assets/c4e7db76-cb97-4fc1-9e07-596865573006" />

### Soal 15

### Langkah Pengerjaan
1. Identifikasi Setiap jaringan usb di Wireshark
2. Bakal notice sesuatu yaitu HID DATA
3. Convert HID Data ke kata seharusnya
4. Didapatkan code berbentuk base64
5. Decode Base 64tersebut dan ketemu hasilnya

Dengan Interaksi Terminal Berikut
```
- tshark -r hiddenmsg.pcapng -Y "usb.transfer_type == 0x01 && usb.data_len == 8" -T fields -e usb.capdata
- tshark -r hiddenmsg.pcapng -Y "usb.transfer_type == 0x01 && usb.data_len == 8 && usb.capdata[2:1] != 00" -T fields -e usb.capdata
- tshark -r hiddenmsg.pcapng -Y "usb.transfer_type == 0x01 && usb.data_len == 8" -T fields -e frame.time -e usb.capdata
- tshark -r hiddenmsg.pcapng -Y "usb.transfer_type == 0x01 && usb.data_len == 8" -T fields -e usb.capdata > usb_data.txt
- cat usb_data.txt
- tshark -r hiddenmsg.pcapng -Y "usb”
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e usb.transfer_type -e usb.endpoint_address -e usb.data_len
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e usb.capdata -e usb.data -e data.data | head -5
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e data.data
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e usb.data
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e usb.iso.data
- tshark -r hiddenmsg.pcapng -Y "usb" -T fields -e leftover_capture_data
- tshark -r hiddenmsg.pcapng -Y "usb" -V | head -50
- tshark -r hiddenmsg.pcapng -Y "usb" -V > usb_detailed.txt
- python3 convert_from_txt.py
```
<img width="928" height="378" alt="Screenshot 2025-09-30 104100" src="https://github.com/user-attachments/assets/b58e2045-37d7-4e2d-ac4d-013a2fae90fb" />

### Soal 16

### Langkah Pengerjaan
1. Cari paket yang berisi indikasi autentikasi / kredensial
- Mencari paket yang mengandung string terkait autentikasi (authorization, username, password, dll).
- -V menampilkan detail paket (verbose) untuk memeriksa payload/headers di paket yang cocok.
- Tujuan: menemukan kredensial plaintext atau header otentikasi (mis. HTTP auth, form fields, dsb).
2. Ringkasan dan filter lalu lintas FTP
- Menampilkan semua paket FTP dan field penting (nomor frame, perintah FTP, argumen, kode respons).
- Filter khusus menampilkan perintah USER dan PASS untuk melihat username/password FTP yang dikirim (plaintext pada protokol FTP biasa).
- Tujuan: identifikasi sesi FTP dan kredensial yang digunakan.
3. Temukan file yang diunduh (RETR)
- Mencari perintah RETR (retrieve) untuk mengetahui nama file yang diminta/ditransfer dari server FTP.
- grep RETR mempermudah menemukan baris yang memuat perintah RETR dalam ringkasan.
4. Identifikasi aliran data FTP (ftp-data) dan stream TCP
mengetahui stream TCP mana yang membawa payload file sehingga bisa diekstrak.
5. Ekstrak payload dari stream tertentu dan reconstruct file biner
- Mengambil field data.data (hex) dari stream TCP tertentu (mis. stream 2) yang membawa data FTP.
- tr -d '\n' menghapus newline agar hex menjadi satu aliran kontinyu.
- xxd -r -p mengubah hex jadi bytes biner dan menyimpannya sebagai q.exe.
- Hasil: file q.exe direkonstruksi dari capture.
6. Ekstraksi seluruh flow TCP (alternatif / komplementer)
- tcpflow membagi pcap menjadi file per-flow (masing-masing arah).
- Cari file flow yang menyertakan string q.exe sehingga berhasil menunjuk flow yang mengandung nama file atau bagian header.
- Tujuan: cara lain untuk menemukan dan mengumpulkan data transfer tanpa langsung mem-parsing hex lewat tshark.
7. tcpflow membagi pcap menjadi file per-flow (masing-masing arah).
- Cari file flow yang menyertakan string q.exe sehingga berhasil menunjuk flow yang mengandung nama file atau bagian header.
- Tujuan: cara lain untuk menemukan dan mengumpulkan data transfer tanpa langsung mem-parsing hex lewat tshark.

<img width="1021" height="965" alt="Screenshot 2025-09-30 121229" src="https://github.com/user-attachments/assets/3ec2d6ba-5b7c-4cbb-8453-cd0d2bfa01c7" />

### Soal 17

### Langkah Pengerjaan
- Ini tinggal export HTTP ya

DAN KENAPA YANG DIEXPORT HTTP ?
1. **HTTP paling umum dipakai untuk transfer file biner** (exe, doc, zip, dll).
    - Kalau kita buka pcap, lalu `File → Export Objects → HTTP`, biasanya langsung kelihatan ada file `.exe` atau `.doc` yang ditransfer.
2. **Protokol lain punya fungsi berbeda**
    - DNS: biasanya cuma query nama domain → IP. Jarang ada file di dalamnya, kecuali ada DNS tunneling (tapi itu advance, jarang dipakai di soal dasar).
    - SMTP/IMAP/POP3: bisa ada attachment email, tapi kalau tidak ada traffic email, ya bukan ini.
    - FTP: kalau memang ada sesi FTP di capture, bisa jadi lewat situ. Tapi kalau di pcap tidak ada traffic FTP, maka tidak relevan.
    - Jadi, kita pilih HTTP karena dari traffic capture memang ada request/response file.
---

Setelah Diexport , Tinggal masukin jawabn ke ncat, sesuai pertanyaan yang ada?. 
<img width="1192" height="953" alt="Screenshot 2025-09-30 195432" src="https://github.com/user-attachments/assets/77d35070-4d11-4335-b3b3-07c7aa402f82" />

### Soal 18

### Langkah Pengerjaan

- Ini juga sama tinggal export yang memiliki protocol SMB

```
- tshark -r MelkorPlan3.pcap --export-objects "smb,./smb_files”
```

### KENAPA PAKAI SMB ? 

**SMB / SMB2** → ini penting, karena:
    1. SMB bisa membawa **file transfer**, misalnya flag disimpan di share.
    2. SMB bisa mengandung **NTLM authentication** (username & hash).
    3. SMB kadang menyimpan metadata server/domain (nama domain, user list, dll).

Makanya, soal mengarahkan ke protocol SMB , berarti clue / flag kemungkinan besar ada di:
- File yang ditransfer lewat SMB.
- Pesan NTLMSSP (bisa ekstrak hash user).
- Share enumeration (lihat share names / file names).

Tinggal Disesuaikan document yang sudah diextract dengan Pertanyaan yang muncul ketika ncat
<img width="1180" height="942" alt="Screenshot 2025-09-30 205254" src="https://github.com/user-attachments/assets/9eb6bbbe-333a-4b11-a395-56a76c7a0c55" />

### Soal 19

### Langkah Pengerjaan
1. Filter paket TCP / SMTP
2. Identifikasi Stream SMTP
3. Ikuti isi percakapan TCP Stream tertentu
4. Baca hasil ekstraksi stream dengan cat

```
- tshark -r MelkorPlan4.pcap -Y 'tcp' -T fields -e tcp.stream -e _ws.col.Info | egrep -i 'MAIL |EHLO|SMTP’
- tshark -r MelkorPlan4.pcap -Y 'tcp' -T fields -e tcp.stream -e _ws.col.Info | egrep -i 'MAIL’
- tshark -r MelkorPlan4.pcap -Y 'smtp' -T fields -e tcp.stream -e _ws.col.Info | sort -u
```

```
- tshark -r MelkorPlan4.pcap -q -z "follow,tcp,ascii,3" > stream3.txt
- cat stream3.txt
```

<img width="1191" height="754" alt="Screenshot 2025-09-30 212001" src="https://github.com/user-attachments/assets/e547d5c0-dbb2-4a4f-9ce6-03bf3405f299" />

<img width="1319" height="809" alt="image" src="https://github.com/user-attachments/assets/211d1e7d-d8a7-4445-b2f9-20e198f2b242" />

### Soal 20

### Langkah Pengerjaan
1. Mengetahui File itu TLS atau bukan dengan `tshark -r MelkorPlan5.pcap -Y "tls" -V | sed -n '1,120p’`
2. Export File Hingga menemukan data yang tersembunyi
```
- tshark -r MelkorPlan5.pcap -o tls.keylog_file:keyslogfile.txt -Y http.request -T fields -e http.request.uri | sort -u
- tshark -r MelkorPlan5.pcap -o tls.keylog_file:keyslogfile.txt --export-objects "http,./exported_http"
ls -l exported_http || true
- tshark -r MelkorPlan5.pcap -o tls.keylog_file:keyslogfile.txt -Y ftp -T fields -e ftp.request.command -e ftp.request.arg | sort -u
```
3. Membuktikan invest_20.dll adalah malware
```
file invest_20.dll
strings invest_20.dll | sed -n '1,80p’
```
4. Export to sha256sum
```
sha256sum exported_http/invest_20.dll
```

### HASIL AKHIR
<img width="991" height="321" alt="Screenshot 2025-10-01 000434" src="https://github.com/user-attachments/assets/787baa12-4b47-45dc-9c42-714a0fd61070" />




