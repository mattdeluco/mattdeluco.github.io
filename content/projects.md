---
title: "Projects"
date: 2021-10-08T08:38:00-04:00
hideMetadata: true
hideComments: true
hideSuggestions: true
hideTweetButton: true
draft: true
---

🚧 Personal Financial Planning SaaS Project 🚧

{{< tweet 1381814633951072257 >}}

Follow me and my long-lived [Twitter Thread](https://twitter.com/MattDeLuco/status/1381814633951072257)
where I post updates to the project as they happen!

## Database
- PostgreSQL
- Heavy on: normalization, triggers, sql and pl/pgsql functions

The primary focus of this project is the database - after all, I believe data *is* the application.

Having forgotten my high school accounting education, my original personal finances spreadsheet was
an approximation of double entry accounting. Each line in the transactions table consisted of a bank
account, a category, and optionally a sub-category - each of these could be considered a "T account".
Where I diverged from double-entry accounting is that each transaction was represented only by a single
entry - one line recording both debits and credits:

`Tue, Apr 29, 2014 | VISA | Starbucks | ($2.78) | Dining | Coffee`

When it came time to formalize my implementation (build this app) I rediscovered double entry accounting
as a means of categorizing transactions. And so I set out to model double entry accounting in PostgreSQL,
with an added feature: an API to the database, written natively in the database using PL/PGSQL, to
abstract the underlying model. This API would accept JSON as input and return JSON as output and requires
little intervention from the backend (session management and adding auth info to the SQL API payload.)

### SQL API

## Backend
- Nodejs
- TODO

## Frontend
- Vuejs
- TODO

## Competition
The following are a list of similar applications to serve as reference and inspiration.
- [Mint.com](https://mint.intuit.com)
- [Billi](https://billi.app) 🇨🇦
- [Lunch Money](https://lunchmoney.app)
