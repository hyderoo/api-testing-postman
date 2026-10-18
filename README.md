# API Testing Portfolio - Postman + Newman

[![Newman API tests](https://github.com/hyderoo/api-testing-postman/actions/workflows/newman.yml/badge.svg)](https://github.com/hyderoo/api-testing-postman/actions/workflows/newman.yml)

API test automation practice against **public** Formula 1 APIs, built with
Postman and run in CI with Newman. Everything here tests free, unauthenticated
public endpoints - no private or employer data involved.

- **Live HTML report (GitHub Pages):** <https://hyderoo.github.io/api-testing-postman/>
- Runs on every collection change, on demand, and weekly as a light
  availability monitor for the two public APIs.

## APIs under test

| API | What it is | Docs |
| --- | --- | --- |
| Jolpica (Ergast-compatible) | Historical F1 data: seasons, races, results, standings | <https://github.com/jolpica/jolpica-f1> |
| OpenF1 | Live and historical timing data: sessions, drivers, laps | <https://openf1.org> |

## What the suite covers

- Functional assertions on real payloads (standings ordering, sequential
  finishing positions, required fields on every record)
- Dynamic data chaining (latest race session key feeds the driver and lap
  requests)
- Pagination behaviour (limit/offset honoured, pages differ)
- Error handling and documented API quirks, verified against the live APIs:
  - Jolpica: invalid season path -> 404; future season -> 200 with an empty
    standings list
  - OpenF1: invalid route -> 400 `Invalid route`; valid endpoint with no
    matches -> 404 `No results found.`; rate limit 3 requests/s (the run uses
    a 1.2 s delay between requests)
- Performance budgets (every response under 3 s)

## Run it locally

```bash
npx -y -p newman -p newman-reporter-htmlextra \
  newman run collections/f1-public-apis.postman_collection.json \
  --delay-request 1200 \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/index.html
```

The collection is a standard Postman v2.1 file, so it also imports into the
Postman app as-is.

## Structure

```
collections/f1-public-apis.postman_collection.json   the test suite
.github/workflows/newman.yml                         CI: Newman + HTML report to GitHub Pages
```

*Practice portfolio by Hafiz Hamdani (QA engineer). Feedback welcome via issues.*
