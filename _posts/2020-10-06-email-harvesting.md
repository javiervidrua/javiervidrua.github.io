---
layout: post
title:  "Email harvesting"
date:   2020-10-06 00:00:00 +0200
categories: jekyll update
---
Email harvesting is the process of getting a large sum of emails from a specific domain, using what are called "*harvesters*".
This, can be used in a pentest, for example, to perform social engineering attacks, targetting specific employees of the company.

There is a tool called "*theHarvester*" which is available in Kali, that does a really good job. It has a lot of very useful options that you can check out by yourself.

To get only the useful part of the output, you can run this command:

`theHarvester -d <DOMAIN> -b all | grep -v 'Target' | grep -E *.$1* | sed -r "s/\x1B\[([0-9]{1,3}(;[0-9]{1,2})?)?[mGK]//g"`