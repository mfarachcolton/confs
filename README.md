# confs.farach-colton.com

Upcoming submission deadlines for theory-of-algorithms conferences, served by GitHub Pages.

- `index.html` renders the table from `confs.json` in the browser (sorting, "days left", past-deadline section).
- `confs.json` is the only file that changes in a weekly update.

## confs.json

```json
{
  "updated": "YYYY-MM-DD",
  "conferences": [
    {
      "acronym": "STOC", "full_name": "ACM Symposium on Theory of Computing", "year": 2027,
      "url": "https://...",            // official site of this edition
      "deadline": "YYYY-MM-DD",        // full-paper deadline
      "deadline_tz": "AoE",            // or null
      "abstract_deadline": null,       // YYYY-MM-DD if separate
      "notification": "YYYY-MM-DD",
      "conf_dates": "YYYY-MM-DD to YYYY-MM-DD",
      "location": "City, Country",     // or null
      "status": "announced",           // or "estimated" (CFP not out; projected from last year)
      "source": "https://...",         // page the dates were read from
      "notes": ""
    }
  ]
}
```

## Weekly update procedure

1. For every conference in the list, check the official site of the edition listed.
2. If a conference's notification date has passed, replace the row with the next edition (estimated from this year's dates + 1 year until its CFP appears).
3. If an `estimated` row's CFP is now out, replace the dates with the announced ones and set `status` to `announced`.
4. Never invent dates or URLs; read them from the official page and record it in `source`.
5. Set `updated` to today, commit, and push to `main`.
