# Analisis Pesanan Amazon 2020–2024

Studi kasus analisis data end-to-end: 100.000 pesanan e-commerce selama
5 tahun, diolah dari data mentah sampai dashboard interaktif.

**Dashboard live:** https://rifqi-ai.github.io/amazon-sales-analysis/
(versi web dari dashboard Sheets, desain dan angka yang sama).
**Dashboard Google Sheets:** berkas "amazon_kerja_bersih" (tab
Dashboard + Calc), dibangun dengan rumus SUMIFS/COUNTIFS/AVERAGEIFS,
dropdown filter Tahun dan Metode Bayar, dan slicer Bulan.

## Pertanyaan Bisnis

1. Bagaimana tren pesanan dan pendapatan per bulan dan per tahun?
2. Bagaimana komposisi status pesanan, dan berapa tingkat batal dan
   retur per tahun?
3. Produk apa yang menghasilkan pendapatan terbesar?
4. Metode pembayaran apa yang paling banyak dipakai?
5. Berapa nilai rata-rata pesanan (AOV) per tahun dan per status?

Definisi dikunci sebelum menghitung: **pendapatan = TotalAmount dari
pesanan berstatus Delivered saja.** Pesanan Shipped dan Pending belum
menjadi uang; Cancelled dan Returned dikeluarkan.

## Audit Kualitas Data (temuan utama proyek ini)

Dataset tidak bisa langsung dipercaya. Setiap kolom diuji konsistensi
antar kolom sebelum dipakai:

| Kolom | Hasil uji | Keputusan |
|---|---|---|
| Category | 50 dari 50 produk muncul di semua 6 kategori | Tidak dipakai |
| Brand | 50 dari 50 produk punya banyak brand | Tidak dipakai |
| Country | 20 dari 20 kota muncul di banyak negara | Tidak dipakai |
| CustomerName | 29.643 dari 43.233 CustomerID punya lebih dari 1 nama | Tidak dipakai |
| UnitPrice | Sekitar 2.000 harga berbeda per produk | Bukan harga tetap produk |
| City dan State | Konsisten 1 kota = 1 state | Bisa dipakai |
| ProductID dan ProductName | Konsisten 1:1 (50 produk) | Dipakai |
| TotalAmount | Lolos audit rumus di 100.000 baris: Quantity × UnitPrice × (1 − Discount) + Tax + ShippingCost, selisih terbesar 0,005 (pembulatan) | Dipakai |

Pemeriksaan dasar: 100.000 baris, 0 duplikat persis, OrderID unik
100.000 (satu baris = satu pesanan), 0 nilai kosong, semua tanggal
terparse (2020-01-01 sampai 2024-12-29).

## Temuan

- **Tren datar.** Pesanan bulanan 1.490–1.783 dan pendapatan Delivered
  bulanan sekitar 1,01–1,26 juta selama 60 bulan, tanpa pertumbuhan.
  Februari konsisten terendah, sesuai jumlah harinya.
- **Komposisi status beku.** Delivered 74,63%, Shipped 15,19%,
  Pending 4,10%, Returned 3,05%, Cancelled 3,03%, nyaris tidak
  berubah per tahun.
- **Tidak ada produk juara.** 50 produk berbagi hampir rata;
  peringkat 1 Electric Kettle (pendapatan 1.468.040) hanya sekitar
  4% di atas peringkat 10.
- **Credit Card dominan:** 35,04% pesanan dan pendapatan Delivered
  23.933.235. Cash on Delivery terkecil (4,93%).
- **AOV stabil:** 916,18 untuk Delivered; per tahun 911,08–926,35.

Total pendapatan Delivered: **68.372.677** dari 74.628 pesanan.

## Kesimpulan dan Keterbatasan

Pola datar di semua metrik, komposisi status yang beku, produk yang
berbagi rata, dan empat kolom yang tertempel acak adalah ciri data
sintetis. Kesimpulan bisnis dari data seperti ini memang terbatas,
dan itu dinyatakan terbuka di sini. Nilai proyek ini ada pada
metodenya: kualitas data dibuktikan dulu sebelum analisis, pertanyaan
yang bergantung pada kolom rusak dipotong, definisi metrik dinyatakan
di depan, dan setiap angka dashboard diverifikasi terhadap hitungan
independen (hasil pandas di Colab dan rumus di Sheets cocok).

## Isi Repo

- `index.html` — versi web dashboard Sheets (desain eksekutif yang
  sama; filter tahun menggerakkan KPI, tren, dan donat status).
- `Analisis_Amazon.ipynb` — notebook Google Colab: profiling, audit
  kualitas, pembersihan, dan analisis (pandas).
- `temuan.md` — rangkuman temuan lengkap.
- `kamus-data.md` — definisi setiap kolom dan status kepercayaannya.
- `data/` — tabel ringkasan hasil analisis (tren bulanan, produk
  teratas, metode bayar).

## Alat

Google Colab (Python, pandas), Google Sheets (rumus, pivot, slicer,
dashboard), HTML/CSS/JavaScript untuk dashboard web.

Sumber data: "Amazon Sales Dataset" oleh rohiteng di Kaggle,
https://www.kaggle.com/datasets/rohiteng/amazon-sales-dataset
(berkas Amazon.csv, 100.000 baris, 20 kolom).
