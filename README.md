# TVC SnowPit Datasheet Parser
## Utility for treating snowpit datasheets (.xlsx) as Python objects


### Requirements
```
conda create -n py3-tvc python=3 pandas openpyxl matplotlib shapely -c defaults
conda activate py3-tvc
```

### Usage
```
>>> from snowpit_datasheet_parser import SnowPitSheet
>>> pit = SnowPitSheet('./demo/Pit_DDMMYY_SS_PP_V2.xlsx')
>>> print(pit)
```
```
TVC 2018 | Timestamp: 2018-10-12T15:34:00 | Data Rows: 4 | Site: site name 
```
The `pit.meta` property is a dictionary containing various metadata fields that may be of interest:
```
>>> pit.meta
```
```        
{'location': 'TVC 2018',
 'timestamp': '2018-10-12T15:34:00',
 'geometry': 'POINT (-120.443000 68.331000)',
 'surveyors': 'survey workers that spans multiple cells and rows so that it reads in the whole value',
 'site': 'site name',
 'pit_id': 'pit ident',
 'slope': 12.0,
 'total_depth': 'total depth val',
 'utm_zone': '8N',
 'comments': 'Comments/Notes/Indicate co-located measurements:',
 'weather': 'Weather stuff',
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
```
>>> pit.data.density.columns
```
```
Index(['HeightAboveGround_Top[cm]', 'HeightAboveGround_Bot[cm]',
       'DensityProfile_A[kg/m^3]', 'DensityProfile_B[kg/m^3]'],
      dtype='object')
```
```      
>>> pit.data.density['DensityProfile_A[kg/m^3]'].head(5)
```
```
0    15.0
1     NaN
2     NaN
3     NaN
4     NaN
Name: DensityProfile_A[kg/m^3], dtype: float32
```
... you can also select same-named columns across multiple categories with slightly more complicated syntax:
```
>>> pit.data.loc[:, (slice(None), 'HeightAboveGround_Top[cm]')].head(5)
```
```
                     density                       ssa              stratigraphy
   HeightAboveGround_Top[cm] HeightAboveGround_Top[cm] HeightAboveGround_Top[cm]
0                        5.0                      50.0                      36.0
1                       16.0                       NaN                       NaN
2                        4.0                       NaN                      24.0
3                        NaN                       NaN                       NaN
4                        NaN                       NaN                      18.0
```
