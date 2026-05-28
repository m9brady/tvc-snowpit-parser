# TVC SnowPit Datasheet Parser
## Utility for treating snowpit datasheets (.xlsx) as Python objects


### Requirements
```
conda create -n py3-tvc python=3 pandas openpyxl matplotlib shapely
conda activate py3-tvc
```

### Usage
```python
>>> from snowpit_datasheet_parser import SnowPitSheet
>>> pit = SnowPitSheet('./demo/Pit_SiteName_SubSiteName_YYYYMMDD.xlsx')
>>> print(pit)
```
```
TVC 2018 | Timestamp: 2018-10-12T15:34:00 | Data Rows: 7 | Site: site name 
```
The `pit.meta` property is a dictionary containing various metadata fields that may be of interest:
```python
>>> pit.meta
```
```python
{'location': 'TVC 2018',
 'timestamp': '2018-10-12T15:34:00',
 'geometry': 'POINT (-120.443000 68.331000)',
 'surveyors': 'survey workers that spans multiple cells and rows so that it reads in the whole value',
 'site': 'site name',
 'pit_id': 'pit ident',
 'slope': None,
 'total_depth': 33.0,
 'utm_zone': '10 N',
 'comments': 'Comments/Notes/Indicate co-located measurements:',
 'weather': None,
 'precipitation': None,
 'sky': None,
 'wind_1': None,
 'wind_2': None,
 'ground_condition': None,
 'soil_moisture': None,
 'ground_roughness': None,
 'ground_vegetation': {},
 'tree_canopy': None}
```
The `pit.data` property is a `pandas` dataframe multi-indexed to the 4 different categories of pit observations: `density`, `stratigraphy`, `temperature`, and `ssa`. Each category can be treated as its own dataframe:
```python
>>> pit.data['density'].columns
```
```python
Index(['HeightAboveGround_Top[cm]', 'HeightAboveGround_Bot[cm]',
       'DensityProfile_A[g]', 'DensityProfile_B[g]'],
      dtype='str')
```
```python
>>> pit.data['density']['DensityProfile_A[g]'].head(5)
```
```python
0    13.2
1    12.7
2    11.0
3     9.0
4     6.0
Name: DensityProfile_A[g], dtype: float32
```
... you can also select same-named columns across multiple categories with slightly more complicated syntax:
```python
>>> pit.data.loc[:, (slice(None), 'HeightAboveGround_Top[cm]')].head(5)
```
```python
                    density               temperature              stratigraphy                       ssa
  HeightAboveGround_Top[cm] HeightAboveGround_Top[cm] HeightAboveGround_Top[cm] HeightAboveGround_Top[cm]
0                      33.0                      33.0                      33.0                       NaN
1                      28.0                      28.0                      23.0                       NaN
2                      23.0                      23.0                      13.0                       NaN
3                      18.0                      18.0                       NaN                       NaN
4                      13.0                      13.0                       NaN                       NaN
```
