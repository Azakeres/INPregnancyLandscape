```python
import pandas as pd
from pyproj import CRS
import geopandas as gpd
import numpy as np
from shapely.geometry import Point
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
from shapely import wkt

```

List of Indiana's Babyfriendly hospitals downloaded from ISDH


```python
df = pd.read_csv('IN_BabeFriendlyHosp.csv')
```


```python
geo_df = gpd.GeoDataFrame(df, crs=CRS('EPSG:4326'), geometry= gpd.points_from_xy(df.long, df.lat))
geo_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
      <th>phone</th>
      <th>lat</th>
      <th>long</th>
      <th>occupation</th>
      <th>babyfriendly</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adams Memorial Hospital</td>
      <td>1100 Mercer Ave</td>
      <td>Decatur</td>
      <td>IN</td>
      <td>46733</td>
      <td>2607242145</td>
      <td>40.81773</td>
      <td>-84.91268</td>
      <td>Hospital</td>
      <td>1</td>
      <td>POINT (-84.91268 40.81773)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Baptist Health Floyd</td>
      <td>1850 State St</td>
      <td>New Albany</td>
      <td>IN</td>
      <td>47150</td>
      <td>8129495500</td>
      <td>38.30051</td>
      <td>-85.83441</td>
      <td>Hospital</td>
      <td>1</td>
      <td>POINT (-85.83441 38.30051)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bluffton Regional Medical Center</td>
      <td>303 S Main St</td>
      <td>Bluffton</td>
      <td>IN</td>
      <td>46714</td>
      <td>2608243210</td>
      <td>40.73735</td>
      <td>-85.17136</td>
      <td>Hospital</td>
      <td>1</td>
      <td>POINT (-85.17136 40.73735)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cameron Memorial Community Hospital</td>
      <td>416 E Maumee St</td>
      <td>Angola</td>
      <td>IN</td>
      <td>46703</td>
      <td>2606652141</td>
      <td>41.63478</td>
      <td>-84.99555</td>
      <td>Hospital</td>
      <td>1</td>
      <td>POINT (-84.99555 41.63478)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Clark Memorial Hospital</td>
      <td>1220 Missouri Ave</td>
      <td>Jeffersonville</td>
      <td>IN</td>
      <td>47130</td>
      <td>8122832142</td>
      <td>38.28289</td>
      <td>-85.74917</td>
      <td>Hospital</td>
      <td>1</td>
      <td>POINT (-85.74917 38.28289)</td>
    </tr>
  </tbody>
</table>
</div>




```python
geo_df.plot()
```




    <AxesSubplot:>




![png](output_4_1.png)


The IN Counties shapefiles downloaded from: https://maps.indiana.edu/layerGallery.html?category=Census


```python

in_cou = gpd.read_file('Census_Counties/Census_County_TIGER00_IN.shp')
in_cou.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AREA</th>
      <th>PERIMETER</th>
      <th>NAME_U</th>
      <th>NAME_L</th>
      <th>NCAPC</th>
      <th>CNTY_FIPS</th>
      <th>STFID</th>
      <th>POP2000</th>
      <th>WHITE</th>
      <th>BLACK</th>
      <th>...</th>
      <th>MARHH_NO_C</th>
      <th>MHH_CHILD</th>
      <th>FHH_CHILD</th>
      <th>FAMILIES</th>
      <th>AVE_FAM_SZ</th>
      <th>HSE_UNITS</th>
      <th>VACANT</th>
      <th>OWNER_OCC</th>
      <th>RENTER_OCC</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.352031e+08</td>
      <td>116410.31836</td>
      <td>STEUBEN</td>
      <td>Steuben</td>
      <td>76</td>
      <td>151</td>
      <td>18151</td>
      <td>33214</td>
      <td>32281</td>
      <td>123</td>
      <td>...</td>
      <td>4208</td>
      <td>375</td>
      <td>715</td>
      <td>8911</td>
      <td>3.00</td>
      <td>17337</td>
      <td>4599</td>
      <td>9951</td>
      <td>2787</td>
      <td>POLYGON ((679527.291 4625396.026, 681172.040 4...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.001560e+09</td>
      <td>129036.36824</td>
      <td>LAGRANGE</td>
      <td>Lagrange</td>
      <td>44</td>
      <td>087</td>
      <td>18087</td>
      <td>34909</td>
      <td>33770</td>
      <td>66</td>
      <td>...</td>
      <td>3777</td>
      <td>249</td>
      <td>434</td>
      <td>8856</td>
      <td>3.54</td>
      <td>12938</td>
      <td>1713</td>
      <td>9151</td>
      <td>2074</td>
      <td>POLYGON ((648336.954 4624638.241, 649912.369 4...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.211453e+09</td>
      <td>139308.15005</td>
      <td>ELKHART</td>
      <td>Elkhart</td>
      <td>20</td>
      <td>039</td>
      <td>18039</td>
      <td>182791</td>
      <td>157931</td>
      <td>9551</td>
      <td>...</td>
      <td>19981</td>
      <td>1839</td>
      <td>4636</td>
      <td>47659</td>
      <td>3.18</td>
      <td>69791</td>
      <td>3637</td>
      <td>47769</td>
      <td>18385</td>
      <td>POLYGON ((609782.908 4623876.537, 611399.543 4...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.194457e+09</td>
      <td>155508.91992</td>
      <td>ST JOSEPH</td>
      <td>St Joseph</td>
      <td>71</td>
      <td>141</td>
      <td>18141</td>
      <td>265559</td>
      <td>218706</td>
      <td>30422</td>
      <td>...</td>
      <td>28122</td>
      <td>2148</td>
      <td>7865</td>
      <td>66802</td>
      <td>3.07</td>
      <td>107013</td>
      <td>6270</td>
      <td>72194</td>
      <td>28549</td>
      <td>POLYGON ((576360.816 4623609.402, 577920.195 4...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.620569e+09</td>
      <td>177807.09411</td>
      <td>LAKE</td>
      <td>Lake</td>
      <td>45</td>
      <td>089</td>
      <td>18089</td>
      <td>484564</td>
      <td>323290</td>
      <td>122723</td>
      <td>...</td>
      <td>49444</td>
      <td>3671</td>
      <td>16887</td>
      <td>127036</td>
      <td>3.19</td>
      <td>194992</td>
      <td>13359</td>
      <td>125249</td>
      <td>56384</td>
      <td>POLYGON ((458433.478 4623270.678, 481438.856 4...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 44 columns</p>
</div>



There are 92 rows, for 92 counties in Indiana. 


```python
in_cou.shape
```




    (92, 44)




```python
in_cou.plot()
plt.show()
```


![png](output_9_0.png)


Make sure both datasets have the same coordinate reference system. 


```python
in_cou.geometry = in_cou.geometry.to_crs(epsg = 4326)
geo_df.geometry = geo_df.geometry.to_crs(epsg = 4326)
```


```python
fig, ax = plt.subplots(figsize=(15,15))

minx, miny, maxx, maxy = in_cou.total_bounds
ax.set_xlim(minx, maxx)
ax.set_ylim(miny, maxy)
ax.set_aspect('auto')
plt.xticks([])
plt.yticks([])

plt.title("Baby Friendly Hospitals")

in_cou.plot(ax=ax, alpha=0.8)
geo_df.plot(ax=ax, marker='P',edgecolor='k', markersize=100, c='green',alpha= 1, label= "Baby Friendly Hospital")



plt.legend(prop={"size": 19},bbox_to_anchor=(0.56, -.09), loc='lower center', ncol=3)

    


plt.show()
```


![png](output_12_0.png)



```python

```


```python

```
