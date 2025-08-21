📌 User Behavior Analysis
1. Deskripsi Project

Project ini bertujuan untuk menganalisis perilaku pengguna berdasarkan data transaksi kartu. Dataset terdiri dari:

- users_data → berisi informasi demografis pengguna (usia, gender, lokasi, pendapatan).
- cards_data → berisi informasi detail kartu (brand, tipe, credit limit, status chip).
- transactions_data → berisi catatan transaksi (tanggal, jumlah, metode penggunaan, merchant, lokasi).

Analisis dilakukan untuk menjawab beberapa pertanyaan utama, seperti:
- Siapa pengguna dengan pengeluaran terbesar?
- Bagaimana distribusi transaksi berdasarkan waktu, gender, dan lokasi?
- Brand serta tipe kartu apa yang paling banyak digunakan?
- Bagaimana pola penggunaan chip/online/swipe?

2. Tools yang Digunakan

- PgAdmin → untuk menyimpan data dan menjalankan query SQL.
- Power BI / Looker Studio → untuk membuat visualisasi interaktif.
- MS PowerPoint / PDF → untuk menyusun presentasi hasil analisis.

3. Cara Menjalankan Kode SQL

- Import ketiga dataset (users_data.csv, cards_data.csv, transactions_data.csv) ke PostgreSQL.
- Buat schema/tabel sesuai dataset.

Jalankan query SQL analisis, contoh:

--- 1. Top 10 user dengan transaksi terbanyak
SELECT 
    u.id AS client_id,
    COUNT(t.id) AS total_transactions,
    SUM(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)) AS total_spent
FROM users_stg u
JOIN transactions_stg t ON u.id = t.client_id
GROUP BY u.id
ORDER BY total_spent DESC
LIMIT 10;

--- 2. Analisis brand kartu
SELECT 
    c.card_brand,
    COUNT(t.id) AS total_transactions,
    SUM(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)) AS total_spent,
    ROUND(AVG(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)), 2) AS avg_transaction_value
FROM transactions_stg t
JOIN cards_stg c ON t.card_id = c.id
GROUP BY c.card_brand
ORDER BY total_spent DESC;

--- 3. Perbandingan transaksi berdasarkan gender
SELECT 
    u.gender,
    COUNT(t.id) AS total_transactions,
    SUM(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)) AS total_spent,
    ROUND(AVG(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)), 2) AS avg_transaction_value
FROM users_stg u
JOIN transactions_stg t ON u.id = t.client_id
GROUP BY u.gender;

Export hasil query (jika diperlukan) ke .csv untuk divisualisasikan di Power BI.
