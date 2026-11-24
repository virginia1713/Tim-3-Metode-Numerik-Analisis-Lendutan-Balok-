## Jenis jenis Pembebanan

* **A. Pembebanan Terpusat (P):** Beban yang menekan hanya pada satu titik tertentu di atas balok. Di dalam jurnal, beban ini digambarkan sebagai beban titik dari pertemuan balok anak ke balok induk, serta gaya gempa atau angin yang bekerja tepat pada titik lantai.
* **B. Pembebanan Terbagi Merata (q):** Beban yang tersebar rata di sepanjang bentang balok. Di dalam jurnal, beban ini berasal dari berat pelat lantai dan penghuni yang diratakan sepanjang balok.
* **C. Pembebanan Kombinasi:** Gabungan dari beberapa beban yang bekerja bersamaan pada satu balok, misalnya beban merata dan beban terpusat yang bekerja bareng, atau gabungan beban mati dan beban hidup.

## Jenis jenis Tumpuan

* **A. Tumpuan Sendi:** Penopang ujung balok yang menahan gerakan ke atas-bawah dan ke kiri-kanan, tapi membiarkan balok bebas melengkung (berotasi).
* **B. Tumpuan Rol:** Penopang ujung balok yang hanya menahan gerakan ke atas-bawah, sehingga balok bebas bergeser memanjang ke kiri-kanan dan bebas melengkung.

## Alasan Memilih Beban Terpusat & Tumpuan Sendi–Rol

* **Alasan pilih Tumpuan Sendi & Rol:** Sesuai soal untuk gelagar sederhana (seperti jembatan kecil atau balok lantai), tumpuan sendi–rol adalah model paling dasar agar balok bisa melentur alami dan bebas memuai tanpa patah di ujungnya. Secara hitungan, ujung kiri dan kanan balok pasti tidak turun (`lendutan = 0`), sehingga mudah dihitung dengan metode numerik.
* **Alasan pilih Beban Terpusat:** Menggambarkan beban nyata yang bertumpu di satu titik, seperti roda kendaraan di jembatan kecil atau titik tumpu balok anak seperti di jurnal. Selain itu, beban terpusat di tengah balok menghasilkan lendutan paling besar (paling kritis), sehingga paling pas dipakai untuk menguji apakah balok masih aman atau sudah melewati batas lendutan izin.

## Balok sederhana dengan beban terpusat

```text
          F
          │ P₁ = 6 t
          ▼
    RHA ◄─△───────────────────────△─ B (Rol)
        A ┴───────────────────────┴
         ▲│◄──── 4 m ────►◄─ 4 m ─►│▲
      VA ││           L            ││ VB
         RVA                      RVB

ΣMA = 0
P₁ . 4 - VB . 8 = 0
6 . 4 - 8 VB = 0
24 - 8 VB = 0
     VB = 3 t (↑)

ΣV = 0
VA - P₁ + VB = 0
VA - 6 + 3 = 0
VA - 3 = 0
     VA = 3 t (↑)

ΣMB = 0
-P . 4 + VB . 8 = 0
-6 . 4 + 3 . 8 = 0
-24 + 24 = 0
       0 = 0

## Perhitungan Persamaan Diferensial

1. Persamaan Momen Lentur M(x)
Berdasarkan reaksi tumpuan VA = 3 t dan beban terpusat P₁ = 6 t di x = 4 m, fungsi momen lentur dibagi menjadi dua bagian:
- Untuk 0 ≤ x ≤ 4 m: M(x) = 3x
- Untuk 4 < x ≤ 8 m: M(x) = 3x - 6(x - 4)

2. Persamaan Diferensial Utama
Hubungan kelengkungan dan momen lentur dirumuskan dengan d²y / dx² = -M(x) / EI:
- Untuk 0 ≤ x ≤ 4 m: d²y / dx² = -3x / EI
- Untuk 4 < x ≤ 8 m: d²y / dx² = -[3x - 6(x - 4)] / EI

3. Sistem Persamaan Orde Satu
Untuk diselesaikan dengan metode numerik secara bertahap per langkah h, persamaan di atas dipecah menjadi dua:
- Persamaan kemiringan: dy / dx = θ
- Persamaan kelengkungan: dθ / dx = -M(x) / EI

4. Syarat Batas
Karena kedua ujung balok ditopang tumpuan sendi dan rol, nilai lendutan di kedua ujungnya sama dengan nol:
- Di tumpuan sendi A: y(0) = 0
- Di tumpuan rol B: y(8) = 0
