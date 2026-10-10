# REVIEW.md

List every defect you found in the starter `app/main.py` (helpers and endpoints). For each one:

| # | Where (function / line) | What is wrong | How you'd notice it (test, input, or symptom) | How you fixed it |
|---|---|---|---|---|
| 1 | `list_employees` (GET /employees), the skip / total / items lines | Three defects: (a) skip was `page * page_size`, so page 1 skipped the first 20 documents; (b) `total` used `count_documents({})`, ignoring the department filter; (c) there was no sort, so page order was not guaranteed. | Request `GET /employees` with the defaults: `total` is 6 but `items` is `[]`. With `?department=X`, `total` still counts every employee. | Changed skip to `(page - 1) * page_size`, changed `count_documents({})` to `count_documents(q)` so total follows the same filter as the list, and added `.sort("emp_code", 1)`. |
| 3 | `punch_in`, the `ts` line | The date was computed in the server's local time, not IST, and milliseconds were not truncated. A punch-in at 20:00 UTC got the wrong date on a UTC server. | POST punch-in with punched_at 1791662400500: the date should be 2026-10-11 (IST). | Built the time with tz=IST and used integer division by 1000 to drop milliseconds. |
| 4 | `PunchInIn` model, the `punched_at` field | Any integer was accepted, including 0 and values in seconds, so a bad timestamp created a record on a wrong date such as 1970-01-01. | POST punch-in with punched_at 0 returned 201. | Added `Field(ge=100000000000)` so values below that, which look like seconds, are rejected with 422. |
Also note anything you looked at and decided was **not** a defect, and why.
