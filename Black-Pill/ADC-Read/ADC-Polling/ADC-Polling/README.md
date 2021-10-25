# ADC Polling

### 1. Selayang Pandang
Pada repo ini, dilakukan pembacaan nilai ADC dengan menggunakan **metode _Polling_** atau bahasa gampangnya dengan menggunakan CPU yang mana proses pembacaan mengakibatkan *blocking* sehingga MCU tidak dapat melakukan aktifitas lain atau *serial*

<img src="./asset/EntireSystem.jpeg" width="500">

Mengapa _blocking_? <br>
Bayangin ini semua diproses CPU dan dilakukan secara _serial_ sehingga dijalanin secara berurutan setiap proses yang terjadi. Sehingga jika ada pembacaan dengan menggunakan **metode _Polling_** maka dilakukan berurutan sampai beres baru dilanjut proses lain.

### 2. Penjabaran Code
Berikut yang perlu diperhatikan dalam penjabaran code repo ini

<img src="./asset/adc-poll1.png" width="500">