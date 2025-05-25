### ১. PostgreSQL কী?

PostgreSQL একটি ওপেন সোর্স, অবজেক্ট-রিলেশনাল ডাটাবেস ম্যানেজমেন্ট সিস্টেম (ORDBMS)। এটি উন্নত ফিচার যেমন ট্রানজেকশন সাপোর্ট, জটিল কুয়েরি, কাস্টম ফাংশন, স্টোরড প্রোসিডিউর এবং কনকারেন্সি হ্যান্ডল করে। এটি বড় স্কেল প্রজেক্টে নির্ভরযোগ্য ও নিরাপদ ডেটা ব্যবস্থাপনায় জনপ্রিয়।

---

### ২. PostgreSQL-এ একটি ডেটাবেস স্কিমার উদ্দেশ্য কী?

স্কিমা হলো ডেটাবেসের মধ্যে লজিক্যাল স্ট্রাকচার বা আলাদা কাজের পরিবেশ। এতে বিভিন্ন টেবিল, ফাংশন, ভিউ আলাদা আলাদা রাখা যায়। একাধিক স্কিমা থাকলে এক ডেটাবেসে বিভিন্ন প্রজেক্ট বা ব্যবহারকারী আলাদা রাখতে সুবিধা হয়।

```sql
CREATE SCHEMA wildlife_data;
CREATE TABLE wildlife_data.rangers (...);
```

---

### ৩. Primary Key এবং Foreign Key এর ধারণা ব্যাখ্যা করো।

- **Primary Key** একটি টেবিলের ইউনিক কলাম যা প্রতিটি রেকর্ডকে আলাদা করে। এটি কখনো NULL হয় না।
- **Foreign Key** এমন একটি কলাম যা অন্য টেবিলের Primary Key-কে রেফার করে। এটি টেবিলগুলোর মধ্যে সম্পর্ক স্থাপন করে।

```sql
CREATE TABLE species (
    species_id SERIAL PRIMARY KEY,
    common_name VARCHAR(100)
);

CREATE TABLE sightings (
    sighting_id SERIAL PRIMARY KEY,
    species_id INT REFERENCES species(species_id)
);
```

---

### ৪. `VARCHAR` এবং `CHAR` এর মধ্যে পার্থক্য কী?

- `CHAR(n)` ফিক্সড লেন্থ: ইনপুট ছোট হলেও n ক্যারেক্টার জায়গা নেয়।
- `VARCHAR(n)` ভ্যারিয়েবল লেন্থ: যতটুকু দরকার ততটুকুই জায়গা নেয়।

 সাধারণভাবে `VARCHAR` বেশি ব্যবহৃত হয়।

---

### ৫. `WHERE` ক্লজের উদ্দেশ্য কী?

`WHERE` ডেটা ফিল্টার করতে ব্যবহৃত হয়। এটি দিয়ে কুয়েরিতে নির্দিষ্ট শর্ত প্রয়োগ করা যায়।

```sql
SELECT * FROM sightings
WHERE location LIKE '%Pass%';
```

---

### ৬. `LIMIT` এবং `OFFSET` কী কাজে ব্যবহৃত হয়?

- **LIMIT**: কয়টি রেকর্ড দেখাবে তা নির্ধারণ করে।
- **OFFSET**: কতটি রেকর্ড স্কিপ করে শুরু করবে।

```sql
SELECT * FROM sightings
ORDER BY sighting_time DESC
LIMIT 2 OFFSET 1;
```

---

### ৭. `UPDATE` দিয়ে ডেটা কিভাবে পরিবর্তন করা যায়?

`UPDATE` স্টেটমেন্ট ব্যবহার করে টেবিলের নির্দিষ্ট রেকর্ডের মান পরিবর্তন করা যায়।

```sql
UPDATE species
SET conservation_status = 'Historic'
WHERE discovery_date < '1800-01-01';
```

---

### ৮. `JOIN` কী এবং কেন দরকার?

`JOIN` একাধিক টেবিল থেকে সম্পর্কযুক্ত ডেটা আনতে ব্যবহৃত হয়। সাধারণত Foreign Key দিয়ে সম্পর্ক তৈরি হয়।

```sql
SELECT r.name, s.location
FROM rangers r
JOIN sightings s ON r.ranger_id = s.ranger_id;
```

---

### ৯. `GROUP BY` ক্লজ এবং Aggregation-এ ব্যবহার:

`GROUP BY` এক বা একাধিক কলামের উপর ভিত্তি করে রেকর্ড গ্রুপ করে, এবং প্রতিটি গ্রুপে Aggregation Function প্রয়োগ করা যায়।

```sql
SELECT ranger_id, COUNT(*) AS total_sightings
FROM sightings
GROUP BY ranger_id;
```

---

### ১০. Aggregate ফাংশন: `COUNT()`, `SUM()`, `AVG()`

Aggregate ফাংশন একাধিক রেকর্ড থেকে একটি রেজাল্ট দেয়।

- `COUNT()` — মোট রেকর্ড গোনে
- `SUM()` — মান যোগফল
- `AVG()` — গড় হিসাব

```sql
SELECT COUNT(*) FROM sightings;
SELECT AVG(species_id) FROM sightings;
```
