# V7RC-Protocol

[Language Index](README.md)

## Sebelum Memulai
Repositori ini digunakan untuk mendefinisikan V7RC Protocol yang dipakai oleh V7RC APP dan papan pengembangan MCU terkait. Biasanya tim V7RC akan merencanakan dan merilis fitur baru di APP terlebih dahulu, lalu makers dapat menggunakan dokumen ini untuk mengembangkan papan atau perangkat mereka sendiri. Jika Anda memiliki pertanyaan saat membaca dokumen ini, silakan hubungi kami.

## Tentang V7RC Protocol
V7RC Protocol dirancang berdasarkan konsep BLE Payload. Karena data perlu dikirim ke papan pengembangan MCU melalui WIFI dan BLE, protokol ini dibuat agar kompatibel dengan skenario transmisi WIFI dan BLE yang umum. Setiap V7RC Payload memiliki panjang tetap 20 Bytes. V7RC APP mengirim dan menerima paket dalam format ini, sehingga dengan mengikuti spesifikasi ini, perangkat Anda dapat lebih mudah diintegrasikan ke V7RC APP untuk kendali jarak jauh.

## Format Payload
Contoh:

`SRV1500150015001500#`

1. Panjang payload harus 20 Bytes dan mungkin diperluas di masa mendatang.
2. 3 Bytes pertama adalah string perintah. Pada contoh ini, `SRV` berarti kontrol kanal motor PWM.
3. Payload selalu diakhiri dengan `#` agar setiap paket mudah dikenali.
4. 16 Bytes di tengah adalah area data. Sebagian besar perintah menggunakan data ASCII, sedangkan beberapa perintah seperti `SS8` memakai nilai ringkas yang lebih padat.

## Perintah Umum
- `SRV`
- `SRT`
- `SS8`

## Tahap Berikutnya
Untuk deskripsi protokol lengkap, lihat [protocol.id.md](protocol.id.md).

