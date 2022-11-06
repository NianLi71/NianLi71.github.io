---
layout: post
title:  "Python library - fire"
date:   2022-10-16 02:25:00 +0800
categories: python library
---

# Python Fire
* https://github.com/google/python-fire
* https://google.github.io/python-fire/


Python Fire is a library for automatically generating command line interfaces (CLIs) from absolutely any Python object.


install with pip
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple fire 
# or 
python -m pip install -i https://pypi.tuna.tsinghua.edu.cn/simple fire 
```

For more examples, check doc above.
```python
import fire

def hello(name="World"):
  return "Hello %s!" % name

if __name__ == '__main__':
  fire.Fire(hello)
```
```bash
python hello.py  # Hello World!
python hello.py --name=David  # Hello David!
python hello.py --help  # Shows usage information.
```

### Example
```python
import fire

def hello(name):
    return 'hello {}'.format(name)

def pathname(base, *path):
    '''
    usage:
    python fire_test.py /home ../*
    '''
    print(base)
    print(path)

def kwargs_test(base, **path):
    '''
    usage:
    python fire_test.py hello --dog1 qq --dog2 gg
    '''
    print(base)
    print(path)

if __name__ == '__main__':
    fire.Fire(pathname)
    # fire.Fire(kwargs_test)
```

* Collect position parameters:
  ```sh
  $ python fire_test.py /home a b c
  /home
  ('a', 'b', 'c')

  $ python fire_test.py /home ../*
  /home
  ('../README.md', '../atexit_test.py', '../cli_argv', '../cmd_module_test', ...)
  ```

* Collect key word parameters:
  ```sh
  $ python fire_test.py hello --dog1 qq --dog2 gg
  hello
  {'dog1': 'qq', 'dog2': 'gg'}
  ```