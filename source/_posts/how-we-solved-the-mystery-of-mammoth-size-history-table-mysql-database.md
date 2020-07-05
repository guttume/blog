---
extends: _layouts.post
section: content
title: "How we solved the mystery of a mammoth size history table in MySQL database"
author: "guttume"
date: 2020-07-05
---

While performing regular audit at my company, I noticed that one of the databases is using more disc space than expected. Once I drilled down to the table levels, it turned out that only a single table is using more than 50% of the total disc space used by the database. Yes, you have guessed it right, this table is a history table that keeps track of changes in the transaction table.

On looking carefully, I discovered that our history table has multiple rows for each row of the transaction table, which was unexpected. Hence, I kept digging. I knew that we use database triggers to log data changes and also that we use only two triggers; one for insert and one for the update, hence there should not be so much of history logs.

After spending hours in debugging and testing our application, I discovered below issues which resulted in the large size of our history table.

## Multiple updates on a single table

With time, we have added numerous unmanaged code to our application which has resulted in multiple classes and methods that updates the same table at various points. While discussing, we realized that we don't need to record all these updates as not all columns that are being updated must be tracked.

## Dumping all data on an update

We are dumping the entire row into our history table on every update which is clearly a bad thing as not all of the columns are changing with each update. Some of these columns are static throughout once inserted.

### Solution

The simple solution to the above problems is to use our triggers carefully. Instead of dumping all columns in our history table on each update, we should use conditional statements to log only what's required. We should also make sure not to dump the entire row but log only those columns to the history table that we need.