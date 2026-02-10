# Tutorial 1: Coding Standards

## Reflection 1
### Clean Code Principles
- Meaningful Names: menamai variabel dan fungsi dengan jelas. Contoh: findById, delete, productData. Orang lain baca langsung paham.
- Single Responsibility Principle (SRP): memisahkan tugas dengan rapi. 
  - Controller: Cuma urus web/permintaan user. 
  - Service: Urus logika bisnis. 
  - Repository: Urus data (simpan/hapus). 
  - Penjelasan: tidak menumpuk semua kodingan di satu file.
- Logika delete yang sangat simpel (removeIf), tidak berbelit-belit.

### Secure Coding Practices
- UUID for ID: menggunakan UUID.randomUUID() untuk membuat ID produk (contoh: a1b2-c3d4...).
Aman, karena ID-nya acak dan panjang, jadi orang iseng ga gampang menebak ID produk lain (misal ganti URL /delete/1 jadi /delete/2).
- Method HTTP yang Benar: pakai GET hanya untuk melihat data. Pakai POST untuk mengubah data (Create, Edit, Delete). 
Ini mencegah perubahan data yang tidak disengaja kalau link-nya cuma diklik biasa.

### Code Fixes
Masalah pas ngerjain tutorial ini salahsatunya pas awal menggunakan Field Injection (@Autowired langsung di variabel). 
Itu membuat kode susah dites (unit testing) dan melanggar prinsip dependency injection. 
Lalu untuk perbaikan aku ubah menjadi Constructor Injection (pakai public ProductServiceImpl(...)). 
Ini lebih aman dan direkomendasikan oleh Spring Boot.

## Reflection 2
Tentu, ini draf refleksi dalam Bahasa Indonesia yang santai tapi tetap akademis, dan sangat sesuai dengan pengalaman kita barusan (terutama bagian Functional Test yang sempat gagal karena masalah ID elemen).

Silakan copy-paste isinya ke README.md kamu.

Markdown
## Reflection 2

### 1. Unit Testing
Jujur, setelah berhasil membuat Unit Test dan berhasil, saya merasa sangat senang dan tenang. Sejujurnya sangat susah dan ribet harus bikin tes dulu, tapi kalo dipikir fungsinya, unit test ini berfungsi sebagai pengaman. Kalau nanti saya mengubah kodingan (refactoring), saya bisa langsung tahu kalau ada fitur yang rusak/error cuma dengan menjalankan tesnya lagi.

Berapa banyak Unit Test yang harus dibuat?
Menurut saya, tidak ada angka pasti untuk jumlah tes dalam satu class. Yang paling penting adalah kualitas tesnya, bukan kuantitasnya. Yang saya tau, tes yang baik itu meliputi:
1.  Positive Case: Memastikan fitur berjalan lancar saat input benar.
2.  Negative Case: Memastikan program tidak crash saat input salah (misalnya hapus produk yang ID-nya ga ada, seperti yang saya buat di `ProductRepositoryTest`).
3.  Edge Case: Tes batas-batas ekstrim input.

Memiliki 100% Code Coverage belum menjamin kode 100% ga ada bug atau error.
Karena code coverage hanya mengukur baris kode mana yang "dieksekusi" saat tes jalan, tapi tidak mengukur kebenaran logicnya.
- Contoh:Saya bisa membuat fungsi tambah (`add`) tapi isinya logika kurang (`return a - b`). Kalau saya tes dengan input tertentu, coverage-nya 100% karena kodenya jalan, tapi logikanya salah. Jadi, coverage tinggi itu bagus, tapi belum cukup menjamin.

### 2. Functional Testing & Clean Code
Masalah Kebersihan Kode (Clean Code Issues):
1.  Duplikasi Code (Code Duplication): Ini melanggar prinsip **DRY (Don't Repeat Yourself)**. Bagian setup seperti inisialisasi port (`@LocalServerPort`), setup URL dasar, dan konfigurasi Selenium WebDriver akan berulang-ulang di setiap file tes.
2.  Susah Dimaintain: Kalau nanti port default berubah atau cara setup WebDriver diganti. Saya harus membuka dan mengedit 10 file tes satu per satu. Itu sangat ga efisien dan mudah error.
3.  Keterbacaan (Readability): File tes penuh dengan kode konfigurasi, sehingga logika tes intinya malah tenggelam dan susah dibaca.
Saran Perbaikan (Improvement):
Solusi terbaik adalah menggunakan konsep **Inheritance** (Pewarisan) dalam OOP.
- Bisa dengan membuat satu **Base Test Class** yang berisi semua konfigurasi umum (setup port, base URL, driver).
- Test class lainnya (`CreateProductTest`, `CountProductTest`, dll) tinggal `extends BaseFunctionalTest`.
- Dengan begitu, kode konfigurasi hanya ditulis sekali, dan file tes fokus hanya ke skenario pengujiannya aja.
