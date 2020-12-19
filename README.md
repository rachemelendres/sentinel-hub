# Satellite Image and Data Extraction
 Exploring sentinel hub python library to extract satellite images and NDVI values

## Dependencies

```
# sentinel hub configurations
from sentinelhub import SHConfig


INSTANCE_ID = ''  # In case you put instance ID into configuration file you can leave this unchanged

if INSTANCE_ID:
    config = SHConfig()
    config.instance_id = INSTANCE_ID
else:
    config = None
```
## Set Area of Interest: Mt. Makiling, Los Baños
```
makiling_bbox = BBox((121.189990,14.118840,121.232678,14.160235), CRS.WGS84)
time_interval = ('2020-03-01', '2020-05-01')
```

## Satellite Images
### True Color (PNG) on a specific date
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/true-color.png)

### Multiple timestamps data
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/multiple%20timestamps.png)
## Statistical Measures of NDVI values

### Min, Max, Mean, and StDev
```
ndvi_script = 'return [(B05 - B04) / (B05 + B04)]'

histogram_request = FisRequest(
    data_collection=DataCollection.LANDSAT8,
    layer='TRUE-COLOR-L8',
    geometry_list=[makiling_bbox],
    time=time_interval,
    resolution='100m',
    bins=20,
    histogram_type=HistogramType.EQUIDISTANT,
    custom_url_params={CustomUrlParam.EVALSCRIPT: ndvi_script},
    config=config
)

histogram_data = histogram_request.get_data()
```
A list of all values per date will be returned in a dictionary
```
# min, max, mean, stDev, and histogram values per date
histogram_data
```
Parse the list using pandas and convert to dataframe
```
# convert extracted data to pandas dataframe format
makiling = fis_data_to_dataframe(histogram_data)
makiling_sort = makiling.sort_index()
makiling_sort
```  
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/NDVI%20stats.png)
### Plot Distribution of NDVI Values
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/NDVI-1.png)
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/NDVI-2.png)
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/NDVI-3.png)
![alt text](https://github.com/rlm4692/sentinel-hub/blob/main/NDVI-4.png)
