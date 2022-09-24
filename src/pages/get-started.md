---
layout: ../layouts/LayoutWrapped.astro
title: "Get started with Assetgrid!"
---

# Getting started

## 1. Install Assetgrid

### Docker
We recommend that you use our docker image which is preconfigured.
Assetgrid uses a MySQL/MariaDB database, which you will need to run and configure. The following examples assumes that a database exist with the name "assetgrid" and a user with the name "assetgrid" and the password "secret".

Example docker-compose.yml file:

```yaml
version: "3.1"
  services:
    db:
      image: mariadb:latest
      container_name: mariadb
      volumes:
        -  ./mariadb:/var/lib/mysql
      environment:
        MYSQL_ROOT_PASSWORD: secret

    assetgrid:
      image: assetgrid/assetgrid:0.1.0
      container_name: assetgrid
      links:
        - db:db
      environment:
        CONNECTION_STRING: "Server=db;Database=assetgrid;Uid=assetgrid;Pwd=secret"
      ports:
        - 80:8080
```

Example docker run commands:

```bash
docker run -v ./mariadb:/var/lib/mysql --env "MYSQL_ROOT_PASSWORD=secret" --name db mariadb:latest
docker run --link db --env "CONNECTION_STRING='Server=db;Database=assetgrid;Uid=assetgrid;Pwd=secret'" -p 80:8080 --name assetgrid assetgrid/assetgrid:0.1.0
```

### Custom installation
To install Assetgrid without docker is a bit more involved. You will first need to install the [.NET core runtime.](https://dotnet.microsoft.com/en-us/download)

You then need to get Assetgrid binaries. You can either clone the github repository and build it from source or find the "Build" action on Github and download the file/artifact called "Docker" which will contain a precompiled version of the program.

No matter if you build from source or download the precompiled file, you will end up with two folders: frontend/dist.production and backend/bin/Release/net6.0. You must copy the backend folder to the desired location. You must then copy the frontend folder to a subfolder of the backend folder ./wwwroot/dist.

Now you can run the application by switching to the backend directory and running

```bash
dotnet backend.dll --urls=http://0.0.0.0:8080/
```

You can change the --urls parameter to run Assetgrid on a different port.

## 2. Importing transactions

When you first open Assetgrid it won't show anything interesting, because you have not yet imported your transactions. You can either manually create transactions or use the CSV import tool.

### Manually create transactions

Manually creating transactions is easy. Just go to the transaction page in the sidebar and click "Create transaction". A transaction must have either a source or destination account which you can create from the account selection dropdown.

In assetgrid a transaction is always a positive amount of money going from the source account to the destination account. Split transaction can have negative sub-totals but the global total must be positive. You can create transactions with negative totals, but if you do so, Assetgrid will simply swap the source and the destination so the total is still positive. 

### Import from CSV

The Assetgrid CSV importer has 3 steps. First you need to parse the raw CSV. Then you need to tell Assetgrid how to create transactions from it. Finally you have to import.

You tell Assetgrid how to create transactions by telling it which columns in the CSV file correspond to which properties on the transaction. When selecting source and destination account, you not only have to tell Assetgrid which column contains information about the account, you also have to specify which property of the account it contains. This is called the identifier. You also have the option of manually selecting an account (or no account) which will then be set for all transactions in the import.

You can find more information on how to import from CSV in the [reference documentation](/reference#csv-import).

## 3. Viewing transactions

Assetgrid has two distinct ways of presenting your transactions. The transaction page and the account page.

**The account page** displays transactions in a chronological order and also calculates the total for the respective account. Calculating the total requires that all transactions are present and in the correct order, thus you cannot filter transactions on this page. It is however grouped by period. In the top right corner you can chose which period to group transactions by. You can group by month, year or custom periods.

Once you reach the end of the transaction table for a given period the arrow will change from ">" to ">>". This means that you will go to the next period.

**The transaction page** the transaction page does not calculate totals and thus has no requirement about the order and completeness of transactions shown. This means you can filter and sort transactions any way you like. Use the quick search or the detailed search. Using the dropdown you can edit all transactions matching a given search query at once.

## 4. Configure preferences

Go to the preferences page and configure Assetgrid how you want it. It's a bit limited at the moment, but this will change in the future. Currently you can change the number and date formats.

Here are some examples of date and time formats. You can convert them into a date format, simply by removing the last part.

* ISO8601 time: "yyyy-MM-dd'T'HH:mm"
* American date and time: "MM/dd/yyyy hh:mm a"
* European date and time: "dd/MM/yyyy HH:mm"

## 5. Have fun

If you have any feedback or issues, contact us on our [Github page](https://github.com/assetgrid/assetgridapp).