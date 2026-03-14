# V7RC IO Command Protocol

[Language Index](protocol.md)

Bagi memenuhi lebih banyak keperluan aplikasi, V7RC Protocol terbuka telah disemak semula untuk memberikan keserasian yang lebih baik. Tahap sokongan sebenar mungkin berbeza mengikut papan pembangunan dan versi perisian tegar.

## Peraturan Asas

Disebabkan had saiz paket BLE, setiap arahan dihadkan kepada **20 Bytes atau kurang**.
Format utama yang disokong pada masa ini ialah 20 Bytes, manakala format lama 16 Bytes tidak lagi disokong oleh APP baharu.

- **Format arahan**: 3 aksara pertama ialah kod arahan.
- **Penanda penamat**: Setiap paket berakhir dengan `#`.
- **Peraturan pelengkap**: Medan yang tidak digunakan diisi dengan `0`, manakala arahan `CMD` menggunakan ruang kosong sebagai pelengkap.

## Huraian Arahan Kawalan

### `SRV` (Kawalan PWM Asas)
- **Penerangan**: Arahan kawalan PWM standard dengan panjang keseluruhan 20 Bytes.
- **Contoh**:
- `SRV1500150015001500#`: Menyokong isyarat PWM untuk P0, P1 dan P2. Versi standard Microbit biasanya menyokong tiga keluaran PWM.
- `SRV1500100018002000#`: Menyokong isyarat PWM untuk C1, C2, C3 dan C4.
- **Nota**: Format lama 16 Bytes seperti `SRV150015001500#` tidak lagi disokong oleh APP baharu dan tidak sepatutnya digunakan.

### `SR2` (Kumpulan PWM Kedua)
- **Penerangan**: Jika papan menyediakan lebih daripada empat saluran PWM, arahan ini digunakan untuk mengawal servo C5 hingga C8.
- **Contoh**:
- `SR21500100018002000#`: Sejajar dengan isyarat PWM bagi C5, C6, C7 dan C8.

### `SS8` (PWM Ringkas untuk 8 Saluran)
- **Penerangan**: Setiap nilai PWM diringkaskan kepada nilai heksadesimal `00` hingga `FF`, dengan ketepatan yang lebih rendah. Tukarkan nilainya ke perpuluhan dahulu, kemudian darab 10 untuk mendapatkan nilai PWM.
- **Contoh**:
- `SS896969696969696#`: `96` menjadi 150 dalam perpuluhan, kemudian 150 didarab 10 menjadi 1500.

### `SSD` (Kawalan Peranti Ringkas dengan Butang, Belum Dilaksanakan)
- **Penerangan**: Setiap output PWM menggunakan 3 digit untuk sudut, dan 4 digit terakhir mewakili empat status GPIO menggunakan `0` atau `1`.
- **Contoh**:
- `SSD0901801800001100#`
- C1 ialah 90 darjah, C2 ialah 180 darjah, C3 ialah 180 darjah, dan C4 ialah 0 darjah.
- GPIO0 ialah ON, GPIO1 ialah ON, GPIO2 ialah OFF, dan GPIO3 ialah OFF.

### `SRT` (Mod Tank Berasaskan PWM Asas)
- **Penerangan**: Variasi kepada arahan PWM asas. CH1 dan CH2 akan ditukar di peringkat perisian tegar menjadi isyarat kawalan tank.
- **Contoh**:
- `SRT1500150015001500#`

### `SST` (Mod Tank Berasaskan Kawalan Ringkas, Belum Dilaksanakan)
- **Penerangan**: Variasi kepada arahan kawalan peranti ringkas. CH1 dan CH2 akan ditukar di peringkat perisian tegar menjadi isyarat kawalan tank.
- **Contoh**:
- `SST0901801800001100#`

## Arahan Lampu (untuk LED WS2812)

Setiap LED menggunakan empat pemboleh ubah dalam bentuk `RGBM`:

- **R (Merah)**: Nilai dari `0` hingga `F`.
- **G (Hijau)**: Nilai dari `0` hingga `F`.
- **B (Biru)**: Nilai dari `0` hingga `F`.
- **M (Mod berkelip)**: Tempoh nyalaan bagi setiap saat. `0` bermaksud sentiasa padam, `1` bermaksud menyala 100ms setiap saat, dan seterusnya. Nilai `A` ke atas bermaksud sentiasa menyala.
- Contohnya, `F00A` bermaksud merah menyala berterusan, manakala `FFF5` bermaksud cahaya putih menyala 500ms dan padam 500ms setiap saat.

### `LED` (Kumpulan Pertama 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LED0111100000111110000022222#`: Mengawal kumpulan LED hadapan.

### `LE2` (Kumpulan Kedua 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LE20111100000111110000022222#`: Mengawal kumpulan LED kedua.

### `LE?` (Kumpulan LED ke-N, 4 LED)
- **Penerangan**: `?` digantikan dengan nombor kumpulan `N` untuk mengawal kumpulan LED ke-N.
- **Contoh**:
- `LE50111100000111110000022222#`: Mewakili kumpulan LED kelima.

## Arahan Tersuai Peranti

### `CMD` (Arahan Laluan Terus ke Peranti)
- **Penerangan**: Arahan ini menghantar data terus ke bahagian peranti, dan maknanya ditentukan sepenuhnya oleh pembangun peranti. Panjang maksimum arahan ialah 16 aksara, tidak termasuk awalan dan penanda penamat. Jika kandungan lebih pendek daripada 16 aksara, tambah ruang kosong sehingga cukup panjang.
- **Contoh**:
- Jika arahannya ialah `RESET`, paket penuh ialah `CMDRESET          #`.

