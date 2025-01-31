# BDA-Kimia-Farma
## About Program
The Rakamin Academy and Kimia Farma Big Data Analytics Project-Based Internship is a self-development and career acceleration program for those interested in Big Data Analytics roles at Kimia Farma. It provides basic learning materials, including Article Reviews and Company Coaching Videos, to introduce the required competencies and skills. Weekly assessments and a final project will serve as your portfolio.

## About Company
Kimia Farma was established in 1817, making it one of the oldest pharmaceutical companies in Indonesia. Originally founded as a small drugstore by the Dutch East Indies colonial government, it has since evolved into a leading pharmaceutical company in the country. Over the years, Kimia Farma has expanded its operations to include the production, distribution, and retail of pharmaceuticals, health supplements, and medical devices. With a commitment to improving public health, Kimia Farma has grown into a trusted name in the healthcare industry, operating through a network of pharmacies, clinics, and distribution channels across Indonesia.

Kimia Farma is one of Indonesia's leading pharmaceutical companies, specializing in the production, distribution, and retail of pharmaceutical products, health supplements, and medical devices. Established in 1817, Kimia Farma has a long history of contributing to the healthcare sector, providing high-quality products and services that meet the needs of the Indonesian community. The company operates through a network of pharmacies, clinics, and distribution channels, with a strong commitment to improving public health and well-being.

## Project Portofolio
The Big Data Analytics project at Kimia Farma focuses on utilizing data-driven insights to optimize business processes and enhance decision-making across various functions. As a leading pharmaceutical company in Indonesia, Kimia Farma has accumulated vast amounts of data from transactions, product inventories, and branch performance. However, there is a need to better analyze and leverage this data to drive improvements in sales performance, operational efficiency, and customer understanding. The project aims to aggregate and analyze data from four key datasets: transaction details from kf_final_transaction.csv, inventory data from kf_inventory.csv, branch information from kf_kantor_cabang.csv, and product data from kf_product.csv. The objective is to uncover patterns and trends that can help Kimia Farma make more informed decisions, identify growth opportunities, and enhance overall performance. The key challenge lies in transforming this data into actionable insights that can support the company's strategic goals and improve its competitive advantage in the pharmaceutical market.

## Final Task

**Dataset**
- kf_final_transaction
- kf_inventory
- kf_kantor_cabang
- kf_product

**Data Analysis**
For this project, you are required to create an analysis table based on the aggregated results from the four previously imported tables. The mandatory columns for this table are:
transaction_id : kode id transaksi,
- date : tanggal transaksi dilakukan,
- branch_id : kode id cabang Kimia Farma,
- branch_name : nama cabang Kimia Farma,
- kota : kota cabang Kimia Farma,
- provinsi : provinsi cabang Kimia Farma,
- rating_cabang : penilaian konsumen terhadap cabang Kimia
Farma
- customer_name : Nama customer yang melakukan
transaksi,
- product_id : kode product obat,
- product_name : nama obat,
- actual_price : harga obat,
- discount_percentage : Persentase diskon yang diberikan
pada obat,
- persentase_gross_laba : Persentase laba yang seharusnya
diterima dari obat dengan ketentuan berikut:
  - Harga <= Rp 50.000 -> laba 10%
  - Harga > Rp 50.000 - 100.000 -> laba 15%
  - Harga > Rp 100.000 - 300.000 -> laba 20%
  - Harga > Rp 300.000 - 500.000 -> laba 25%
  - Harga > Rp 500.000 -> laba 30%,
- nett_sales : harga setelah diskon,
- nett_profit : keuntungan yang diperoleh Kimia Farma,
- rating_transaksi : penilaian konsumen terhadap transaksi
yang dilakukan.

**BigQuery Syntax**
<details>
  <summary> Click to see query </summary>
    <br>
    
```sql
CREATE TABLE kimia_farma.kf_analysis AS
SELECT tr.transaction_id, 
  tr.date, 
  tr.branch_id, 
  kc.branch_name, 
  kc.kota, 
  kc.provinsi, 
  kc.rating AS rating_cabang, 
  tr.customer_name, 
  tr.product_id, 
  pd.product_name, 
  pd.price AS actual_price, 
  tr.discount_percentage,
  CASE
    WHEN pd.price <= 50000 then 0.1
    WHEN pd.price > 50000 AND pd.price <= 100000 then 0.15
    WHEN pd.price > 100000 AND pd.price <= 300000 then 0.2
    WHEN pd.price > 300000 AND pd.price <= 500000 then 0.25
    ELSE 0.3      --pd.price >500000 
    END AS persentase_gross_laba,
  (pd.price * (1-tr.discount_percentage)) AS nett_sales,
  ((pd.price * (1-tr.discount_percentage)) - tr.price)AS nett_profit,
  tr.rating AS rating_transaksi
FROM kimia_farma.kf_final_transaction AS tr 
  LEFT JOIN kimia_farma.kf_kantor_cabang AS kc 
  ON (tr.branch_id = kc.branch_id)
  LEFT JOIN kimia_farma.kf_product AS pd
  ON (tr.product_id = pd.product_id)
;
```
    
<br>
</details>
<br>

> The query can also be accessed through the file: query.sql

<p align="center">
    <kbd> <img width="1000" alt="analysis result" src="./assets/analysis-result.png"> </kbd> <br>
    Tabel Hasil Analisis
</p>
<br>

---



