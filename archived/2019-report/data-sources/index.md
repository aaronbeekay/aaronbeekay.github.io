---
layout: project
project-slug: 2019-report
title: Data sources
---

On the sources for my report data:

### Time tracking data
Recorded manually - details on the [methods page](/projects/{{project-slug}}/methods).

### Finance/spending data
I track spending and do budgeting in [GnuCash](https://gnucash.org). GnuCash has a reasonable data format (XML) and reasonable export options (CSV, SQL), but I used the built-in reporting tools to summarize my [food spending](/projects/{{page.project-slug}}/plots/food-spending-by-month.html) instead of reinventing the wheel.

Most of the raw data comes from my banks ([Simple](www.simple.com), [Discover](www.discover.com)). I also have an Apple Card, which [doesn't support any export formats whatsoever](https://support.apple.com/en-us/HT209489). I retyped every Apple Card transaction from 2019 from their PDF statements into GnuCash. After I did that, I found [this Apple Card statement converter](https://csv.wtf/apple-card/) which does the conversion automatically. Cool cool cool

### To-do item performance
I track my to-do items in [Things](https://culturedcode.com/things/). 

I have been using Things for almost 10 years now, and I like a lot of things about it, but Cultured Code has a haughty and dismissive attitude towards the concept of data integration. There are no supported methods for programmatically reading or writing task data to/from Things (except for a [custom URL scheme](https://culturedcode.com/things/blog/2018/02/hey-things/), which I don't count as a real method).

I've been working on building a library to read the Things cloud sync data for some time -- which would give me the ability to recreate the state of my to-do list at any point in time -- but it's proven to be a challenging project. 

In the meantime, I did these analyses by reading the [Things.app SQLite database](https://support.culturedcode.com/customer/en/portal/articles/2982272-export-your-data-from-things-3#get-the-things-3-database-file) on the Mac app, which is pretty straightforward to interpret.

### Messaging data (iMessage, SMS)
The macOS Messages.app stores message data in another SQLite database, [chat.db](https://stmorse.github.io/journal/iMessage.html). There are several solid resources for analyzing the contents of this database out there on the interbutt.

I've had a Mac with Messages.app since iMessage came out, and I don't delete messages, so I have a pretty comprehensive database (653MB, _not_ including images!). It's a pretty neat dataset.

### Spotify activity
Spotify has a [GPDR data download](https://support.spotify.com/us/account_payment_help/privacy/understanding-my-data/) page. They say it can take 30 days (!) to get the data, but it was closer to a week for me.

### Miles driven
I've been logging all of the fuel-ups for all of my vehicles using [Fuelly](fuelly.com) for the last... nine years. Yikes.

Fuelly has reasonable CSV export options so this was pretty easy.

### Health data
I consolidate all of my health data in the iOS Health app, which has a built-in XML export function.

Most of my weight data comes from a [eufy smart scale](https://www.amazon.com/eufy-Bluetooth-Wireless-Measurements-Composition/dp/B01MFAABKO), which appears to have no model number. It works fine.

Blood pressure data comes from a [Greater Goods Bluetooth Blood Pressure Monitor](https://www.amazon.com/gp/product/B06XSYBLT8/), again apparently too advanced to have a model number. It is also "fine".