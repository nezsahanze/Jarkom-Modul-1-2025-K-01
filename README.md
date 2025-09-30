# Jarkom-Modul-1-2025-K-01

| Nama                                  | NRP        |
| ------------------------------------- | ---------- | 
| Muhammad Fatihul Qolbi Ash Shiddiqi   | 5027241023 | 
| Hansen Chang                          | 5027241028 |        

### Soal 1
<img width="1114" height="504" alt="image" src="https://github.com/user-attachments/assets/8a664953-6377-42f6-9b87-5cce23e49386" />

### Soal 2
<img width="1019" height="321" alt="image" src="https://github.com/user-attachments/assets/dd5d33f2-4003-4ccc-b86b-30732e8caf65" />

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

### Soal 4
Pada terminal Eru jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE` lalu tes dengan `ping`
<img width="995" height="317" alt="image" src="https://github.com/user-attachments/assets/2bbea819-edfe-4dd1-be8f-26cfcab137b0" />

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

```

lakukan start capture pada link antara Menwa dan Switch1 di GNS3 dan wireshark terbuka

<img width="1319" height="809" alt="image" src="https://github.com/user-attachments/assets/211d1e7d-d8a7-4445-b2f9-20e198f2b242" />

