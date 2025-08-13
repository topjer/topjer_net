+++
date = '2021-04-21T21:00:00+02:00'
draft = false
title = 'Schwarzenegger Quotes and BeautifulSoup'
tags = ['python', 'twitch', 'tutorial']
+++

This post will be a very brief introduction into html parsing with BeautifulSoup.

I am currently in the process of implementing a chat bot for Twitch in Python that listens to various commands. After some thinking I decided to make him respond with Schwarzenegger quotes because ... why not?

The implementation itself would be no problem as I have done similar things before with the bot. The big question was where to get the quotes from. 
Previously, I was lucky to get all my quotes from APIs which make it laughably easy to use them with the bot. 

A search for Schwarzenegger quote specific APIs bore no fruit. Well, to be precise, I found a repository on github were someone was implementing an API with Schwarzenegger quotes but it seemed not to be online.

Lucky for me, it contained a list of quotes and the movies they were from. It just had one little flaw ... it looked like this:

```html
"<div class=\"quote\">\"It's showtime!\"</div><div class=\"movie-title\">-- The Running Man (1987)</div>",
"<div class=\"quote\">\"Alright everyone, chill.\"</div><div class=\"movie-title\">-- Batman & Robin (1997)</div>",
"<div class=\"quote\">\"Allow me to break the ice.\"</div><div class=\"movie-title\">-- Batman & Robin (1997)</div>",
"<div class=\"quote\">\"I need your clothes, your boots, and your motorcycle.\"</div><div class=\"movie-title\">-- Terminator 2: Judgment Day (1991)</div>"
```

Being as unfamiliar with HTML as I was (and still am), I was a little overwhelmed. Maybe this problem could been solved with some search and replace, but I decided for a different, more informative approach.

Because in my desperation I remembered a similar problem at work which a colleague of mine solved in Python using the package **BeautifulSoup**.

Installation was as easy as you are used of Python packages.

```bash
pip install BeautifulSoup4
```

The next steps were just as easy

```python
from bs4 import BeautifulSoup

example = "<div class=\"quote\">\"It's showtime!\"</div><div class=\"movie-title\">-- The Running Man (1987)</div>"
soup = BeautifulSoup(example, "html.parser")
```

You might not notice it, but we are almost done, because the following command produces an output that is very close to what we are looking for:

```python
>>>soup.get_text()
'"It\'s showtime!"-- The Running Man (1987)'
```
To find the individual tags one can simply use:
```python
>>>soup.find("div", class_="quote").text
'"It\'s showtime!"'
>>>soup.find("div", class_="movie-title").text
'-- The Running Man (1987)'
```
If we wanted a list of all tags, then the following would be a fitting solution:
```python
>>>soup.currentTag()
[<div class="quote">"It's showtime!"</div>, <div class="movie-title">-- The Running Man (1987)</div>]
```

My final solution - including some clean up - looked like this:
```python
quotes = []
for entry in unparsed_quotes:
    soup = BeautifulSoup(entry.replace("<br />", "\n"), "html.parser")
    quotes.append(soup.find("div", class_="quote").text + "\nfrom: " + soup.find("div", class_="movie-title").text.replace("-- ", ""))
```

Even after this, I realized what a powerful tool **BeautifulSoup** is and I have not even scratched the surface.
It wasn't for long until I needed these next level features. But that is for another article. 

For now the only thing that remains to say is this:
```python
>>>soup = BeautifulSoup(example, "html.parser")
>>>soup.text
'"I\'ll be back."-- Terminator 2: Judgment Day (1991)'
```
