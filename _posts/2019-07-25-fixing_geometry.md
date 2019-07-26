---
layout: post
published: true
title: Fixing invalid geometry Shapefiles obtained by Rasters
subtitle: by Rafael W. Albuquerque, PhD candidate
<!-- subtitle: Rastered polygons -->
tags:
  - raster
  - vector
  - conversion
  - geometry
  - invalid
  - fix
  - GRASS
  - QGIS
---

GIS routines often demands converting raster data to vector data.  

| [![]({{site.baseurl}}/img/fix_geom/Raster_to_Vector.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/Raster_to_Vector.png) |
|:--:| 
| *Example of a vector file obtained from a raster* |
{: .table-borderless}
<br>


When working with QGIS and after obtaining a shapefile from raster data, not rarely that vector file contains invalid geometries, what makes some GIS operations not possible anymore. 

Ok. I Always solved this issue using GRASS v.clean tool on QGIS processing toolbox. Though yesterday I spent a few hours to fix a shapefile with invalid geometry. In my case, I obtained a vector from raster data. Then I was trying to use this vector as a mask to extract data from another raster.  

| [![]({{site.baseurl}}/img/fix_geom/Extract_by_Mask.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/Extract_by_Mask.png) |
|:--:| 
| *Extract raster by a mask shapefile* |
{: .table-borderless}
<br>


Unfortunately, QGIS tool “Clip raster by mask layer” was not working because the shapefile had invalid geometry. Seemed like GRASS v.clean tool on QGIS was not a solution anymore. Besides, none of the Internet solutions solved the problem and I could not think of anything effective.

What was wrong with this shapefile or with the GIS tools? I Always performed raster extraction based on shapefiles, but what was wrong this time?

Talking to my teammates on SPAMLab I found two general solutions to this. Hopefully other GIS professionals will not spend as much time as I did to solve this kind of issue, what motivated me to write this post.

The two possible solutions are:
- Creating a Mask layer on GRASS 
or
- Using Fix Geometries on QGIS

PS: The two solutions mentioned above and described below are independent from each other, so you may choose the one you think is more appropriate.

1) Creating a Mask layer on GRASS 

First, create a GRASS database. This tutorial can help if you have never done such a thing: https://grass.osgeo.org/grass77/manuals/helptext.html

Once the GRASS database is created and the project is accessed, use r.in.gdal (click on File -> Import raster data -> Import of common raster formats) to import the raster you wanna clip and v.in.ogr (click on File -> Import vector data -> Import of common vector formats) to import the shapefile you will use as a mask.  

| [![]({{site.baseurl}}/img/fix_geom/r_in_gdal.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/r_in_gdal.png) |
|:--:| 
| *On GRASS, click on File -> Import raster data -> Import of common raster formats* |
{: .table-borderless}
<br>


Use r.mask (click on Raster -> Mask) to create a mask file. Please note that you will need to input only your previously imported vector file on the Vector tab of the [r.mask] tool. The Raster tab of the [r.mask] tool must be empty.  

| [![]({{site.baseurl}}/img/fix_geom/r_mask.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/r_mask.png) |
|:--:| 
| *On GRASS, click on Raster -> Mask* |
{: .table-borderless}
<br>


After creating your Mask file, check on the Display window if the previously imported raster is shown only on the areas covered by the shapefile that was also previously imported (like the figure above). After all, this shapefile is now your mask.

Once the Mask is created and if you see your raster covering only the areas of your shapefile, it’s time to export your raster. Use r.out.gdal (click on File -> Export raster map -> Common raster formats).  

| [![]({{site.baseurl}}/img/fix_geom/r_out_gdal.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/r_out_gdal.png) |
|:--:| 
| *On GRASS, click on File -> Export raster map -> Common raster formats* |
{: .table-borderless}
<br>

Now you can open your raster on other software.

2) Using Fix Geometries on QGIS

On QGIS Processing Toolbox (if you can’t see your processing toolbox window, click on Process -> Toolbox. PS: if you can’t see the Process Menu, you might have to enable it with the menu Plugins > Manage and Install Plugins menu), type fix geometries on search tab. On the search results, click on the option with same name: “fix geometries”.  

| [![]({{site.baseurl}}/img/fix_geom/Fix_geometries.png "Click to see larger image"){:width="300px"}]({{site.baseurl}}/img/fix_geom/Fix_geometries.png) |
|:--:| 
| *On QGIS, type fix geometries on search tab.*  
*Please note that this figure is in Portuguese (pt-BR)* |
{: .table-borderless}
<br>


On Fix Geometries window, select your invalid geometry shapefile as the input. On the output options, select where you want to save and type the name of the fixed shapefile you are about to obtain.  


| [![]({{site.baseurl}}/img/fix_geom/Fix_geometries2.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/Fix_geometries2.png) |
|:--:| 
| *On QGIS, select input and output on fix geometries window.*  
*Please note that this figure is in Portuguese (pt-BR)* |
{: .table-borderless}
<br>


Now you will have to extract data from your raster using your fixed shapefile as the mask layer. Click on Raster -> Extract -> Clip raster by mask layer.  

| [![]({{site.baseurl}}/img/fix_geom/QGIS_Extract_mask.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/QGIS_Extract_mask.png) |
|:--:| 
| *On QGIS, click on Raster -> Extract -> Clip raster by mask layer.*  
*Please note that this figure is in Portuguese (pt-BR)* |
{: .table-borderless}
<br>


On the “Clip raster by mask layer” window, select your raster as input data and your fixed shapefile as mask layer. To avoid data loss and keep it as integrate as possible, it is suggested to click on “Keep resolution of output raster” option. Then, on the output options, select where you want to save and also the name of your final raster file.  

| [![]({{site.baseurl}}/img/fix_geom/QGIS_Extract_mask_window.png "Click to see larger image"){:width="500px"}]({{site.baseurl}}/img/fix_geom/QGIS_Extract_mask_window.png) |
|:--:| 
| *On QGIS “Clip raster by mask layer” window, it is suggested to click on “Keep resolution of output raster” option.*  
*Please note that this figure is in Portuguese (pt-BR)* |
{: .table-borderless}
<br>


Now that your shapefile's geometry is fixed you can continue your work from where it stopped! Hope this post helps!
