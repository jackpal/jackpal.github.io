---
date: 2024-07-27 15:34:00
description: A LLM agent framework.
tags: sqlite, FTS5, Flask, Python
title: Searching 20,000 Text Files With SQLite FTS5 and Flask
---

I recently needed to search through a collection of 20,000 text files. After trying basic tools like `find` and `grep`, I decided to give [sqlite3](https://sqlite.org/) a try.

**Data Preparation**

Using a [Jupyter Notebook](https://jupyter.org/), I cleaned the documents and imported them into a sqlite3 database. I used [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/) to extract text from both HTML and plain text documents. The 
entire cleaning process took around 5 hours. Once I had everything figured out, the actual database import step took about 30 seconds.

**Full Text Search with FTS5**

I created an [FTS5](https://www.sqlite.org/fts5.html) virtual table to enable fast full-text search with boolean logic and fuzzy matching. This allowed me to quickly find relevant 
files.

I wrote a second Jupyter Notebook to provide a simple notebook cell based UI for querying and retrieving documents. That worked, but I wanted to make things even more convenient. 

**Web-Based Search UI with Flask**

I built a simple web-based search UI using [Flask](https://flask.palletsprojects.com/en/3.0.x/). To create and debug the Flask server, I used [Visual 
Studio Code](https://code.visualstudio.com/) with their excellent [Flask tutorial](https://code.visualstudio.com/docs/python/tutorial-flask).

**Lessons Learned**

* Jupyter Notebooks are a useful tool for data cleaning and import.
* sqlite and FTS5 works well for simple document search applications.
* Flask is an easy-to-use library for simple web servers.
* Visual Studio Code is great for creating and debugging Flask servers.

**Room for Improvement**

* The UI could be improved by using a design system like [Bootstrap](https://getbootstrap.com/).

Overall, I was able to create a functional search tool that can handle a my collection of text files. It took about 10 hours of work, and I learned a few things along the way.

Two thumbs up, would hack again! 
