# Restful-Booker API Test Suite

![CI Status](https://github.com/SanthiyaSakthi/restful-booker-api-testing/actions/workflows/postman-tests.yml/badge.svg)

🔗 **Repository:** https://github.com/SanthiyaSakthi/restful-booker-api-testing

An enterprise-style REST API test automation project built with Postman, Newman, and GitHub Actions — testing the [Restful-Booker](https://restful-booker.herokuapp.com/apidoc/index.html) hotel booking API.

## Overview

This project demonstrates a full API test automation workflow: authentication, CRUD operations, schema validation, negative/edge case testing, data-driven testing, and CI/CD integration — built to reflect real enterprise QA practices rather than just happy-path testing.

## Tech Stack

- **Postman** — request design, assertions, environment management
- **Newman** — CLI test execution
- **newman-reporter-htmlextra** — HTML test reports
- **GitHub Actions** — CI/CD pipeline (runs on every push/PR)

## What's Covered

- ✅ Token-based authentication with request chaining
- ✅ Full CRUD (Create, Read, Update, Delete) with response validation
- ✅ JSON Schema / contract validation
- ✅ Pre-request scripts (dynamic date generation)
- ✅ Data-driven testing via CSV, isolated in its own CI run
- ✅ 7+ negative and edge case scenarios, each documented as a defect, design observation, or confirmed-correct behavior
- ✅ Automated CI/CD pipeline with separate reports for functional and data-driven tests

## Project Structure

```
restful-booker-api-testing/
├── .github/                         
│   └── workflows/                         # CI/CD pipeline
│       └── postman-tests.yml              
├── collections/                           # Postman collection
│   └── restful-booker-collection.json
├── environments/                          # Postman environment (secrets scrubbed)
│   └── restful-booker-env.json
├── data/                                  # CSV test data for data-driven tests
│   └── booking-test-data.csv
├── reports/                               # Generated HTML test reports
│   └── (generated HTML reports)
├── README.md
└── .gitignore
```

## Running Locally

```bash
npm install -g newman newman-reporter-htmlextra

newman run collections/restful-booker-collection.json \
  -e environments/restful-booker-env.json \
  --folder "auth" --folder "Health & Smoke Tests" --folder "Booking - CRUD" \
  -r cli,htmlextra --reporter-htmlextra-export reports/main-report.html

newman run collections/restful-booker-collection.json \
  -e environments/restful-booker-env.json \
  --folder "Data-Driven Tests" -d data/booking-test-data.csv \
  -r cli,htmlextra --reporter-htmlextra-export reports/data-driven-report.html
```

## Test Reports

- [Main Test Report (HTML)](./reports/main-report.html)
- [Data-Driven Test Report (HTML)](./reports/data-driven-report.html)

> Download and open locally in a browser to view the full interactive report — GitHub does not render HTML files inline.

Latest automated test reports are available as downloadable artifacts on the [GitHub Actions runs page](https://github.com/SanthiyaSakthi/restful-booker-api-testing/actions).


## Key Findings / Defects Documented

| Scenario | Result | Category |
|---|---|---|
| Missing required field (`firstname`, `totalprice`) | 500 Internal Server Error | Defect — should be 400 |
| Checkout date before checkin date | 200, silently accepted | Defect — no business logic validation |
| Empty string field (`firstname: ""`) | 200, silently accepted | Defect — no blank-field validation |
| Wrong data type (`totalprice` as string) | 200, silently coerced to number | Design observation |
| Missing/invalid auth token on Update | 403 Forbidden (not 401) | Minor spec deviation, not a defect |
| Non-existent resource lookup | 404 Not Found | ✅ Correct behavior |
