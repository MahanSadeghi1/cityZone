# CityZone API Documentation (v1)

## مقدمه
این API امکان دسترسی به اطلاعات شهرها، استان‌ها و محله‌ها را فراهم می‌کند. تمام درخواست‌ها باید با استفاده از API key معتبر ارسال شوند.

## احراز هویت
برای احراز هویت، API key خود را در هدر درخواست تحت کلید `x-api-key` ارسال کنید:

```http
x-api-key: YOUR_API_KEY_HERE
```

### وضعیت‌های مربوط به API Key

| کد وضعیت | توضیحات |
|-----------|----------|
| 3 | API key ارسال نشده است |
| 4 | فرمت API key اشتباه است |
| 5 | API key نامعتبر است |
| 6 | اشتراک فعال/تایید نشده است |
| 7 | اشتراک منقضی شده است |
| 8 | فرمت ورودی کاربر اشتباه است |
| 500 | خطای داخلی سرور |

### وضعیت‌های عمومی پاسخ

| کد وضعیت | توضیحات | مثال خروجی |
|-----------|----------|-------------|
| 1 | عملیات موفق، داده‌ها ارسال می‌شوند | `{"status": 1, "data": [...]}` |
| 2 | عملیات موفق، اما داده‌ای یافت نشد | `{"status": 2, "data": []}` |

## نقاط دسترسی (Endpoints)

### 1. دریافت تمام شهرها
```http
GET https://api.cityzone.ir/getdata/v1/cities
```

**پاسخ موفق:**
```json
{
  "status": 1,
  "data": [
    {"id": 118, "name": "آب علی"},
    {"id": 119, "name": "آستارا"}
  ]
}
```

### 2. دریافت اطلاعات شهر به همراه محله‌هایش
```http
GET https://api.cityzone.ir/getdata/v1/cities/:cityId
```

**پارامترها:**
- `cityId` (عدد): شناسه شهر

### 3. دریافت محله‌های یک شهر با نام
```http
GET https://api.cityzone.ir/getdata/v1/districts/:cityName
```

**پارامترها:**
- `cityName` (متن): نام فارسی شهر

### 4. دریافت اطلاعات یک محله
```http
GET https://api.cityzone.ir/getdata/v1/districts/:districtId
```

**پارامترها:**
- `districtId` (عدد): شناسه محله

### 5. دریافت استان‌های یک کشور
```http
GET https://api.cityzone.ir/getdata/v1/provinces/:country
```

**پارامترها:**
- `country` (متن): کد دو حرفی کشور (مثال: IR)

### 6. دریافت اطلاعات استان به همراه شهرهایش
```http
GET https://api.cityzone.ir/getdata/v1/provinces/cities/:provinceId
```

**پارامترها:**
- `provinceId` (عدد): شناسه استان

## نکات مهم
- تمام درخواست‌ها باید حاوی هدر `x-api-key` باشند
- پاسخ‌های موفق همیشه حاوی کلید data هستند
- پاسخ‌های خطا حاوی کد وضعیت و پیغام خطای مربوطه هستند
- مختصات جغرافیایی به صورت `[longitude, latitude]` ارسال می‌شوند

## مثال‌های پاسخ

<details>
<summary>مثال پاسخ دریافت محله‌ها</summary>

```json
{
  "status": 1,
  "data": {
    "name": "اصفهان",
    "districts": [
      {
        "name": "اتوبان بندرگز",
        "type": "district",
        "geo_center": [53.982132, 36.761244]
      }
    ]
  }
}
```
</details>

<details>
<summary>مثال پاسخ دریافت استان‌ها</summary>

```json
{
  "status": 1,
  "data": {
    "name": "مازندران",
    "cities": [
      {"name": "آمل", "type": "city"},
      {"name": "بابل", "type": "city"}
    ]
  }
}
```
</details>