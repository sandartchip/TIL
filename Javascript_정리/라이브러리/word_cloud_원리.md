

### How the Word Cloud Generator Works

The layout algorithm for positioning words without overlap is available on GitHub under an open source license as d3-cloud. 

Note that this is the only the layout algorithm and any code for converting text into words and rendering the final output requires additional development.

As word placement can be quite slow for more than a few hundred words, the layout algorithm can be run asynchronously, with a configurable time step size. This makes it possible to animate words as they are placed without stuttering. It is recommended to always use a time step even without animations as it prevents the browser’s event loop from blocking while placing the words.

The layout algorithm itself is incredibly simple. For each word, starting with the most “important”:

Attempt to place the word at some starting point: usually near the middle, or somewhere on a central horizontal line.

If the word intersects with any previously-placed words, move it one step along an increasing spiral. Repeat until no intersections are found.

The hard part is making it perform efficiently! According to Jonathan Feinberg, Wordle uses a combination of hierarchical bounding boxes and quadtrees to achieve reasonable speeds.

### Glyphs in JavaScript
There isn’t a way to retrieve precise glyph shapes via the DOM, except perhaps for SVG fonts. 
Instead, we draw each word to a hidden canvas element, and retrieve the pixel data.

Retrieving the pixel data separately for each word is expensive, so we draw as many words as possible and then retrieve their pixels in a batch operation.

Sprites and Masks
My initial implementation performed collision detection using sprite masks. Once a word is placed, it doesn't move, so we can copy it to the appropriate position in a larger sprite representing the whole placement area.

The advantage of this is that collision detection only involves comparing a candidate sprite with the relevant area of this larger sprite, rather than comparing with each previous word separately.

Somewhat surprisingly, a simple low-level hack made a tremendous difference: when constructing the sprite I compressed blocks of 32 1-bit pixels into 32-bit integers, thus reducing the number of checks (and memory) by 32 times.

In fact, this turned out to beat my hierarchical bounding box with quadtree implementation on everything I tried it on (even very large areas and font sizes). I think this is primarily because the sprite version only needs to perform a single collision test per candidate area, whereas the bounding box version has to compare with every other previously-placed word that overlaps slightly with the candidate area.

Another possibility would be to merge a word’s tree with a single large tree once it is placed. I think this operation would be fairly expensive though compared with the analagous sprite mask operation, which is essentially ORing a whole block.

Hierarchical Bounding Boxes
I didn’t want the hierarchical bounding box code to go to waste. Click to see more glyphs, or drag to test for collisions!

Future Work
I think a bit more performance can be squeezed out of the sprite collision detections e.g. by using a hierarchy of coarse sprite masks, but the 32-bit compression trick is a bit fiddly to implement.
It should be possible to use HTML and CSS3 instead of SVG, but the positions currently refer to the bottom-centre of each word so this makes it slightly trickier in CSS3.
Beware!
Use word clouds with care; while they’re æsthetically pleasing, there are almost always better ways to visualise and analyse data. For an in-depth rant about how they can be bad, see:
