# GST 102: Spatial Analysis
## Lab 7 - Raster Data Analysis - Working with Topographic Data
### Objective – Learn the Basics of Terrain Analysis

Document Version: 4/8/2015

**FOSS4G Lab Author:**
Kurt Menke, GISP  
Bird's Eye View GIS

**Original Lab Content Author:**
Richard Smith, Ph.D., GISP
Texas A&M University - Corpus Christi

---

The development of the original document was funded by the Department of Labor (DOL) Trade Adjustment Assistance Community College and Career Training (TAACCCT) Grant No.  TC-22525-11-60-A-48; The National Information Security, Geospatial Technologies Consortium (NISGTC) is an entity of Collin College of Texas, Bellevue College of Washington, Bunker Hill Community College of Massachusetts, Del Mar College of Texas, Moraine Valley Community College of Illinois, Rio Salado College of Arizona, and Salt Lake Community College of Utah.  This work is licensed under the Creative Commons Attribution 3.0 Unported License.  To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.

This document continues to be modified and improved by generous public contributions.

---

### 1. Introduction

In this lab, you will learn about topographic data and how to use it for analysis. You will learn how to create datasets such as slope, hillshades using QGIS Desktop. You will then learn how to combine them using raster algebra.

This lab includes the following tasks:

+ Task 1 Terrain Analysis
+ Task 2 Reclassification
+ Task 3 Raster Calculator

### 2 Objective: Learn the Basics of Terrain Analysis

The objective of this lab is to learn the basics of terrain analysis using QGIS Desktop. 

### Task 1 Terrain Analysis

In this task, you will use a digital elevation model to create several terrain related datasets: slope, aspect, and hillshade. These elevation derived datasets can be important in site selection and other terrain based spatial analyses.

1. Open QGIS Desktop.
3. Add the 35106-B4.dem raster from the lab directory to QGIS Desktop using the Add Raster Layer button.

This raster layer has elevation values for each cell. This type of data is referred to as a digital elevation model, or DEM, for short. This particular dataset covers the Sandia Mountains on the east side of Albuquerque, New Mexico (shown in figure below). The light areas have the highest elevation and the dark areas the lowest elevation.

![Digital Elevation Model (DEM) QGIS Desktop](figures/Digital_Elevation_Model_(DEM)_QGIS_Desktop.png "Digital Elevation Model (DEM) QGIS Desktop") 

4. Let's explore the properties of the raster dataset.

	a. Open the Layer Properties for the DEM and choose the General tab. Notice that the raster is in the UTM coordinate system. UTM has X/Y coordinate values in meters. 

	b. Now switch to the Metadata tab. FInd the Properties section and scroll down until you see the Pixel Size properties. Notice that the Pixel size is 10 x 10. This means each cell represents a 10 by 10 meter area. 

	c. Now switch to the Style tab. The elevation values (Z) of a DEM are typically either feet or meters. For me, the min value reads 1841 and the max value 3094. Your values may differ slightly. By default, the Load min/max values is set to Cumulative count cut and the Accuracy is set to Estimate (faster). Switch the Load min/max values to Min/Max and the Accuracy to Actual (slower) and click the Load button. The values should now read 1775 to 3255. The Sandia Mountain range reaches 10,678 feet above sea level. Therefore, you can deduce that these elevation units are in meters. Before working with DEMs, it is important to understand what unit the X, Y, and Z values are in. In this case, all three are in meters. 

	d. Close the Layer Properties window.

5. Save your project as Lab7.qgs. in the lab directory.
6. You will use the Raster Terrain Analysis plugin to create the three elevation related datasets. From the menu bar choose Plugins | Manage and Install Plugins.
7. Type ‘Terrain’ into the Search bar.
8. Find the Raster Terrain Analysis plugin and click the box to enable it. Close the Plugins window.
7. First you will create a hillshade image which will allow you to get a better feel for the terrain in this area. Hillshade images are very useful for creating nice maps of an area. From the menu bar choose Raster | Terrain Analysis | Hillshade.
8. Use the following parameters (shown in figure below).

	a. Elevation layer = 35106-B4

	b. Output layer = Lab 7 Data/MyData/Hillshade.img

	c. Output format = Erdas Imagine Images (.img)

	d. Z factor – 1.0 (this is a conversion factor between the X/Y and Z units. Since all three are meters you can leave this at 1.0)

	e. Check Add result to project

	f. Leave defaults for Azimuth and Vertical angle (sun position).

	g. Click OK.

![Hillshade Parameters](figures/Hillshade_Parameters.png "Hillshade Parameters")

The resulting hillshade should resemble the figure below.

![Hillshade Layer](figures/Hillshade_Layer.png "Hillshade Layer")

This is a grayscale hillshade rendering. Now you will use both the original DEM and the hillshade to create a color hillshade image. 

8. Drag the Hillshade below the DEM in the Layers panel.
9. Open the Layer Properties | Style tab for the DEM (35106-B4)
10. Set the following Style properties (shown in figure below):

	a. Change the Render type to Singleband pseudocolor

	b. Change the color ramp to BrBG.

	c. Change the Load min/max values to Min/max and the Accuracy to Actual (slower).

	d. Click Load

	d. Click Classify

![Styling the DEM](figures/Styling_the_DEM.png "Styling the DEM")

9. Switch to the Transparency tab and set the Global transparency to 50%.
10. Click OK and close the Layer Properties.
10. Your map should now resemble the figure below.

![Color Hillshade Image](figures/Color_Hillshade_Image.png "Color Hillshade Image")

11. Now you will create a Slope dataset. From the menu bar choose Raster | Terrain Analysis | Slope.
12. Fill out the Slope tool as shown in the figure below then click OK.

![Slope Tool](figures/Slope_Tool.png "Slope Tool")

The slope raster shows the steepest areas in white and the flattest terrain in black. The tool determines the steepness of each pixel by comparing the elevation value of each pixel to that of the eight surrounding pixels. The slope values are degrees of slope (shown in figure below).

![Slope Raster](figures/Slope_Raster.png "Slope Raster")

Now you will create an Aspect raster. Aspect measures which cardinal direction the terrain in each pixel is facing (north facing vs. south facing etc.)

12. From the menu bar choose Raster | Terrain Analysis | Aspect. 
13. Fill out the Aspect tool as shown in the figure below then click OK.

![Aspect Tool](figures/Aspect_Tool.png "Aspect Tool")

The output should resemble the figure below with values ranging from ~0-360 representing degrees (0=north, 90= east, 180 = south and 270 = west).

![Aspect Raster](figures/Aspect_Raster.png "Aspect Raster")

13. Save your project.

### Task 2 Reclassification

Now that you have created the slope and aspect data you will reclassify them into meaningful categories. Raster reclassification is a method for aggregating data values into categories. In this case, you will be reclassifying them into categories important to identifying habitat suitability for a plant. Once the slope and aspect data have been reclassified you will combine them in Task 3 to identify suitable habitat areas.

1. Open QGIS Desktop and open Lab 7 Data/Lab7.qgs if it is not already open.
2. This plant requires steep slopes. You will classify slope raster into three categories: 0-45, 45-55, and > 55. First you will create a text file that contains the classification rules.

	a. Open NotePad or a similar text editor and create a text file with in the format of the figure below.

	b. The first line tells QGIS to recode cells with slope values between 0 and 45 degrees with a new value of 1.

	c. Cells with slope values from 45-55 degrees will receive a new value of 2 and those cells with values greater than 55 will receive a new value of 3. 

	d. Save the text file to the Lab 7 Data/MyData folder and name it Slope_Recode_Rules.txt. Close the text editor.

![Slope Classification Rules](figures/Slope_Classification_Rules.png "Slope Classification Rules") 

3. From the menu bar choose Processing | Toolbox.
4. Expand and open the GRASS commands toolset | Raster (r.*) | r.recode - Recodes categorical raster maps.
5. Set the tool options to the following (shown in figure below):

	a. Set the Input layer to Slope. 

	b. Navigate to the Lab 7 Data/MyData folder and select the Slope_Recode_Rules.txt as the File containing recode rules. 

	c. Name the output file Slope_ReCode.img. 

	d. Click Run.

![r.recode Parameters](figures/r_recode_Parameters.png "r.recode Parameters")

4. The new layer will be called Output raster layer in the Layers panel. It appears to have only two categories: 1) black and 2) white.
5. Open the Layer Properties | Style tab and set the following options:

	a. Change the Renderer type to Singleband pseudocolor. 

	b. Change the color ramp to RdYlGn. 

	c. Change the Mode to Equal Interval. 

	d. Set the number of classes to 3. 

	e. Change the Load min/max values to Min / max.

	f. Change the Accuracy to Actual (slower)

	g. Click Load.

	h. Click Classify.

	i. Before closing Layer Properties go to the General tab and change the Layer Name to Slope Reclassified.

	j. Click OK to set the Layer Properties.

5. Now the best habitat in terms of slope has a value of 3 and the worst a value of 1 (shown in figure below).

![Reclassified_and_Styled_Slope](figures/Reclassified_and_Styled_Slope.png "Reclassified_and_Styled_Slope")

Now you will recode the Aspect data in the same fashion. This plant prefers west facing slopes. Hence the west facing slopes will be set to 3, the north and south are the next best location so set them to 2, and the eastern slopes can be set 1. Remember that the values of the aspect raster are compass bearings or azimuths (270 is due west, 0 is north, 180 is south and 90 is east). You will classify the aspect data into eight cardinal directions. 

6. Open Notepad and create a text file that looks like the figure below.  Save the text file to your MyData folder and name it Aspect_Recode_Rules.txt.
7. Close the text editor.

![Aspect_Recode_Rules](figures/Aspect_Recode_Rules.png "Aspect_Recode_Rules") 

7. In the Processing Toolbox, expand the GRASS commands toolset | Raster (r.*) | r.recode - Recodes categorical raster maps.
8. Set the following tool options (shown in figure below):

	a. Set the Input layer to Aspect.

	b. Navigate to the Lab 7 Data/MyData folder and select the Aspect_Recode_Rules.txt as the File containing recode rules. 

	c. Name the output file Aspect_ReCode.img. 

	d. Click Run

![r.recode_Aspect_Parameters](figures/r_recode_Aspect_Parameters.png "r.recode_Aspect_Parameters")

8. Rename the newly reclassified Aspect layer from Output raster layer to Aspect Reclassified.
9. Save your QGIS project.

### Task 3 Raster Calculator

Now you will use the Raster Calculator to combine the reclassified slope and aspect data. The Raster Calculator allows you to combine raster datasets mathematically to produce new outputs. For example, raster datasets can be added, subtracted, multiplied, and divided against one another. This procedure is also known as raster algebra. In this task you will add the two reclassified rasters together. Since each raster has ideal conditions coded with the value '3', an area that ends up with a pixel value of 6 would be ideal. 

1. Open QGIS Desktop and open Lab 7 Data/Lab7.qgs if it is no already opened.
2. From the menu bar choose Raster | Raster Calculator. The loaded raster datasets are listed in the upper right window. Below that there are a panel of operators and an expression window (see figure below).
3. Do the following to add the two reclassified rasters:

	a. Double click on "Slope Reclassified@1" to place it in the Raster calculator expression.

	b. Click the addition sign. 

	c. Then click on the "Aspect Reclassified@1" raster. 
 
	d. In the Result layer section name the output layer Lab 7 Data/MyData/PlantHabitat.img. 

	e. Choose an Output format of Erdas Imagine Images (*.img)

	f. Click OK.

![Raster Calculator](figures/Raster_Calculator.png "Raster Calculator")

3. Open the Layer Properties of the PlantHabitat layer and symbolize the data with a pseudoband color with 6 equal interval classes.
4. The final raster will resemble the figure below.

![Final Habitat Analysis](figures/Final_Habitat_Analysis.png "Final Habitat Analysis")

5. Save your QGIS project.

### 3 Conclusion
In this lab, you were exposed to terrain analysis, creating derived datasets from elevation data (DEMs). You then went on to reclassify two terrain related datasets (aspect and slope), and combine them to produce a suitable habitat layer for a plant species. This is another method of doing site selection analysis. Raster data are well suited for these types of analyses. 

### 4 Discussion Questions

1. What other real world applications of terrain analysis can you think of? Describe.
2. How does this suitability analysis compare to the site selection analysis done with the vector data model in Lab 5?
3. What other linear networks could this apply to other than roads?

### 5 Challenge Assignment (optional)

Another scientist is interested in developing a map of potential habitat for another species that prefers rugged, steep west facing slopes. Use the  Raster Terrain Analysis plugin to develop a Ruggedness Index. Recode the Ruggedness Index into three categories:

0:20:1

20:40:2

40:*:3 

Combine the resulting recoded ruggedness index with the recoded slope and aspect from the lab to create the final result. Compose a map showing the results.