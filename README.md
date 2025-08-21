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
SELECT <br>
    u.id AS client_id, <br>
    COUNT(t.id) AS total_transactions, <br>
    SUM(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)) AS total_spent <br>
FROM users_stg u <br>
JOIN transactions_stg t ON u.id = t.client_id <br>
GROUP BY u.id <br>
ORDER BY total_spent DESC <br>
LIMIT 10; <br>

--- 2. Analisis brand kartu
SELECT <br>
    c.card_brand, <br>
    COUNT(t.id) AS total_transactions, <br>
    SUM(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)) AS total_spent, <br>
    ROUND(AVG(CAST(REPLACE(REPLACE(t.amount, '$', ''), ',', '') AS NUMERIC)), 2) AS avg_transaction_value <br>
FROM transactions_stg t <br>
JOIN cards_stg c ON t.card_id = c.id <br>
GROUP BY c.card_brand <br>
ORDER BY total_spent DESC; <br>

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
