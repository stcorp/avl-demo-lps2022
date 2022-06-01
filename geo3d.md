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

```python
import sentinelsat
import harp
import avl
from avl import vis
```

3d grid

```python
result = avl.download("201601-ESACCI-L3C_CLOUD-CLD_PRODUCTS-AVHRR_NOAA-19-fv3.0.nc")
```

```python
product = harp.import_product('201601-ESACCI-L3C_CLOUD-CLD_PRODUCTS-AVHRR_NOAA-19-fv3.0.nc')
avl.Geo3D(product, 'cloud_top_pressure', showcolorbar=False)
```

3d swath

```python
filename = 'S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc'
api = sentinelsat.SentinelAPI('s5pguest', 's5pguest', 'https://s5phub.copernicus.eu/dhus')
result = api.download_all(api.query(filename=filename))
```

```python
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)
avl.Geo3D(product, 'tropospheric_NO2_column_number_density', colorrange=(0, 1.7e-5), centerlon=180, zoom=10)
```

3d points

```python
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', 'keep(longitude,latitude, tropospheric_NO2_column_number_density)')
avl.Geo3D(product, 'tropospheric_NO2_column_number_density', colorrange=(0, 1.7e-5), size=(1024,768), centerlon=180, pointsize=2)
```

multiple layers

```python
filename = "S5P_OFFL_L2__NO2____20200123T022240_20200123T040410_11800_01_010302_20200126T123433.nc"
result = api.download_all(api.query(filename=filename))
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
producta = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)
productb = harp.import_product('S5P_OFFL_L2__NO2____20200123T022240_20200123T040410_11800_01_010302_20200126T123433.nc', filter)

plot = vis.MapPlot3D(centerlon=180, zoom=5, colorrange=(0,1.7e-5))
plot.add(avl.Geo3D(producta, 'tropospheric_NO2_column_number_density'))
plot.add(avl.Geo3D(productb, 'tropospheric_NO2_column_number_density'))
plot
```

```python

```
