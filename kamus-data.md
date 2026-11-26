# Kamus Data: Amazon.csv

Satu baris = satu pesanan (OrderID unik, 100.000 baris).
Kolom turunan dibuat di tahap Process.

| Kolom | Isi | Status |
|---|---|---|
| OrderID | ID pesanan, unik | Dipercaya |
| OrderDate | Tanggal pesanan (teks, diparse ke tanggal) | Dipercaya |
| CustomerID | ID pelanggan | Dipercaya |
| CustomerName | Nama pelanggan | TIDAK dipercaya: 29.643 dari 43.233 ID punya lebih dari 1 nama |
| ProductID | ID produk, konsisten 1:1 dengan ProductName | Dipercaya |
| ProductName | Nama produk (50 produk) | Dipercaya |
| Category | Kategori produk | TIDAK dipercaya: semua produk muncul di semua kategori |
| Brand | Merek | TIDAK dipercaya: semua produk punya banyak brand |
| Quantity | Jumlah item, 1–5 | Dipercaya |
| UnitPrice | Harga satuan per baris, 5,00–599,99 | Dipercaya sebagai nilai baris; bukan harga tetap produk |
| Discount | Diskon 0–0,3 | Dipercaya |
| Tax | Pajak per pesanan | Dipercaya |
| ShippingCost | Ongkos kirim 0–15 | Dipercaya |
| TotalAmount | Total bayar; lolos audit rumus di 100.000 baris | Dipercaya |
| PaymentMethod | Metode bayar (6 nilai) | Dipercaya |
| OrderStatus | Delivered, Shipped, Pending, Returned, Cancelled | Dipercaya |
| City | Kota (20 kota) | Dipercaya bersama State |
| State | Negara bagian, konsisten 1 kota = 1 state | Dipercaya |
| Country | Negara | TIDAK dipercaya: kota yang sama muncul di banyak negara |
| SellerID | ID penjual | Dipercaya |
| Tanggal (turunan) | OrderDate hasil parse | Dipercaya |
| Bulan (turunan) | Label bulan YYYY-MM dari Tanggal | Dipercaya |
| Tahun (turunan) | Tahun dari Tanggal | Dipercaya |

Definisi metrik: pendapatan = TotalAmount dari OrderStatus Delivered
saja. AOV = pendapatan dibagi jumlah pesanan Delivered. Tingkat
batal = pesanan Cancelled dibagi semua pesanan.
