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




