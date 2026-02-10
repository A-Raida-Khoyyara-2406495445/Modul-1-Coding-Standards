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