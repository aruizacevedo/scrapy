# Scrapy

Set of web scrapping projects, taken from [freeCodeCamp](https://www.youtube.com/watch?v=mBoX_JCKZTE)

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



