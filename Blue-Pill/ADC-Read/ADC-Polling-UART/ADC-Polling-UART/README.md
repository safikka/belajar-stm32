# ADC Polling - BluePill F103C8

## 1. Selayang Pandang
Pada repo ini, ditulis hasil belajar mengenai pembacaan ADC pada STM32 khususnya dalam metode _**Polling**_.<br>
### Nah apatuh _**Polling**_ ?
Jadi, metode _polling_ itu ibarat kek serial. CPU pada MCU difokuskan untuk melakukan suatu proses dalam hal ini baca ADC, sehingga apabila di CPU ada proses lain maka dijeda dulu sampe proses baca ADC ini kelar baru lanjut ke proses lain. <br> _Just like serial did~_

<img src="./asset/EntireSystem.jpeg" width="500"><br>
Sumber gambar: Digikey

Nah dari contoh skema diatas, keliatan kan si CPU banyak bet proses yang dijalanin disana. Jadi kalo pake metode _**Polling**_ ini pastinya bikin _**blocking**_ atau ngehambat proses lain yang ada di CPU. gituu~~

## 2. Penjabaran Code
### Deklarasi Variabel
```C
// Deklarasi variabel (kebetulan ditaronya global)
// di line 48-49

uint16_t raw; // buat nampung nilai konversi ADC
char msg[10]; // nampung raw jadi string buat dikirim via uart
```
### Di dalem While Loop
```C
// Masuk ke loop utama si CPU
// di line 102

// Inisialisasi pin ADC
HAL_ADC_Start(&hadc1);

// Insialisasi Fungsi HAL ADC Polling
HAL_ADC_PollForConversion(  &hadc1,
                            HAL_MAX_DELAY);

// Variabel raw jadi wadah nilai ADC
raw = HAL_ADC_GetValue(&hadc1);

// Masukin variabel raw ke variabel msg
// Biar jadi string ditambahin \r\n
sprintf(msg,"%hu\r\n",raw);

// Kirim msg via UART
// Jan lupa di typecast dulu ke uint8_t (jadi ke HEX)
HAL_UART_Transmit(  &huart1,
                    (uint8_t *)msg,
                    strlen(msg),
                    HAL_MAX_DELAY);
HAL_Delay(10);
```

## 3. Settingan Platform.io
### Settingan platformio.ini
```ini
[env:bluepill_f103c8]
platform = ststm32
board = bluepill_f103c8
framework = stm32cube
debug_tools= stlink
upload_protocol = stlink
```
### Edit stm32f1x.cfg
Dikarenakan belajar BluePill pake board _chinese_ jadi perlu adanya modifikasi dalam settingan OpenOCD nya biar bisa diupload. <br>

Kalo Linux, akses dimari:
```shell
cd ~/.platformio/packages/tool-openocd/scripts/target/

nano stm32f1x.cfg
```
Edit bagian CPUTAPID _**0x1ba01477**_ jadi _**0x2ba01477**_ di line 42 
```cfg
if { [using_jtag] } {
    # See STM Document RM0008 Section 26.6.3
    set _CPUTAPID 0x3ba00477
} {
    # this is the SW-DP tap id not the jtag tap id
    # set _CPUTAPID 0x1ba01477 settingan ori
    set _CPUTAPID 0x2ba01477
}
```