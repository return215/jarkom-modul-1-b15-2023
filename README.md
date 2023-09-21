# Laporan Resmi Modul 1 Jarkom 2023

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

![[WhatsApp Image 2023-09-18 at 21.59.15.jpeg]]

![[WhatsApp Image 2023-09-18 at 21.59.03.jpeg]]

![[WhatsApp Image 2023-09-18 at 21.59.36.jpeg]]
## Soal 3 [MH]

Awalnya kami mencari IP 239.255.255.250. Kami menemukan bahwa UDP dan SSDP (melalui protokol UDP) memberikan informasi port. Ini yang mendasari jawaban kami untuk (b).

Kalimat display filter yang digunakan:

`ip.host == 239.255.255.250 && udp.port == 3702`

Untuk (a), banyak paket yang tampil dengan filter di atas terdapat 21. Ini didapat dari pojok kanan bawah tampilan Wireshark.

![](img/Pasted%20image%2020230918193211.png)

![](img/Screenshot_20230918_192010.png)

## Soal 4 [VG]

![[WhatsApp Image 2023-09-18 at 21.59.36.jpeg]]

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

unsolved

## Soal 7 [MH]

Sangat mudah, filternya `ip.dst_host == 184.87.193.88`, ditemukan 6 packet.

![](img/Pasted%20image%2020230918195305.png)

![](img/Screenshot_20230921_214305.png)

![](img/Pasted%20image%2020230918195317.png)

## Soal 8 [VG]

Gunakan `dstport` untuk port keluar.

`tcp.dstport == 80 || udp.dstport == 80`

Yang melakukan submit adalah [MH]

![[Pasted image 20230918212111.png]]

![[WhatsApp Image 2023-09-18 at 22.00.18.jpeg]]

## Soal 9 [MH]

File pcap tidak digunakan. Hanya query-nya saja yang diminta.

`ip.src == 10.51.40.1 && ip.dst != 10.39.55.34`

![](img/Pasted%20image%2020230918202647.png)

## Soal 10 [VG]

![[WhatsApp Image 2023-09-18 at 22.01.00.jpeg]]

![[WhatsApp Image 2023-09-18 at 22.01.08.jpeg]]![[WhatsApp Image 2023-09-18 at 22.01.31.jpeg]]![[WhatsApp Image 2023-09-18 at 22.01.37.jpeg]]![[WhatsApp Image 2023-09-18 at 22.01.45.jpeg]]![[WhatsApp Image 2023-09-18 at 22.01.53.jpeg]]![[WhatsApp Image 2023-09-18 at 22.01.58.jpeg]]![[WhatsApp Image 2023-09-18 at 22.02.07.jpeg]]![[WhatsApp Image 2023-09-18 at 22.02.14.jpeg]]

