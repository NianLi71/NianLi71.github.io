---
layout: post
title:  "Python library - fire"
date:   2022-10-16 02:25:00 +0800
categories: python library fire
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