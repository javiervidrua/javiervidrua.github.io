---
layout: post
title:  "Google hacking cheat sheet"
date:   2020-10-03 00:00:00 +0200
categories: jekyll update
---

## Google search basics
*Google* indexed entire web pages instead of just titles and descriptions. That's the reason you can do comprehensive searches based upon key (query) words.

*Google*'s Boolean default is the *AND operator*. That means if you enter words without modifiers, *Google* will search for all of them.

Examples:
* `nissan gtr r34`: *Google* will search for all the words.

* `nissan OR gtr OR r34`: This specifies that either word is acceptable.

* `nissan (gtr OR r34)`: This specifies that you can have:
    * `nissan gtr`
    * `nissan r34`
    * but never `nissan gtr r34` nor `gtr r34`
* `nissan (gtr | r34)`: The same as the last one, using `|` instead of `OR`.

* `nissan gtr -r34`: This will search for everything that has `nissan gtr` but does not have `r34`.

## Google syntax words
*Google* has a special syntax that allows the user to do more specific searches. Some of the possible modifiers that you can use are:
* `Site`: Allows to search either a site or a top-level domain (*TLD*). Example: `nissan gtr r34 site:'forocoches'`, `nissan gtr r34 site:'es'`

* `Intitle`: This searches for the keywords in the title of the web pages. Example: `nissan gtr r34 intitle:'cars'`

* `Inurl`: This restricts your search to the *URLs* of the web pages. Example: `nissan gtr r34 inurl: cars`

* `Intext`: This searches only on the body of the text (ignores title, url and links). Example: `nissan gtr r34 intext:'good condition'`

* `Inanchor`: Searches for text in a page's link anchors. Example: `nissan gtr r34 inanchor:'forocoches'`

* `Link`: Returns a list of pages linking to the specified *URL*: Example: `nissan gtr r34 link:'forocoches.es'`

* `Cache`: Finds a copy of the page that *Google* indexed even if that page is no longer available. Example: `nissan gtr r34 cache:'forocoches.es'`

* `Filetype`: Searches the extensions. Example: `nissan gtr r34 filetype:'png'`

* `Related`: Finds pages that are related to the specified page. Good way to find categories of pages. Example: `nissan gtr r34 related:'forocoches.es'`

* `Info`: Page of links to more information about the specified *URL*. Example: `nissan gtr r34 info:'forocoches.es'`
`phonebook`: Looks up phone numbers. Example: `nissan gtr r34 phonebook:'Jon Doe CA'`
