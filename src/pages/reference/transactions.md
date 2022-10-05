---
layout: ../../layouts/LayoutDocs.astro
title: "Assetgrid | Reference documentation"
---

# Transactions

Transactions are at the core of Assetgrid. A transaction represents a positive transfer from a source account to a destination account. Transactions with negative amounts will be made positive by swapping the source and destination.

## Browsing transactions

There are several ways to find transactions in Assetgrid.

The transactions page lists all transactions from all accounts. It has a quick search field which performs a simple search. It also supports an advanced query editor, that allows you to more precisely find the correct transactions.

The account page also shows transactions for a specific account. But unlike the transaction page, you cannot reorder them or filter them. This is because the page calculates an account balance, which would be wrong if transactions were missing or the order wasn't chronological.

The account page is grouped by period, which limits which transactions are shown in the table. You can use the period selector in the top menu to switch period. You can also just use the pagination for the table. When you reach the end of a period, the next and previous buttons on the table will switch from ">" to ">>" indicating that pressing it will advance the period.

## Modifying transactions

If you just want to make a quick modification to a transaction you can use the pencil button and open the quick editor.

However, you often want to modify or delete multiple transactions at once. By using the dropdown at the top of the table, you can chose between modifying all selected transactions at once or all transactions matching the search query. The latter option allows you to do things like adding all transactions with the text "pizza place" in the description to the category "takeout".

On the account page, the "modify all" button will modify all transactions for that account.

## Merging transactions

Some times you end up with duplicate transactions from different imports. For example if you imported all transactions on two different accounts and they had a transaction between them. Simply deleting one transaction, has the problem that the next import will recreate it.

Instead you can merge the transactions. This will still delete one transaction, but it will transfer the unique identifier from the deleted transaction to the one remaining, so during the next import, it will not be recreated.