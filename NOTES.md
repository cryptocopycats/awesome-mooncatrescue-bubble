# Notes

About colors:

MidnightLightning writes

Looking at the `mooncatparser` function, the five bytes of data in the `catId` are (in order): `genesis`, `k`, `r`, `g`, `b`.

`genesis` is a boolean (zero or one) that flags a genesis cat.

`k` indicates the pose of the cat, from 128 possibilities.
If the `k` byte value is greater than 128, it's colors are "inverted".

The `r`, `g`, and `b` bytes becomes the Red, Green, and Blue components of the main color of the cat. That gets converted to HSL values to grab the main hue of the cat and fully-saturates it. Four other colors are derived from that hue by fully-saturating it, and adjusting the lightness. Note, all the colors in each cat will always be fully-saturated colors (the `derivePalette` function in using the `HSLToRGB` function always uses `1` as the `s` parameter it's inputting, indicating a fully-saturated color. This helps make all the cats look vibrant, since randomly-generated RGB values tend to look washed out and tend toward brown/gray. However, the RGB values stored in the cat's `catId` value are the potentially-duller RGB values. And that's a way to guarantee that no randomly-generated cat will be white or black; only the genesis cats can be that, and this seems to be the way the developers are enforcing that. Even if a mined cat gets in with a color that's very close to white, it will get shunted toward the color that it's most like. But that does beg the question of what happens with the rare edge-case where a cat is mined with all three RGB values exactly the same (e.g. #424242, #A1A1A1, #3F3F3F, etc.)! Those are pure gray colors, and no one hue is going to be dominant. If it were rendered just like that it would be a gray cat. However, the function used in the `mooncatparser.js` file sets colors like that to a fully-de-saturated red color, and then the palette-generating function sets it to fully-saturated, so a cat that on the inside is a pure gray will actually render as a bright red cat.


ponderware responds:

Adding more color uniqueness, possibly by varying the saturation, at least within a bounded range, could have been cool. Hindsight. We might play around with it some more, even if the new code can never truly be official.



