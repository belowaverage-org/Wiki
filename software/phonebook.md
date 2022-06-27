---
title: Phone Book
description: 🔎 A fast, tag based, flat file, PHP Phone Book.
published: true
date: 2022-06-27T22:15:39.755Z
tags: 
editor: markdown
dateCreated: 2020-12-08T14:25:44.088Z
---

<p align="center">
	<img src="/assets/software/phonebook/logo_512_circle.svg" height="150"/>
	<h2 align="center">Phone Book</h2>
</p>
🔎 The Phone Book is a fast, tag based, flat file, PHP powered Phone Book web application.

The goal of the Phone Book is to make it easy to build and automate importing data into the application.

This wiki will go over installing & administrating the Phone Book.

## Installation

### Download
https://github.com/belowaverage-org/phonebook/releases

### Windows Install

* Follow the directions at: https://php.iis.net/ to get PHP set-up on IIS.
* Extract the ZIP from the download above in `C:\inetpub\wwwroot\phonebook`
* Ensure the following PHP modules are enabled:
  * SQLITE_PDO
  * OPCACHE
* Visit http://localhost/phonebook


### Linux / Docker Install

**Pulling the image:**

```
docker pull belowaverageorg/phonebook
```

**Running the image:**

```
docker run -p 8081:80 -l PhoneBook -d belowaverageorg/phonebook
```

**Paths to mount for persistence:**

* /var/www/html/data
* /var/www/html/api/schema.cfg.json (Optional)

**Try it out:**

Visit: http://[DOCKER_HOST]:8081/

## Administration

Administrating the database can be accomplished by visiting:

http://instance-url/phonebook-path/api/?db

All other administrative tasks can be accomplished by running this PowerShell script:

https://github.com/belowaverage-org/phonebook/blob/master/scripts/Enter-PhoneBookAdmin.ps1

### Enter-PhoneBookAdmin Guide

##### Menu

![pb_admin.png](/assets/software/phonebook/pb_admin.png)

###### Phone Numbers

These options are for managing Phone Book entries.

0. Search the Phone Book.
	* Use this option to search the Phone Book for entries.
1. Edit a Phone Number.
	* Use this option to modify a Phone Book entry.
2. Add a Phone Number.
	* Use this option to create a new Phone Book entry.
3. Remove a Phone Number.
	* Use this option to remove a Phone Book entry.

###### Translations

These options are for managing Phone Book translations.

5. View all translations.
	* Use this option to view all translations in the database.
6. Add / set a translation.
	* Use this option to add or set a translation entry.
7. Remove a translation.
	* Use this option to remove a translation entry.

###### Statistics

These options will show analytic data.

8. View all query logs.
	* Use this option to view all logs from the last hour.
9. View statistics.
	* Use this option to view all statistics from the last hour.

###### Other

10. Rebuild database.
	* Use this option to purge and the re-generate the index / database. This will re-build all tags with new translations.
11. View all tags.
	* View all indexed tags.
  
## What is a Tag?

**Tags** are how the Phone Book creates its index points. When a user searches the Phone Book, they will be forced to use existing tags in the database ensuring that the user is always guided to a result.

https://github.com/belowaverage-org/phonebook/blob/master/api/schema.cfg.json

The above file shows what entries' attributes are tagged when added or modified.

## What is a Translation?

A **translation** is a `from` and `to` key value pair in the database that is applied to every Phone Book entry on creation. This ensures that abreviations are translated into meaningful words when entries are being indexed / tagged.

![pb_trans_examples.png](/assets/software/phonebook/pb_trans_examples.png)

> **Tip:** Translate a tag to multiple tags by using a space to delimite in the `to` field.

## Modifying the schema
Below is the default schema file. It can be modified to add / remove columns and change the behavior of the front-end and back-end.

`./api/schema.cfg.json`

```json

{
    "created": {                   //Column Name
        "export": false,           //Should this column appear in the front-end CSV export?
        "name": "Created",         //The name of the column that appears in the front-end.
        "print": false,            //Should this column appear in the front-end print dialog?
        "tagged": false,           //Should this column be tagged / indexed on import?
        "visible": false,          //Should this column be visible on the front end?
        "type": "timestamp",       //The type of this column. Choices are: timestamp, choice, and number.
        "sortable": true           //Should this column be sortable via the API?
    },
    "modified": {
        "export": false,
        "length": 10,              //The max length allowed as a value in this column.
        "name": "Modified",
        "print": false,
        "tagged": false,
        "visible": false,
        "type": "timestamp",
        "sortable": true
    },
    "description": {
        "export": true,
        "length": 1000,
        "name": "Description",
        "print": true,
        "tagged": true,
        "visible": true,
        "sortable": true
    },
    "callerid": {
        "export": true,
        "length": 1000,
        "name": "Caller ID",
        "print": true,
        "tagged": true,
        "visible": true,
        "sortable": true
    },
    "type": {
        "choices": [               //A list of choices when the column type is set to "choice".
            "Business",
            "Fax",
            "Location",
            "Person",
            "Shared"
        ],
        "export": true,
        "length": 10,
        "name": "Type",
        "print": false,
        "tagged": true,
        "type": "choice",
        "visible": true,
        "sortable": true
    },
    "number": {
        "export": true,
        "indexed": true,
        "length": 50,
        "name": "Phone Number",
        "print": true,
        "tagged": true,
        "type": "number",
        "unique": true,            //Should this column only contain unique entries?
        "visible": true,
        "sortable": true,
        "orderby": "ASC"           //The order in which the results are displayed on the front-end. (only one column can have this attribute set).
    },
    "email": {
        "export": true,
        "length": 100,
        "name": "Email",
        "print": true,
        "tagged": true,
        "visible": true
    },
    "employeeid": {
        "export": true,
        "indexed": true,
        "length": 10,
        "name": "Employee ID",
        "print": false,
        "tagged": true,
        "type": "number",
        "unique": false,
        "visible": true
    },
    "firstname": {
        "export": true,
        "length": 100,
        "name": "First Name",
        "print": false,
        "tagged": true,
        "visible": true,
        "sortable": true
    },
    "lastname": {
        "export": true,
        "length": 100,
        "name": "Last Name",
        "print": false,
        "tagged": true,
        "visible": true,
        "sortable": true
    },
    "location": {
        "export": true,
        "length": 100,
        "name": "Location",
        "print": false,
        "tagged": true,
        "visible": true,
        "sortable": true
    },
    "username": {
        "export": true,
        "length": 50,
        "tagged": true,
        "visible": false
    },
    "importsource": {
        "export": true,
        "print": false,
        "length": 50
    }
}
```

## Development

### Script Examples

https://github.com/belowaverage-org/phonebook/tree/master/scripts

### API Documentation

You can find a copy of this documentation hosted at https://phonebook-demo.belowaverage.org/api/
Or by browsing to your own instance at http://instance-url/phonebook-path/api/

There you will find all the nessesary information to create imports that hook into the Phone Book database.