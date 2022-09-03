---
layout: default
---

[/index](../bios.md)

Keyboard
--------

The keyboard is easy to configure. The PIC does not send us the ASCII code for the pressed key, but the scancode for the key-down and the key-up events, so we will need to translate those.

Check out `keyboard.cpp` where there are two functions: the callback and the initialization which configures the interrupt callback. 

`kbScancodeTranslator.cpp` also has a table to translate scancodes to ASCII keys. You can read more [about scancodes here](http://www.win.tue.nl/~aeb/linux/kbd/scancodes-1.html)

Keyboard characters
-------------------

Access the typed characters

- When a key is pressed, the callback gets the ASCII code 
- The callback then appends that character to a buffer
- It can also be printed on the screen

Responding to user input
------------------------

The keyboard callback checks for a newline, and then calls the kernel, telling it that the user has input something. The function `parseCommand()` in `screen.cpp` allow the user to run some basic commands like `help, clear, echo [something]`

---

#### Excercise 

- Try to get input from mouse and draw on screen. Note that the IRQ that handles mouse input is of number 12. Once you get input from mouse create a program that allows user to draw on screen.
- Calibrate the cursor move so that it doesnt move in discrete points and change the cursor icon to your own preference.