# Watopoly

Side Note

Due to the University of Waterloo's policy, I am unable to directly share the code on GitHub. However, if you are interested in viewing the code, please feel free to email me at i2kashif@uwaterloo.ca.

Introduction

Welcome to my Watopoly design, below you will find an overview which goes over our project, an updated UML, a design which discusses my class hierarchy, our resilience to change which discusses how I structured the code so that I could make changes and these changes wouldn’t affect my implementation.

Overview
The project is comprised of 7 main classes:

Player:
The player class handles all information related to the player such as their position, what properties they own, amount of money, and also their playing piece character.

Block:
The base class for the Property and NonProperty classes. Block provides a general method called steppedOn that Property and NonProperty override to determine what kind of effect occurs when a player steps on it. It also contains the variables and methods required for the derived classes to keep track of their positions on the board.

Property:
Is the derived class of Block. Property is the class for all Blocks that can be owned by a player. Property holds methods such as setOwner, setImprovement and all other methods related to its rent cost when players step on it and for keeping track of its owner. Most of the important information for Property is contained within the Building struct which stores its tuition, block name and improvement level.

NonProperty:
Is the derived class of Block. NonProperty basically holds non-property blocks that cannot be owned. It does not hold much information but rather a complicated method for triggering different events as players step on. NonProperty is distinguished mostly by the name of the blocks.

Board:
The board is the main controller for the game which includes the necessary methods to play the game. The Board initialises 40 blocks in a fixed order using the initBlocks method and players are added using the addPlayers method. The contents of Board are displayed using the TextDisplay class.

TextDisplay:
Contains the necessary methods and structures to automatically update and print out the game board.

Design
For this section, I will talk about how I implemented various features and the design patterns we used for each function.

Player Turns and running the game:
A player is able to roll the dice when the private boolean variable “turnOver” is false. When a player fails to roll a double, the turnOver is set to true. When the next() function is called then this variable is reset to false and the currPlayer integer increments by 1 to signify the next player’s start.

Game Display:
When the program starts, all of the blocks are initialised and added to the TextDisplay class. TextDisplay was implemented using a subject, observer design pattern where the TextDisplay is the Observer and both the Player and Property classes are Subjects that notify the TextDisplay. This way the display will automatically update its display every time a player moves or a block gets bought or sold.

The TextDisplay is also very big so instead of printing the Display after every move, we instead created another command called “show” which will display the Game when the user wants.

Declaring Bankruptcy:
When a player declares bankruptcy, all of the player's properties are reset to normal, and the player is removed from the player list. The idea was that once the player list reaches 1, the game will end.

Saving and Loading:
This is one of the most important features to consider during the design. In order to be able to test different situations, this is implemented in the very beginning. However, as the designs of Player and Block changed rapidly. Saving and Loading had to also be updated frequently during the implementation process. Saving is simply implemented by adding an output operator and a member function in Player and Block invoking that operator, respectively. As different ways for storing info for Players and Blocks had been added, the ways to output them was changed. Load has been changed 3 times. At first, it was being separated with initialization of the board, having its own logic to initialise every detail of the board. The first time, I changed it to such that a board is first initialised, and then blocks and players data got read in and changed in the initialised board. The second time is simply changing the way of accessing the blocks and players from being in a vector to searching by name in a map. The last time, Blocks and Players became pointers, especially for Blocks, so I could directly use a pointer to get its children, Property class.

Buying/Selling improvements:
In order to be able to buy improvements, you must own all of the properties in the specific area. I used a Subject, Observer pattern in order to address this problem. When a Property is bought, it notifies all of the related properties that it has an owner now. The property and its related properties will then update a private variable that keeps track of how many related properties its current owner holds. Once this variable reaches the max, then that means the property’s owner also owns all of its related properties. Thus now the property is now improvable.

Roll Dice:
I wanted to have an implementation of roll dice that was able to do just more than generate random numbers for the dice so I also used the dice function in our board class to keep track of the DC Tims Line. Roll dice used a variety of if statements to make sure that all conditions regarding an individual being stuck in DC TIms Line were implemented. Moreover I implemented two versions of dice one for testing and the other for game logic. The testing version received inputs as specified in the documentation for watopoly. While the game logic variation of roll die computed a random number which allowed random movements across the board.

Non Property:
To implement most of the non property blocks we used a Non property class that would be notified whenever someone would step on a non property block. Using this information I was able to move players by accessing functions from Player class about the player, which consequently allowed me to change the players position, change the amount of money the player had according to the requirement. The non-property class required a random number generator for SLC and Needles Hall so I also had to implement that to make sure that all the players were getting the SLC and Needles Hall cards based on a random probability function.
