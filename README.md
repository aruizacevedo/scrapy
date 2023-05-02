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

- `fetch('https://books.toscrape.com')`
- `response`
- `books = response.css('article.product_pod')`  // TAG: article, CLASS: product_pod
- `len(books)`
- `book = books[0]`  // Get first book
- `book.css('h3 a::text').get()`  // Extract the text enclosed in the 'a' tag, child of 'h3'
- `book.css('h3 a').attrib['href']`  // Extract the 'href' attribute from the 'a' tag


