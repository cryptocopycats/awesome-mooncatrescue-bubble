# Notes


## Popularity & Rarity

**All possible MoonCat pose / patterns**

Midnight Lightning
writes in all All possible MoonCat pose / patterns
<https://www.reddit.com/r/MoonCatRescue/comments/6tmp86/all_possible_mooncat_patterns/>:


There's four possible poses for MoonCats,
plus 32 different pattern variations
for a total of 128 variations.

Plus one flag to "invert" the colors (make the dominant color of the cat a lighter version of the color), for a grand total of 256 variations.

Seen here are the 256 variations all using the same color base (pure red). The number under each cat is the variation number (the k value that's used in the mooncatparser.js script),
which is the second byte of the cat's ID.

And in another posting:

There's four different fur patterns, and four different mouth poses, 
which you can see the 16 permutations of in the first 16 horizontal rows of this image <https://imgur.com/4fWQs2s>.

Cats can have "inverted" colors, so that's 32 different appearance possibilities. 
There's then four different poses possible to make the 256 possibilities in that image.


**All color variations for MoonCats**

Midnight Lightning
writes in all All color variations for MoonCats
<https://www.reddit.com/r/MoonCatRescue/comments/6tmu09/all_color_variations_for_mooncats/>:

Colors of MoonCats are based on the random RGB values that are encoded into their IDs, 
but are always a fully-saturated version of that color. 
This array of sample cats walks around the color wheel (360 degrees) showing all the possibilities, 
plus the "inverted" version (where a lighter version of the color is the dominant color of the cat).

Technically you can get hue values that are fractions of a degree, 
so you can get colors between each of these variations, 
but you probably won't be able to distinguish them with the naked eye.


And in another posting:

A cat's ID is always 5 bytes long. The first byte is whether it's a Genesis cat or not. 
The second byte controls pose and pattern for the cat, 
and the final three bytes are the red, green, and blue color channels for defining the cat's color. 
However when rendered a cat will always appear as the fully-saturated version of that color 
(rendering code takes the RGB values and converts them to a "hue" and then fully-saturates it), not the literal RGB values.

Yeah, the color math takes some wrapping your head around; it's moving from "RGB" space, 
which represents colors as how much Red, Green, and Blue pigment they have 
(therefore three "sliders" that go from zero to 255), and is sometimes visualized as a cube, 
and converts it to "HSL" space, which represents colors by which Hue they are 
(and how "saturated" that hue is, and the "luminosity" (lightness/darkness) of that color), 
which is often visualized as a cylinder (because "hue" goes from zero to 360 degrees). 
Going from "a cube" to "a cylinder" does indeed take some weird math 
to map every point in one volume to another point in the other volume.




**More about generate art**


MidnightLightning writes

Looking at the `mooncatparser` function, the five bytes of data in the `catId` are (in order): `genesis`, `k`, `r`, `g`, `b`.

`genesis` is a boolean (zero or one) that flags a genesis cat.

`k` indicates the pose of the cat, from 128 possibilities.
If the `k` byte value is greater than 128, it's colors are "inverted".

The `r`, `g`, and `b` bytes becomes the Red, Green, and Blue components of the main color of the cat. That gets converted to HSL values to grab the main hue of the cat and fully-saturates it. Four other colors are derived from that hue by fully-saturating it, and adjusting the lightness. Note, all the colors in each cat will always be fully-saturated colors (the `derivePalette` function in using the `HSLToRGB` function always uses `1` as the `s` parameter it's inputting, indicating a fully-saturated color. This helps make all the cats look vibrant, since randomly-generated RGB values tend to look washed out and tend toward brown/gray. However, the RGB values stored in the cat's `catId` value are the potentially-duller RGB values. And that's a way to guarantee that no randomly-generated cat will be white or black; only the genesis cats can be that, and this seems to be the way the developers are enforcing that. Even if a mined cat gets in with a color that's very close to white, it will get shunted toward the color that it's most like. But that does beg the question of what happens with the rare edge-case where a cat is mined with all three RGB values exactly the same (e.g. #424242, #A1A1A1, #3F3F3F, etc.)! Those are pure gray colors, and no one hue is going to be dominant. If it were rendered just like that it would be a gray cat. However, the function used in the `mooncatparser.js` file sets colors like that to a fully-de-saturated red color, and then the palette-generating function sets it to fully-saturated, so a cat that on the inside is a pure gray will actually render as a bright red cat.


ponderware responds:

Adding more color uniqueness, possibly by varying the saturation, at least within a bounded range, could have been cool. Hindsight. We might play around with it some more, even if the new code can never truly be official.



