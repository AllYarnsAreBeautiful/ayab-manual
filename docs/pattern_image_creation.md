[TOC]

# Pattern creation

You can use free open source software to design patterns. Create an image with a 
width of 200 pixels or less with indexed colors. For two color knitwork, 
make the image monochromatic: black and white (1 bit) with each pixel 
representing a stitch. Save it in the PNG file format. 
The following instructions show how to achieve this using GIMP and how to benefit
from the vector graphics program Inkscape to create scalable patterns.


## Using GIMP to create patterns and save as PNG file

1. Download GIMP 2.10.10 (or newer) from [the GIMP homepage](https://www.gimp.org/downloads/) 
   and install it.
2. Run GIMP
3. In GIMP, left-click File -> New
4. Select the hight and width in pixels (= number of rows and stitches per row, 
   respectively), then left-click "OK".
5. In GIMP, left-click Image -> Modus -> Indexed. In the dialog that pops up, 
   select "black/white (1bit)" for textiles with two colors. 
6. Select the pencil tool (hotkey: N) 
7. Set the pencil tool settings to Size = 1.00 (1x1 pixel), Hardness = 100, 
   keep other tool options at default settings (Dynamics Off). Keep default 
   foreground color (black) and background color (white).
8. Optionally show grid: Left-click View -> Show Grid. Then, left-click 
   Image -> Configure Grid. Set Spacing:Horizontal = 1.00 pixels and 
   Spacing:Vertical = 1.00 pixels. Left-click "OK".    
9. Draw the pattern (Zoom in using View -> Zoom to see individual pixels):  
   so that background color is represented by white pixels and contrast color 
   (foreground) is represented by black pixels. 
10. In GIMP, left-click File -> "Export as". In the "Export Image" dialog that 
   pops up, type in a file name and select the target directory, then 
   left-click "Select File Type (By Extension)" -> "PNG (.PNG)", 
   then left-click "Export". In the "Export as PNG" dialog that pops up, you can 
   keep Compression level = 9 and optionally deselect all other options, then 
   left-click "Export" at the bottom of that dialog.
11. You can close GIMP now.


## Creating scalable patterns with Inkscape

To create patterns that can be scaled with optimal quality, design them in 
   a vector graphics program. The following instructions use Inkscape which is open source.
1. Download Inkscape 0.92)(or newer) for free from the [Inkscape homepage](https://inkscape.org/) and install it.
2. Run Inkscape and left-click File -> Document properties. In the dialog that pops 
   up, Set user-defined hight and width of the document in mm, so that the document 
   is about twice as big as the desired size of the textile to be designed. 
   Left-click the tab "grid" and set grid units to mm. Then set "spacing X" = 1 and "spacing Y" = 1.
   Close the dialog. 
3. Inside Inkscape, left-click "View" and activate the option "show grid".
4. Left-click on the tool "Create rectangle and squares"  tool (hotkey: F4) in 
   the left toolbar and draw. 
5. Choose the select tool (hotkey: F1). Select your rectangle and adjust it using the 
   tool controls that appear in the toolbar at the top. The width (W) and hight (H)
   in mm of that rectangle should equal exactly the desired width and hight of the fabric 
   part you want to produce.
6. Left-click "Fill and Countour" ([Shift]+[Ctrl]+[F]) -> Fill -> "Solid color". Set the value RGBA = ffffffff (white). 
   Left-click the tab "Contour color" and set it to "No color". 
7. Left-click on one of the drawing tools in the left toolbar and draw a shape 
   of the desired size inside the existing (invisible) rectangle from step 4-6. 
   For Bézier-curves, the drawing mode has to be selected from the toolbar at the 
   top before drawing and double-click or pressing the [enter] key ends drawing.
8. Choose the select tool (hotkey: F1). Select your second shape and adjust it using 
   the tool controls or by dragging the handles of the shape with the mouse.
9. Left-click "Fill and Countour" ([Shift]+[Ctrl]+[F]) and set contour color and/or 
   Fill color to "No color", RGBA = ffffffff (white) or RGBA = 000000ff (black). 
10. Choose your yarn and make a gauge swatch with it on your knitting machine.
    Determine the gauge (x = number of stitches per mm, y = number of rows per mm). 
11.  Calculate the required stitches per row (s) the required number of rows (r) for 
    your textile using the following formulas:
    s = x * W ,
    r = y * H ,
    Here, W is the width- and H is the hight of the background rectangle (see step 5) in mm. 
12. Left-click File -> "Save as..." and choose a destination and file name. It is 
    recommended to indicate the part size in the file name. 
13. In Inkscape, left-click "Export as PNG" ([Shift]+[Ctrl]+[E]), then select the 
    tab "Page" and set unit to px. In the section "Image size" set 
    Width = s (required stitches per row) and set Hight = r (required number of rows). 
    Left-click "Export as", set the target path, type in a file name and 
    left-click "Export". 
14. Optionally, open the exported PNG in GIMP 2.10 (or newer) and Left-click 
    Image -> Mode -> Indexed -> black/white (1bit). Then edit the pattern using the 
    pencil tool and export as PNG as described [above](pattern_image_creation.md#using-gimp-to-create-patterns-and-save-as-png-file).


### Using Inkscape to scale patterns for creation of differently sized textiles 
If you have a pattern saved as SVG file and you want to change the resulting fabric size, you 
can scale it using Inkscape: 
1. Open the SVG file in Inkscape.
2. If you want to scale the width of contours and Bézier-curves with the rest of the shapes, 
   select those objects and left-click Path -> "Convert object to path".   
3. Select all objects, including the invisible background-rectangle.
4. Left-click Object -> Transform... ([Shift]+[Ctrl]+[M]). If you want to ceep the aspect ratio,
   activate the option "Scale proportionally".
   Set the scaling factor in percent (or change % to mm and set the desired Width (and Hight) in mm). 
   Then left-click Apply.
5. Continue with steps 11-14 of the [previous section](pattern_image_creation.md#creating-scalable-patterns-with-inkscape).      


This article was written by [DerAndere](https://it-by-derandere.blogspot.com/p/blog-page_46.html)  
[Feel free to improve it!](https://github.com/AllYarnsAreBeautiful/ayab-manual)