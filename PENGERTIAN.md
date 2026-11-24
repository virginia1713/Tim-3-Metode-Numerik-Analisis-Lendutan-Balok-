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

import math

# ==========================================================
# 1. DATA STRUKTUR & BEBAN (SESUAI CATATAN & ARTIKEL)
# ==========================================================
L = 8.0          # Panjang bentang balok (m)
a = 4.0          # Jarak beban dari titik A (m)
P1 = 6.0 * 9810  # Beban 6 ton dikonversi ke Newton (N)
VA = P1 / 2.0    # Reaksi tumpuan A = 3 ton (N)

# Kekakuan Lentur (EI) dari penampang 25x50 cm & mutu beton fc' = 25 MPa
b = 0.25
h = 0.50
I = (b * h**3) / 12.0
E = 4700 * math.sqrt(25.0) * 1e6
EI = E * I

# ==========================================================
# 2. PERSAMAAN DIFERENSIAL (DARI PENURUNAN RUMUS)
# ==========================================================
# Persamaan dasar: d^2y/dx^2 = -M(x) / EI
# Diubah menjadi 2 persamaan orde 1:
#   1) dy/dx = theta
#   2) d(theta)/dx = -M(x) / EI

def momen(x):
    if x <= a:
        return VA * x
    else:
        return VA * x - P1 * (x - a)

def dy_dx(theta):
    return theta

def dtheta_dx(x):
    return -momen(x) / EI

def solusi_analitik(x):
    # Rumus eksak hasil integrasi ganda: y(x) = (1/EI) * (-0.5*VA*x^3 + C1*x)
    C1 = (P1 * L**2) / 16.0
    if x <= a:
        return (- (VA / 6.0) * x**3 + C1 * x) / EI
    else:
        x_sisa = L - x
        return (- (VA / 6.0) * x_sisa**3 + C1 * x_sisa) / EI

# ==========================================================
# 3. PERHITUNGAN NUMERIK (EULER, HEUN, RK4)
# ==========================================================
dx = 1.0  # Langkah perhitungan tiap 1 meter (x = 0, 1, 2, ..., 8)
n = int(L / dx)

# Syarat batas awal di x = 0
x = 0.0
theta_awal = -(P1 * L**2) / (16.0 * EI)

y_euler, t_euler = 0.0, theta_awal
y_heun,  t_heun  = 0.0, theta_awal
y_rk4,   t_rk4   = 0.0, theta_awal

print(f"{'x (m)':<6} | {'Analitik (mm)':<14} | {'Euler (mm)':<12} | {'Heun (mm)':<12} | {'RK4 (mm)':<12}")
print("-" * 68)
print(f"{x:<6.1f} | {0.0:<14.4f} | {0.0:<12.4f} | {0.0:<12.4f} | {0.0:<12.4f}")

for i in range(n):
    # --- 1. METODE EULER ---
    k1_y = dy_dx(t_euler)
    k1_t = dtheta_dx(x)
    
    y_euler += dx * k1_y
    t_euler += dx * k1_t

    # --- 2. METODE HEUN ---
    k1_y = dy_dx(t_heun)
    k1_t = dtheta_dx(x)
    
    t_pred = t_heun + dx * k1_t
    k2_y = dy_dx(t_pred)
    k2_t = dtheta_dx(x + dx)
    
    y_heun += (dx / 2.0) * (k1_y + k2_y)
    t_heun += (dx / 2.0) * (k1_t + k2_t)

    # --- 3. METODE RUNGE-KUTTA ORDE 4 (RK4) ---
    k1_y = dy_dx(t_rk4)
    k1_t = dtheta_dx(x)

    k2_y = dy_dx(t_rk4 + 0.5 * dx * k1_t)
    k2_t = dtheta_dx(x + 0.5 * dx)

    k3_y = dy_dx(t_rk4 + 0.5 * dx * k2_t)
    k3_t = dtheta_dx(x + 0.5 * dx)

    k4_y = dy_dx(t_rk4 + dx * k3_t)
    k4_t = dtheta_dx(x + dx)

    y_rk4 += (dx / 6.0) * (k1_y + 2*k2_y + 2*k3_y + k4_y)
    t_rk4 += (dx / 6.0) * (k1_t + 2*k2_t + 2*k3_t + k4_t)

    # Majukan posisi x dan hitung nilai eksak (dalam mm)
    x += dx
    y_eksak = -solusi_analitik(x)

    print(f"{x:<6.1f} | {y_eksak*1000:<14.4f} | {y_euler*1000:<12.4f} | {y_heun*1000:<12.4f} | {y_rk4*1000:<12.4f}")
