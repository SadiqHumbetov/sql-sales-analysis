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


### 2. Satış artır, yoxsa azalır?

| Ay | Total | Fərq | Faiz (%) |
|----|-------|------|----------|
| 2026-07 | 10380 | - | - |
| 2026-08 | 19400 | 9020 | 86.9 |
| 2026-09 | 20110 | 710 | 3.7 |
| 2026-10 | 5580 | -14530 | -72.3 |
| 2026-11 | 1720 | -3860 | -69.2 |
| 2026-12 | 6230 | 4510 | 262.2 |

**Tapıntı:** Satış iyuldan sentyabra qədər artıb və sentyabrda zirvəyə çatıb (20110). Oktyabr və noyabrda kəskin düşüş olub (-72.3% və -69.2%). Dekabrdakı +262.2% artım aşağı bazadan gəldiyi üçün şişkindir, dekabr hələ də sentyabrın təxminən 31%-i qədərdir. Düşüşün səbəbi araşdırılmalıdır.

> Qeyd: Layihədə sintetik (saxta) məlumat istifadə olunub.


