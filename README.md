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

This creates the general template for a scraping project.


- Create a Spider

Navigate to the 'spiders' directory, and create a spider:

```
$ scrapy genspider <name-of-spider> <website>
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

**CSS Selectors**

- `book.css('h3 a::text').get()`  // Extract the text enclosed in the 'a' tag, child of 'h3'
- `book.css('h3 a').attrib['href']`  // Extract the 'href' attribute from the 'a' tag
---
- `response.css('li.next a ::attr(href)').get()`  // Extract the 'href' from the a tag, child of the 'li' tag with class 'next'

**XPath**

- `fetch('https://books.toscrape.com/catalogue/a-light-in-the-attic_1000/index.html')`
- `response.xpath("//ul[@class='breadcrumb']/li[@class='active']/preceding-sibling::li[1]/a/text()").get()`

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

### Saving outputs

#### 1. Save output to CSV/JSON

```
$ scrapy crawl bookspider -O bookdata.csv
$ scrapy crawl bookspider -O bookdata.json
```

Options:

- `-O`: Outputs to a file (eg. csv, json) (overrides existing output)
- `-o`: Appends to an existing file (appends to existing output)

The location can be set in `settings.py`:

```
FEEDS = {
    'bookdata.json': {'format': 'json', 'overwrite': True}
}
```

If you want custom settings for a specific spider (not global settings), use in your spider definition:

```
custom_settings = {
    'FEEDS': {
        'booksdata.json': {'format': 'json', 'overwrite': True}
    }
}
```

#### 2. Save outputs to database

Using MYSQL: Create database to store scraped data

- Install MySQL (Homebrew): `$ brew install mysql`
- Start service: `$ brew services start mysql`
- Configure it: `$ mysql_secure_installation`
- Login with: `$ mysql -u root -p`

```
[mysql]
>>> CREATE DATABASE books;
>>> SHOW DATABASES;
>>> EXIT
```

Make sure your Python environment has the following packages: `pip install mysql mysql-connector-python`

- Add a Class in *pipelines* to save to SQL: **SaveToMySQLPipeline()**
- Update *settings* to include the pipeline: **ITEM_PIPELINES**

---

### Using **Items** and **Pipelines**


#### On `items.py`:

Define a class to capture each scraped item (e.g. Books)

```
import scrapy

class BookItem(scrapy.Item):
    # define the fields for your item here like:
    name = scrapy.Field()
```

For simple data parsing, you can define a function here to serialize the scraped data. It can be passed to the *serialize* argument of `scrapy.Field()`.

```
def serialize_price(value):
    return f'£ {str(value)}'
```

For more advanced data parsing, use a pipeline.

#### On 'pipelines.py`:

Define a pipeline to process/clean each data item. For example:

```
from itemadapter import ItemAdapter

class BookscraperPipeline:
    def process_item(self, item, spider):

        adapter = ItemAdapter(item)

        ## Strip all whitespaces from strings
        field_names = adapter.field_names()
        for field_name in field_names:
            if field_name != 'description':
                value = adapter.get(field_name)
                adapter[field_name] = value[0].strip()

        return item
```

Note that the pipeline needs to be enabled in `settings.py`.

```
ITEM_PIPELINES = {
   "bookscraper.pipelines.BookscraperPipeline": 300,
}
```



