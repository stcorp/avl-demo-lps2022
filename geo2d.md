---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import sentinelsat
import harp
import avl
```

```python
filename = 'S5P_OFFL_L2__SO2____20210412T151823_20210412T165953_18121_01_020104_20210414T175908.nc'

api = sentinelsat.SentinelAPI('s5pguest', 's5pguest', 'https://s5phub.copernicus.eu/dhus')
result = api.download_all(api.query(filename=filename))
result = avl.download("o3col2002091612.hdf")
```

```python tags=[]
varname = 'SO2_column_number_density'
operations = ";".join([
    "latitude>-40;latitude<60",
    "SO2_column_number_density_validity>50",
    "keep(datetime_start,scan_subindex,latitude_bounds,longitude_bounds,SO2_column_number_density)",
    "derive(SO2_column_number_density [DU])",
])
l2product = harp.import_product('S5P_OFFL_L2__SO2____20210412T151823_20210412T165953_18121_01_020104_20210414T175908.nc', operations)

avl.Geo(l2product, varname, colorrange=(1, 8), colormap="batlow", opacity=0.9)
```

```python
l3product = harp.import_product('o3col2002091612.hdf')

avl.Geo(l3product, 'O3_column_number_density', colorrange=(100,500), colormap="jet", opacity=0.7)
```

```python

```
