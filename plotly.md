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
import harp
import sentinelsat
import avl
from avl import vis
```

```python
filename = 'S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc'

api = sentinelsat.SentinelAPI('s5pguest', 's5pguest', 'https://s5phub.copernicus.eu/dhus')
result = api.download_all(api.query(filename=filename))
result = avl.download("cams_ndacc_lidar_o3_hauteprovence_0001_fc1d_observation_20220316T043007.nc")
result = avl.download("s5p_o3_profile_offl_ndacc_lidar_o3_ohp_smoothed_reference_20220317T041229.nc")
result = avl.download("s5p_no2_total_column_offl_pandora_alice_springs_satellite_20220317T034540.nc")
```

2d lines

```python
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)

avl.Scatter(product, 'tropospheric_NO2_column_number_density')
```

2d lines (average over time)

```python
product = harp.import_product('cams_ndacc_lidar_o3_hauteprovence_0001_fc1d_observation_20220316T043007.nc')
plotdata = avl.scatter_data(product, 'O3_volume_mixing_ratio', average=True)

vis.Scatter(**plotdata)
```

histogram

```python
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)

plot = vis.Plot()
plot.add(avl.Histogram(product, 'tropospheric_NO2_column_number_density', bins=20))
plot.add(avl.Histogram(product, 'tropospheric_NO2_column_number_density', bins=50))
plot
```

histogram shortcut

```python
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)
avl.Histogram(product, 'tropospheric_NO2_column_number_density')
```

```python
filter = 'tropospheric_NO2_column_number_density_validity>75;keep(datetime*,latitude*,longitude*,tropospheric_NO2_column_number_density)'
product = harp.import_product('S5P_OFFL_L2__NO2____20200123T004109_20200123T022240_11799_01_010302_20200126T123552.nc', filter)
plot = avl.Histogram(product, 'tropospheric_NO2_column_number_density')
plot.add(vis.Scatter([-7.7e-6, 1.7e-5], [3000,3000]))
plot
```

image

```python
product = harp.import_product('s5p_o3_profile_offl_ndacc_lidar_o3_ohp_smoothed_reference_20220317T041229.nc')

avl.Heatmap(product, 'O3_number_density')
```

uncertainty

```python
product = harp.import_product('s5p_no2_total_column_offl_pandora_alice_springs_satellite_20220317T034540.nc')

avl.Scatter(product, 'NO2_column_number_density')
```
```python

```
