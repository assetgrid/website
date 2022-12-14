---
layout: ../../../layouts/LayoutDocs.astro
title: "Assetgrid | Reference documentation"
---

# CSV Import

The CSV import tool in Assetgrid has 3 stages:
1. Define how to parse the raw CSV.
2. Chose how data in the raw CSV is mapped/translated into Assetgrid data
3. Run the actual import

## Parse the raw CSV
On this page you specificy how to parse the CSV file. This page presents the following options:

* **Parse header**: Whether to treat the first line of the CSV file as a header. If selected the first line will be used as column names for the CSV data. Otherwise columns will be numbered 1, 2, 3, etc.
* **Auto-detect delimiter**: Checking this tells Assetgrid to try to automatically figure out which symbol was used to separate columns in the CSV file. Unchecking it allows you to manually enter the relevant symbol. You'd think that a "comma seperated values" file would be separated by commas. Well, for some reason that's often not the case.
* **Newline character**: Which codepoint determines a new line. Assetgrid can figure it out automatically in most cases, but you can chose it manually as well.

The "CSV data" window shows a preview of the parsed CSV file. It is this table that Assetgrid will later try to map into transactions.

## Map CSV fields to transaction data
On this page you specify which information in the CSV file corresponds to which information in Assetgrid.

### Duplicate handling
Assetgrid can prevent duplicates by storing a unique identifier for each transaction. If you try to upload a transaction with an identifier which already exists on another transaction, the newly uploaded transcation will be discarded. You can store multiple identifiers for each transaction, for examlpe if the same transaction exists on exports from multiple accounts. Assetgrid provides several ways of calculating this identifier:

* **Auto** automatically calculates a unique identifier. It does this based on source account, destination account, amount and timestamp. The identifier is calculated at the time of import, so if you change the transaction later and reimport the original file, it will be registered as duplicate. If there are multiple transactions in the file that match on all the properties included in the auto identifier, then a number will be appended to the end for each occurence. This will correctly identify transactions as long as you always import all transactions for a given timestamp.
* **Unique ID column** uses a column from the CSV file as the unique identifier. This is useful if your CSV file contains a transaction ID or another unique reference.
* **Ignore duplicates** will not assign a unique identfier to your transaction, and thus not prevent duplicates on future imports.

Duplicate handling will typically not work very well when importing multiple accounts with transactions between them, as the timestamp often varies slightly. So you will create the same transaction once per accuont with a slightly different identifier. In this case you can merge the duplicate transactions after import. Merging deletes one transaction, but adds it's identifier to the other transaction, such that the deleted transaction will not be recreated on future imports.

### Timestamp
First select the column in the CSV file containing the timestamp. Then specify how the timestamp should be parsed. You can find detailed information about how dates are parsed on this site: https://github.com/moment/luxon/blob/master/docs/parsing.md

* "yyyy" 4 digit year.
* "yy" 2 digit year.
* "MM" month number prefixed with zero.
* "M" Month number without leading zero.
* "dd" Day number prefixed with zero.
* "d" Day without leading zero.
* "HH:mm" Hour and minute with leading zeroes in 24 hour time.
* "hh:mm a" 12 hour time follow by AM or PM.

You can put text between single quotes ' ' to make the date parser ignore it.

### Accounts
All transactions in Assetgrid are a positive amount transfered from the source account to the destination account. Entering a negative amount will make Assetgrid swap the source and destination. A transaction in Assetgrid must have a source or destination account, but only one is required. If you want to keep track of transfers to accounts that you don't own, we recommend that you still create them as Accounts within Assetgrid, but uncheck the "include in net worth" option.

You select which column in the CSV represents the account. Assetgrid does not care what information about the account the column contains. It can be the account name, account number or something else entirely. As long as it does not change between exports. When selecting a CSV column for accounts, Assetgrid will ask you to assign all values to accounts. By doing this, Assetgrid adds this value as an identifier to the account and will remember it for future exports. A single account can have multiple identifiers if it is referenced differently in different files.

You can also select an account manually, which will be assigned to all transactions.

### Other fields
Simply select which column in the CSV file, that transaction field should be calculated from.

### Parse options
If the CSV file does not provide the information in a format Assetgrid can parse, you can try editing the parse options. Parse options transform the raw CSV value, before Assetgrid attempts to parse it.

You can use a regex to only keep parts of the raw CSV value. Regexes can also be used with groups. You can then use the pattern field to construct a new value based on the captured regex groups. Say you want to switch a date from the format "02-01-2020" to "2020-01-02". You can capture the different parts with the following Regex: "(\\d\\d)-(\\d\\d)-(\\d\\d\\d\\d)" and then tell Assetgrid to reconstruct the data in the new format by specifying this pattern: "{3}-{2}-{1}".

{0} means the first group and will output everything matched by the regex

{1}, {2}, {3}&hellip; will output the values of capture groups.

Another example is that if you want to add a prefix to a value, you can set the pattern to "prefix-{0}".

### All my transactions are positive - what happened?

Some banks export one column with the account exported, a column for source, a column for destination and finally a column for the amount that is positive or negative depending on whether the exported account is the source or destination. This creates a problem as Assetgrid will invert a transaction with a negative amount. This can be solved by making all transactions positive using the following regex: "[\\d,.]+" which will select everything except the negative sign.

Example:
|Exported account   |Source     |Destination    |Amount |
|---	            |---	    |---	        |---	|
|Account A          |Account B  |Account A      |200    |
|Account A          |Account A  |Account C      |-200   |

If you use the source, destination and amount columns you will run into issues with the second transaction. Assetgrid will interpret it as -200 moves from A to C and convert that into 200 from C to A. This will create the following transactions.

|Source     |Destination    |Amount |
|---	    |---	        |---	|
|Account B  |Account A      |200    |
|Account C  |Account A      |200    |

If you use the exported account column, you cannot also add the source or destination as it will lead to transactions with the same source and destination, which cannot be imported.

The solution is to use the "[\\d,.]+" regex on the amount column, which will make all transactions positive and the transactions will be correctly imported.