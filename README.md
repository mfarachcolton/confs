# confs.farach-colton.com

Upcoming conference submission deadlines, served by GitHub Pages.

- `index.html` renders the table from `confs.json` in the browser (sorting, "days left"; once its submission deadline passes, a conference moves to a second table until its notification date; there, an earlier cycle of a multi-cycle edition is not shown unless it has `my_submissions`).
- `confs.json` is the only file that changes in a weekly update.

## confs.json

```json
{
  "updated": "YYYY-MM-DD",
  "conferences": [
    {
      "acronym": "STOC", "full_name": "ACM Symposium on Theory of Computing", "year": 2027,
      "url": "https://...",            // official site of this edition
      "deadline": "YYYY-MM-DD",        // full-paper deadline, converted to New York time
      "deadline_tz": "7:59 am EDT",    // New York time; may add a cycle name, e.g. " · spring"
      "abstract_deadline": null,       // YYYY-MM-DD if separate, New York time
      "abstract_tz": null,             // e.g. "6:59 am EST", New York time
      "rebuttal": "YYYY-MM-DD to YYYY-MM-DD", // author response period; null if none listed
      "notification": "YYYY-MM-DD",
      "conf_dates": "YYYY-MM-DD to YYYY-MM-DD",
      "location": "City, Country",     // "City, ST" in the US; or null
      "cycle": 2,                      // this row's submission cycle number (1 if the edition has a single deadline)
      "cycles": 4,                     // total number of cycles in this edition; badge shows "cycle/cycles" (1/1 = single deadline)
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
5. Set `cycle` and `cycles` on every row: `1`/`1` when this edition has a single submission deadline (ICALP counts as single: Track A has one deadline; ignore Track B rounds). An edition with several cycles (SPAA, PODS, ICDT, SIGMOD, FAST, …) has one row per cycle, each with `cycle` (its number) and `cycles` (the total). The page shows a `cycle/cycles` badge on every row, in both tables. Keep all cycle rows of an edition, past ones included (the page hides a row once its notification passes), and replace them with the next edition's cycle rows only after the last cycle's notification. Don't put the cycle number in `deadline_tz`; a cycle name may follow the time (`"7:59 am EDT · spring"`).
6. Set `updated` to today, commit, and push to `main`.

## My papers

`my_papers` in `confs.json` lists upcoming conferences where I have accepted papers:
`{acronym, year, full_name, url, conf_dates, location, hotel: {name, url, note}, source, papers: [{title, authors: [...]}]}`.
They appear at the top of the second table, by conference date, until the conference ends.
Each paper may have `talk: {date, time, session, room}` once the program is out. `invited: [{kind, title, talk}]` lists keynote/plenary/invited talks; an entry with only an invited talk has `papers: []`.
Rows in `conferences` may have `hotel: {name, url, note}`; it is shown in the second table.
A row may have `pc` ("PC member", "PC chair", …): shown as an "On PC" badge; rows with `pc` or `my_submissions` have no Hide button and are never hidden.
A row may have `my_planned: [{title, authors}]`, papers I plan to submit there, shown as "Planning to submit" under the row (set by the paper-status skill).
A planned paper may carry `overleaf: {project, file, title_seen}` (its Overleaf project id, main .tex, and the last title read there); the page ignores it, and a background job (`~/.claude/skills/paper-status/scripts/sync_planned_titles.py`) uses it to update `title` when the paper is retitled on Overleaf.

## Theory / applied split

The upcoming-deadlines part of the page is split into a Theory table and an Applied table. The theory series are listed in the `THEORY` array in index.html (STOC, FOCS, SODA, SOSA, ICALP, ESA, ITCS, LATIN, COLT, PODC, SPAA, PODS); every other series appears under Applied.

## Rolling series (VLDB)

PVLDB has 12 monthly deadlines per volume (the 1st of each month, 5:00 pm Pacific; abstract by the 25th of the previous month). Its rows carry `"rolling": true` and `"cycles": 12`, one row per monthly cycle, for two volumes (VLDB YYYY = PVLDB volume YYYY-2007, deadlines April 1 of YYYY-1 through March 1 of YYYY). The page shows only the next upcoming VLDB deadline; clicking its n/12 badge steps the row to the following month. Past rolling cycles appear in the second table only if they have my_submissions.
