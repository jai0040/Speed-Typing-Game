# Speed-Typing-Game

<h2>Project in Python – Typing Speed Test</h2>

<p>Have you played a typing speed game? It’s a very useful game to track your typing speed and improve it with regular practice. Now, you will be able to build your own typing speed game in Python by just following a few steps.</p>

<h3>About the Python Project</h3><br>
<p>In this Python project idea, we are going to build an exciting project through which you can check and even improve your typing speed. For a graphical user interface, we are going to use the pygame library which is used for working with graphics. We will draw the images and text to be displayed on the screen.</p>

<h4>Prerequisites</h4>
The project in Python requires you to have basic knowledge of python programming and the pygame library.<br>
To install the pygame library, type the following code in your terminal.<br>

```
pip install pygame
```
**Steps to Build the Python Project on Typing Speed Test**<br><br>

_We created such file for our project_

**Background.jpg** – A background image we will use in our program<br>
**Icon.png** – An icon image that we will use as a reset button.<br>
**Sentences.txt** – This text file will contain a list of sentences separated by a new line.<br>
**Speed typing.py** – The main program file that contains all the code<br>
**Typing-speed-open.png** – The image to display when starting game<br>

First, we have created the sentences.txt file in which we have added multiple sentences separated by a new line.<br>

This time we will be using an Object-oriented approach to build the program.<br><br>

<h3>1. Import the libraries</h3>
For this project based on Python, we are using the pygame library. So we need to import the library along with some built-in modules of Python like time and random library.<br>

```
import pygame
from pygame.locals import *
import sys
import time
import random
```
<br>
<h3>2. Create the game class</h3>

<p>Now we create the game class which will involve many functions responsible for starting the game, reset the game and few helper functions to perform calculations that are required for our project in Python.</p>
<p></p>Let’s go ahead and create the constructor for our class where we define all the variables we will use in our project</p><br>

```
class Game:

    def __init__(self):
        self.w=750
        self.h=500
        self.reset=True
        self.active = False
        self.input_text=''
        self.word = ''
        self.time_start = 0
        self.total_time = 0
        self.accuracy = '0%'
        self.results = 'Time:0 Accuracy:0 % Wpm:0 '
        self.wpm = 0
        self.end = False
        self.HEAD_C = (255,213,102)
        self.TEXT_C = (240,240,240)
        self.RESULT_C = (255,70,70)


        pygame.init()
        self.open_img = pygame.image.load('type-speed-open.png')
        self.open_img = pygame.transform.scale(self.open_img, (self.w,self.h))


        self.bg = pygame.image.load('background.jpg')
        self.bg = pygame.transform.scale(self.bg, (500,750))

        self.screen = pygame.display.set_mode((self.w,self.h))
        pygame.display.set_caption('Type Speed test')
```

<p>In this constructor, we have initialized the width and height of the window, variables that are needed for calculation and then we initialized the pygame and loaded the images. The screen variable is the most important on which we will draw everything</p><br>

<h3>3. draw_text() method</h3>
<p>The draw_text() method of Game class is a helper function that will draw the text on the screen. The argument it takes is the screen, the message we want to draw, the y coordinate of the screen to position our text, the size of the font and color of the font. We will draw everything in the center of the screen. After drawing anything on the screen, pygame requires you to update the screen.</p>

```
def draw_text(self, screen, msg, y ,fsize, color):
    font = pygame.font.Font(None, fsize)
    text = font.render(msg, 1,color)
    text_rect = text.get_rect(center=(self.w/2, y))
    screen.blit(text, text_rect)
    pygame.display.update()
```

<br><h3>4. get_sentence() method</h3>

<p>Remember that we have a list of sentences in our sentences.txt file? The get_sentence() method will open up the file and return a random sentence from the list. We split the whole string with a newline character.</p><br>

```
def get_sentence(self):
    f = open('sentences.txt').read()
    sentences = f.split('\n')
    sentence = random.choice(sentences)
    return sentence
```
<br><br>
<h3>5. show_results() method</h3>
<p>The show_results() method is where we calculate the speed of the user’s typing. The time starts when the user clicks on the input box and when the user hits return key “Enter” then we perform the difference and calculate time in seconds.</p>
<p>To calculate accuracy, we did a little bit of math. We counted the correct typed characters by comparing input text with the display text which the user had to type.</p><br>
<p>The formula for accuracy is:</p>
<p>(correct characters)x100/ (total characters in sentence)</p>
<p>The WPM is the words per minute. A typical word consists of around 5 characters, so we calculate the words per minute by dividing the total number of words with five and then the result is again divided that with the total time it took in minutes. Since our total time was in seconds, we had to convert it into minutes by dividing total time with 60.</p>
<p>At last, we have drawn the typing icon image at the bottom of the screen which we will use as a reset button. When the user clicks it, our game would reset. We will see the reset_game() method later in this article.</p>

```
def show_results(self, screen):
    if(not self.end):
        #Calculate time
        self.total_time = time.time() - self.time_start

        #Calculate accuracy
        count = 0
        for i,c in enumerate(self.word):
            try:
                if self.input_text[i] == c:
                    count += 1
            except:
                pass
        self.accuracy = count/len(self.word)*100

        #Calculate words per minute
        self.wpm = len(self.input_text)*60/(5*self.total_time)
        self.end = True
        print(self.total_time)

        self.results = 'Time:'+str(round(self.total_time)) +" secs Accuracy:"+ str(round(self.accuracy)) + "%" + ' Wpm: ' + str(round(self.wpm))

        # draw icon image
        self.time_img = pygame.image.load('icon.png')
        self.time_img = pygame.transform.scale(self.time_img, (150,150))
        #screen.blit(self.time_img, (80,320))
        screen.blit(self.time_img, (self.w/2-75,self.h-140))
        self.draw_text(screen,"Reset", self.h - 70, 26, (100,100,100))

        print(self.results)
        pygame.display.update()
```

<br><h3>6. run() method</h3>
<p>This is the main method of our class that will handle all the events. We call the reset_game() method at the starting of this method which resets all the variables. Next, we run an infinite loop which will capture all the mouse and keyboard events. Then, we draw the heading and the input box on the screen.</p>
<p>We then use another loop that will look for the mouse and keyboard events. When the mouse button is pressed, we check the position of the mouse if it is on the input box then we start the time and set the active to True. If it is on the reset button, then we reset the game.</p>
<p>When the active is True and typing has not ended then we look for keyboard events. If the user presses any key then we need to update the message on our input box. The enter key will end typing and we will calculate the scores to display it. Another event of a backspace is used to trim the input text by removing the last character.</p><br>

```
def run(self):
    self.reset_game()


    self.running=True
    while(self.running):
        clock = pygame.time.Clock()
        self.screen.fill((0,0,0), (50,250,650,50))
        pygame.draw.rect(self.screen,self.HEAD_C, (50,250,650,50), 2)
        # update the text of user input
        self.draw_text(self.screen, self.input_text, 274, 26,(250,250,250))
        pygame.display.update()
        for event in pygame.event.get():
            if event.type == QUIT:
                self.running = False
                sys.exit()
            elif event.type == pygame.MOUSEBUTTONUP:
                x,y = pygame.mouse.get_pos()
                # position of input box
                if(x>=50 and x<=650 and y>=250 and y<=300):
                    self.active = True
                    self.input_text = ''
                    self.time_start = time.time()
                 # position of reset box
                 if(x>=310 and x<=510 and y>=390 and self.end):
                    self.reset_game()
                    x,y = pygame.mouse.get_pos()


            elif event.type == pygame.KEYDOWN:
                if self.active and not self.end:
                    if event.key == pygame.K_RETURN:
                        print(self.input_text)
                        self.show_results(self.screen)
                        print(self.results)
                        self.draw_text(self.screen, self.results,350, 28, self.RESULT_C)
                        self.end = True

                    elif event.key == pygame.K_BACKSPACE:
                        self.input_text = self.input_text[:-1]
                    else:
                        try:
                            self.input_text += event.unicode
                        except:
                            pass

        pygame.display.update()


    clock.tick(60)
```

<br><h3>7. reset_game() method</h3>
<p>The reset_game() method resets all variables so that we can start testing our typing speed again. We also select a random sentence by calling the get_sentence() method. In the end, we have closed the class definition and created the object of Game class to run the program.</p><br>

```
def reset_game(self):
        self.screen.blit(self.open_img, (0,0))

        pygame.display.update()
        time.sleep(1)

        self.reset=False
        self.end = False

        self.input_text=''
        self.word = ''
        self.time_start = 0
        self.total_time = 0
        self.wpm = 0

        # Get random sentence
        self.word = self.get_sentence()
        if (not self.word): self.reset_game()
        #drawing heading
        self.screen.fill((0,0,0))
        self.screen.blit(self.bg,(0,0))
        msg = "Typing Speed Test"
        self.draw_text(self.screen, msg,80, 80,self.HEAD_C)
        # draw the rectangle for input box
        pygame.draw.rect(self.screen,(255,192,25), (50,250,650,50), 2)

        # draw the sentence string
        self.draw_text(self.screen, self.word,200, 28,self.TEXT_C)

        pygame.display.update()



Game().run()
```

<br><h3>Summary</h3>
<p></p>In this article, you worked on the Python project to build your own game of typing speed testing with the help of pygame library.</p>
<p>I hope you got to learn new things and enjoyed building this interesting Python project. Do share the article on social media with your friends and colleagues.</p><br>
