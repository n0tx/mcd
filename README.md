# Technical Information

## Daftar Isi

1. [Request Methods](#request-methods)
2. [Form-data Dan x-www-form-urlencoded](#form-data-dan-x-www-form-urlencoded)
3. [Aplikasi Dokumentasi API](#aplikasi-dokumentasi-api)
4. [HTTP Status Code](#http-status-code)
5. [Proses Login Dan Authentication](#proses-login-dan-authentication)
6. [Face recognition antara klien dan API](#face-recognition-antara-klien-dan-API)
7. [WebSocket antara klien dan API](#websocket-antara-klien-dan-api)


----


1. ## `Request Methods`

Request methods adalah cara yang digunakan oleh klien (seperti browser atau aplikasi) untuk berkomunikasi   
dengan server melalui protokol HTTP.   

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

  Digunakan untuk mengirim data ke server untuk diproses. Metode ini sering digunakan untuk membuat atau menambahkan data baru di server.

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

  Digunakan untuk memperbarui atau mengganti data di server. Biasanya menggantikan seluruh data dengan data baru.
  
- **Contoh Penggunaan**

  - Memperbarui profil pengguna.
  - Mengganti informasi produk dalam database.
  
- **Ciri-ciri**

  Idempotent (Permintaan yang sama menghasilkan hasil yang sama).

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

  Digunakan untuk memperbarui sebagian data di server. Tidak seperti PUT, hanya sebagian dari data yang diperbarui.

- **Contoh Penggunaan**

  - Mengubah hanya satu atribut pada profil pengguna (misalnya nama saja).
  - Menghapus postingan dari forum.
  
- **Ciri-ciri**

  Idempotent (dengan asumsi data yang sama dikirim dalam setiap request).

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

  Menghapus akun pengguna.
  
- **Ciri-ciri**

  Idempotent (jika data sudah terhapus, request berikutnya tidak akan menyebabkan efek tambahan).

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

  Memeriksa apakah suatu resource ada tanpa mengunduh kontennya.
  
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

  Memeriksa metode yang diperbolehkan sebelum melakukan request sebenarnya.
  
- **Ciri-ciri**

  Aman dan idempotent.
  
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

<< [Daftar Isi](#daftar-isi)
---

2. ## `Form-data dan x-www-form-urlencoded`

**Form-data** dan **x-www-form-urlencoded** adalah dua format yang digunakan untuk mengirim data melalui HTTP request, terutama dengan metode POST.  
Keduanya memiliki karakteristik dan cara kerja yang berbeda, tergantung pada jenis data yang dikirim.  

**Adapun perbedaannya**:

### 1. **Form-data**

- **Penjelasan**

  Format ini digunakan untuk mengirimkan data dalam bentuk multipart (berpotongan),
  memungkinkan pengiriman file atau data yang kompleks seperti teks dan binary secara bersamaan.
  
- **Karakteristik**
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
  Sedikit lebih rumit dan berat dibandingkan dengan x-www-form-urlencoded karena data dikirim dalam bentuk multipart.

### 2. **x-www-form-urlencoded**

- **Penjelasan**

  Data dikodekan sebagai pasangan kunci-nilai dalam format URL-encoded (seperti query string pada URL).
  
- **Karakteristik**
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


### Catatan
- **`form-data`** lebih fleksibel untuk aplikasi yang membutuhkan pengiriman file atau data kompleks. 
- **`x-www-form-urlencoded`** lebih cocok untuk aplikasi yang hanya mengirimkan data teks sederhana seperti login atau query string.

<< [Daftar Isi](#daftar-isi)
----

3. ## `Aplikasi Dokumentasi API`

### 1. **Swagger**  
  
**Swagger** adalah sebuah alat yang digunakan untuk mendokumentasikan dan mendesain API. Dengan Swagger,   
pengembang dapat dengan mudah membuat, menguji, dan memahami cara kerja API, baik bagi klien maupun server.  
  
- **Fitur Utama**

  - **OpenAPI Specification (OAS)**
  
    Swagger mendukung spesifikasi OpenAPI untuk mendeskripsikan endpoint API secara standar.
    
  - **Swagger UI**
  
    Menyediakan antarmuka berbasis web untuk mencoba endpoint API secara interaktif.

  - **Code Generation**

    Dapat menghasilkan kode klien atau server berdasarkan dokumentasi API.
    
  - **Kolaborasi**

    Mempermudah kolaborasi antar tim dengan dokumentasi API yang selalu up-to-date.
  
- **Keuntungan**

  - Mempermudah integrasi antar sistem.
  - Mengurangi kesalahan pemahaman karena dokumentasi otomatis.
  - Mendukung hampir semua bahasa pemrograman dan platform API modern.
  
Swagger banyak digunakan dalam proyek RESTful API untuk menjaga transparansi dan efisiensi pengembangan.  
  
### 2. **Postman**
  
**Postman** adalah alat yang populer untuk menguji, mendokumentasikan, dan mengelola API.   
Postman digunakan oleh pengembang untuk memastikan API bekerja sesuai spesifikasi   
dengan fitur antarmuka pengguna yang intuitif.
  
- **Fitur Utama**

  - **API Testing**

    Memungkinkan pengujian endpoint API dengan metode HTTP seperti GET, POST, PUT, DELETE, dll.
    
  - **Environment Variables**

    Mendukung penggunaan variabel untuk mengelola pengujian di berbagai lingkungan (development, staging, production)

  - **Automation**

    Menyediakan fitur untuk membuat pengujian otomatis melalui koleksi (collections) dan skrip.

  - **API Documentation**

    Dapat menghasilkan dokumentasi API secara langsung dari koleksi request.

  - **Collaboration**

    Fitur berbasis cloud untuk berbagi dan mengelola koleksi API antar tim.
  
**Keuntungan**  

- Mudah digunakan, bahkan untuk pemula.
- Mendukung debugging API dengan menampilkan request dan response secara detail.
- Menghemat waktu dengan koleksi yang dapat dieksekusi ulang.
  
Postman banyak digunakan dalam pengembangan API modern karena fleksibilitas dan kemudahan penggunaannya.

<< [Daftar Isi](#daftar-isi)
----

4. ## `HTTP Status Code`
  
**HTTP status code**  adalah kode respons yang diberikan oleh server untuk menunjukkan status permintaan HTTP yang dilakukan oleh klien.  

Berikut adalah penjelasan untuk status code **200**, **400**, dan **500**  
  
### 1. **HTTP Status Code 200 (OK)**

- **Pengertian**  

Status ini menunjukkan bahwa permintaan dari klien berhasil diproses oleh server tanpa masalah.

- **Konteks Penggunaan**

  - **GET**: Data yang diminta berhasil dikembalikan (misalnya, halaman web, API JSON).
  - **POST**: Permintaan berhasil diproses, dan server telah melakukan tindakan sesuai permintaan.
  - **PUT/DELETE**: Tindakan berhasil diterapkan.

- **Contoh Kasus**

  - **GET** Request
  
  Klien meminta daftar produk dari API dengan endpoint /products, dan server mengembalikan data daftar produk dalam format JSON.

```
Status: 200 OK  
Body: [
  { "id": 1, "name": "Laptop" },
  { "id": 2, "name": "Smartphone" }
]
```

  - **POST** Request

  Klien mengirimkan data pengguna baru ke endpoint /users, dan server berhasil menyimpan data tersebut.
  
### 2. **HTTP Status Code 400 (Bad Request)**

- **Pengertian**

Status ini menunjukkan bahwa permintaan yang dikirim oleh klien tidak valid atau tidak dapat dipahami oleh server.  
Ini biasanya terjadi karena kesalahan dari sisi klien.

- **Konteks Penggunaan**

  - Data yang dikirim tidak sesuai dengan format yang diharapkan server.
  - Parameter yang wajib ada tidak disertakan dalam permintaan.
  - Kesalahan dalam sintaksis permintaan (misalnya, JSON tidak valid).

- **Contoh Kasus**

  - Klien mengirimkan permintaan untuk menambahkan pengguna baru ke endpoint `/users`, tetapi tidak menyertakan data wajib seperti `email`:
  
```
Request: POST /users  
Body: { "name": "John" }
Response: 
Status: 400 Bad Request  
Body: { "error": "Email is required" }
```

  - Klien mengirimkan JSON yang salah format (misalnya, tanda kurung tutup hilang):  

```
Request Body: { "name": "John", "email": "john@example.com"  
Response: 
Status: 400 Bad Request  
Body: { "error": "Invalid JSON format" }
```

### 3. **HTTP Status Code 500 (Internal Server Error)**  

- **Pengertian**

Status ini menunjukkan bahwa terjadi kesalahan di sisi server saat memproses permintaan,  
tetapi penyebab spesifiknya tidak dapat dijelaskan dalam respons.

- **Konteks Penggunaan**

  - Server gagal menyelesaikan permintaan karena bug dalam kode server.
  - Masalah konfigurasi atau kesalahan sistem internal di server.
  - Server tidak dapat menangani permintaan karena kondisi tak terduga.

- **Contoh Kasus**

  - Klien meminta data produk dari endpoint `/products`, tetapi server mengalami kegagalan saat mengakses database karena koneksi terputus.
    
```
Request: GET /products  
Response: 
Status: 500 Internal Server Error  
Body: { "error": "Database connection failed" }
```

  - Klien mencoba memperbarui data pengguna, tetapi ada bug di server yang menyebabkan operasi gagal:

```
Request: PUT /users/123  
Body: { "name": "John Doe" }
Response: 
Status: 500 Internal Server Error  
Body: { "error": "Unexpected server error" }
```

### Kesimpulan Singkat  

| **Status Code** | **Kategori**     | **Arti Singkat**                                                                 |
|------------------|------------------|----------------------------------------------------------------------------------|
| 200              | Success          | Permintaan berhasil diproses.                                                   |
| 400              | Client Error     | Permintaan tidak valid atau ada kesalahan dari klien.                           |
| 500              | Server Error     | Terjadi kesalahan di server saat memproses permintaan, biasanya bukan kesalahan dari klien. |


<< [Daftar Isi](#daftar-isi)
----

5. ## `Proses Login Dan Authentication`

**Source Code Link**

```
https://github.com/n0tx/simple-login-auth
```

### 1. **Login**

- **Login request payload**

  ```json
  {
     "username": "admin",
     "password": "password123"
  }
  ```

- **Login Code**  

  - **AuthController.java**

    ```java
    // Login Endpoint
    @PostMapping("/login")
    public ResponseEntity<AuthResponse<AuthToken>> login(@RequestBody AuthRequest request) {
        String token = authService.login(request.getUsername(), request.getPassword());
        if (token != null) {
            return ResponseEntity.ok(new AuthResponse<>(
                    "success", "Login successful", new AuthToken(token)
            ));
        }
        return ResponseEntity.status(401).body(new AuthResponse<>(
                "error", "Invalid username or password", null
        ));
    }
    ```
    
  - **AuthService.java**
    ```java
    // Login Method
    public String login(String username, String password) {
        if (userDatabase.containsKey(username) && userDatabase.get(username).equals(password)) {
            // Generate a dummy token
            return "dummy-token-" + username;
        }
        return null;
    }
    ```
    
- **Login Response Json**

  Mendapatkan response yang berisi status, message, dan data
  
    ```json
    {
        "status": "success",
        "message": "Login successful",
        "data": {
            "token": "dummy-token-admin"
        }
    }
    ```
    
- **Login Response Code**

  - **AuthResponse.java**
    ```java
    package com.riki.auth.dto;

    public class AuthResponse<T> {
        private String status;
        private String message;
        private T data;
    
        public AuthResponse(String status, String message, T data) {
            this.status = status;
            this.message = message;
            this.data = data;
        }
    
        // Getters and Setters
    
    }
    ```

  - **AuthToken.java**
    ```java
    package com.riki.auth.model;


    public class AuthToken {
        private String token;
    
        // Default constructor
        public AuthToken() {}
    
        public AuthToken(String token) {
            this.token = token;
        }
    
        // Getter and Setter
        
    }
    ```

### 2. **Authentication**  

- **Authentication request payload**

  ```json
  {
      "token": "dummy-token-admin"
  }
  ```

- **Authentication Code**  

  - **AuthController.java**

    ```java
    // Authentication Endpoint
    @PostMapping("/authenticate")
    public ResponseEntity<AuthResponse<String>> authenticate(@RequestBody AuthToken token) {
        boolean isValid = authService.authenticate(token.getToken());
        if (isValid) {
            return ResponseEntity.ok(new AuthResponse<>(
                    "success", "Authentication successful", "You are authenticated!"
            ));
        }
        return ResponseEntity.status(403).body(new AuthResponse<>(
                "error", "Invalid or expired token", null
        ));
    }
    ```
    
  - **AuthService.java**
    ```java
    // Autentikasi Method
    public boolean authenticate(String token) {
        // Dummy check for token validation
        return token != null && token.startsWith("dummy-token-");
    }
    ```
    
- **Authentication Response Json**

  Mendapatkan response yang berisi status, message, dan data
  
    ```json
    {
        "status": "success",
        "message": "Authentication successful",
        "data": "You are authenticated!"
    }
    ```
    
- **Authentication Response Code**

  Menggunakan template response yang sama seperti pada proses login  

  - **AuthResponse.java**
    ```java
    package com.riki.auth.dto;

    public class AuthResponse<T> {
        private String status;
        private String message;
        private T data;
    
        public AuthResponse(String status, String message, T data) {
            this.status = status;
            this.message = message;
            this.data = data;
        }
    
        // Getters and Setters
    
    }
    ```

  - **AuthToken.java**
    ```java
    package com.riki.auth.model;


    public class AuthToken {
        private String token;
    
        // Default constructor
        public AuthToken() {}
    
        public AuthToken(String token) {
            this.token = token;
        }
    
        // Getter and Setter
        
    }
    ```

<< [Daftar Isi](#daftar-isi)
----
    
6. ## `Face recognition antara klien dan API`

Penerapan **face recognition** antara klien dan API umumnya melibatkan pengiriman gambar wajah ke server melalui API. 
Server akan memproses gambar tersebut menggunakan pustaka atau layanan **face recognition** dan mengembalikan hasil seperti identitas, 
keakuratan pencocokan, atau status validasi.

Berikut adalah gambaran langkah-langkah dan contoh kode sederhana.  

### Gambaran Alur Face Recognition  

1. **Client Side**

   - Klien mengambil gambar wajah menggunakan kamera (atau mengunggah file).
   - Gambar dikirim ke server melalui API dalam format form-data atau base64.

2. **Server Side**

   - Server menerima gambar melalui endpoint.
   - Server memproses gambar menggunakan pustaka face recognition seperti Face Recognition (Python), OpenCV, atau DeepFace.
   - Server membandingkan hasil dengan database wajah yang ada dan mengembalikan hasil validasi.

### Kode Client  

```javascript
const sendFaceRecognitionRequest = async (imageFile) => {
    const formData = new FormData();
    formData.append("file", imageFile);

    try {
        const response = await fetch("http://localhost:8080/api/face/recognize", {
            method: "POST",
            body: formData,
        });

        const result = await response.json();
        console.log("Response from server:", result);
    } catch (error) {
        console.error("Error during API call:", error);
    }
};

// Simulate file upload (e.g., from an <input type="file"> element)
const fileInput = document.querySelector("#fileInput");
fileInput.addEventListener("change", (event) => {
    const imageFile = event.target.files[0];
    sendFaceRecognitionRequest(imageFile);
});
```
   
### Kode Server  

Berikut adalah contoh server Spring Boot yang menerima gambar dan memprosesnya menggunakan pustaka Python untuk face recognition.  

### 1. **Endpoint untuk Face Recognition**

**FaceController.java**

```java
package com.example.face;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;

@RestController
@RequestMapping("/api/face")
public class FaceController {

    @PostMapping("/recognize")
    public ResponseEntity<FaceResponse> recognizeFace(@RequestParam("file") MultipartFile file) {
        try {
            // Simpan file sementara di server
            File tempFile = File.createTempFile("face-", ".jpg");
            file.transferTo(tempFile);

            // Panggil script Python untuk face recognition
            ProcessBuilder processBuilder = new ProcessBuilder("python3", "face_recognition.py", tempFile.getAbsolutePath());
            Process process = processBuilder.start();
            process.waitFor();

            // Baca hasil dari script Python
            String result = new String(process.getInputStream().readAllBytes());

            // Hapus file sementara
            tempFile.delete();

            // Return hasil ke client
            return ResponseEntity.ok(new FaceResponse("success", "Face recognition completed", result));

        } catch (IOException | InterruptedException e) {
            return ResponseEntity.status(500).body(new FaceResponse("error", "Failed to process face recognition", null));
        }
    }
}
```
### 2. **Model Response**

**FaceResponse.java**

```java
package com.example.face;

public class FaceResponse {
    private String status;
    private String message;
    private String data;

    public FaceResponse(String status, String message, String data) {
        this.status = status;
        this.message = message;
        this.data = data;
    }

    // Getters and Setters
    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getData() {
        return data;
    }

    public void setData(String data) {
        this.data = data;
    }
}
```


### 3. **Script Python untuk Face Recognition**

**face_recognition.py**

```java
import face_recognition
import sys

# Ambil file gambar dari argumen
image_path = sys.argv[1]

try:
    # Load gambar
    image = face_recognition.load_image_file(image_path)

    # Ekstraksi encoding wajah
    face_encodings = face_recognition.face_encodings(image)

    if len(face_encodings) > 0:
        print("Face recognized. Encoding extracted.")
    else:
        print("No face detected in the image.")
except Exception as e:
    print(f"Error processing image: {str(e)}")
```

### Contoh Response

- **Input**: Gambar wajah yang valid.

- **Output(Server)**: Gambar wajah yang valid.

```json
{
    "status": "success",
    "message": "Face recognition completed",
    "data": "Face recognized. Encoding extracted."
}
```

- **Input**: Gambar wajah tanpa wajah.

- **Output(Server)**: Gambar wajah yang valid.

```json
{
    "status": "success",
    "message": "Face recognition completed",
    "data": "No face detected in the image."
}
```

### Penjelasan

1. **Client**: Mengirim file gambar menggunakan Fetch API.
2. **Server (Java Spring Boot)**:

  - Menyimpan gambar sementara.
  - Memproses gambar menggunakan script Python.
  - Membaca hasil dan mengembalikannya ke klien.

3. **Python Script**: Menggunakan pustaka face_recognition untuk mendeteksi wajah dan mengekstraksi encoding.


<< [Daftar Isi](#daftar-isi)
----

8. ## `WebSocket antara klien dan API`

**WebSocket** adalah protokol komunikasi yang memungkinkan komunikasi dua arah secara real-time antara klien dan server. Penerapan WebSocket melibatkan pengaturan server untuk mendukung koneksi WebSocket dan klien untuk membuka dan berkomunikasi melalui koneksi tersebut.

Berikut adalah contoh sederhana penerapan WebSocket:

**Source Code Link**

```
https://github.com/n0tx/websocket
```

  
![image](https://github.com/user-attachments/assets/046f0e06-9e35-4e85-9fa4-d52b15783b83)  

  
  
### Penjelasan Singkat

1. **Server**:

  - Membuat server WebSocket di port 8080.
  - Menerima dan merespons pesan dari klien.
  - Menangani koneksi masuk dan keluar.

2. **Client**:

  - Membuka koneksi ke server WebSocket (http://localhost:8080).
  - Mendengarkan pesan dari server dan mengirim pesan ke server.
  - Menampilkan pesan di halaman web.

