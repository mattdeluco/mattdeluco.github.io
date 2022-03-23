---
title: "Projects"
date: 2022-03-22T22:24:18-0400
hideMetadata: true
hideComments: true
hideSuggestions: true
hideTweetButton: true
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

The example below is the double-entry version of the example above (in the database, some normalization
takes place.) A credit is applied to increase the liability account (VISA) while a debit is applied to
increase the expense account (Dining/Coffee):
```
Tue, Apr 29, 2014 | Starbucks | $2.78 | CR | VISA   |
Tue, Apr 29, 2014 | Starbucks | $2.78 | DR | Dining | Coffee
```

### SQL API
I don't recall from where I got this idea, but I wanted to try building a database with a "native API" -
which is to say, an API to the database on the database built with sql and the included procedural language.
This is [not a new](https://sive.rs/pg) or [original idea](https://twitter.com/adamhjk/status/1440406931080843271?s=21).

I believe strongly that a database should be designed with data integrity in mind, and one of the means of
maintaining integrity is to define the ways in which it is possible to interact with the database. It
should not be necessary to rebuild this API in each client that wishes to interact with the database.

Further, with the [advent of JSON](https://www.postgresql.org/docs/13/datatype-json.html) [in PostgreSQL](https://www.postgresql.org/docs/13/functions-json.html)
I thought it would be novel to create a typical CRUD API with JSON inputs and outputs, similar to a REST
API. The backend webserver then becomes an HTTP proxy to the database, responsible for session management
but otherwise passing JSON (i.e. *data*) from the frontend directly to the database.

There are five schemas in the database each with at least the four basic CRUD operations. These are written
in a combination of sql and plpgsql functions, many using Common Table Expressions (CTEs) to query the JSON
and do one or more actions as a result (e.g. INSERT or UPDATE.)

#### JSON
The following is a small example of how JSON is used in this database - a function that updates a journal entry:

```
CREATE OR REPLACE FUNCTION journal.update_journal(in_json JSON)
RETURNS JSON
AS $$
  WITH results AS (
    UPDATE journal.journal j SET
      transaction_date=x.transaction_date,
      posted_date=x.posted_date,
      description=x.description
    FROM json_populate_recordset(null::journal.journal, in_json->'data') as x
    WHERE
      j.client_id = (in_json#>>'{context, client_id}')::BIGINT
      AND j.id = x.id
    RETURNING j.id
  ) SELECT json_agg(r) FROM results r;
$$ LANGUAGE sql;
```

Taking advantage of Postgres' [JSON Functions and Operators](https://www.postgresql.org/docs/13/functions-json.html)
it's possible to reference various properties of the JSON input. For example, `in_json#>>'{context, client_id}'` extracts
a value from this structure:

```
{
    "context": {
        "client_id": 1234
    },
    "data": [...]
}
```

While `json_populate_recordset(null::journal.journal, in_json->'data')` expands the data array
property into a set of rows whose type is that of the journal schema's journal table (so matching
the column types to the corresponding JSON properties for each array element.)

### pgtap
One of the absolute best tools for developing a database, [pgtap.org](https://pgtap.org) by
[David E. Wheeler](https://twitter.com/theory) is a tool for unit testing databases. It is
used extensively throughout this project to validate the functionality of the API described
earlier with 16 test suites and over 150 unit tests.

The following is an example of the test output for a suite that verifies the functionality of
the `journal.add_split()` API:
```
    # Subtest: tests.test_journal_split_transaction()                           
    ok 1 - Create split transaction - groceries/household from chequing         
    ok 2 - Create split transaction - groceries/household from chequing/visa    
    1..2                                                                        
ok 16 - tests.test_journal_split_transaction
```

### sqitch
From the same author as pgtap is [sqitch.org](http://sqitch.org):
> Sensible database-native change management for framework-free development and dependable deployment.

Sqitch provides tools to help manage database migrations, it is a relatively new tool for
this project, but the project has been fully "ported" to the tool.

## Backend
- Nodejs
- TODO

## Frontend
- Vuejs
- TODO

## Competition
The following are a list of similar applications to serve as reference and inspiration.
- [Billi](https://billi.app) 🇨🇦
- [Lunch Money](https://lunchmoney.app)
- [Mint.com](https://mint.intuit.com)
- [Rehive.com](https://rehive.com)
- [Wealthica](https://wealthica.com) 🇨🇦
