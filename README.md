# Calibration spreadsheet for calculating the threshold curve differentiating rock and snow in RGB imagery

This spreadsheet is supplementary to the paper:
Burton-Johnson, A. and Wyniawskyj, N. (2020). Rock and snow differentiation from colour (RGB) images.  The Cryosphere.

This spreadsheet has been produced to aid calculation of the calibration curve differentiating rock and snow in colour images. The equation of this curve will then be used to derive the threshold raster differentiating rock and snow from the red values of the original image. For a detailed discussion of the methodology refer to the associated publication.

To use this spreadsheet for rock and snow delineation:
- Load your image into a GIS programme (we recommend ESRI ArcMap or QGIS).
- Create two point shapefiles: one for snow and one for rock. These will be your training sets.
- Create point features in these shapefiles to select pixels of snow and rock in your image. We recommend using at least 50 pixel values for each subtype. Ensure that pixels are sampled for a range of reflectance values, particularly in shaded regions. Most importantly, sample multiple pixels across the contacts between rock and snow, as mixed pixels in these regions are the hardest to differentiate.
- Extract red and blue band raster values from the image into the training sets ("Extract Values to Points" in the ArcMap Spatial Analyst Toolbox).
- Enter these pixel values in to their relevant worksheet within this spreadsheet [Delete example rows first].
- Within the worksheet "Threshold Calibration", in the table "Manually Defined Calibration Points", change the location of the four "Manually Defined Calibration Points" on the curve so that they best separate the values for snow and rock.
- A second order polynomial trendline is automatically fitted to the "Manually Defined Calibration Points" and its equation displayed on the graph. Use this equation in your GIS or image analysis software to calculate the threshold raster delineating snow and rock from the red and blue bands in your original image (use the "Raster Calculator" in the ArcMap Spatial Analyst Toolbox).
The equation to use in Raster Calculator for "y = ax2 + bx + c", where y is the calculated threshold for red band raster (Threshold.tif) and x is the ratio of the red/blue raster values ("red-blue.tif"), is:

a * Float("red-blue.tif") * Float("red-blue.tif") -b * Float("red-blue.tif") +c

Copy the values of a, b, and c from the polynomial equation in this spreadsheet. Calculate "red-blue.tif" (i.e. x in the polynomial equation) in the Raster Calculator from:

Float("[Red Raster Band].tif")/Float("[Blue Raster Band].tif")

- Threshold the red raster band ("[Red Raster Band].tif") from the original image using the new threshold raster to delineate rock and snow pixels. Use the following equation in ArcMap's "Raster Calculator":

Con("[Red Raster Band].tif"  < "Threshold.tif",1,0)

The pixels representing rock outcrop will have red band values lower than the threshold raster pixel that they intersect, so will be assigned values of "1" in the output raster. Snow pixels will have red values exceeding the associated pixel in the threshold raster, so will be assigned "0".


If you have further questions, please contact the lead author. Updates to this spreadsheet will be hosted here:

https://github.com/Alex-Burton-Johnson/RGB_Rock-Snow_Differentiation

All the best,

Dr Alex Burton-Johnson

British Antarctic Survey
High Cross, Madingley Road, Cambridge, CB3 0ET
Email: alerto@bas.ac.uk
