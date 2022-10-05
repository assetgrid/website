---
layout: ../../layouts/LayoutDocs.astro
title: "Assetgrid | Reference documentation"
---

# Accounts

Accounts in Assetgrid are simply places money can be transfered to or from. They do not have to be your accounts. In fact we recommend creating all accounts you frequently transfer to or from in Assetgrid, even if they are not yours, as it allows you to better classify the information.

## Creating accounts
The main way to create accounts is on the "manage accounts" page. You can however also create new accounts whenever you select an account for transactions or during imports.

Accounts have the following properties:

* **Name** The name of the account
* **Description** A short text to help you organize your accounts
* **Identifiers** A list of text values that identify this account. This is used during import. If you for example export a list of transactions and the account is referenced by it's account number, you can add the account number as an identifier and Assetgrid will automatically know which account is referenced. You don't have to type this in manually though. The import tool will automatically create the correct identifiers.
* **Favorite** Favorite accounts are shown in the sidebar for quick access.
* **Include in net worth** Check this for all accounts you own. They will be included in your net worth calculation. This allows you to for example create a friends account and keep track of how much money you transfer to them or create your workplace as an account, but without these accounts interfering with the calculation of your assets

## The account page
When you click an account you will be taken to the page of the corresponding account.

At the top of the page will be information about the account.

1. General information. You can edit the account details by clicking the ✏️ or favorite it by clicking the ⭐
2. A category chart showing profits and expenses for each category for the selected period
3. A chart showing the development in balance as well as income and expenses.

### Periods
Assetgrid does not operate with fixed accounting periods. You can query your data in any way you want. In the top right corner you select the period. There are the following period types:

1. By default Assetgrid will group your data by month. By using the dropdown and clicking the calendar, you can group by months starting on different days. So if you are paid the 15th each month, you may want to organize you data by months starting on the 15th.

2. Similarly you can also group by year and using the calendar, you can set the day of the year that is the cutoff.

3. Finally you can chose any period. This could be a week, or 14 days, or 30 days. Simply select the period in the calendar

## Transaction table
The transactions for the account will be shown in a table grouped by period. When you reach the end of the period, the buttons to switch page will switch period instead.

This is why it may say "showing 0 out of 0 items" even though you know you have transactions on the account, if you have selected a period where nothing happens.

You can use the dropdown to edit multiple transactions. You can even edit all transactions on the account as once, if you should want that.
