# chamber-crawler-game
Please email me in order to have access to my code and view my project



Design Document

Design of the Program:
Introduction:
ChamberCrawler3000 (CC3k) is an exciting Rogue-like single player game which was implemented using modern object-oriented design concepts and frameworks creating an enjoyable and interactive game. These concepts created an adaptable and flexible to modifications. We used Git as the version control system to collaborate and share our codes, which enabled a flawless workflow throughout the entire development of the progress. 
Overview:
Our implementation of the CC3k game uses a Game class which manages the general direction of the game which includes beginning the game, quitting, selecting a player type, restarting and many more. It is done by controlling all the keyboard inputs and methods are called based on them. That is, the Game can control which events must occur and a particular event is brought closer by a set of calls through different classes. For instance, if the player enters so,Game will call player->advance (Cell South of player). Then, in Player::advance, it is checked if a player can walk on that cell and if yes, it will advance to it through a call to Character::advance. So, our system has various smaller classes to handle these specific cases making the entire program very systematic. We will discuss in more details in the further sections.

Movement/Advancement
Since there is a restriction like Enemies cannot move on certain Cells, while Players can, there is a method in Cell which takes a character and checks whether a character can advance on it. This method is overwritten by the different subclasses of Cells so that different ways of walking are regulated by particular Cell types, thus keeping restriction on advancement.
The actual movement of the Player and Enemy are similar so we use advance(Cell *) operation in Character class following the Observer pattern to place Player or Enemy on the new cell and clearing out the present cell. The Cell can clear its Object pointer if the Object notifies the Cell that the Character has advanced.

Potions
The Cell class is alerted when a potion is used "u" in a direction through the Cell::collectedby(Character *) function. Thus, it looks for an Object and, if one exists, it invokes the virtual Object::collect method. We can filter out any misapplication, like using on any Cell without a Potion, by this method. However, if it is a Potion, the Potion::collect method will be invoked instead. As a result, we employ inheritance and virtual methods to delegate the (u)se result.

Gold
When a Player advances onto a Cell, the isStepped operation is called, but this has no effect. But various types of Treasure then overwrite this operation in order to add gold to the Player.

Combat
Just like Cell::collectedby methods for potions, we incorporated the same strategy. We used  Cell::striken_by method to see if the object in the Cell could be attacked. If it could be attacked, we called Object::striken_by on the object. To model the different types of combat between any two characters, the Visitor Design Pattern was used.
The character superclass has different overloaded methods for striken_by and strike. These methods are overwritten in the subclasses, to correctly mimic the difference of the attack based on the attacker or the person being attacked. Each different character uses the appropriate striken_by or strike function.

Spawning
Random Tiles
Most of the spawning is done by the Floor class. Floor chooses a random chamber that chooses a random tile within the chamber that is unoccupied. Floor can spawn any other type of object. The process is the same as above.

Random Object
When it comes to the decision of what to spawn, the tile class has methods to spawn each type of object. For Player and Stair, the spawning process was quite normal. As for the rest of the objects the Factory Design Pattern was used to generate the objects. This gives subclasses the opportunity to decide which object to create, It encapsulates object creation.


Resilience To Change:
The design patterns and methods discussed in the plan previously, help solve functionality and design problems like memory management, various implementation errors and make the program more flexible to respond to any changes in specifications. This level of flexibility allow for modifications, additions, deletions of different features, which is very important for the program to run smoothly with all the structural components of the game. We tried to achieve maximum cohesion and minimum coupling which means that the project should work towards a common goal but not be dependent on each other to the extent that one small change leads to an unwanted change in the other.  Inheritance and class hierarchies are used to minimize coupling and enable abstract coupling. This causes the clients to only observe the abstract base class and its interfaces and not the implementation of the subclasses. Additional features are then implemented in subclasses which overridde the existing functionalitiees with no effect on the existing ones. This enables resilience to a broad range of potential changes in our large program.




Design Questions Revisited:

Question 1: How could you design your system so that each race could be easily generated? Additionally, how difficult does such a solution make adding additional races?

Answer: Our system has various concrete subclasses that inherit from the abstract superclass Character. This superclass has all the common fields and methods that are common to all races and there are Player and Enemy classes extending from the Character class, thus generating classes get easier with the constructor in Character class initializing common characteristics.  It is quite easy to add additional races as they can just inherit the common fields and methods from the Player and Enemy classes with the addition of their own specific fields and methods.

Question 2: How does your system handle generating different enemies? Is it different from how you generate the player character? Why or why not?

Answer: For generating different enemies, our system used the Factory Design Pattern as it allows the subclasses to decide which object to create. This is because the enemies are determined probabilistically (except dragons). 
This is different from how we generated the player character as the player is determined at the beginning of the game from user input and there is only one instance of the player object. So, when the getInst method of the player class is called, it returns a particular race of player. Although, one method decides which race is being returned for both Player and Enemy, however, the difference is that a new different enemy is returned for each call but the player remains the same for every call.

Question 3: How could you implement the various abilities for the enemy characters? Do you use the same techniques as for the player character races? Explain.

Answer: The skills for both the enemies and player races were implemented using the Visitor Design Pattern, with consequences based on the attacker and defender. This enable specific enemies to respond in the desired way according to their abilities, which are implemented in the particular subclasses of the Enemy class. Although same process has been used for the Player class, it is implemented in a way to handle other events like collecting potions, calling passive actions (for example: for a Troll, +5HP a turn), etc. These events are not directly connected to fighting, so enemies do  not have them.

Question 4: The Decorator and Strategy patterns are possible candidates to model the effects of potions so that we do not need to explicitly track which potions the player character has consumed on any particular floor. In your opinion, which pattern would work better? Explain in detail, by discussing the advantages/disadvantages of the two patterns.

Answer: The Decorator Pattern is complicated and time-consuming to implement, however, it lets methods to effectively stack on top of each other and to be implicitly tracked. So using the Decorator pattern, multiple decorators can be stacked on top of the character to handle multiple effects. Although the Strategy Pattern is less complicated and easier to implement, it simply allows a function call to be replaced by another. It would have been okay if there was only one possible effect at a time but in our game, a player can take multiple potions and have multiple effects at the same time. Therefore, we used the Decorator pattern for stacking multiple effects in our game due to its intuitiveness, rather than using the Strategy pattern to model the effects of potions using a single function.

Question 5: How could you generate items so that the generation of Treasure and Potion reuses as much code as possible? That is, how would you structure your system so that the generation of a potion and then generation of treasure does not duplicate code?

Answer: By selecting a random chamber and a random cell, the floor generates certain items. The responsibility of item production is subsequently delegated to the individual cells. A Factory Design Pattern is used to generate specific items in the cells. As a result, code will be reused. The process of finding a random cell to spawn an item is the same for all items.

Final Questions:

Question (a): What lessons did this project teach you about developing software in teams?

Answer: The project taught us the importance of communication and collaboration with team members in developing successful software. Through this project, we realized we should start small and extend the program as required. Initially, we were more involved in working with the bigger portions of the program with being less open with the changes we were making. But gradually, as the project progressed, we started to become more vocal with the changes we were making as well as used our knowledge of Git to commit frequently and share our implementations of different classes as planned. This enabled us to understand the implementations of code better with effective communication and make sure our program runs flawlessly

Question (b): What would you have done differently if you had the chance to start over?

Answer: If we had the chance to start over, we would definitely add tests so that we have a better understanding of the expected behavior of the program. Moreover, we would have taken the time to perform regression testing, so that new features were not breaking the existing functionality of the program. We faced a number of memory leakages so maybe a different approach would be using Valgrind at every step to mitigate those problems. Lastly, we would like to do more thorough research on the DLCs so that we know how to implement them properly to further enhance our program.


