# V7RC IO Command Protocol

[Language Index](protocol.md)

Untuk mendukung lebih banyak kebutuhan aplikasi, V7RC Protocol terbuka telah diperbarui agar memberikan kompatibilitas yang lebih baik. Tingkat dukungan aktual dapat berbeda tergantung papan pengembangan dan versi firmware yang digunakan.

## Aturan Dasar

Karena paket transmisi BLE memiliki batas ukuran, setiap perintah dibatasi hingga **20 Bytes atau kurang**.
Format utama yang saat ini didukung adalah 20 Bytes, sedangkan format lama 16 Bytes sudah tidak didukung lagi pada APP versi baru.

- **Format perintah**: 3 karakter pertama adalah kode perintah.
- **Penanda akhir**: Setiap paket diakhiri dengan `#`.
- **Aturan padding**: Kolom yang tidak digunakan diisi dengan `0`, sedangkan perintah `CMD` diisi menggunakan spasi.

## Penjelasan Detail Perintah Kontrol

### `SRV` (Kontrol PWM Dasar)
- **Deskripsi**: Perintah PWM standar dengan panjang total 20 Bytes.
- **Contoh**:
- `SRV1500150015001500#`: Mendukung sinyal PWM untuk P0, P1, dan P2. Versi standar Microbit biasanya mendukung tiga keluaran PWM.
- `SRV1500100018002000#`: Mendukung sinyal PWM untuk C1, C2, C3, dan C4.
- **Catatan**: Format lama 16 Bytes seperti `SRV150015001500#` sudah tidak didukung oleh APP versi baru dan sebaiknya tidak digunakan lagi.

### `SR2` (Grup PWM Kedua)
- **Deskripsi**: Jika papan menyediakan lebih dari empat kanal PWM, perintah ini digunakan untuk mengontrol servo C5 sampai C8.
- **Contoh**:
- `SR21500100018002000#`: Sesuai dengan sinyal PWM untuk C5, C6, C7, dan C8.

### `SS8` (PWM Sederhana untuk 8 Kanal)
- **Deskripsi**: Setiap nilai PWM disederhanakan menjadi nilai heksadesimal `00` sampai `FF`, dengan presisi yang lebih rendah. Ubah nilai tersebut ke desimal terlebih dahulu, lalu kalikan 10 untuk mendapatkan nilai PWM.
- **Contoh**:
- `SS896969696969696#`: `96` menjadi 150 dalam desimal, lalu 150 dikalikan 10 menjadi 1500.

### `SSD` (Kontrol Perangkat Sederhana dengan Tombol, Belum Diimplementasikan)
- **Deskripsi**: Setiap output PWM menggunakan 3 digit untuk sudut, dan 4 digit terakhir mewakili empat status GPIO menggunakan `0` atau `1`.
- **Contoh**:
- `SSD0901801800001100#`
- C1 adalah 90 derajat, C2 adalah 180 derajat, C3 adalah 180 derajat, dan C4 adalah 0 derajat.
- GPIO0 adalah ON, GPIO1 adalah ON, GPIO2 adalah OFF, dan GPIO3 adalah OFF.

### `SRT` (Mode Tank Berbasis PWM Dasar)
- **Deskripsi**: Variasi dari perintah PWM dasar. CH1 dan CH2 akan diubah di sisi firmware menjadi sinyal kontrol tank.
- **Contoh**:
- `SRT1500150015001500#`

### `SST` (Mode Tank Berbasis Kontrol Sederhana, Belum Diimplementasikan)
- **Deskripsi**: Variasi dari perintah kontrol perangkat sederhana. CH1 dan CH2 akan diubah di sisi firmware menjadi sinyal kontrol tank.
- **Contoh**:
- `SST0901801800001100#`

## Perintah Lampu (untuk LED WS2812)

Setiap LED menggunakan empat variabel dalam format `RGBM`:

- **R (Merah)**: Nilai dari `0` hingga `F`.
- **G (Hijau)**: Nilai dari `0` hingga `F`.
- **B (Biru)**: Nilai dari `0` hingga `F`.
- **M (Mode kedip)**: Lama lampu menyala per detik. `0` berarti selalu mati, `1` berarti menyala 100ms setiap detik, dan seterusnya. Nilai `A` ke atas berarti selalu menyala.
- Contohnya, `F00A` berarti merah menyala terus, sedangkan `FFF5` berarti lampu putih menyala 500ms dan mati 500ms setiap detik.

### `LED` (Grup Pertama 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LED0111100000111110000022222#`: Mengontrol grup LED depan.

### `LE2` (Grup Kedua 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LE20111100000111110000022222#`: Mengontrol grup LED kedua.

### `LE?` (Grup ke-N, 4 LED)
- **Deskripsi**: `?` diganti dengan nomor grup `N` untuk mengontrol grup ke-N yang berisi empat LED.
- **Contoh**:
- `LE50111100000111110000022222#`: Mewakili grup LED kelima.

## Perintah Kustom Perangkat

### `CMD` (Perintah Pass-Through ke Perangkat)
- **Deskripsi**: Perintah ini meneruskan data langsung ke sisi perangkat, dan maknanya sepenuhnya ditentukan oleh pengembang perangkat. Panjang maksimum perintah adalah 16 karakter, tidak termasuk prefiks dan penanda akhir. Jika isi perintah kurang dari 16 karakter, tambahkan spasi sebagai padding.
- **Contoh**:
- Jika perintahnya adalah `RESET`, paket lengkapnya adalah `CMDRESET          #`.

