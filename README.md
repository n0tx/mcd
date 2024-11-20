# Technical Information

## Daftar Isi

1. [Request Methods](#request-methods)  


----


1. ## `Request Methods` | [Daftar Isi](#daftar-isi)

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












