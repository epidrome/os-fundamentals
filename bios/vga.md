---
layout: default
---

[/index](../bios.md)

We will try to print to the screen. A VGA video card gives us a chunk of memory that we write both attribute byte and character byte pairs in order to show information on the screen. 

The text memory is simply a chunk of memory in our address space, located at 0xB8000, in physical memory. This buffer is of the datatype 'short', meaning that each item in this text memory array takes up 16-bits. Each 16-bit element in the text memory buffer can be broken into an 'upper' 8-bits and a 'lower' 8-bits. The lower 8 bits of each element tells the display controller what character to draw on the screen. The upper 8-bits is used to define the foreground and background colors of which to draw the character.


The upper 8-bits of each 16-bit text element is called an 'attribute byte', and the lower 8-bits is called the 'character byte'. The attribute byte gets broken up further into 2 different 4-bit chunks: 1 representing background color and 1 representing foreground color. Now, because of the fact that only 4-bits define each color, there can only be a maximum of 16 different colors to choose from (num bits ^ 2) - 4^2 = 16. Below is a table of the default 16-color palette.

| Value	 | Color	| Value	 | Color        |
| ------ | -------- | ------ | ------------ |
|     0  |BLACK	    |8       |DARK GREY     |
|     1  |BLUE	    |9       |LIGHT BLUE    |
|     2  |GREEN	    |10      |LIGHT GREEN   |
|     3  |CYAN	    |11      |LIGHT CYAN    |
|     4  |RED	    |12      |LIGHT RED     |
|     5  |MAGENTA   |13      |LIGHT MAGENTA |
|     6  |BROWN	    |14      |LIGHT BROWN   |   
|     7  |LIGHT GREY|15      |WHITE         |

Finally, to access a particular index in memory, there is an equation that we must use. The text mode memory is a simple 'linear' (or flat) area of memory, but the video controller makes it appear to be an 80x25 matrix of 16-bit values. Each line of text is sequential in memory; they follow eachother. We therefore try to break up the screen into horizontal lines. The best way to do this is to use the following equation:

index = (y_value * width_of_screen) + x_value;

This equation shows that to access the index in the text memory array for say (3, 4), we would use the equation to find that 4 * 80 + 3 is 323. This means that to draw to location (3, 4) on the screen, we need to write to do something similar to this:

unsigned short *where = (unsigned short *)0xB8000 + 323;
*where = character | (attribute << 8);