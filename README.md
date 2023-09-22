# Laporan Resmi Modul 1 Jarkom 2023

Kelompok **B15**

Anggota:

- [MH] Muhammad Hidayat (05111940000131)
- [VG] Victor Gustinova (5025211159)

## Soal 1 [MH]

Filter yang digunakan pertama adalah `ftp.request.command == "STOR"` untuk perintah upload dan ditemukan frame 147.

<!-- TODO change wikilink format -->

![](img/Pasted%20image%2020230918194148.png)

![](img/Pasted%20image%2020230918194952.png)

Didapatkan nilai berikut:
- sequence number (raw): 258040667
- acknowledgement number (raw): 1044861039

Diketahui nama filenya adalah `"c75-GrabThePhisher.zip"`, jadi dicari dengan filter
`ftp.response.arg contains "c75-GrabThePhisher.zip"` dan ditemukan pada frame 149.

![](img/Pasted%20image%2020230918194311.png)

![](img/Pasted%20image%2020230918194913.png)

Didapatkan nilai berikut:
- sequence number (raw): 1044861039
- acknowledgement number (raw): 258040696

Berikut kedua frame yang dimaksud:

![](img/Screenshot_20230921_212543.png)

Dari sana dapat didapatkan flagnya.

![](img/Pasted%20image%2020230918194518.png)

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

Awalnya kami mencari IP 239.255.255.250. Kami menemukan bahwa UDP dan SSDP (melalui protokol UDP) memberikan informasi port. Ini yang mendasari jawaban kami untuk (b).

Kalimat display filter yang digunakan:

`ip.host == 239.255.255.250 && udp.port == 3702`

Untuk (a), banyak paket yang tampil dengan filter di atas terdapat 21. Ini didapat dari pojok kanan bawah tampilan Wireshark.

![](img/Pasted%20image%2020230918193211.png)

![](img/Screenshot_20230918_192010.png)

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

Awalnya sempat ada kendala karena kesalahan pada file `soal5.pcapng`. Setelah adanya koreksi dengan `soal5.pcap`, soal dapat dikerjakan.

a. Banyak packet yang ditangkap adalah 60.

   ![](img/Pasted%20image%2020230918210803.png)

b. Port SMTP adalah port 25.

c. Hanya ada satu public IP, yaitu 74.53.140.153

Pertama, dicari adanya data fragment dalam email menggunakan display filter `smtp.data.fragment`. Ditemukan satu frame (no. 45) yang mengandung beberapa boundary.

![](img/Screenshot_20230921_221044.png)

Boundary pertama adalah
```
------=_NextPart_000_0004_01CA45B0.095693F0
```

maka dicari frame pertama yang memenuhi. Filter yang digunakan adalah sebagai berikut dan ditemukan pada frame 22.

```
smtp contains "------=_NextPart_000_0004_01CA45B0.095693F0"
```

Ditemukan isi email yang bersi password pada frame tersebut:

```
Hello

I send u a p45sword of a zip file, but you should decode it in Base64.

Here is the p45sword:

NWltcGxlUGFzNXdvcmQ=
```

![](img/Screenshot_20230921_223013.png)

> RALAT: Sebenarnya keseluruhan konten email dapat dilihat dari frame 45 tersebut. Frame 22 hanya mengandung sebagian saja (_fragment_). Kebetulan frame inilah yang mengandung bagian yang terdpat password untuk file zip.

Kemudian password di-decode menggunakan `base64` yang terpasang di sistem Linux saya. Password ini digunakan untuk membuka file `zipppfileee.zip`. Isi dari zip tersebut adalah sebuah file teks berisikan perintah untuk menyambungkan ke instance tertentu untuk menjawab pertanyaan dan mendapatkan flag.

![](img/Pasted%20image%2020230918211046.png)
![](img/Pasted%20image%2020230918211001.png)
![](img/Pasted%20image%2020230918211112.png)
![](img/Pasted%20image%2020230918210744.png)

> Kowalski, analysis.

(also love the penguins reference)

Fun fact: sisa dari isi email tersebut mengandung changelog untuk Dev-C++ 4.9.9.0.

- [forum NeoWin](https://www.neowin.net/forum/topic/198159-dev-c-4990-released/)

## Soal 6 [VG]

### Seorang anak bernama Udin Berteman dengan SlameT yang merupakan seorang penggemar film detektif. sebagai teman yang baik, Ia selalu mengajak slamet untuk bermain valoranT bersama. suatu malam, terjadi sebuah hal yang tak terdUga. ketika udin mereka membuka game tersebut, laptop udin menunjukkan sebuah field text dan Sebuah kode Invalid bertuliskan "server SOURCE ADDRESS 7812 is invalid". ketika ditelusuri di google, hasil pencarian hanya menampilkan a1 e5 u21. jiwa detektif slamet pun bergejolak. bantulah udin dan slamet untuk menemukan solusi kode error tersebut.

Pada saat mengerjakan soal ini kami membuka semua hint yang ada. Menggunakan hint tersebut maka kami menggabungkan huruf kapital yang ada pada soal dan didapatkan SUBSTITUSI. Substitusi apa yang digunakan? soal memberikan klue melalui a1 e5 u21 yang berarti angka akan sesuai dengan urutan alphabet dan begitu juga sebaliknya. Namun kami hanya dapat mengerjakan soal hingga tahap ini.

Selanjutnya kami berdiskusi lebih lanjut dan menemukan bahwa perlu dilakukan substitusi pada ip source address dari paket nomor 7812. 

```104.18.14.101```

![image](https://github.com/VictorGstn/jarkom-modul-1-b15-2023/assets/125529445/c850ea28-c864-4060-83b7-8431f52e3b63)

Menggunakan hint bahwa jawabannya sepanjang 6 huruf maka dibagi IP tersebut ke 6 angka yang mungkin menjadi abjad.

```10 4 18 14 10 1```

Lalu dilakukan substitusi dan didapat **JDRNJA**


## Soal 7 [MH]

Sangat mudah, filternya `ip.dst_host == 184.87.193.88`, ditemukan 6 packet.

![](img/Pasted%20image%2020230918195305.png)

![](img/Screenshot_20230921_214305.png)

![](img/Pasted%20image%2020230918195317.png)

## Soal 8 [VG]
### Berikan kueri filter sehingga wireshark hanya mengambil semua protokol paket yang menuju port 80! (Jika terdapat lebih dari 1 port, maka urutkan sesuai dengan abjad)

Gunakan `dstport` untuk port keluar. Karena diminta urut maka bisa dilakukan operasi or sehingga kueri filter menjadi seperti berikut.

`tcp.dstport == 80 || udp.dstport == 80`

Yang melakukan submit adalah [MH]

## Soal 9 [MH]

File pcap tidak digunakan. Hanya query-nya saja yang diminta.

`ip.src == 10.51.40.1 && ip.dst != 10.39.55.34`

![](img/Pasted%20image%2020230918202647.png)

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
