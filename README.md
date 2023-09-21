# Laporan Resmi Modul 1 Jarkom 2023

Anggota:
- [MH] Muhammad Hidayat (05111940000131)
- [VG] Victor Gustinova (5025211159)

## Soal 1 [MH]

Filter yang digunakan pertama adalah `ftp.request.command == "STOR"` untuk perintah upload dan ditemukan frame 147.

<!-- TODO change wikilink format -->

![[Pasted image 20230918194148.png]]
![[Pasted image 20230918194952.png]]

Diketahui nama filenya adalah `"c75-GrabThePhisher.zip"`, jadi dicari dengan filter
`ftp.response.arg contains "c75-GrabThePhisher"` dan ditemukan pada frame 149.

![[Pasted image 20230918194311.png]]
![[Pasted image 20230918194913.png]]

Dari sana dapat didapatkan flagnya.

![[Pasted image 20230918194518.png]]

## Soal 2 [VG]
### Sebutkan web server yang digunakan pada portal praktikum Jaringan Komputer!
Nama web server dapat dilihat pada bagian Hypertext Transfer Protocol pada paket yang berasal dari ip 10.21.78.111 (portal praktikum).
Mencari packet dapat menggunakan display filter berikut.
```
ip.src == 10.21.78.111
```
![image](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/40eac6e5-27ed-4d8c-8a83-3e32912e93a1)

Didapatkan web server **gunicorn**

## Soal 3 [MH]

Awalnya kami mencari IP 239.255.255.250. Kami menemukan bahwa SSDP dan UDP memberikan informasi port, jadi kami coba filter berdasarkan itu. Kami temukan dari keduanya bahwa hanya UDP yang memiliki property `port`.  Ini yang mendasari jawaban kami untuk (b).

Kalimat display filter yang digunakan:

`ip.host == 239.255.255.250 && udp.port == 3702`

Untuk (a), banyak paket yang tampil dengan filter di atas terdapat 21. Ini didapat dari pojok kanan bawah tampilan Wireshark.

![[Pasted image 20230918193211.png]]

![[Screenshot_20230918_192010.png]]

## Soal 4 [VG]
### Berapa nilai checksum yang didapat dari header pada paket nomor 130?

Nilai checksum bisa dilihat langsung jika mengklik paket nomor tersebut. Akan tetapi, mungkin saja terlihat seperti berikut.
![image](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/e51b504a-8e39-4814-8f0b-85464b2700d2)

Agar checksum bisa divalidasi maka dapat diklik kanan, pilih protocol preferences, dan pilih validate checksum if possible.
![image](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/82c7424c-aa87-46e2-8dce-43219bd00124)

Setelah itu akan terlihat nilai checksum yang benar.
![image](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/8169ce5b-902b-41ed-8945-e6ec3f4183ea)

Didapatkan nilai checksum **0x3656**

## Soal 5 [MH]

a. Banyak packet yang ditangkap adalah 60.
   ![[Pasted image 20230918210803.png]]
b. Port SMTP adalah port 25.
c. Hanya ada satu public IP, yaitu 74.53.140.153

Ditemukan isi email yang bersi password pada frame 22:

```
Hello

 

I send u a p45sword of a zip file, but you should decode it in Base64.

Here is the p45sword:

NWltcGxlUGFzNXdvcmQ=
```

![[Pasted image 20230918211046.png]]
![[Pasted image 20230918211001.png]]
![[Pasted image 20230918211112.png]]
![[Pasted image 20230918210744.png]]

## Soal 6 [VG]

unsolved

## Soal 7 [MH]

Sangat mudah, filternya `ip.dst_host == 184.87.193.88`, ditemukan 6 packet.

![[Pasted image 20230918195305.png]]

![[Pasted image 20230918195317.png]]

## Soal 8 [VG]

Gunakan `dstport` untuk port keluar.

`tcp.dstport == 80 || udp.dstport == 80`

Yang melakukan submit adalah [MH]

![[Pasted image 20230918212111.png]]

![[WhatsApp Image 2023-09-18 at 22.00.18.jpeg]]

## Soal 9 [MH]

Tidak perlu melakukan filter pada file pcap. Hanya query-nya saja.

`ip.src == 10.51.40.1 && ip.dst != 10.39.55.34`

![[Pasted image 20230918202647.png]]

## Soal 10 [VG]
### Sebutkan kredensial yang benar ketika user mencoba login menggunakan Telnet
Jika user login menggunakan telnet maka pada paket telnet akan ada data yang berisi Login: ... dan Password: ....

Dengan melihat data pada paket telnet maka kredensial bisa didapatkan.

Pertama gunakan ```telnet``` sebagai display filter lalu cek data paket sehingga terlihat Login: dan Password:

![WhatsApp Image 2023-09-18 at 21 55 52](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/0270cdac-75f5-4871-ad9a-e8e2a29e67c4)
![WhatsApp Image 2023-09-18 at 21 56 15](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/bca8745e-d26a-4197-9360-3c7ef5c6c32d)
![WhatsApp Image 2023-09-18 at 21 56 24](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/5dd87278-1c56-4492-958e-e927ee386be7)
![WhatsApp Image 2023-09-18 at 21 56 32](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/03d62f9e-fb18-4b0b-b920-cbb28939d578)
![WhatsApp Image 2023-09-18 at 21 56 40](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/568114db-6da1-4fdd-bebc-0a9632e72b36)
![WhatsApp Image 2023-09-18 at 21 56 53](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/746d60f2-6108-4f2d-b0dc-cbb8a0e61ad5)
![WhatsApp Image 2023-09-18 at 21 57 01](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/0e1eb574-8667-4a8d-a231-f9fd4159cbed)


![WhatsApp Image 2023-09-18 at 21 55 13](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/7114641a-254b-40b1-8a69-4d60aab325be)
![WhatsApp Image 2023-09-18 at 21 55 31](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/c92c44a4-765d-4492-93d0-eb22fe178e53)

Didapatkan Login: Dhafin dan Password: kesayangannyak0k0
Data ini tersebar di paket nomor 236 - 262
