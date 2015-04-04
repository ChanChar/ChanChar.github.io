---
layout: post
comments: true
title: "Scrape Unsplash 2.0"
date: 2015-04-03
backgrounds:
    - https://download.unsplash.com/uploads/14129863345840df499fc/0165574c
thumb: https://unsplash.com/photos/irUQaBK3ydI/download
categories: web scraping
tags: python
---

## What is unsplash?

[Unsplash](https://unsplash.com/) is a website that provides ten high resolution photos on a weekly basis. It
was built by the developers over at [Crew](https://pickcrew.com/?utm_source=Unsplash&utm_medium=website&utm_campaign=unsplash),
a dev shop who help start, develop and design projects for the web. Unsplash is my source to go for beautiful backgrounds
for this website. The pictures in the thumbnails and the backgrounds are mostly from  the hundreds of pictures hosted
on their site. For web apps, I can just link to the download url and have it render but to have pictures for the
desktop/laptop, they need to manually be downloaded one by one. You can see where this going.

I've made a web scraper for this exact purpose when I began to learn python but having found a better way to
implement it, I thought it would try to improve on it. There are two main improvements to this version compared to
the original.

1. It uses the [requests](http://docs.python-requests.org/en/latest/) package instead of [urllib2](https://docs.python.org/2/library/urllib2.html).
This not only cleaned up the code but made it significantly easier for a new programmer to read the code. Although the
variable names could use work, the overall code just flows.

2. It uses LXML instead of JSON. This wasn't a particulary significant change but I did have to learn how XPath and
parsing through html trees worked.

Here's the program:

    from lxml import html
    import os
    import random
    import requests
    import string

    unsplash = requests.get("https://unsplash.com/")
    html_tree = html.fromstring(unsplash.text)

    # Collects picture source as URL
    weekly_ten_pictures = html_tree.xpath('/html/body/div/div/div/div/div/a/img/@src')

    # Collects the names of the photographers
    picture_credits = html_tree.xpath('/html/body/div/div/div/div/div/h2/a[2]/text()')

    credits_and_pictures = zip(picture_credits, weekly_ten_pictures)

    desktop_unsplash_dir = os.path.expanduser('~/Desktop/unsplash')

    # Creates an "unsplash" directory in Desktop if the directory doesn't already exist
    if not os.path.exists(desktop_unsplash_dir):
        os.makedirs(desktop_unsplash_dir)

    for credit, picture in credits_and_pictures:

        picture_data = requests.get(picture)

        if picture_data.status_code == 200:

            picture_html_tree = html.fromstring(picture_data.text)
            picture_name = ''.join(random.choice(string.hexdigits) for i in range(16))

            file_path = os.path.expanduser('~/Desktop/unsplash/{}-{}.jpeg'.format(credit, picture_name))

            print("Downloading {}-{}.jpeg".format(credit, picture_name))
            with open(file_path, 'wb') as f:
                f.write(picture_data.content)
            print("Finished.\n")

Looks simple right? Requests and lxml's xpath have made it incredibly easy to pull the information out of a website
and onto a downloadable format. Currently, it creates a directory on the desktop using os' ```path.expanderuser```
and fills it with the ten most current pictures on unsplash.com. Future additions are to have it run on a schedule
possibly using python's [crontab](https://pypi.python.org/pypi/python-crontab) and to check for existing pictures.
I've tried using UUID's hex to create the file names but I figured it might just be more clear to write out the function
for each ```picture_name```.

I hope to post the original version for a side by side comparison but the current program seems to be more than sufficient
in getting the task done.

Check out [unsplash's](https://unsplash.com/) picture library!
