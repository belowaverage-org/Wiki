---
title: Phone Book
description: 🔎 A fast, tag based, flat file, PHP Phone Book.
published: true
date: 2020-12-08T14:50:33.998Z
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