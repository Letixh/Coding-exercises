#Laetitia Hagadone

#This game is a breakout like game. The objective is to collide into and "break" each block without letting the ball hit the bottom of the canvas.
#Hitting he bottom of the canvas results in a game over
#Breaking all 12 blocks result in winning the game
#Players can see how much time it took them to win or lose
#The user's input is clicking a green block to redirect the ball to the red blocks and to avoid the bottom of the canvas.

from tkinter import *
from random import choice
import time

def clock():
    return time.perf_counter()
#defining the clock with time.perf_counter() which shows how much time passed since a certain time. In this game its since the moment the player presses start.

def ballPosition():
    x1, y1, x2, y2 = field.coords(ball)
    return [(x1+x2)/2, (y1+y2)/2]
#defining the ball's positioning, The ballPosition function uses the coords canvas method, which find the cordinates of the bounding box the ball is in and find the center position

blocks = [] #Because i have block objects in this game that will be deleted, i start with an empty list
block, ball, endText,sx, sy = None,None, None, 2, 2
#one line assigns multiple variables to multiple values. Assigning the value None to block,ball,and endText and 2 to sx and sy


def startGame():
    global startTime, ball, blocks, endText

    if endText is not None:
        field.delete(endText)

    upperLeftX = field.winfo_width()//2
    upperLeftY = field.winfo_height()//2
    ball = field.create_oval(upperLeftX, upperLeftY,
                             upperLeftX+10, upperLeftY+10,
                             fill='blue')

    blocks = []
    w = 80
    h = 20

    starting_x = (field.winfo_width()- 4 * w) // 2  
    starting_y = 0
    for i in range(4):
        for j in range(3):
            block = field.create_rectangle(starting_x + i*w, starting_y + j*h,
                                           starting_x + w*(i+1), starting_y + h*(j+1), fill='red')
            blocks.append(block)
            
    startButton.config(state=DISABLED)
    

    startTime = clock()
    animate()

#defining of to start the game
#I use the global variables startTime, ball, blocks, and endText
#Because this game is restartable and ends with a text, I need to make sure the text is deleted when i restart the game
#I don't want the ball to start in a random psition because it can mess up the objective of the game so i place it in the center of the canvas
#I do this by taking the field's hight and width and dividing them by 2 to center the ball
#i create an oval for the ball, to be a circle upperLeftX+10, upperLeftY+10 evens out the positioning and i fill it blue
#once again establishing i have an empty list of blocks, i create some with the width of 80 and height of 20.
#the starting x position uses the field's width info, i had to subtract the width of a block 4 times and divide by 2 because it wasnt centered as i like and i found that to be the best place.
#the starting y position is 0 to be as high as possible
#i is the rows, each row has 4 blocks, while j is the columes, each colume has 3 blocks
#I create the  block by creating a field rectangle which i got from example 4, but instead of 100 i place the starting_X with the width and i and place starting_y with the height and j. I fill th blocks red
#i also append all these blocks into my blocks list
#I want my start button to be disabled while the game plays, it disables after the user clicks it
#once the start button is clicked, the clock starts and animation starts

def animate():
    global sx, sy, blocks, ball

    pattern = 'Elapsed time: {0:.1f} seconds'
    timeDisplay['text'] = pattern.format(clock()-startTime)
    x, y = ballPosition()
    hitVertical = hitBlock() and blockType == 'vertical'
    if x+sx>300 or x+sx<0 or hitVertical:
        sx *= -1
    hitHorizontal = hitBlock() and blockType == 'horizontal'
    if y+sy>300 or y+sy<0 or hitHorizontal:
        sy *= -1
    field.move(ball, sx, sy)

#i use the info from the "learn something new document" that if the ball hits either the verticle or horizontal wall, it will bounce off reversing its direction
#i also display the time


    hit_block = None  
    for block in blocks:
        block_x1, block_y1, block_x2, block_y2 = field.coords(block)
        if block_x1 <= x <= block_x2 and block_y1 <= y <= block_y2:
            hit_block = block
            break
#I tried out the code for collisions that was given in the test instructions but i kept running into errors, i dont know if its because i dont use velocity and aceleration in this game so instead i use the block list i have
#i initialize the variable hit_block to None to indicate no block has been hit yet.
#for block in blocks i get the coordinates of the blocks to check if the ball (x,y) has hits within the boundary of any of the blocks
#if it does, the block becomes a hit_block and the loops breaks
        
    if y > 290: 
        game_over()

    else:
        if hit_block:
            field.delete(hit_block)
            blocks.remove(hit_block)
            sy *= -1

        if len(blocks) > 0:
            root.after(20, animate)
        else:
            game_over()

#there are two ways the game can end, winning or losing
#i wanted that if the ball hits the bottom of the canvas then the player loses because that makes the Click function i have later pretty useless and the player could not use the input and still win
#to do that i did if the ball's y-cordinate hits the value of 290 (removed 10 from 300 because the ball is 10), the game ends, this is the losing ending
#if the player doesnt let the ball hit the bottom there is an alternate way to end the game, if a block was hit, that hit block will get deleted from the frame and the blocks list will remove that block as well
#hitting a block will also change the direction of the ball
#it will then check how many blocks are left, if theres more than 0, the animate function is called to restart the loop after 20 millisecond and continue the game
#if its 0 then the game will end, this is the winning ending

def Click(event):
    global block, blockType
    if block:
        field.delete(block)
    block = field.create_rectangle(event.x-20, event.y,
                                   event.x+20, event.y+6,
                                   fill='light green')
    blockType = 'horizontal'

#i decided i only want a horizontal block so there is only one type of click
#this function will trigger with the user clicking within the frame
#checking if theres a block (from this function) on the canvas, it will be deleted when the user clicks again
#I create the block at the wanted size and color and assign this block type as horizontal


def hitBlock():
    if not block or not field.coords(block):
        return False

    ball_x1, ball_y1, ball_x2, ball_y2 = field.coords(ball)
    block_x1, block_y1, block_x2, block_y2 = field.coords(block)
    if (ball_x2 >= block_x1 and ball_x1 <= block_x2) and (ball_y2 >= block_y1 and ball_y1 <= block_y2):
        return True
    return False
#defining hitblock, checking if block is none or the cordinates of the block is none to see if its empty will return as false
#it checks the coordinates of the ball (field.coords(ball)) and of the blocks (field.coords(block))
#the if statement checks if the ball's edges overlap with any block's edges. If it does it returns as true and if it doesnt it returns as false. This tells us if the ball makes contact with a block
#Even though hitBlock() and hit_block are not the same, they work hand in hand

def game_over():
    global endText, ball, block

    if ballPosition()[1] >= 290:
        endText = field.create_text(150, 150, text='You Lose', font=('Arial', 30))
    else:
        endText = field.create_text(150, 150, text='You Win!', font=('Arial', 30))

    field.delete(ball)
    field.delete(block)
    field.update()
    startButton.config(state=NORMAL)


    reset_blocks()

#there are two game endings, lose and win
#if the ball's position hit 290 aka the bottom of the canvas, the player will receive a "You Lose" ending text
#else, which is if all blocks are hit, the player receives a "You win!" ending text
#because i want the player to play as many games as they want, i deleted the balla dn blocks, update the field and enable the start button again
#because i was having issues restarting my blocks i had to make sure they especially were reset
    
def reset_blocks():
    global blocks

    for block in blocks:
        field.delete(block)
    blocks.clear()

#defining resent_blocks I make sure all the blocks in the blocks list clear out and on the field (in the case the player loses)



root = Tk()
root.title('Block Game')
timeDisplay = Label(root)
timeDisplay.pack()
field = Canvas(root, width=300, height=300, bg='light blue')
field.pack()
startButton = Button(root, command=startGame, text='Start', state=NORMAL)
startButton.pack()
field.bind('<ButtonPress-1>', Click)
mainloop()

#i create my main window with Tk and name it "block game"
#i create a label widget that displays the time
#my field is a canvas widget that has a width and height of 300 pixles and the color is lightblue
#i create a start button which is associated with startGame(). the initial state is Normal which may change later
#Field.bind, bind the user mouse clicking with Click()
#and it will starts the Tkinter's main event loop which will continue till the window closes
