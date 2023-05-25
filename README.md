# chess-clock
School project written in assembly language

# Development environment and hardware used in project
- Keil uVision
- Intel 8051 Microcontroller

# Detailed description
The program displays the chess clock on the LCD display in the following way:

FP--:--L--:--M--

SP--:--L--:--M--

where:

FP - first player time left,

SP - second player time left,

L - time of last / current move,

M - the number of performed moves,

At the beginning of the “MAIN” procedure, we assign initial values to variables used for displaying time and number of moves for each player and to register R0 (this register indicates whether the game was started or not). Then we call procedures responsible for initializing LCD display and counter T0 and printing chess  clock. After that, we go to the “LOOP” procedure which checks the state of the R0 register at the very beginning. We will not go any further until the state on the R0 is 1 (this can be achieved by pressing the appropriate button responsible for starting the game). Then we check whether the variable CL_100 is equal to 0, and if it is not, we return to the beginning of the procedure (we do this because whenever CL_100 is equal to 0 it means that one second has passed and there is no need to print something on display more frequently than every one second in our case). Lines 56 and 57 are responsible for clearing the display. Next lines of the procedure respectively: print the chess clock, check that the first and second players have not run out of time (if so, the program is stopped by setting the value of the R0 register to 0) and finally loop to the beginning of this procedure.

In the “LCD CONTROL” section of code are placed various procedures used for controlling the LCD display.

The “TIMER” section contains procedures: T0_init, which sets the T0 counter to the appropriate state (so that it will measure 10 miliseconds), and T0_ISR, which handles the interrupt in the following way: set appropriate values to the most and least significant byte of the T0 counter, check the state of buttons, increment variable CL_100 and check if it is equal to 100, if it is not, go to the end of the procedure. If the CL_100 was equal to 100 at this point, it means that one second has passed. The next lines of code are responsible for changing the time for the player who is currently making the move.

The “CHESS CLOCK” section contains procedure PRINT_CHESS_CLOCK that is responsible for printing every character or number on the display. Inside of it, a different procedure is called several times when we want to print a number instead of a character: PRINT_NUMBER. It divides the value stored in the accumulator by 10 and then prints the result and remainder separately. Procedures CHECK_P1_TIME and CHECK_P2_TIME are used to check if a player has run out of time.

The “BUTTONS” section contains procedures responsible for handling each of three used buttons. Procedure BUTTON_1 checks if the first button is pressed and if the R1 register is set to 0 (the first player is currently playing) and then it resets the time of the last move for the second player, increases the number of moves for the first player by 1, and sets register R1 to 1. Procedure BUTTON_2 checks if the second button is pressed and if the R1 register is set to 1 (the second player is currently playing) and then it resets the time of the last move for the first player, increases the number of moves for the second player by 1, and sets register R1 to 0. Procedure BUTTON_3 checks if the third button is pressed and restarts the game by changing the values of most variables to the initial ones.
