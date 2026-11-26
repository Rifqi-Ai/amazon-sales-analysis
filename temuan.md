# Rangkuman Temuan: Analisis Pesanan Amazon 2020 sampai 2024

Penulis: Muhamad Rifqi Salim. Data: Amazon.csv dari Kaggle, 100.000 baris,
20 kolom, satu baris adalah satu pesanan. Pengerjaan: Google Colab
(pandas), semua angka dari keluaran notebook penulis sendiri.

## Pemeriksaan kualitas data (sebelum analisis)

- Duplikat baris persis: 0. OrderID unik: 100.000 dari 100.000.
- Nilai kosong: 0 di semua kolom.
- Kolom TotalAmount diaudit dengan menghitung ulang dari komponennya:
  Quantity dikali UnitPrice dikali (1 dikurangi Discount), ditambah Tax
  dan ShippingCost. Cocok di 100.000 baris, selisih terbesar 0,005
  (pembulatan). Kolom ini dapat dipercaya.
- Rentang angka wajar: Quantity 1 sampai 5, UnitPrice 5,00 sampai
  599,99, Discount 0 sampai 0,3, tidak ada nilai negatif atau nol yang
  tidak sah.
- Kolom yang terbukti tertempel acak dan TIDAK dipakai untuk analisis
  berbasis kolom tersebut:
  - Category: 50 dari 50 nama produk muncul di semua 6 kategori.
  - Brand: 50 dari 50 produk tercatat dengan banyak brand berbeda.
  - Country: 20 dari 20 kota tercatat di banyak negara berbeda.
  - CustomerName: 29.643 dari 43.233 CustomerID tercatat dengan nama
    lebih dari satu.
  - UnitPrice bukan harga tetap produk: sekitar 2.000 nilai harga
    berbeda untuk tiap nama produk, di rentang penuh 5 sampai 600.
- Pasangan yang konsisten: ProductID dengan ProductName (satu lawan
  satu), City dengan State.

## Definisi yang dipakai

Pendapatan adalah TotalAmount dari pesanan berstatus Delivered saja
(74.628 pesanan). Shipped dan Pending belum menjadi uang, Cancelled dan
Returned tidak dihitung.

## Lima pertanyaan dan jawabannya

1. Tren pesanan dan pendapatan per bulan: DATAR selama 60 bulan.
   Pesanan 1.490 sampai 1.783 per bulan, pendapatan 1.014.912 sampai
   1.255.548 per bulan. Pendapatan tertinggi Agustus 2020 (1.255.548),
   terendah Februari 2022 (1.014.912). Februari cenderung terendah,
   sesuai jumlah hari yang lebih sedikit. Tidak ada pola pertumbuhan.
2. Komposisi status per tahun: BEKU. Delivered 74,42 sampai 74,95
   persen setiap tahun, Shipped sekitar 15 persen, Pending sekitar
   4 persen, Cancelled 2,88 sampai 3,20 persen, Returned 2,90 sampai
   3,21 persen. Tingkat batal dan retur tidak membaik dan tidak
   memburuk selama 5 tahun.
3. Produk terlaris: TIDAK ADA produk juara. 50 produk berbagi hampir
   rata: pesanan per produk 2.004 sampai 2.098, pendapatan per produk
   1.348.186 sampai 1.468.040. Teratas menurut jumlah pesanan LED Desk
   Lamp (2.098), teratas menurut pendapatan Electric Kettle
   (1.468.040). Selisih peringkat atas dan bawah hanya beberapa
   persen, konsisten dengan harga per baris yang acak.
4. Metode pembayaran: Credit Card dominan, 35.038 pesanan (35,04
   persen) dengan pendapatan 23.933.235. Debit Card 20.024 (20,02
   persen), UPI 15.066 (15,07 persen), Amazon Pay 15.017 (15,02
   persen), Net Banking 9.927 (9,93 persen), Cash on Delivery 4.928
   (4,93 persen).
5. Nilai rata rata pesanan (AOV): total pendapatan Delivered
   68.372.677 dari 74.628 pesanan, AOV 916,18. Per tahun stabil di
   911,08 sampai 926,35. Per status juga rapat: 910,93 sampai 941,59.
   Total ini cocok dengan penjumlahan kolom pendapatan pada tabel
   metode pembayaran, dua jalur hitung yang independen.

## Kesimpulan dan batasan

Dataset ini menunjukkan ciri data sintetis: tren datar tanpa
pertumbuhan, komposisi status yang beku, produk yang berbagi rata,
dan empat kolom yang tertempel acak. Nilai pekerjaan ini ada pada
metodenya: kualitas data dibuktikan dulu sebelum analisis, pertanyaan
yang bergantung pada kolom rusak dipotong, definisi pendapatan
dinyatakan terbuka, dan total kunci diverifikasi lewat dua jalur
hitung yang berbeda. Analisis per kategori, per brand, per negara,
dan per pelanggan tidak dapat dipertanggungjawabkan dari data ini
dan sengaja tidak dibuat.
