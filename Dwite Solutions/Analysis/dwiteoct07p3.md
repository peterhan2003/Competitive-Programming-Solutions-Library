The best way to approach this problem is probably to separately hardcode the QWERTY and Dvorak keyboards 

into your program, rather than to try to create a single conversion table, which is rather error-prone. That way, you can look up each 

character of the input to find its position in the Dvorak keyboard, then find the character in the same position of the QWERTY keyboard
