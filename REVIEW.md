# REVIEW.md

List every defect you found in the starter `app/main.py` (helpers and endpoints). For each one:

| # | Where (function / line) | What is wrong | How you'd notice it (test, input, or symptom) | How you fixed it |
|---|---|---|---|---|
| 1 | `list_employees` (GET /employees), the skip / total / items lines | Three defects: (a) skip was `page * page_size`, so page 1 skipped the first 20 documents; (b) `total` used `count_documents({})`, ignoring the department filter; (c) there was no sort, so page order was not guaranteed. | Request `GET /employees` with the defaults: `total` is 6 but `items` is `[]`. With `?department=X`, `total` still counts every employee. | Changed skip to `(page - 1) * page_size`, changed `count_documents({})` to `count_documents(q)` so total follows the same filter as the list, and added `.sort("emp_code", 1)`. |

Also note anything you looked at and decided was **not** a defect, and why.
