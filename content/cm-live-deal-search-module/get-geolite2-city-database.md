---
title: Get GeoLite2 City database
weight: 20
---
GeoLite2 City is required to detect user's location. Because the database file's size is big, we don't include it in CM Live Deal's package, you need to download it from MaxMind website then upload it to your server manually.

MaxMind no longer offers the file as a plain download. You need a **free MaxMind account** first:

1.  Go to [https://www.maxmind.com/en/geolite2/signup](https://www.maxmind.com/en/geolite2/signup) and create an account. It costs nothing, but MaxMind asks for your name, company, industry and country, and the email address becomes your user name.
2.  Confirm the email MaxMind sends you and set your password.
3.  Sign in, and if the account does not have GeoLite2 yet, sign up for it from the account portal.
4.  In the portal, go to **Download Files** under **GeoIP2 / GeoLite2**, find the **GeoLite2 City** row and download the **GeoIP2 Binary (.mmdb)** gzipped file. The CSV file is not the one you want.

Use your favorite file compression software to uncompress the downloaded file. Inside you get a folder holding `GeoLite2-City.mmdb`. Upload that `.mmdb` file to <Joomla! root folder>/administrator/components/com\_cmlivedeal/helpers/geoip/database/ folder, replacing the file already there.

MaxMind releases a new GeoLite2 City twice a week, and its licence asks everyone using GeoLite2 to keep their copy up to date. An older file keeps working, it only gets less accurate, but download a fresh one regularly. MaxMind's `geoipupdate` tool can do it for you if your host lets you run it.
