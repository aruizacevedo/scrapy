# python-template

Modified from Noah Gift's approach in 'Assimilate Python'.

## Project scafold steps

* Create GitHub repo: 'python-template'

* Create Conda/Venv, matching reponame: 
```
$ conda create -n python-template python=3 -y
$ conda activate python-template
```

* Create Makefile for common tasks:

In 'Makefile':
```
install:
    pip install --upgrade pip &&\
        pip install -r requirements.txt
```

Install with: $ make install

* Test Python Script

In 'hello.py':
```
#!usr/bin/env python
print("hello")
```

1. Change permission: $ chmod +x hello.py
2. Run with:          $ ./hello.py

