# Reload and Rivalry - A Duelling Game Implemented on the DE1-SoC
Developed by Deniz Pala and Joonseo Park, Electrical and Computer Engineering Students at the University of Toronto

## Overview
Reload and Rivalry is a two-player decision making game developed using embedded C on the DE1-SoC.  PS2 Keyboard and switch inputs can be taken to generate outputs that employ static and animated VGA displays, audio, the 7-segment hex display, and the board’s LED lights. The in-hardware
timer is also employed and integrated with these I/O devices.

## Gameplay Summary
The project, Reload and Rivalry is a two-player decision making game. Every round, each
player takes turns selecting one of three moves; reload, shoot, or shield. Each player is given 10
seconds to choose an action. Being idle once the 10 seconds have passed results in “shield”
being chosen by default. One can also skip the countdown by pressing the “enter” key. The
game logic is simple and easy to understand. If the player gets shot and does not have a shield
to protect them, they lose. The moves are revealed after each turn. If a player gets shot, a game
over screen is displayed after revealing the moves. In addition, the program counts the number
of bullets each player has and displays it each round. A bullet is lost by shooting, and gained by
reloading.

Edge cases we had to consider:
- Trying to select “shoot” without bullets: The hex display shows an error message, and
the “shoot” action selected by the player is not registered in the program
- Trying to reload when bullet count is 5 (maximum): The hex display shows an error
message, and the “reload” action selected by the player is not registered in the program
- Both players select “shoot”: Both players will be killed and the game returns to the menu
screen without a winner

## Features
### 1. Sound
To enhance the user experience during the game, a repeating song has been added every time
the player reaches the menu screen, which stops whenever the player types on the keyboard or
presses a key to play (by implementing a function that polls the RVALID bit, returning a boolean
value when anything on the keyboard is pressed). When a player is killed, the outcome of the
game (i.e. Player 1 loses) is displayed on the screen and ending music is played. Sound files
were converted to C arrays and written to the output left and right data registers through a
function

### 2. Keyboard and Keys
At the start of the game, players have the choice to use either the keyboard or pushbuttons to
play the game. This is determined in the menu screen, by pressing either a key on the PS2
keyboard or the KEY3 pushbutton. This is stored in the variable “OneMeansUseKeys”, which is
passed by value into the game function. As the name suggests, when the variable is 1, the
game function only polls for the input from the keys and uses the keyboard otherwise. During
the gameplay, the three arrow keys on the keyboard and the three keys on the board
correspond to shoot, reload and shield. This is handled in the player function by polling.

### 3. Usernames
The keyboard is also used to get the usernames of the players in the very beginning with the
playerGetName function. Inside this function, it either saves the character into a character array
which will be the name, or erases the previous character by replacing it with space (‘ ‘) if
backspace has been pressed. The getKey4Name waits until a key is reeleased by polling for the
value of 0xF0, and then calls keyPress2Alphabet function which returns the character such as
0X1C to ‘a’. As a result, if a user enters “Pooya” as their name, the HEX would show “Pooya’s
turn” each time it is their turn to choose a move.

### 4. Hex Display
Since players shouldnt know about their opponents move, these are shown to the current player
on the hex display such as displaying ‘shoot’ or ‘you need more bullets’ error. To display longer
messages with more than 6 characters, an animated hexdisplay function was implemented.
Firstly, the elements of the word array are passed into char2Num function to return the
numerical value which will display that character on the hex display. It can also display longer
characters like M formed by 2 upside down U’s. All of the integer values are saved into a new
integer array called display. This is then stored into the hex display, and through a for loop, the
array positions are constantly incremented to give the animated look of a moving word display.

### 5. Timer and LEDs
The timer has two main uses: determining the speed of the animations on the the hex and VGA
display, and the 10 second countdown during each players turn. There are two functions using
two different clocks on the board. The wait function runs for a given amount of time, making the
rest of the game wait until its over. The oneSecTimer function starts a 1 second timer, and
polling is used to check if the displayed countdown should be updated. This is also displayed on
the LEDs. Each time the timer is over, time left is decremented, updating the LEDs and VGA.

### 6. VGA: Static Display and Animations
The game makes use of the VGA display by printing static VGA displays at each point in the
game. Images were converted to C arrays and printed using implemented functions. On top of
these backgrounds are animations such as moving bullets, timer countdowns, and bloodstains
appearing on player sprites when shot at. Double buffering was employed to achieve animation,
and to prevent tearing, a function that clears and re-prints only the moving object was
implemented. This was done by saving the pixels of the background on which the object would
be printed in an array, and printing this background over the object again when clearing it

## Project Demonstration
The submitted code may be slow to run on the computer system simulator, CPULator (https://cpulator.01xz.net/) due to its size. However, the file runs
without issues on the DE1-SoC hardware. Here is a link to a video demonstrating the project:

https://drive.google.com/file/d/122N-30Be5v5BwfiN76tNVRy0NfF_iFk1/view 





