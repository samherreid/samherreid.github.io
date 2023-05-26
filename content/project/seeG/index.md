---
date: "2010-04-27"
show_date: false
external_link: ""
image:
  placement: 3
  caption: 'One frame of a method video output from See-G.'
  focal_point: Smart
  preview_only: true

summary: A tool to visualize spatial data in complex methods
tags:
- Jypyter Notebook
- Software
- GIS
title: See-G(IS code)
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---

<img src="johnsonMethod.gif" width="2000" height="2000" Caption = "hello"/>




GIS-based research is a perfect medium for visual learners, however, as your analysis becomes more sophisticated you might end up using a strictly programming environment. In the process of writing a method only with code, a visual learner might miss seeing what is actually happening. See-G(IS code) is a tool to see your method in action and possibly identify redundancies. After defining directories

```python
baseMap = r"" # .shp (optional)
arcGisProject = r"" # .aprx
frameFolderPath = r""
```

importing the required packages 

```python
import arcpy
import sys
from inspect import currentframe
# See-G local
import config
import mod

useSeeG = 'yes'
```

and calling some arcpy function

```python
arcpy.someFunction(input,output)
```


all you have to do is add the following


```python
makeFrame(output, useSeeG, arcGisProject, baseMapLayer, frameFolderPath, lineNumber(), sys._getframe().f_code.co_name)
```

to export a frame of this step in your method which is pushed to a timelapse video at the end of your process. Currently this tool only works for ArcGIS, which is annoyingly not open source, but I think is still the industry standard (I pay $100 annually for a full pro licence.) The GitHub repo for this tool is [here](https://github.com/samherreid/See-G/blob/main/See_G.ipynb).

<img src="canwellMethod.gif" width="2000" height="2000" />


<script defer src="https://cdn.commento.io/js/commento.js"></script>
<div id="commento"></div>


