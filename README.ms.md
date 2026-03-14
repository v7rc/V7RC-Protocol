# V7RC-Protocol

[Language Index](README.md)

## Sebelum Bermula
Repositori ini digunakan untuk mentakrifkan V7RC Protocol yang digunakan oleh V7RC APP dan papan pembangunan MCU yang berkaitan. Lazimnya pasukan V7RC akan merancang serta melepaskan ciri baharu pada APP terlebih dahulu, kemudian makers boleh merujuk dokumen ini untuk membangunkan papan atau peranti mereka sendiri. Jika anda mempunyai sebarang soalan semasa membaca dokumen ini, sila hubungi kami.

## Tentang V7RC Protocol
V7RC Protocol direka berasaskan konsep BLE Payload. Oleh sebab data perlu dihantar ke papan pembangunan MCU melalui WIFI dan BLE, protokol ini direka supaya serasi dengan kebanyakan senario penghantaran WIFI dan BLE. Setiap V7RC Payload mempunyai panjang tetap 20 Bytes. V7RC APP menghantar dan menerima paket dalam format ini, jadi dengan mengikuti spesifikasi ini, peranti anda boleh diintegrasikan dengan lebih mudah ke dalam V7RC APP untuk kawalan jauh.

## Format Payload
Contoh:

`SRV1500150015001500#`

1. Panjang payload mestilah 20 Bytes, dan mungkin diperluaskan pada masa hadapan.
2. 3 Bytes pertama ialah rentetan arahan. Dalam contoh ini, `SRV` bermaksud kawalan saluran motor PWM.
3. Payload sentiasa berakhir dengan `#`, supaya setiap paket mudah dikenal pasti.
4. 16 Bytes di bahagian tengah ialah ruang data. Kebanyakan arahan menggunakan data ASCII, manakala sesetengah arahan seperti `SS8` menggunakan nilai padat berbentuk ringkas.

## Arahan Biasa
- `SRV`
- `SRT`
- `SS8`

## Fasa Seterusnya
Untuk penerangan protokol penuh, rujuk [protocol.ms.md](protocol.ms.md).

