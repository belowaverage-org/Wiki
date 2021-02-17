---
title: Phone Book
description: 🔎 A fast, tag based, flat file, PHP Phone Book.
published: true
date: 2021-02-17T16:53:47.172Z
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



## Development

### Script Examples

https://github.com/belowaverage-org/phonebook/tree/master/scripts

### API Documentation

You can find a copy of this documentation hosted at https://phonebook-demo.belowaverage.org/api/
Or by browsing to your own instance at http://instance-url/phonebook-path/api/

There you will find all the nessesary information to create imports that hook into the Phone Book database.