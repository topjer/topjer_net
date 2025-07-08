+++
date = '2022-11-02T21:00:00+02:00'
draft = false
title = 'I saved money, time (and the environment) with Python'
tags = ['python', 'tutorial']
+++


## Introduction

Admittedly, the title of this article is ambitious. So allow me to explain.

Not too long ago, I have started a subscription for some workout program. My plan was to watch the program on my phone while being in the gym.

It soon turned out that this program was merely a collection of YouTube videos. The ones you can only access when you have the link.
Paying 50+ Euro for that every month, by the way.

The scrooge in me came up with an idea.

"Why not download these videos and cancel the subscription?"

So, I downloaded an App to my phone and started the dirty work. But here comes the catch: Since I did not want to spend money for this App, I went with a free version.

This came with its own set of problems. It forced me to watch a ton of adds. Adds when I downloaded a video. Adds when I wanted to watch a video.

Now, the programmer in me came up with an idea.

"Should be possible to download these videos with some Python code."

And so it began.

## Let's Talk About Morale

I won't hide the fact that I feel a tiny bit of guilt for downloading the videos.

My justification is that I think that the product provided does not work well as a subscription model.

It just feels wrong to pay 50 bucks every month to access an existing library of youtube videos with a handful of new additions every month. Youtube videos which you could probably still access without the subscription if you store the IDs somewhere.

In a desperate attempt to divert the blame, I would even say that it is their own fault for making a product that you can simply download.
It's like leaving your valuables lying around in your front yard.

This diversion works only as long as no one asks why I did not just search a different product if this one is not worth the money for me. To that I can only answer:

Because I like to eat my cake and keep it!

With all that being said and nothing gained, dearest reader, just forget about all of this and get distracted by the shiny things in the rest of this article.

## Back To The Title

Let's address the points in the title individually.

It should be obvious how I have saved money. I no longer had to pay for the subscription.

This way I would miss out on the new content, but I could come back after a few months, download the new stuff and quit again.

Because I really know no shame.

Also, I have a limited amount of mobile data. Since I do not have to upgrade that in order to watch videos I save again money. Admittedly a bit far fetched, I know.

How was time saved?

Well, since I was watching these videos on my phone, I would have to endure Ads. Either in YouTube or in the App used for downloading. Not anymore, Sir!

Time lost with buffering issues will also be saved on top of that.

But how in the hell did I save the environment?

My thought process on this is simple: There are videos I like which I henceforth watch multiple times. If I were to
stream that video multiple times, servers would have to provide that to me and the transmission infrastructure would have to transport that to me.

This requires electricity and leaves a carbon foot print. If it is avoided, the environment is saved (a tiny bit).

With a high likelihood my thought process is too simplistic and studies are a bit vague about the [actual impact streaming has on the environment](https://www.iea.org/commentaries/the-carbon-footprint-of-streaming-video-fact-checking-the-headlines).

So ... did I mention the environment in the title in order to get more attention? The answer to that is a clear ... maybe. But since we are already here, let's talk about ...

# The Code

Three major problems had to be solved:

* How can I access the website of the product?
* How can I extract the video IDs from said website?
* How can I download these videos?

To sum it up quickly:

* The first problem was solved with the [requests package](https://requests.readthedocs.io/en/latest/) and providing the active session credentials as cookies and headers.
* The second problem was solved with the [BeautifulSoup4 package](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) to perform some - crude - html parsing.
* The last problem was solved with [pytube](https://pytube.io/en/latest/) which makes downloading youtube videos a breeze.

In hopes that you are not some kind of coding savant that already connected all the dots, you will find the details in the following sections.

## How To Get In

This is not my first rodeo, I have scraped websites before. This usually did the trick for me:

```python
import requests

awesome = requests.get('https://topjer.net')
```

Firing the piece of code usually resulted in a return code of 200 - huge success - and full access to the code of the website.

Yet the website required authentication. Instead of a shiny 200, I got a gloomy 403. So I did what every developer would do ... I went over to stackoverflow.

Io and behold, I found [this](https://stackoverflow.com/questions/23102833/how-to-scrape-a-website-which-requires-login-using-python-and-beautifulsoup)

The solution I ended up using was to navigate to the network tab in the developer tools of my browser after logging in and extracting the request as a cURL. This can be copied to https://curlconverter.com/ and it will create the python code needed to access the site.

Now the code looks something like this, with COOKIES and HEADERS taken straight from the before mentioned website.

```python
response = requests.get(CURRENT_URL + '/index', cookies=COOKIES, headers=HEADERS)
```

These are the solution I like. They work first try and don't require me to understand what the hell is even going on.

Am I concerned because I have entered potentially sensitive information into some random website?

No, you dummy! Because the Privacy Note on the website promises that everything happens in my browser and no information is sent anywhere.

Why would I be distrustful if a site promises me my data is safe?

## The Extraction

On to the next step. What we have right now is the pure html code, so full of pointy '>' and '<' that I am afraid I might hurt myself if I stare at it for too long.

What to do? Like in real life: if you take something dangerous and cook it for some time, it becomes less dangerous ... and maybe delicious in the process.

Terrible analogy, but all for the sake of transitioning to the Python library I have been using: BeautifulSoup.

According to the website:

> Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.

My highly sophisticated workflow looks like this:

* Load the website in my browser.
* Right click the object I am interested in and choose 'Inspect'.
* With the element highlighted in the html code of the website, I try to identify parents with hopefully unique tags to make my way down to the element.
* Rinse and repeat if necessary.

Here is the structure I had to navigate in my case:

The starting page had several drop down menus on it. The third one contained entries that would lead me to the site with the videos I wanted.

 So here is what I had to do:
* Find all drop down menus and get the content of the third one.
* Extract all links from said drop down menu.
* Loop over the list of links and in every instance find the embedded YouTube video and extract the ID.

Lets have a look at some code snippets. Note that this is not the actual code I have used. It merely is intended to show the functionalities I have been working with.

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html_mess, "html.parser")
dropdown_menus = soup.find('div', class_='col-xs-12').findAll('li', class_='dropdown')

movement_menu = dropdown_menus[2]

entries = movement_menu.findAll(lambda tag: tag.name == 'li' and tag.has_key('data-post_id'))

for entry in entries:
    video_source_id_raw = entry.contents[0]['data-video_source_id']
```

Also, since this is not an in depth guide about BeautifulSoup, I will just briefly explain what the purpose of my code is. I claim by no means that it is the most efficient way of doing it.

The first step is to parse the html string, which in my case comes from 'response.content'.

In order to find the tags I am looking for I mainly use the 'find' and 'findAll' functions. Mind blowing, I know.
If only the first tag that matches a description is needed, use 'find'. If all instances are of interest, use 'findAll' (or 'find_all').

The first argument you must provide is the type of the tag, i.e. the first thing that comes after the '<' and then whichever combination of attributes is fitting.

 In my case it was sufficient to look for the class attribute of the tag.

But it is also possible to provide a function that returns True or False. In my case I use lambda functions which check for the tag to be 'li' and whether the tag has an attribute 'data-post_id'.

For me personally, it was easier to work with lambda functions and I also assume that they give a greater flexibility. But I might as well be wrong with that statement...

Note that the result of the find function is again a Soup object, so multiple consecutive searches can be executed.

In case you want to go through the list of direct children of the tag, i.e. all top-level tags within said tag, use the 'contents' attribute.

If you want to access the value of a given attribute, then you can do that as if the soup object were a dictionary, as I do it in the last line.

And with this, I want to conclude the section about how I extracted the video IDs. If you want to know more about html parsing with BeautifulSoup, then check out the tutorial, which is excellent. Or let me know in the comment section and I might go more 'in depth' on it.

## Securing The Goods

After all this heavy lifting is done, we can finally get to the interesting part. Downloading the videos themselves. A task that is pretty easy due to the hard work of other people.

There is this package called 'Pytube' which takes care of everything. Here is all the code you need:

```python
from pytube import YouTube

video = YouTube('https://www.youtube.com/watch?v=' + video_id)
stream = video.streams.filter(file_extension='mp4', resolution='720p', progressive=True).first()
title = video.title.replace(' ', '_') + '.mp4'
file_path = stream.download(filename=title, output_path='./downloads/')
```

First, you 'connect' to the video by supplying its url.

Then you can get the list of all available streams via 'video.stream'.

This list can be filtered by varying attributes like the file extension or resolution.

One comment to the attribute 'progressive=True'. If you check a video, you will notice that most have 'progressive=False'. Which means that for most streams audio and video are separated. In that case you have to download video and audio separately and combine them afterwards.

Because I am lazy, I hope for a stream that has 'progressive=True'.

Then I choose the first result I get, remove those pesky spaces from the title and proceed to downloading the video.

Sadly, by default, you do not get much feedback on the current progress of the download. The code simply halts execution until the download is done.
It seems like you could provide a callback function upon progress of the download and thus get some insight on how much is left to download. But I have not looked into that much.

## Added Bonus: Download Music

Since we are here, I might as well mention that slight modifications of the above code will allow you to only download the audio track of a video. Ideal if you want to download some music.

```python
video = YouTube('https://www.youtube.com/watch?v=' + video_id)
title = video.title.replace(' ', '_') + '.webm'
file_path = video.streams.filter(mime_type="audio/webm").last().download(filename=title)
```

All you need to do is change the filter. In my examples it was most reliable to check for the 'mime_type' and since mp3 was not always available, I used 'webm'.

## Conclusion

Now you should have all the pieces to glue together your own tool to extract youtube video ids from a website and then download them.

I'd love to hear your feedback in the comment section below.
