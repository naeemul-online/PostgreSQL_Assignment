# PostgreSQL – Bonus Questions

---

## 1. What is PostgreSQL?

**PostgreSQL** হলো একটি শক্তিশালী, ওপেন-সোর্স, অবজেক্ট-রিলেশনাল ডেটাবেস ম্যানেজমেন্ট সিস্টেম (ORDBMS)। এটি মূলত SQL ব্যবহার করে, কিন্তু এতে কিছু আধুনিক প্রোগ্রামিং ধারণা যেমন Table Inheritance, User-defined Types, এবং Full-text Search অন্তর্ভুক্ত রয়েছে।

### বিশেষ বৈশিষ্ট্যসমূহ:

- **ACID** (Atomicity, Consistency, Isolation, Durability) সম্পূর্ণভাবে সমর্থন করে, ফলে এটি ট্রান্সঅ্যাকশন সিস্টেমে নির্ভরযোগ্য।
- **JSON** এবং **XML** data সমর্থন করে, ফলে semi-structured data হ্যান্ডল করা যায়।
- **MVCC** (Multi-Version Concurrency Control) ব্যবহার করে, যা ডেটাবেইস ব্যবহারে লকিং কমিয়ে দেয় এবং পারফরম্যান্স বাড়ায়।

### ব্যবহার ক্ষেত্র:
ব্যাঙ্কিং সিস্টেম, জিওগ্রাফিক ইনফরমেশন সিস্টেম (GIS), এনালাইটিকস প্ল্যাটফর্ম, এবং বড় স্কেল SaaS অ্যাপ্লিকেশন।

---

## 2. What is the purpose of a database schema in PostgreSQL?

একটি **ডেটাবেইস স্কিমা** হলো একধরনের namespace যা ডেটা অবজেক্ট যেমন টেবিল, ভিউ, ফাংশন, এবং ইনডেক্সকে গ্রুপ করে। স্কিমা মূলত ব্যবহৃত হয় ডেটার সংগঠন এবং একাধিক ইউজারের মধ্যে আইসোলেশন তৈরি করার জন্য।

### মূল উদ্দেশ্য:

- বিভিন্ন প্রজেক্ট বা মডিউলের মধ্যে অবজেক্ট আলাদা রাখতে।
- একই নামে বিভিন্ন টেবিল রাখার সুযোগ দিতে ভিন্ন স্কিমাতে।
- নিরাপত্তা ও পারমিশন নিয়ন্ত্রণের সুবিধা প্রদান।

### উদাহরণ:

```sql
CREATE SCHEMA hr;

CREATE TABLE hr.employees (
  id SERIAL PRIMARY KEY,
  name TEXT,
  department TEXT
);
```


## 3. Explain the Primary Key and Foreign Key Concepts in PostgreSQL

### Primary Key

একটি **Primary Key** এমন একটি কলাম বা কলামগুলোর সমষ্টি যা প্রতিটি রেকর্ডকে ইউনিকভাবে চিহ্নিত করে। এটি কখনো `NULL` হতে পারে না এবং PostgreSQL এটি অটোমেটিকভাবে ইনডেক্স করে।

#### উদাহরণ:
```sql
CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);
```

এখানে student_id হচ্ছে Primary Key যা প্রতিটি শিক্ষার্থীকে আলাদা করে।

#### Foreign Key
Foreign Key ব্যবহার করে একটি টেবিলের মধ্যে অন্য একটি টেবিলের Primary Key রেফারেন্স করা হয়। এর মাধ্যমে টেবিলগুলোর মধ্যে সম্পর্ক (relationship) তৈরি হয় এবং ডেটার অখণ্ডতা নিশ্চিত হয়।

#### উদাহরণ:

```sql
CREATE TABLE enrollments (
  id SERIAL PRIMARY KEY,
  student_id INT REFERENCES students(student_id),
  course_name TEXT
);
```
এখানে enrollments.student_id ফিল্ডটি students.student_id কে রেফারেন্স করে। এর ফলে আপনি নিশ্চিত করতে পারেন যে student_id কেবল তখনই ইনসার্ট হবে যদি তা students টেবিলে আগে থেকে থাকে।


##  4. What is the Difference Between the `VARCHAR` and `CHAR` Data Types?

###  `CHAR(n)`

* Fixed-length ডেটা টাইপ।
* আপনি যদি `CHAR(10)` ডিফাইন করেন এবং `"abc"` ইনপুট দেন, তাহলে এটি `"abc       "` এইভাবে স্টোর করে (extra space দিয়ে পূর্ণ করে)।
* সাধারণত legacy systems-এ ব্যবহার করা হতো।

### `VARCHAR(n)`

* Variable-length ডেটা টাইপ।
* ইনপুট যতটুকু, ঠিক ততটুকুই স্পেস নেয় (space efficiency)।
* আধুনিক অ্যাপ্লিকেশনে ব্যাপকভাবে ব্যবহৃত হয়।

#### উদাহরণ:

```sql
name CHAR(10)    -- 'Ali       '
name VARCHAR(10) -- 'Ali'
```

**বাস্তবিক ব্যবহারে:** `VARCHAR` বেশি কার্যকর কারণ এটি স্টোরেজ-এ কার্যকর এবং ফ্লেক্সিবল।

---

## 5. Explain the Purpose of the `WHERE` Clause in a `SELECT` Statement

**`WHERE` ক্লজ** একটি `SELECT`, `UPDATE`, বা `DELETE` স্টেটমেন্টে ব্যবহার করা হয় ডেটা ফিল্টার করার জন্য। এটি নির্দিষ্ট শর্ত অনুযায়ী রেকর্ড নির্বাচন করে, অর্থাৎ আপনি যেসব শর্ত পূরণ করে এমন ডেটা চান, কেবল সেগুলোকেই আনে।

### এর প্রয়োজনীয়তা:

* সবসময় পুরো টেবিল থেকে ডেটা দেখা প্রয়োজন হয় না।
* নির্দিষ্ট শর্ত অনুযায়ী ফিল্টার করে শুধুমাত্র প্রয়োজনীয় রেকর্ড পাওয়া যায়।
* ডেটা প্রোসেসিং এবং রিপোর্টিং সহজ হয়।

#### উদাহরণ:

```sql
SELECT * FROM employees WHERE department = 'IT';
```

এখানে কেবল ‘IT’ বিভাগের এমপ্লয়িদের তথ্য রিটার্ন করবে।

```sql
DELETE FROM orders WHERE status = 'Cancelled';
```

এখানে কেবল ‘Cancelled’ অর্ডারগুলো ডিলিট হবে।

