---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.8
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python tags=[]
from datetime import date, timedelta
import harp
import avl
```


```python
result = avl.download([(date(2002,9,16) + timedelta(x)).strftime("o3col%Y%m%d12.hdf") for x in range(30)])
```

```python
product = harp.import_product('o3col*.hdf', 'keep(O3_column_number_density)')
avl.Volume(product, 'O3_column_number_density', size=(1024,1024))
```

```python

```
