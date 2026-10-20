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


### 3. Hansı bölgədə güclüyük, hansında zəifik?

| Sıra | Şəhər | Total |
|------|-------|-------|
| 1 | Bakı | 23580 |
| 2 | Şəki | 17300 |
| 3 | Sumqayıt | 10460 |
| 4 | Gəncə | 6340 |
| 5 | Lənkəran | 3350 |
| 6 | Naxçıvan | 2390 |

**Tapıntı:** Bakı satışın təxminən 37%-ni verir, Bakı və Şəki birlikdə 64,5%-ni. İlk 3 şəhər satışın təxminən 81%-ni təşkil edir. Lənkəran (5,3%) və Naxçıvan (3,8%) ən zəif bölgələrdir. Güclü şəhərləri qorumaq, zəif bölgələrdə isə satışı artırmaq üçün kampaniyalar lazımdır.


### 4. Hansı müştərilər artıq alış-veriş etmir?

Sorğu heç alış etməyən və ya son 3 ayda alış etməyən müştəriləri göstərir.

| Müştəri ID | Müştəri adı | Son alış | Tamamlanan say |
|------------|-------------|----------|----------------|
| 5 | Vüsal Məmmədov | - | 0 |
| 14 | Məryəm Əsgərova | - | 0 |
| 9 | Rəşad Quliyev | - | 0 |
| 7 | Tural Bayramov | - | 0 |

**Tapıntı:** 4 müştəri heç vaxt tamamlanmış alış etməyib. Son 3 ayda alışı dayandıran müştəri yoxdur. Bu müştərilərə ilk alış üçün fərdi təklif göndərmək olar. Onların sifarişlərinin ləğv və ya qaytarılma ilə bitib-bitmədiyini ayrıca yoxlamaq faydalıdır.


### 5. Ən dəyərli müştərilərimiz kimlərdir?

Sorğu hər müştəri üçün məhsul üzrə və ümumi (Subtotal) cəmləri göstərir (ROLLUP). Aşağıda ən çox alış edən 5 müştəri verilib.

| Müştəri | Ədəd | Total | Pay (%) |
|---------|------|-------|---------|
| Səbinə Orucova | 17 | 17300 | 27.3 |
| Kamran Əliyev | 23 | 15340 | 24.2 |
| Günel Rəhimova | 13 | 9200 | 14.5 |
| Əli Hüseynov | 10 | 7600 | 12.0 |
| Cavid Ağayev | 17 | 5140 | 8.1 |

**Tapıntı:** İlk 2 müştəri satışın təxminən 51.5%-ni, ilk 5 müştəri 86.1%-ni verir. Səbinə Orucovanın alışının təxminən 87%-i, Kamran Əliyevin alışının təxminən 78%-i noutbukdur. Gəlir az sayda müştəridən və bir məhsuldan asılıdır. Bu müştərilərə sadiqlik təklifləri vermək, eyni zamanda yeni müştəri cəlb etmək lazımdır.


### 6. Bir məhsuldan nə qədər asılıyıq?

| Məhsul | Total | Faiz (%) | Kumulyativ faiz (%) |
|--------|-------|----------|---------------------|
| Noutbuk | 46500 | 73.3 | 73.3 |
| Monitor | 7600 | 12 | 85.3 |
| Printer | 3500 | 5.5 | 90.8 |
| Planşet | 2800 | 4.4 | 95.2 |
| Qulaqlıq | 1680 | 2.6 | 97.9 |
| Klaviatura | 800 | 1.3 | 99.1 |
| Siçan | 540 | 0.9 | 100 |

**Tapıntı:** Noutbuk tək başına qazancın 73.3%-ni verir, ilk 3 məhsul isə 90.8%-ni. Gəlir bir məhsuldan çox asılıdır. Monitor, printer və planşet satışını artırmaq, aksesuarları noutbukla birgə təklif etmək asılılığı azalda bilər.


### 7. Hansı məhsul "ölü qalır"?

Sorğu heç tamamlanmış satışı olmayan məhsulları göstərir.

| Məhsul |
|--------|
| Veb kamera |

**Tapıntı:** Veb kamera heç vaxt tamamlanmış satış etməyib və anbarda pul bağlayır. Endirim, noutbukla birgə paket təklifi və ya tələbin araşdırılması məsləhətdir.

## Ümumi nəticələr

- Gəlirin 73.3%-i bir məhsuldan (Noutbuk) gəlir, asılılıq riski yüksəkdir.
- Satış sentyabrda zirvəyə çatıb, oktyabr və noyabrda kəskin düşüb.
- Bakı və Şəki satışın 64.5%-ni verir, Lənkəran və Naxçıvan zəifdir.
- İlk 2 müştəri satışın təxminən 51.5%-ni verir.
- 4 müştəri heç vaxt alış etməyib, 1 məhsulun (Veb kamera) satışı yoxdur.

> Qeyd: Layihədə sintetik (saxta) məlumat istifadə olunub.
