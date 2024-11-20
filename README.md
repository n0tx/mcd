# Technical Information

## Daftar Isi

---

1. [Request Methods](#request-methods)  
2. [Form-data dan x-www-form-urlencoded](#form-data-dan-x-www-form-urlencoded)


---

1. ## `Request Methods`

Request methods adalah cara yang digunakan oleh klien (seperti browser atau aplikasi) untuk berkomunikasi dengan server   
melalui protokol HTTP.   

Beberapa metode request yang umum digunakan:

### 1. **GET**

- **Penjelasan**

  Digunakan untuk meminta data dari server tanpa mengubah keadaan server.

- **Contoh Penggunaan**

  Mengambil daftar pengguna atau detail produk.

- **Ciri-ciri**
  - Tidak membawa data dalam body request. 
  - Aman (safe) Tidak mengubah data di server.
  - Idempotent (Mengulangi request menghasilkan respons yang sama).

- **Contoh Request**

```
GET /users HTTP/1.1 Host: api.example.com
```

- **Contoh Response**
```json
{
    "status": "success",
    "data": [
        { "id": 1, "name": "John Doe" },
        { "id": 2, "name": "Jane Smith" }
    ]
}
```

### 2. **POST**

- **Penjelasan**

  Digunakan untuk mengirim data ke server untuk diproses.  
  Metode ini sering digunakan untuk membuat atau menambahkan data baru di server.

- **Contoh Penggunaan**

  - Mengisi dan mengirim formulir.
  - Mengunggah file.

- **Ciri-ciri**

  - Data dikirim dalam body request.
  - Tidak idempotent (mengulangi request bisa menyebabkan perubahan berulang).

- **Contoh Request**

```
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john.doe@example.com"
}
```

- **Contoh Response**
```json
{
    "status": "success",
    "message": "User created successfully",
    "data": {
        "id": 123,
        "name": "John Doe",
        "email": "john.doe@example.com"
    }
}
```

### 3. **PUT**

- **Penjelasan**

  Digunakan untuk memperbarui atau mengganti data di server.  
  Biasanya menggantikan seluruh data dengan data baru.

- **Contoh Penggunaan**

  - Memperbarui profil pengguna.
  - Mengganti informasi produk dalam database.
  
- **Ciri-ciri**

  - Idempotent (Permintaan yang sama menghasilkan hasil yang sama).

- **Contoh Request**

```
PUT /users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john.doe@example.com"
}
```

- **Contoh Response**
```json
{
    "status": "success",
    "message": "User updated successfully",
    "data": {
        "id": 123,
        "name": "John Doe",
        "email": "john.doe@example.com"
    }
}
```

### 4. **PATCH**

- **Penjelasan**

  Digunakan untuk memperbarui sebagian data di server.  
  Tidak seperti PUT, hanya sebagian dari data yang diperbarui.

- **Contoh Penggunaan**

  - Mengubah hanya satu atribut pada profil pengguna (misalnya nama saja).
  - Menghapus postingan dari forum.
  
- **Ciri-ciri**

  - Idempotent (dengan asumsi data yang sama dikirim dalam setiap request).

- **Contoh Request**

```
PATCH /users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
    "name": "John Doe"
}
```

- **Contoh Response**
```json
{
    "status": "success",
    "message": "User updated successfully",
    "data": {
        "id": 123,
        "name": "John Doe",
        "email": "john.doe@example.com"
    }
}
```

### 5. **DELETE**

- **Penjelasan**

  Digunakan untuk menghapus data di server.
  
- **Contoh Penggunaan**

  - Menghapus akun pengguna.
  
- **Ciri-ciri**

  - Idempotent (jika data sudah terhapus, request berikutnya tidak akan menyebabkan efek tambahan).

- **Contoh Request**
```
DELETE /users/123 HTTP/1.1
Host: api.example.com
```

- **Contoh Response**
```json
{
    "status": "success",
    "message": "User deleted successfully"
}
```

### 6. **HEAD**

- **Penjelasan**

  Sama seperti GET, tetapi hanya meminta header dari response tanpa isi body.
  
- **Contoh Penggunaan**

  - Memeriksa apakah suatu resource ada tanpa mengunduh kontennya.
  
- **Ciri-ciri**

  - Aman dan idempotent.
  - Tidak memiliki body response.

- **Contoh Request**
```
HEAD /users HTTP/1.1
Host: api.example.com
```

- **Contoh Response** (Hanya header)
```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 123
```

### 7. **OPTIONS**

- **Penjelasan**

  Digunakan untuk meminta informasi tentang metode HTTP yang didukung oleh server atau resource tertentu.
  
- **Contoh Penggunaan**

  - Memeriksa metode yang diperbolehkan sebelum melakukan request sebenarnya.
  
- **Ciri-ciri**

  - Aman dan idempotent.
  
- **Contoh Request**
```
OPTIONS /users HTTP/1.1
Host: api.example.com
```

- **Contoh Response**
```
HTTP/1.1 204 No Content
Allow: GET, POST, PUT, DELETE
```

---

2. ## `Form-data dan x-www-form-urlencoded`

**Form-data** dan **x-www-form-urlencoded** adalah dua format yang digunakan untuk mengirim data melalui HTTP request,   
terutama dengan metode POST. Keduanya memiliki karakteristik dan cara kerja yang berbeda, tergantung pada jenis data yang dikirim.  

**Adapun perbedaannya**:

### 1. **Form-data**

- **Penjelasan**  
  Format ini digunakan untuk mengirimkan data dalam bentuk multipart (berpotongan),
  memungkinkan pengiriman file atau data yang kompleks seperti teks dan binary secara bersamaan.
  
- **Karakteristik**:
  - Data dipisah menjadi bagian-bagian (multipart) dengan boundary unik.
  - Cocok untuk mengunggah file karena dapat mengirimkan binary data.
  - Setiap bagian data memiliki header terpisah (termasuk metadata seperti nama file dan tipe konten).

- **Header HTTP**
  Content-Type: multipart/form-data; boundary=...

- **Contoh Penggunaan**
  - Mengunggah gambar, video, atau file lainnya.
  - Formulir dengan kombinasi file dan teks (misalnya, nama pengguna dan foto profil).

- **Keuntungan**
  - Dapat menangani berbagai tipe data, termasuk binary.
  - Fleksibel untuk data besar.

- **Kekurangan**
  - Sedikit lebih rumit dan berat dibandingkan dengan x-www-form-urlencoded karena data dikirim dalam bentuk multipart.

[Daftar Isi](#daftar-isi)

### 2. **x-www-form-urlencoded**

- **Penjelasan**
  Data dikodekan sebagai pasangan kunci-nilai dalam format URL-encoded (seperti query string pada URL).
  
- **Karakteristik**:
  - Data dikirim dalam satu blok sebagai teks biasa.
  - Setiap pasangan kunci-nilai dipisahkan oleh &, dan karakter khusus (seperti spasi) dikodekan (misalnya, spasi menjadi %20).
  - Tidak mendukung pengiriman file atau binary data.

- **Header HTTP**
  Content-Type: application/x-www-form-urlencoded

- **Contoh Penggunaan**
  - Formulir dengan teks sederhana (misalnya, nama dan email).
  - Data yang akan dikirim dalam format yang lebih ringan.

- **Keuntungan**
  - Lebih ringan dan sederhana dibandingkan dengan form-data.
  - Sangat cocok untuk data kecil dan hanya teks.

- **Kekurangan**
  - Tidak dapat mengirim file atau data binary.
  - Tidak ideal untuk data yang sangat besar.

### Perbandingan Utama:

| **Aspek**               | **form-data**                                     | **x-www-form-urlencoded**                         |
|--------------------------|--------------------------------------------------|--------------------------------------------------|
| **Format Data**          | Data dikirim dalam format multipart, dipecah menjadi bagian-bagian terpisah dengan boundary khusus. | Data dikirim dalam format URL-encoded, seperti `key=value&key2=value2`. |
| **Ukuran Data**          | Mendukung file dan data besar tanpa batas praktis (tergantung batas server). | Cocok untuk data kecil hingga sedang (tidak optimal untuk file atau data besar). |
| **Media Type**           | `multipart/form-data`                           | `application/x-www-form-urlencoded`             |
| **Dukungan File Upload** | Mendukung upload file (binary data, seperti gambar atau dokumen). | Tidak mendukung file upload (hanya data teks).   |
| **Efisiensi**            | Lebih kompleks dan sedikit lebih lambat karena multipart processing. | Lebih sederhana dan lebih cepat karena encoding sederhana. |
| **Penggunaan Umum**      | Digunakan untuk form yang mengirimkan file atau kombinasi file dan teks. | Digunakan untuk form sederhana seperti login, pencarian, atau data kecil lainnya. |
| **Contoh Payload**       | ``` --boundary\r\nContent-Disposition: form-data; name="file"; filename="image.png"\r\nContent-Type: image/png\r\n\r\n...binary data...\r\n--boundary-- ``` | ``` username=johndoe&password=123456 ``` |
| **Kompatibilitas Browser** | Didukung oleh browser untuk pengiriman form dengan file. | Didukung oleh browser untuk semua pengiriman form tanpa file. |
| **Keamanan**             | Sama aman dengan HTTPS, tetapi boundary memisahkan data sehingga lebih fleksibel. | Sama aman dengan HTTPS, tetapi semua data di-encode dalam satu string. |

---

### Catatan
- **`form-data`** lebih fleksibel untuk aplikasi yang membutuhkan pengiriman file atau data kompleks. 
- **`x-www-form-urlencoded`** lebih cocok untuk aplikasi yang hanya mengirimkan data teks sederhana seperti login atau query string.

[Daftar Isi](#daftar-isi)


## Using-Docker

asdfasdfasdf












