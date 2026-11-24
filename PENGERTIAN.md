## 1. Latar Masalah

Balok lantai pada gedung bertumpu sederhana (sendi-rol) dan mengalami lendutan akibat beban yang bekerja. Lendutan yang berlebihan dapat mengganggu fungsi struktur, misalnya menyebabkan retak pada finishing lantai dan dinding partisi. Proyek ini berfokus pada **analisis lendutan layan**, bukan perencanaan tulangan atau kapasitas balok.

## 2. Jenis jenis Pembebanan

* **A. Pembebanan Terpusat (P):** Beban yang menekan hanya pada satu titik tertentu di atas balok. Di dalam jurnal, beban ini digambarkan sebagai beban titik dari pertemuan balok anak ke balok induk, serta gaya gempa atau angin yang bekerja tepat pada titik lantai.
* **B. Pembebanan Terbagi Merata (q):** Beban yang tersebar rata di sepanjang bentang balok. Di dalam jurnal, beban ini berasal dari berat pelat lantai dan penghuni yang diratakan sepanjang balok.
* **C. Pembebanan Kombinasi:** Gabungan dari beberapa beban yang bekerja bersamaan pada satu balok, misalnya beban merata dan beban terpusat yang bekerja bareng, atau gabungan beban mati dan beban hidup.

## 3. Jenis jenis Tumpuan

* **A. Tumpuan Sendi:** Penopang ujung balok yang menahan gerakan ke atas-bawah dan ke kiri-kanan, tapi membiarkan balok bebas melengkung (berotasi).
* **B. Tumpuan Rol:** Penopang ujung balok yang hanya menahan gerakan ke atas-bawah, sehingga balok bebas bergeser memanjang ke kiri-kanan dan bebas melengkung.

## 4. Alasan Memilih Beban Terpusat & Tumpuan Sendi–Rol

* **Alasan pilih Tumpuan Sendi & Rol:** Sesuai soal untuk gelagar sederhana (seperti jembatan kecil atau balok lantai), tumpuan sendi–rol adalah model paling dasar agar balok bisa melentur alami dan bebas memuai tanpa patah di ujungnya. Secara hitungan, ujung kiri dan kanan balok pasti tidak turun (`lendutan = 0`), sehingga mudah dihitung dengan metode numerik.
* **Alasan pilih Beban Terpusat:** Menggambarkan beban nyata yang bertumpu di satu titik, seperti roda kendaraan di jembatan kecil atau titik tumpu balok anak seperti di jurnal. Selain itu, beban terpusat di tengah balok menghasilkan lendutan paling besar (paling kritis), sehingga paling pas dipakai untuk menguji apakah balok masih aman atau sudah melewati batas lendutan izin.
## 5. Skenario

| Item | Pilihan | Justifikasi |
|---|---|---|
| Struktur | Balok lantai, tumpuan sederhana (sendi di A, rol di B) | Lihat bagian 4 |
| Pembebanan | Beban terpusat P = 6 ton di tengah bentang | Lihat bagian 4 |
| Fokus analisis | Lendutan layan | Sesuai tujuan tugas: analisis lendutan secara numerik |

## 6. Parameter dan Justifikasi

| Parameter | Simbol | Nilai | Justifikasi / Sumber |
|---|---|---|---|
| Panjang bentang | L | 8 m | Asumsi bentang balok lantai gedung |
| Beban terpusat | P | 6 ton-gaya = 6.000 kg × 9,81 m/s² = 58,86 kN | Asumsi beban titik; dikonversi dari satuan massa ke gaya |
| Mutu beton | fc' | 25 MPa | Mutu umum untuk struktur gedung |
| Modulus elastisitas | E | 4700√fc' ≈ 23.500 MPa = 23,5 × 10⁶ kN/m² | SNI 2847:2019 |
| Lebar penampang | b | 0,30 m | Asumsi dimensi balok |
| Tinggi penampang | h | 0,60 m | Memenuhi h ≥ L/16 = 0,50 m untuk balok tumpuan sederhana (SNI 2847:2019) |
| Momen inersia | I | b·h³/12 = 5,4 × 10⁻³ m⁴ | Sifat geometri penampang persegi utuh (gross); lihat bagian 8.1 |
| Kekakuan lentur | EI | 126.900 kN·m² | Hasil kali E × I |
| Lendutan izin | δ_izin | L/360 = 22,22 mm | Batas lendutan untuk lantai (SNI 2847:2019, tabel batas lendutan) |

## 7. Asumsi Model

1. Balok prismatik (penampang konstan sepanjang bentang).
2. Material elastis linier dan homogen.
3. Lendutan kecil (teori balok Euler-Bernoulli).
4. Beban statis; berat sendiri balok diabaikan.
5. Deformasi akibat geser diabaikan.
6. Penampang utuh (pengaruh retak beton tidak diperhitungkan).

## 8. Model Matematis

### 8.1 Persamaan kurva elastis

Untuk balok yang melentur, regangan di titik berjarak z dari sumbu netral adalah ε = z·κ (κ = kelengkungan). Dengan hukum Hooke σ = E·ε, momen lentur menjadi:

```
M = ∫ σ·z dA = E·κ·∫ z² dA = E·κ·I
```

Integral ∫ z² dA adalah **momen inersia penampang I**, yaitu sifat geometri penampang (bukan massa). Untuk lendutan kecil, κ ≈ y'', sehingga:

```
EI · d²y/dx² = M(x)
```

Sumbu y positif ke atas dan momen sagging positif (lendutan ke bawah bernilai negatif).

### 8.2 Reaksi tumpuan dan momen lentur

Model balok sederhana dengan beban terpusat di tengah bentang:

```
                 P = 6 ton
                     |
                     v
   A ----------------+---------------- B
 (sendi)                              (rol)
   ^                                    ^
   VA                                   VB
   |<------ 4 m ------>|<------ 4 m ------>|
   |<------------ L = 8 m ------------->|
```

Keterangan: VA dan VB adalah reaksi vertikal di tumpuan A dan B. Tidak ada beban horizontal, sehingga reaksi horizontal di A bernilai nol.

Perhitungan reaksi tumpuan (momen searah jarum jam bernilai positif):

```
ΣMA = 0
P·4 − VB·8 = 0
6·4 − 8·VB = 0
24 − 8·VB = 0
VB = 3 ton (↑)

ΣV = 0
VA − P + VB = 0
VA − 6 + 3 = 0
VA = 3 ton (↑)

Kontrol: ΣMB = 0
−P·4 + VA·8 = 0
−6·4 + 3·8 = 0
−24 + 24 = 0
0 = 0 (OK)
```

Dalam satuan gaya: VA = VB = 3 ton-gaya = 3.000 kg × 9,81 = 29,43 kN, atau sama dengan P/2.

Momen lentur sepanjang bentang (x diukur dari tumpuan A):

```
M(x) = (P/2) · x          untuk 0 ≤ x ≤ L/2
M(x) = (P/2) · (L − x)    untuk L/2 < x ≤ L
```

### 8.3 Sistem PDB orde satu

```
dy/dx = θ
dθ/dx = M(x) / EI
```

### 8.4 Syarat batas

y(0) = 0 dan y(L) = 0 (tumpuan sendi dan rol).

## 9. Rencana Metode Numerik

| Metode | Fungsi dalam proyek | Alasan |
|---|---|---|
| **RK4** (PDB) | Menyelesaikan sistem y' dan θ' sepanjang bentang | Akurat dan umum dipakai untuk PDB; Euler dan Heun dipakai sebagai pembanding |
| **Secant / Bisection** (akar persamaan) | Mencari kemiringan awal θ(0) = s yang membuat y(L) = 0 (metode shooting) | Syarat batas berada di dua ujung, sedangkan RK4 membutuhkan kondisi awal |
| **Diferensiasi numerik** (beda hingga pusat) | Menentukan posisi lendutan maksimum, yaitu titik dengan dy/dx = 0 | Dibutuhkan untuk output posisi lendutan maksimum |

**Parameter numerik (rencana):** jumlah segmen N genap (agar x = L/2 jatuh pada titik grid), toleransi akar persamaan 10⁻⁶ atau lebih kecil, dan tebakan awal shooting ditentukan dari perkiraan kemiringan di tumpuan.

## 10. Rencana Validasi

**Solusi analitik** untuk beban terpusat di tengah bentang:

```
δ_maks = P·L³ / (48·EI)    di x = L/2
θ(0)   = −P·L² / (16·EI)
```

Dengan parameter di atas, perhitungan manual memberikan δ_maks ≈ 4,95 mm sebagai angka acuan.

**Pembanding pustaka:** `scipy.integrate.solve_ivp`.

**Analisis galat:**

```
galat relatif (%) = |δ_numerik − δ_analitik| / |δ_analitik| × 100
```

Selain itu, galat Euler, Heun, dan RK4 dibandingkan terhadap jumlah segmen N.

## 11. Output yang Direncanakan

1. Kurva lendutan sepanjang bentang balok.
2. Nilai lendutan maksimum dan posisinya.
3. Perbandingan terhadap solusi analitik dan pustaka, beserta analisis galat.
4. Perbandingan terhadap lendutan izin (L/360) dan kesimpulan memenuhi atau tidak memenuhi.

## 12. Struktur Repository (Rencana)

```
README.md
src/        # kode program, dipisah per metode/anggota
data/       # parameter dan sumber/justifikasinya
hasil/      # grafik dan tabel keluaran
```

## 13. Referensi

1. Chapra, S.C., *Applied Numerical Methods with MATLAB for Engineers and Scientists*, 4th ed. (PDB, akar persamaan, diferensiasi numerik).
2. Gere, J.M. & Goodno, B.J., *Mechanics of Materials* (persamaan kurva elastis dan rumus lendutan balok).
3. SNI 2847:2019, *Persyaratan Beton Struktural untuk Bangunan Gedung* (modulus elastisitas, tinggi minimum balok, batas lendutan).
4. Dokumentasi SciPy, `scipy.integrate.solve_ivp`.


