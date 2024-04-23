# Steganografi LSB (Least Significant Bit) Dasar

Steganografi LSB merupakan salah satu teknik steganografi yang paling sederhana dan umum digunakan. Teknik ini memanfaatkan bit paling tidak signifikan dari nilai piksel dalam gambar untuk menyembunyikan informasi tambahan, seperti pesan teks. Meskipun sederhana, steganografi LSB tetap efektif karena perubahan pada bit paling tidak signifikan umumnya tidak terlihat secara visual pada gambar. Pustaka yang digunakan sangat dasar, hanya menggunakan PIL dan numpy.

## Teknik Steganografi LSB

Dalam LSB, pesan teks atau data lainnya disematkan ke dalam gambar dengan mengubah bit paling tidak signifikan dari setiap nilai piksel. Proses ini dilakukan dengan cara berikut:

1. **Membaca Gambar**: Gambar yang akan digunakan sebagai wadah untuk menyembunyikan pesan dibaca. Dalam kode Python, hal ini dilakukan menggunakan fungsi `tambah_gambar(image_path)` yang menggunakan modul PIL (Python Imaging Library) dan NumPy untuk membaca gambar dan mengonversinya menjadi array NumPy.

2. **Konversi Pesan ke Biner**: Pesan teks yang akan disembunyikan dalam gambar dikonversi menjadi representasi biner. Setiap karakter dalam pesan diubah menjadi representasi biner dengan panjang 8 bit (byte). Ini dilakukan menggunakan fungsi `teks_binary(text)`.

3. **Menyembunyikan Pesan dalam Gambar**: Pesan biner kemudian disematkan dalam gambar menggunakan teknik LSB. Setiap bit pesan disematkan ke bit paling tidak signifikan dari nilai piksel dalam gambar. Ini dilakukan dalam fungsi `pesan_tersembunyi(im, pesan_biner)`.

4. **Ekstraksi Pesan Tersembunyi**: Ketika diperlukan, pesan yang tersembunyi dalam gambar dapat diekstrak menggunakan fungsi `ekstrak_pesan(sembunyikan_pesan)`. Fungsi ini mengekstrak bit paling tidak signifikan dari setiap nilai piksel dalam gambar dan mengonversinya kembali menjadi pesan biner.

5. **Konversi Biner ke Teks**: Pesan biner yang diekstrak kemudian dikonversi kembali menjadi pesan teks yang dapat dibaca manusia menggunakan fungsi `binary_to_text(binary_message)`.

## Penjelasan Kode Python

Berikut adalah penjelasan untuk setiap bagian kode Python yang signifikan dalam eksekusi program tersebut:

### Membaca Gambar

```python
def tambah_gambar(image_path):
    return np.array(Image.open(image_path))
```
Fungsi tambah_gambar digunakan untuk membaca gambar dari file yang ditentukan dan mengonversinya menjadi array NumPy. Ini dilakukan dengan menggunakan modul PIL (Python Imaging Library) dan NumPy.

### Konversi pesan ke Biner
```python
def teks_binary(text):
    binary_message = ''.join(format(ord(char), '08b') for char in text)
    return binary_message
```
Fungsi teks_binary mengonversi pesan teks menjadi representasi biner. Setiap karakter dalam pesan diubah menjadi representasi biner dengan panjang 8 bit (byte).

### Menyembunyikan Pesan dalam Gambar
```python
def pesan_tersembunyi(im, pesan_biner):
    index = 0
    for row in range(im.shape[0]):
        for col in range(im.shape[1]):
            for channel in range(im.shape[2]):
                if index < len(pesan_biner):
                    # im [row, col, channel] adalah agian untuk mengubah bit terkecil dari nilai piksel
                    im[row, col, channel] = im[row, col, channel] & ~1 | int(pesan_biner[index])
                    index += 1
                else:
                    return im
```
Fungsi pesan_tersembunyi menyembunyikan pesan biner dalam gambar menggunakan teknik LSB. Setiap bit pesan disematkan ke bit paling tidak signifikan dari nilai piksel dalam gambar.

### Ekstraksi Pesan Tersembunyi
```python
def ekstrak_pesan(pesan_tersembunyi):
    pesan_biner = ""
    index = 0
    message_length = len(pesan_tersembunyi) // 8  # perkiraan panjang pesan berdasarkan ukuran gambar
    while index < message_length:  # Limit ekstraksi dari perkiraan panjang pesan
        for row in range(pesan_tersembunyi.shape[0]):
            for col in range(pesan_tersembunyi.shape[1]):
                for channel in range(pesan_tersembunyi.shape[2]):
                    if index < message_length:
                        # Ekstrak bit paling tidak signifikan dari setiap piksel
                        pesan_biner += str(pesan_tersembunyi[row, col, channel] & 1)
                        index += 1
                    else:
                        return pesan_biner
    return pesan_biner
```
Fungsi ekstrak_pesan digunakan untuk mengekstrak pesan yang disembunyikan dari gambar stego. Ini mengekstrak bit paling tidak signifikan dari setiap nilai piksel dalam gambar dan mengonversinya kembali menjadi pesan biner.

### Ekstraksi Pesan Tersembunyi dan Konversi ke Teks
```python
# ekstraksi pesan
ex_pesan = ekstrak_pesan(sembunyikan_pesan)

def binary_to_text(pesan_biner):
    text = ""
    for i in range(0, len(pesan_biner), 8):
        byte = pesan_biner[i:i + 8]
        text += chr(int(byte, 2))
    return text
```
`def binary_to_text(pesan_biner):`definisi dari fungsi binary_to_text bertugas mengonversi pesan dalam format biner menjadi teks yang dapat dibaca.<br>
`for i in range(0, len(pesan_biner), 8):` Perulangan dilakukan setiap 8 bit (1 byte) dalam pesan biner.<br>
`byte = pesan_biner[i:i + 8]:` Setiap 8 bit biner diambil dari pesan biner.<br>
`text += chr(int(byte, 2)):` lalu setiap byte biner diubah menjadi bilangan bulat dan kemudian diubah menjadi karakter ASCII yang sesuai. Karakter-karakter ini ditambahkan ke variabel text.

### Terima Kasih
