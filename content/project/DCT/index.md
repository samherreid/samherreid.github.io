---
date: "2000-04-27"
show_date: false
external_link: 
image:
  placement: 3
  caption: 'West Fork Robertson Glacier, Alaska Range.'
  focal_point: Smart
  preview_only: false

summary: Python-based toolkit to include debris cover in glacier melt models
tags:
- Software
- Debris cover
title: Debris Cover Tools
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---

Development of [Debris Cover Tools (DCT)](https://github.com/samherreid/DebrisCoverTools/blob/master/DebrisCoverTools.py) started in May 2018. The intent was an open-source suite of Python based tools that any climate modeler could easily run to add the effects of debris cover to their work in a modular form. At the time, and still today, many global glacier models neglect debris cover entirely.

While it's not a top development priority for me, all of the small scripts I write will eventually be packaged into a cohesive DCT Jupyter notebook for either community use or just to sit and accumulate dust on github. Several of the tools I have developed have been published in peer-reviewed articles and they are linked below.

While it's annoying and not open-source, I use the package arcpy (I pay $100USD/year to have a licence). Being something of an indusrty standard makes it slightly okay, I think, but I would one day like for these tool to run exclusivly with open-source Python packages. 

The preamble to [DCT version 1](https://github.com/samherreid/DebrisCoverTools/blob/master/DebrisCoverTools.py) -- defining source data and variables:

```python
## Debris Cover Tools 
## Version 1.0
##
## By Sam Herreid (samherreid@gmail.com)
## 
## A suite of automated tools to:
##      Map supraglacial debris cover (DebisMap.py)
##      Locate ice cliffs (DebrisAreaSegmentation.py, IceCliffLocation.py)
##      Locate supraglacial lakes/ponds [in prep., not in v1.0]
##      Measure changes in debris cover [in prep., not in v1.0]
##      Calculate debris map and area change omission and commission errors [in prep., not in v1.0]
##      Identify glacier flow instabilities (optional: in near real time) [in prep., not in v1.0]
##      
## For use of Debris Cover Tools v1.0, required input data are:
##      Glacier outline(s) (.shp)
##      Landsat TM, ETM+, or OLI images (.tif, do not change default file names)
##      Digital Elevation Model roughly coincident in time with Landsat imagery (.tif, aprox. 5m resolution)
##
## Required software are:
##      ArcGIS/arcpy (advanced licence, proprietary)
##      Python (2.x to be compatible with arcpy)
##      numpy (Python package)
##      scipy (Python package)
##      matplotlib (Python package)
##
## Please cite the following article(s) if using this code to
## 
## map ice cliffs:
##      Herreid, Sam, and Francesca Pellicciotti. "Automated detection of ice cliffs within supraglacial debris cover." The Cryosphere, 2018.
##
## derive debris area changes [code in prep., not in v1.0]:
##      Herreid, Sam, et al. "Satellite observations show no net change in the percentage of supraglacial debris-covered area in northern Pakistan from 1977 to 2014." Journal of Glaciology 61.227 (2015): 524-536.
##
## detect glacier flow instabilities [code in prep., not in v1.0]:
##      Herreid, Sam, and Martin Truffer. "Automated detection of unstable glacier flow and a spectrum of speedup behavior in the Alaska Range." Journal of Geophysical Research: Earth Surface 121.1 (2016): 64-81.
##
##--------------------------------------------------------------------------------------------------------------------------------
##
## To use Debris Cover Tools, follow the directions in comments and boxes like this:
###-------------------------------------------###
###                Directions                 ###
###-------------------------------------------###            

##--------------------------------------------------------------------------------------------------------------------------------

###-------------------------------------------###
###             What do you want?             ###
###-------------------------------------------###

Want_DebrisMap = 'True'                  # DEFINE: workspace, data_dir, shp_dir, mask_dir (mask_dir currently does nothing in v1.0) 
Want_CloudRemoval = 'False'              # in prep., not in v1.0, 'True' will not currently work 
Want_SupraglacialPondLocation = 'False'  # in prep., not in v1.0, 'True' will not currently work 
Want_IceCliff = 'True'                   # DEFINE: workspace, data_dir, shp_dir and dem (that is relatively coincident in time with Landsat (ETM+ or OLI) image). if Want_DebrisMap = 'False', also define: debarea;    
Want_dDebris = 'False'                   # in prep., not in v1.0, 'True' will not currently work
Want_LocateUnstableFlow = 'False'        # in prep., not in v1.0, 'True' will not currently work

###-------------------------------------------###
###          Define directories here:         ###
###-------------------------------------------###

# Example format for directories: "C:\\Users\\Sam\\Desktop\\CanwellGlacier\\" or r"C:\Users\Sam\Desktop\CanwellGlacier\" or "C:/Users/Sam/Desktop/CanwellGlacier/"
# Replace the example paths below to your local data. Note "\\" at end.

workspace = "C:\\Users\\Sam\\Desktop\\CanwellGlacier\\" # Existing location where new files will be created

data_dir = "C:\\Users\\Sam\\Desktop\\Landsat\\" # Location of uncompressed landsat images. Do not change NASA file names.

shp_dir  = "C:\\Users\\Sam\\Desktop\\Glacier\\" # Location of glacier outlines as shapefiles

mask_dir = "" # Location of cloud and error masks [in prep., not in v1.0]

dem = "C:\\Users\\Sam\\Desktop\\dem\\CanwellGlacier_072016.tif" # Location of DEM. NOTICE: rename the .tif file such that the ending is _mmyyyy.tif where mmyyyy = the month and year of DEM data aquisition

debarea = "" # Location of debris map .shp, only requred if Want_DebrisMap = 'False'

###-------------------------------------------###
###         Define parameters here:           ###
###-------------------------------------------###

##----------------------------------------------
## DebrisMap:
##----------------------------------------------

A_remove = 2700             # Area (m2) to be filtered out of result
A_fill = 2700               # Area (m2) to be filled and considered debris cover
landsat = 8                 # Number of data acquiring Landsat satellite number. Accepted valueds are 5,7 or 8

                            # Threshold values from Herreid et al., 2015. Go into DebisMap.py to change these values 
                            # File name convention: (glacier name)_---y(year)---d(day of year)_---t(integer threshold *100)----r(area removed in m2)----f(area filled in m2)
                            # e.g.: CanwellGlacier_2015y249d_145t2700r2700f

##----------------------------------------------
## DebrisAreaSegmentation:
##----------------------------------------------

L_t = 1500                  # Edge length (meters) of approximate square area for which cliffs will be solved for. This might be necessary due to memory limitations in ArcGIS
n_c = 1                     # Coefficient to L_t (fishnetRes) that will look for tiles to merge area with if none share a boundary

##----------------------------------------------
## IceCliffLocation:
##----------------------------------------------

skinny = 'false'            # If 'true' the ice cliff end extension portion of the method is skipped to speed up computation time
pixel = 'auto'              # If 'auto': working and final resolution is set to DEM resolution. If not 'auto' define an integer spatial resolution (m) outside of quotes
n_iterations = 36           # Number of iterations; fewer for faster computation 
L_e = 10                    # m on ice cliff ends 
alpha = 2                   # Centerline buffer distance (n_pixels*root2 over 2) 
beta_e = 3                  # Degrees by which beta*_i is reduced to define {A_e}_i (see Herreid and Pellicciotti, 2018)
A_min = 10                  # Minimum ice cliff area threshold in n pixels 
phi = 0.5                   # Ice cliff probability, p(x=ice cliff given surface slope and omega), not affirmed by the code will be multiplied (reduced) by this factor (omega defined in Herreid and Pellicciotti, 2018)
gamma = 0.0001              # Tolerance to asymptote, defines line used to find optimized value
                            # File name convention: mmyyyy(demDate)_beta_e-(beta_e)_alpha--(10*alpha)_lineExt--(L_e)m_minPixs--(A_min)_res-(pixel)m
                            # e.g.: 072016_beta_e3_alpha20_lineExt10m_minPixs10_res5m

```