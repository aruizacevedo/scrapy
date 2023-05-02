# Scrapy

Set of web scrapping projects, taken from [freeCodeCamp](https://www.youtube.com/watch?v=mBoX_JCKZTE)

- [The Python Scrapy Playbook](https://scrapeops.io/python-scrapy-playbook/)
- [Python Scrapy Beginners Course](https://thepythonscrapyplaybook.com/freecodecamp-beginner-course/)

## Setup GitHub and Conda Environment

* Create GitHub repo: 'scrapy'

* Create Conda/Venv, matching reponame: 
```
$ conda create -n scrapy python=3 -y
$ conda activate scrapy
```

* Use Makefile to install 'scrapy':

In 'requirements.txt:
```
scrapy
```

In 'Makefile':
```
install:
    pip install --upgrade pip &&\
        pip install -r requirements.txt
```

Install with: $ make install


## Create Basic Scraping project

- Create a Scrapy project

```
$ scrapy startproject <name-of-project>
```

This creates the general template for a scrapin project.


- Create a Spider

Navigate to the 'spiders' directory, and create a spider:

```
$ scrapy genspider <noame-of-spider> <website>
```

- Scrapy shell

Install ipython:

```
pip install ipython
```

Include the shell in the config file: 'scrapy.cfg':
```
shell = ipython
```

Activate by using: $ scrapy shell

### Scrapy shell

Use it to identify the elements to be scraped.

- `fetch('https://books.toscrape.com')`
- `response`
- `books = response.css('article.product_pod')`  // TAG: article, CLASS: product_pod
- `len(books)`
- `book = books[0]`  // Get first book
- `book.css('h3 a::text').get()`  // Extract the text enclosed in the 'a' tag, child of 'h3'
- `book.css('h3 a').attrib['href']`  // Extract the 'href' attribute from the 'a' tag
---
- `response.css('li.next a ::attr(href)').get()`  // Extract the 'href' from the a tag, child of the 'li' tag with class 'next'

### Update the scrapy spider

In 'spiders/bookspider.py', update the `parse()` method:

```
    def parse(self, response):
        books = response.css('article.product_pod')
        for book in books:
            yield {
                'name': book.css('h3 a::text').get(),
                'price': book.css('.product_price .price_color::text').get(),
                'url': book.css('h3 a'.attrib['href'])
            }
```

### Test the spider

```
$ scrapy crawl bookspider
```


