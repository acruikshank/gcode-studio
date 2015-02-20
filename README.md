# gcode-studio will not fix your prints. Its goal is to break them.

Many people have put a lot of effort into ensuring that slicers convert 3D geometry into machine instructions (G-Code) that prints accurately and reliably. Gcode Studio allows you to take that G-Code and transform it in ways that almost certainly will end in ruin. If you are thoughtful and careful this might be an interesting ruin of your print. Otherwise it may be the ruin of your printer.

![gcode studio screenshot](http://acruikshank.github.io/gcode-studio/screenshot.jpg)

## How to use

Start with a gcode file. This is the file that comes out when you save a print from a slicer application like Cura or MakerBot Desktop. Open the file in a text editor (preferably one that can handle large files). The format isn't very complicated. For the most part it specifies the next X, Y, and Z coordinates of the nozzle, how much filament to extrude (E), and how fast to move (F).

Now [open gcode-studio](http://acruikshank.github.io/gcode-studio/gcode-studio.html) and click the open button to open the gcode file. Something resembling the model should appear on the screen. This is a drawing of the actual path the 3D printer's nozzle will make during the print. The editor at the bottom of the screen allows you to write a transform function that will be applied to each line of the gcode (actually just the G0 and G1 lines). The transform function starts with X, Y, Z, E, F and G variables defined which correspond to the current values of G-Code explained. G is 1 or 0 indicating the command is a normal or quick (non-extruding) move. There is also a special context variable that lets you save state between G-Code commands. Any JavaScript can be used in the transform function. Hit Command-Enter to run the transform. Altering any of these variables in the editor will alter the G-Code. If you alter the X, Y, or Z variables you should see the changes in the renderings above.

Some example transforms:
```
if (! isNan(Y) && Z) Y += Z*Z*Z/1000;
```
Stretch the print toward the front of the printer increasingly as the print rises. Note that Y and Z might not always be defined.

```
if (!isNaN(X) && !isNaN(Z) && Z > 30) X += 10;
```
Shift all parts of the print more than 30mm high 10 mm to the right.
