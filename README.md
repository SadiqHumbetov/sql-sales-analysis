# E-ticarət satış analizi (Oracle SQL)

Bu layihədə `orders`, `customers` və `products` cədvəlləri üzərində biznes suallarına SQL ilə cavab axtarılıb.

**İstifadə olunan texnikalar:** JOIN, GROUP BY, window functions (RANK, LAG, RATIO_TO_REPORT), ROLLUP, CTE, CASE

Bütün sorğular [queries.sql](queries.sql) faylındadır. Yalnız `Completed` statuslu sifarişlər nəzərə alınır.

## Analizlər

### 1. Hansı məhsul qazancın əsasını verir?

| Məhsul | Total | Faiz (%) |
|--------|-------|----------|
| Noutbuk | 46500 | 73.3 |
| Monitor | 7600 | 12 |
| Printer | 3500 | 5.5 |
| Planşet | 2800 | 4.4 |
| Qulaqlıq | 1680 | 2.6 |
| Klaviatura | 800 | 1.3 |
| Siçan | 540 | 0.9 |

**Tapıntı:** Noutbuk qazancın 73,3%-ni verir, ilk 2 məhsul isə 85,3%-ni. Gəlir bir məhsuldan çox asılıdır, bu da risk yaradır. Digər məhsulların satışını artırmaq lazımdır.
