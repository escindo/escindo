# 📉 Jurnal Data Trading: Insiden FOMC & Kesalahan Lot Size
**Tanggal Kejadian:** 29 Juli 2026 (menjelang FOMC)  
**Pasangan / Instrumen:** [XAUUSD / EURUSD]  
**Akun yang terlibat:** Micro → Standard  
**Kerugian Realized:** **-$1.100 USD**  
**Penyebab Utama:** Human error – salah volume (1 lot Standard karena kebiasaan Micro)

---

## 1. Kronologi Kejadian (Data Fakta)

| Waktu          | Peristiwa                                      | Nilai          | Akun        |
|----------------|------------------------------------------------|----------------|-------------|
| Sebelum FOMC   | Dana di akun Micro                             | $1.400         | Micro       |
| Sebelum FOMC   | Transfer pengamanan dana                       | $1.100         | → Standard  |
| Setelah transfer| Sisa di Micro                                  | $300           | Micro       |
| Saat FOMC      | Akun Micro terkena Stop Out (SO)               | -$300          | Micro       |
| Saat FOMC      | Tim mengelola akun Standard                    | -              | Standard    |
| Saat entry     | Kebiasaan Micro: klik **1 lot**                | 1.00 lot       | Standard    |
| Hasil          | 1 lot Standard → 1 point = **$1** (menurut broker Anda) | Kerugian cepat | Standard    |
| **Total Rugi** | **-$1.100**                                    | -$1.100        | Standard    |

> Catatan: Di akun Micro, 1 lot biasanya = 0.01 standard lot (micro lot).  
> Di akun Standard, 1 lot = 1.00 standard lot → risiko 100× lebih besar.

---

## 2. Analisis Matematika Risiko

### 2.1 Perhitungan Nilai Point / Pip
Menurut pengalaman Anda:
$$
1 \text{ point pada 1 lot Standard} = \$1
$$

Sehingga kerugian $1.100 berarti pergerakan harga sekitar:
$$
\text{Jumlah point} = \frac{1100}{1} = 1100 \text{ point}
$$

### 2.2 Perbandingan Risiko Lot
Misalkan risk per trade yang aman = **1%** dari equity.

**Formula Position Sizing klasik:**
$$
\text{Lot Size} = \frac{\text{Equity} \times \text{Risk \%}}{\text{Stop Loss (point)} \times \text{Nilai 1 point per lot}}
$$

#### Contoh Perhitungan yang Seharusnya:
- Equity Standard saat itu: **$1.100**
- Risk yang diizinkan: 1% = **$11**
- Stop Loss yang digunakan: misal 50 point
- Nilai 1 point per 1 lot Standard: **$1**

$$
\text{Lot yang aman} = \frac{1100 \times 0.01}{50 \times 1} = \frac{11}{50} = 0.22 \text{ lot}
$$

**Yang terjadi:**
Anda menggunakan **1.00 lot** → risiko aktual:
$$
\text{Risiko aktual} = 1.00 \times 50 \times 1 = \$50 \quad (4,5\% \text{ dari equity})
$$
atau bahkan lebih besar tergantung seberapa jauh harga bergerak sebelum ditutup.

---

## 3. Root Cause Analysis (Matematika + Human Factor)

| Faktor                  | Penjelasan Matematis / Operasional                  | Bobot |
|-------------------------|-----------------------------------------------------|-------|
| Kebiasaan Micro         | 1 lot Micro ≈ 0.01 Standard → otak masih “1 lot aman” | Tinggi |
| Pindah akun mendadak    | Tidak ada checklist volume saat ganti akun          | Tinggi |
| Tidak ada risk calculator | Tidak menghitung lot berdasarkan equity & SL        | Tinggi |
| Tim mengelola           | Komunikasi volume tidak diseragamkan                | Sedang |
| Volatilitas FOMC        | Spread + slippage membesar → kerugian dipercepat    | Tinggi |

---

## 4. Solusi & Aturan Baru (Berbasis Matematika)

### Aturan Position Sizing Wajib (copy ke setiap akun)
```python
def hitung_lot_aman(equity, risk_percent, stop_loss_point, nilai_point_per_lot):
    """
    equity              : modal saat ini
    risk_percent        : 0.5 - 1.0 (disarankan max 1%)
    stop_loss_point     : jarak SL dalam point
    nilai_point_per_lot : $1 (sesuai broker Anda di Standard)
    """
    risk_amount = equity * (risk_percent / 100)
    lot = risk_amount / (stop_loss_point * nilai_point_per_lot)
    return round(lot, 2)

# Contoh pemakaian kemarin:
print(hitung_lot_aman(1100, 1.0, 50, 1))   # Hasil: 0.22 lot
```

# Checklist Wajib Sebelum Entry (terutama FOMC)
1. Cek tipe akun (Micro / Standard / Cent)
2. Hitung lot dengan formula di atas
3. Pastikan volume di platform = hasil perhitungan
4. Screenshot sebelum klik Buy/Sell
5. Maksimal risk 0.5–1% per trade saat high impact news

# Ringkasan Pembelajaran
1. $1.100 hilang bukan karena analisa salah, melainkan karena kesalahan skala (scale error) antara Micro dan Standard.
2. Kebiasaan adalah musuh terbesar saat berpindah akun.
3. Matematika position sizing harus menjadi kebiasaan baru, bukan opsional.
4. Ke depan: setiap kali pindah akun → anggap seperti trader baru (reset kebiasaan volume).

# Status Jurnal: Selesai & Ditutup
Tindakan Lanjutan:  Buat template risk calculator di Excel / Google Sheet  / https://cindo.pages.dev/jurnaltrading
Pasang reminder di platform: “CEK AKUN SEBELUM KLIK”  
Latihan 30 hari hanya pakai 0.01–0.10 lot di akun Standard sampai kebiasaan terbentuk

Modal tersisa setelah kejadian: ≈ $0 di Standard (habis) + tim deposit lagi di akun Micro 2jt dapat 110 dolar dengan bonus kredit 55.73 dolar
Target recovery: Mulai lagi dari akun Micro dengan disiplin lot ketat.

# Kejadian Yang Terulang
Kejadian sebelumnya sudah pernah terjadi, yaitu ketika membagi 2 akun, setelah akun yang satunya SO (Stop Out) maka akun yang satunya juga SO.



