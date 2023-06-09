Download Link: https://assignmentchef.com/product/solved-cs61a-project-3-ants-vs-somebees
<br>



The bees are coming!

Create a better soldier With inherit-ants.

<h1>Download starter les</h1>

The <a href="https://cs61a.org/proj/ants/ants.zip">ants.zip</a> <a href="https://cs61a.org/proj/ants/ants.zip">(ants.zip)</a> archive contains several les, but all of your changes will be made to

ants.py .

ants.py : The game logic of Ants Vs. SomeBees ants_gui.py : The original GUI for Ants Vs. SomeBees gui.py: A new GUI for Ants Vs. SomeBees.

graphics.py : Utilities for displaying simple two-dimensional animations utils.py : Some functions to facilitate the game interface ucb.py : Utility functions for CS 61A state.py : Abstraction for gamestate for gui.py assets : A directory of images and les used by gui.py img : A directory of images used by ants_gui.py

ok : The autograder proj3.ok : The ok con guration le tests : A directory of tests used by ok

<h1>Logistics</h1>

The project is worth 25 points. 21 points are assigned for correctness, 2 points for composition, 1 point for submitting Phase 1 by the rst checkpoint date, and 1 point for submitting Phases 2 and 3 by the second checkpoint date.

Additionally, there are some extra credit point opportunities. You can get 1 EC point for submitting the entire project by Thursday, October 21, and 2 EC points for submitting the extra credit problem.

Important: In order to receive all of the extra credit points for Ants, your

implementation of the entire project, including the EC problem, must be submitted by the early submission deadline.

You will turn in the following les:

ants.py

You do not need to modify or turn in any other les to complete the project. To submit the project, run the following command: python3 ok –submit

You will be able to view your submissions on the <a href="http://ok.cs61a.org/">Ok</a> <a href="http://ok.cs61a.org/">dashboard</a> <a href="http://ok.cs61a.org/">(http://ok.cs61a.org)</a>.

For the functions that we ask you to complete, there may be some initial code that we provide. If you would rather not use that code, feel free to delete it and start from scratch. You may also add new function de nitions as you see t.

However, please do not modify any other functions. Doing so may result in your code failing our autograder tests. Also, please do not change any function signatures (names, argument order, or number of arguments).

Throughout this project, you should be testing the correctness of your code. It is good practice to test often, so that it is easy to isolate any problems. However, you should not be testing too often, to allow yourself time to think through problems.

We have provided an autograder called ok to help you with testing your code and tracking your progress. The rst time you run the autograder, you will be asked to log in with your Ok account using your web browser. Please do so. Each time you run ok , it will back up your work and progress on our servers.

The primary purpose of ok is to test your implementations.

We recommend that you submit after you nish each problem. Only your last submission will be graded. It is also useful for us to have more backups of your code in case you run into a submission issue. If you forget to submit, your last backup will be automatically converted to a submission.

If you do not want us to record a backup of your work or information about your progress, you can run python3 ok –local

With this option, no information will be sent to our course servers. If you want to test your code interactively, you can run

python3 ok -q [question number] -i

with the appropriate question number (e.g. 01 ) inserted. This will run the tests for that question until the rst one you failed, then give you a chance to test the functions you wrote interactively.

You can also use the debugging print feature in OK by writing  print(“DEBUG:”, x)

which will produce an output in your terminal without causing OK tests to fail with extra output.

<h1>The Game</h1>

A game of Ants Vs. SomeBees consists of a series of turns. In each turn, new bees may enter the ant colony. Then, new ants are placed to defend their colony. Finally, all insects (ants, then bees) take individual actions. Bees either try to move toward the end of the tunnel or sting ants in their way. Ants perform a di erent action depending on their type, such as collecting more food or throwing leaves at the bees. The game ends either when a bee reaches the end of the tunnel (you lose), the bees destroy the QueenAnt if it exists (you lose), or the entire bee eet has been vanquished (you win).

<h2>Core concepts</h2>

The Colony. This is where the game takes place. The colony consists of several Place s that are chained together to form a tunnel where bees can travel through. The colony also has some quantity of food which can be expended in order to place an ant in a tunnel.

Places. A place links to another place to form a tunnel. The player can put a single ant into each place. However, there can be many bees in a single place.

The Hive. This is the place where bees originate. Bees exit the beehive to enter the ant colony.

Ants. Players place an ant into the colony by selecting from the available ant types at the top of the screen. Each type of ant takes a di erent action and requires a di erent amount of colony food to place. The two most basic ant types are the HarvesterAnt , which adds one food to the colony during each turn, and the ThrowerAnt , which throws a leaf at a bee each turn. You will be implementing many more!

Bees. In this game, bees are the antagonistic forces that the player must defend the ant colony from. Each turn, a bee either advances to the next place in the tunnel if no ant is in its way, or it stings the ant in its way. Bees win when at least one bee reaches the end of a tunnel.

<h2>Core classes</h2>

The concepts described above each have a corresponding class that encapsulates the logic for that concept. Here is a summary of the main classes involved in this game:

GameState : Represents the colony and some state information about the game,

including how much food is available, how much time has elapsed, where the AntHomeBase is, and all the Place s in the game.

Place : Represents a single place that holds insects. At most one Ant can be in a

single place, but there can be many Bee s in a single place. Place objects have an exit to the left and an entrance to the right, which are also places. Bees travel

through a tunnel by moving to a Place ‘s exit .

H<span style="text-decoration: line-through;">i</span>ve : Represents the place where Bee s start out (on the right of the tunnel).

AntHomeBase : Represents the place Ant s are defending (on the left of the tunnel). If

Bees get here, they win &#x1f641;

Insect : A superclass for Ant and Bee . All insects have health attribute, representing their remaining health, and a place attribute, representing the Place where they are currently located. Each turn, every active Insect in the game performs its action .

Ant : Represents ants. Each Ant subclass has special attributes or a special action that distinguish it from other Ant types. For example, a HarvesterAnt gets food for the colony and a ThrowerAnt attacks Bee s. Each ant type also has a food_cost attribute that indicates how much it costs to deploy one unit of that type of ant.

Bee : Represents bees. Each turn, a bee either moves to the exit of its current Place if the Place is not blocked by an ant, or stings the ant occupying its same Place .

<h2>Game Layout</h2>

Below is a visualization of a GameState. As you work through the unlocking tests and problems, we recommend drawing out similar diagrams to help your understanding.

<h2>Object map</h2>

To help visualize how all the classes t together, we’ve also created an object map for you to reference as you work, which you can nd <a href="https://cs61a.org/proj/ants/diagram/ants_diagram.pdf">here</a> <a href="https://cs61a.org/proj/ants/diagram/ants_diagram.pdf">(diagram/ants_diagram.pdf)</a>:

<h2>Playing the game</h2>

The game can be run in two modes: as a text-based game or using a graphical user interface (GUI). The game logic is the same in either case, but the GUI enforces a turn time limit that makes playing the game more exciting. The text-based interface is provided for debugging and development.

The les are separated according to these two modes. ants.py knows nothing of graphics or turn time limits.

To start a text-based game, run python3 ants_text.py

To start a graphical game, run python3 gui.py

When you start the graphical version, a new browser window should appear. In the starter implementation, you have unlimited food and your ants can only throw leaves at bees in their current Place . Before you complete Problem 2, the GUI may crash since it doesn’t have a full conception of what a Place is yet! Try playing the game anyway! You’ll need to place a lot of ThrowerAnt s (the second type) in order to keep the bees from reaching your queen.

The game has several options that you will use throughout the project, which you can view with python3 ants_text.py –help .

usage: ants_text.py [-h] [-d DIFFICULTY] [-w] [–food FOOD] Play Ants vs. SomeBees

optional arguments:

-h, –help     show this help message and exit

-d DIFFICULTY  sets difficulty of game (test/easy/normal/hard/extra-hard)

-w, –water    loads a full layout with water

–food FOOD    number of food to start with when testing

<h1>Getting Started Videos</h1>

These videos may provide some helpful direction for tackling the coding problems on the project.

<a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">YouTube</a> <a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">link</a> <a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">(https://youtu.be/watch?</a>

<a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">v</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">=</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">szmjnq</a> <a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">jxn4</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">&amp;</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">list</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">=</a><a href="https://youtu.be/watch?v=szmjnqjjxn4&amp;list=PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO">PLx38hZJ5RLZcuFYxU1HlX9xmPGAFFAHOO)</a>

<h1>Phase 1: Basic gameplay</h1>

In the rst phase you will complete the implementation that will allow for basic gameplay with the two basic Ant s: the HarvesterAnt and the ThrowerAnt .

<h2>Problem 0 (0 pt)</h2>

Answer the following questions with your partner after you have read the entire ants.py le.

To submit your answers, run: python3 ok -q 00 -u

If you get stuck while answering these questions, you can try reading through ants.py again, consult the core concepts/classes sections above, or ask a question in the Question 0 thread on Piazza.

What is the signi cance of an Insect’s health attribute? Does this value change? If so, how?

Which of the following is a class attribute of the Insect class?

Is the health attribute of the Ant class an instance attribute or a class attribute?

Why?

Is the damage attribute of an Ant subclass (such as ThrowerAnt ) an instance attribute or class attribute? Why?

Which class do both Ant and Bee inherit from?

What do instances of Ant and instances of Bee have in common?

How many insects can be in a single Place at any given time (before Problem 8)? What does a Bee do during one of its turns? When is the game lost?

Remember to run: python3 ok -q 00 -u

<h2>Problem 1</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 01 -u

Part A: Currently, there is no cost for placing any type of Ant , and so there is no challenge to the game. The base class Ant has a food_cost of zero. Override this class attribute for HarvesterAnt and ThrowerAnt according to the “Food Cost” column in the table below.

<table width="475">

 <tbody>

  <tr>

   <td width="272">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="272">HarvesterAnt</td>

   <td width="90">2</td>

   <td width="113">1</td>

  </tr>

  <tr>

   <td width="272">ThrowerAnt</td>

   <td width="90">3</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

Part B: Now that placing an Ant costs food, we need to be able to gather more food! To x this issue, implement the HarvesterAnt class. A HarvesterAnt is a type of Ant that adds one food to the gamestate.food total as its action .

After writing code, test your implementation: python3 ok -q 01

Try playing the game by running python3 gui.py . Once you have placed a HarvesterAnt , you should accumulate food each turn. You can also place ThrowerAnt s, but you’ll see that they can only attack bees that are in their Place , making it a little di   cult to win.

<h2>Problem 2</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 02 -u

In this problem, you’ll complete Place.__init__ by adding code that tracks entrances. Right now, a Place keeps track only of its exit . We would like a Place to keep track of its entrance as well. A Place needs to track only one entrance . Tracking entrances will be useful when an Ant needs to see what Bee s are in front of it in the tunnel.

However, simply passing an entrance to a Place constructor will be problematic; we would <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">need</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">to</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">have</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">both</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">the</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">exit</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">and</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">the</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">entrance</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">before</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">creating</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">a</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">Place</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">! </a><a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">(It</a><a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">‘</a><a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">s</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">a</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">chicken</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">or</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">the egg</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">(https://en.wikipedia.org/wiki/Chicken_or_the_egg)</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">problem.)</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">To</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">get</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">around</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">this</a> <a href="https://en.wikipedia.org/wiki/Chicken_or_the_egg">problem</a>, we will keep track of entrances in the following way instead. Place.__init__ should use this logic:

python3 ok -q 02

<h2>Problem 3</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 03 -u

In order for a ThrowerAnt to throw a leaf, it must know which bee to hit. The provided implementation of the nearest_bee method in the ThrowerAnt class only allows them to hit bees in the same Place . Your job is to x it so that a ThrowerAnt will throw_at the nearest bee in front of it that is not still in the H<span style="text-decoration: line-through;">i</span>ve . This includes bees that are in the same Place as a ThrowerAnt

Hint: All Place s have an is_hive attribute which is True when that place is the Hive .

Change nearest_bee so that it returns a random Bee from the nearest place that contains bees. Your implementation should follow this logic:

python3 ok -q 03

After implementing nearest_bee , a ThrowerAnt should be able to throw_at a Bee in front of it that is not still in the Hive . Make sure that your ants do the right thing! To start a game with ten food (for easy testing): python3 gui.py –food 10

Make sure to submit by the checkpoint deadline using the following command: python3 ok –submit

You can check to ensure that you have completed Phase 1’s problems by running: python3 ok –score

Congratulations! You have nished Phase 1 of this project!

Phase 2: Ants!

Now that you’ve implemented basic gameplay with two types of Ant s, let’s add some avor to the ways ants can attack bees. In this phase, you’ll be implementing several di erent Ant s with di erent attack strategies.

After you implement each Ant subclass in this section, you’ll need to set its implemented class attribute to True so that that type of ant will show up in the GUI. Feel free to try out the game with each new ant to test the functionality!

With your Phase 2 ants, try python3 gui.py -d easy to play against a full swarm of bees in a multi-tunnel layout and try -d normal , -d hard , or -d extra-hard if you want a real challenge! If the bees are too numerous to vanquish, you might need to create some new ants.

<h2>Problem 4</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 04 -u

A ThrowerAnt is a powerful threat to the bees, but it has a high food cost. In this problem, you’ll implement two subclasses of ThrowerAnt that are less costly but have constraints on the distance they can throw:

The LongThrower can only throw_at a Bee that is found after following at least 5 entrance transitions. It cannot hit Bee s that are in the same Place as it or the rst 4

Place s in front of it. If there are two Bees , one too close to the LongThrower and the other within its range, the LongThrower should only throw at the farther Bee , which is within its range, instead of trying to hit the closer Bee .

The ShortThrower can only throw_at a Bee that is found after following at most 3 entrance transitions. It cannot throw at any bees further than 3 Place s in front of it.

Neither of these specialized throwers can throw_at a Bee that is exactly 4 Place s away.

<table width="475">

 <tbody>

  <tr>

   <td width="272">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="272">ShortThrower</td>

   <td width="90">2</td>

   <td width="113">1</td>

  </tr>

  <tr>

   <td width="272">LongThrower</td>

   <td width="90">2</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

To implement these new throwing ants, your ShortThrower and LongThrower classes should inherit the nearest_bee method from the base ThrowerAnt class. The logic of choosing which bee a thrower ant will attack is the same, except the ShortThrower and LongThrower ants have a maximum and minimum range, respectively.

To do this, modify the nearest_bee method to reference min_range and max_range attributes, and only return a bee if it is within range.

Make sure to give these min_range and max_range attributes appropriate values in the

ThrowerAnt class so that the behavior of ThrowerAnt is unchanged. Then, implement the

subclasses LongThrower and ShortThrower with appropriately constrained ranges.

You should not need to repeat any code between ThrowerAnt , ShortThrower , and LongThrower .

Hint: You can chain inequalities in Python: e.g. 2 &lt; x &lt; 6 will check if x is between 2 and 6. Also, min_range and max_range should mark an inclusive range.

Important: Make sure your class attributes are called max_range and min_range The tests directly reference these attribute names, and will error if you use another name for these attributes.

Don’t forget to set the implemented class attribute of LongThrower and ShortThrower to True .

After writing code, test your implementation (rerun the tests for 03 to make sure they still work):

python3 ok -q 03 python3 ok -q 04

<a href="https://cs61a.org/articles/pair-programming"> </a><a href="https://cs61a.org/articles/pair-programming">Pair</a> <a href="https://cs61a.org/articles/pair-programming">programming?</a> <a href="https://cs61a.org/articles/pair-programming">(/articles/pair-programming)</a> Remember to alternate between driver and navigator roles. The driver controls the keyboard; the navigator watches, asks questions, and suggests ideas.

<h2>Problem 5</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 05 -u

Implement the FireAnt , which does damage when it receives damage. Speci cally, if it is damaged by amount health units, it does a damage of amount to all bees in its place (this is called re ected damage). If it dies, it does an additional amount of damage, as speci ed by its damage attribute, which has a default value of 3 as de ned in the FireAnt class.

To implement this, override FireAnt ‘s reduce_health method. Your overriden method should call the reduce_health method inherited from the superclass ( Ant ) to reduce the current

FireAnt instance’s health. Calling the inherited reduce_health method on a FireAnt

instance reduces the insect’s health by the given amount and removes the insect from its place if its health reaches zero or lower.

However, your method needs to also include the re ective damage logic:

Determine the re ective damage amount: start with the amount in icted on the ant, and then add damage if the ant’s health has dropped to 0.

For each bee in the place, damage them with the total amount by calling the appropriate reduce_health method for each bee.

Important: The FireAnt must do its damage before being removed from its place , so pay careful attention to the order of your logic in reduce_health .

<table width="299">

 <tbody>

  <tr>

   <td width="96">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="96">FireAnt</td>

   <td width="90">5</td>

   <td width="113">3</td>

  </tr>

 </tbody>

</table>

Once you’ve nished implementing the FireAnt , give it a class attribute implemented with the value True .

Note: Even though you are overriding the superclass’s reduce_health function

( Ant.reduce_health ), you can still use this method in your implementation by calling it. Note this is not recursion. (Why not?)

After writing code, test your implementation:

python3 ok -q 05

You can also test your program by playing a game or two! A FireAnt should destroy all colocated Bees when it is stung. To start a game with ten food (for easy testing): python3 gui.py –food 10

Phase 3: More Ants!

<h2>Problem 6</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 06 -u

We are going to add some protection to our glorious home base by implementing the WallAnt , an ant that does nothing each turn. A WallAnt is useful because it has a large health value.

<table width="299">

 <tbody>

  <tr>

   <td width="96">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="96">WallAnt</td>

   <td width="90">4</td>

   <td width="113">4</td>

  </tr>

 </tbody>

</table>

Unlike with previous ants, we have not provided you with a class header. Implement the

WallAnt class from scratch. Give it a class attribute name with the value ‘Wall’ (so that the graphics work) and a class attribute implemented with the value True (so that you can use it in a game).

After writing code, test your implementation:

python3 ok -q 06

<h2>Problem 7</h2>

Implement the HungryAnt , which will select a random Bee from its place and eat it whole. After eating a Bee , a HungryAnt must spend 3 turns chewing before eating again. If there is no bee available to eat, HungryAnt will do nothing.

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 07 -u

We have not provided you with a class header. Implement the HungryAnt class from scratch. Give it a class attribute name with the value ‘Hungry’ (so that the graphics work) and a class attribute implemented with the value True (so that you can use it in a game).

Hint: When a Bee is eaten, it should lose all its health. Is there an existing function we can call on a Bee that can reduce its health to 0?

<table width="299">

 <tbody>

  <tr>

   <td width="96">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="96">HungryAnt</td>

   <td width="90">4</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

Give HungryAnt a chew_duration class attribute that stores the number of turns that it will take a HungryAnt to chew (set to 3). Also, give each HungryAnt an instance attribute

chew_countdown that counts the number of turns it has left to chew (initialized to 0, since it hasn’t eaten anything at the beginning. You can also think of chew_countdown as the number of turns until a HungryAnt can eat another Bee ).

Implement the action method of the HungryAnt : First, check if it is chewing; if so, decrement its chew_countdown . Otherwise, eat a random Bee in its place by reducing the Bee ‘s health to 0. Make sure to set the chew_countdown when a Bee is eaten!

Hint: Other than the action method, make sure you implement the __init__ method too so the HungryAnt starts o with the appropriate amount of health !

After writing code, test your implementation:

python3 ok -q 07

We now have some great o ensive troops to help vanquish the bees, but let’s make sure we’re also keeping our defensive e orts up. In this phase you will implement ants that have special defensive capabilities such as increased health and the ability to protect other ants.

<a href="https://cs61a.org/articles/pair-programming"> </a><a href="https://cs61a.org/articles/pair-programming">Pair</a> <a href="https://cs61a.org/articles/pair-programming">programming?</a> <a href="https://cs61a.org/articles/pair-programming">(/articles/pair-programming)</a> This would be a good time to switch roles. Switching roles makes sure that you both bene t from the learning experience of being in each role.

<h2>Problem 8</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 08 -u

Right now, our ants are quite frail. We’d like to provide a way to help them last longer against the onslaught of the bees. Enter the BodyguardAnt .

<table width="475">

 <tbody>

  <tr>

   <td width="272">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="272">BodyguardAnt</td>

   <td width="90">4</td>

   <td width="113">2</td>

  </tr>

 </tbody>

</table>

A BodyguardAnt di ers from a normal ant because it is a ContainerAnt ; it can contain another ant and protect it, all in one Place . When a Bee stings the ant in a Place where one ant contains another, only the container is damaged. The ant inside the container can still perform its original action. If the container perishes, the contained ant still remains in the place (and can then be damaged).

Each ContainerAnt has an instance attribute ant_contained that stores the ant it contains.

This ant, ant_contained , initially starts o as None to indicate that there is no ant being stored yet. Implement the store_ant method so that it sets the ContainerAnt ‘s ant_contained instance attribute to the passed in ant argument. Also implement the

ContainerAnt ‘s action method to perform its ant_contained ‘s action if it is currently containing an ant.

In addition, you will need to make the following modi cations throughout your program so that a container and its contained ant can both occupy a place at the same time (a maximum of two ants per place), but only if exactly one is a container:

There is an Ant.can_contain method, but it always returns False . Override the method ContainerAnt.can_contain so that it takes an ant other as an argument and returns True if:

This ContainerAnt does not already contain another ant.

The other ant is not a container.

Modify Ant.add_to to allow a container and a non-container ant to occupy the same place according to the following rules:

If the ant originally occupying a place can contain the ant being added, then both ants occupy the place and original ant contains the ant being added.

If the ant being added can contain the ant originally in the space, then both ants occupy the place and the (container) ant being added contains the original ant.

If neither Ant can contain the other, raise the same AssertionError as before

(the one already present in the starter code).

Important: If there are two ants in a speci c Place , the ant attribute of the

Place instance should refer to the container ant, and the container ant should

contain the non-container ant.

Add a BodyguardAnt.__init__ that sets the initial amount of health for the ant.

Hint: You may nd the is_container attribute that each Ant has useful for checking if a speci c Ant is a container. You should also take advantage of the can_contain method you wrote and avoid repeating code.

The constructor of ContainerAnt.__init__ is implemented as follows:

def <u>__</u><span style="text-decoration: line-through;">i</span>n<span style="text-decoration: line-through;">i</span>t<u>__</u>(self, *args, **kwargs):        super().__init__(*args, **kwargs)        self.ant_contained = None

As we saw in Hog, args is bound to all positional arguments (which are all arguments passed without keywords), and kwargs is bound to all the keyword arguments. This ensures that both sets of arguments are passed to the Ant constructor.

E ectively, this means the constructor is exactly the same as its parent class’s constructor ( Ant.__init__ ) but here we also set self.ant_contained = None .

Once you’ve nished implementing the BodyguardAnt , give it a class attribute implemented with the value True .

After writing code, test your implementation: python3 ok -q 08

<h2>Problem 9 (1 pt)</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 09 -u

The BodyguardAnt provides great defense, but they say the best defense is a good o ense. The TankAnt is a container that protects an ant in its place and also deals 1 damage to all bees in its place each turn.

<table width="475">

 <tbody>

  <tr>

   <td width="272">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="272">TankAnt</td>

   <td width="90">6</td>

   <td width="113">2</td>

  </tr>

 </tbody>

</table>

We have not provided you with a class header. Implement the TankAnt class from scratch. Give it a class attribute name with the value ‘Tank’ (so that the graphics work) and a class attribute implemented with the value True (so that you can use it in a game).

You should not need to modify any code outside of the TankAnt class. If you nd yourself needing to make changes elsewhere, look for a way to write your code for the previous question such that it applies not just to BodyguardAnt and TankAnt objects, but to container ants in general.

After writing code, test your implementation: python3 ok -q 09

<h1>Phase 4: Water and Might</h1>

In the nal phase, you’re going to add one last kick to the game by introducing a new type of place and new ants that are able to occupy this place. One of these ants is the most important ant of them all: the queen of the colony!

<h2>Problem 10 (1 pt)</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 10 -u

Let’s add water to the colony! Currently there are only two types of places, the Hive and a basic Place . To make things more interesting, we’re going to create a new type of Place called Water .

Only an insect that is waterproof can be placed in Water . In order to determine whether an

Insect is waterproof, add a new class attribute to the Insect class named is_waterproof that is set to False . Since bees can y, set their is_waterproof attribute to True , overriding the inherited value.

Now, implement the add_insect method for Water . First, add the insect to the place regardless of whether it is waterproof. Then, if the insect is not waterproof, reduce the insect’s health to 0. Do not repeat code from elsewhere in the program. Instead, use methods that have already been de ned.

After writing code, test your implementation:

python3 ok -q 10

Once you’ve nished this problem, play a game that includes water. To access the wet_layout , which includes water, add the –water option (or -w for short) when you start

the game. python3 gui.py –water

<a href="https://cs61a.org/articles/pair-programming"> </a><a href="https://cs61a.org/articles/pair-programming">Pair</a> <a href="https://cs61a.org/articles/pair-programming">programming?</a> <a href="https://cs61a.org/articles/pair-programming">(/articles/pair-programming)</a> Remember to alternate between driver and navigator roles. The driver controls the keyboard; the navigator watches, asks questions, and suggests ideas.

<h2>Problem 11</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 11 -u

Currently there are no ants that can be placed on Water . Implement the ScubaThrower , which is a subclass of ThrowerAnt that is more costly and waterproof, but otherwise

identical to its base class. A ScubaThrower should not lose its health when placed in Water .

<table width="317">

 <tbody>

  <tr>

   <td width="114">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="114">ScubaThrower</td>

   <td width="90">6</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

We have not provided you with a class header. Implement the ScubaThrower class from scratch. Give it a class attribute name with the value ‘Scuba’ (so that the graphics work) and remember to set the class attribute implemented with the value True (so that you can use it in a game).

After writing code, test your implementation: python3 ok -q 11

<h2>Problem 12 (3 pt)</h2>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q 12 -u

Finally, implement the QueenAnt . The queen is a waterproof ScubaThrower that inspires her fellow ants through her bravery. In addition to the standard ScubaThrower action, the

QueenAnt doubles the damage of all the ants behind her each time she performs an action.

Once an ant’s damage has been doubled, it is not doubled again for subsequent turns.

Note: The re ected damage of a FireAnt should not be doubled, only the extra damage it deals when its health is reduced to 0.

Note: If you downloaded the project early, there were a couple unlock questions that are no longer relevant. Rather than asking you to redownload the project, we will give you the answers to the a ected questions:

Which QueenAnt instance is the true QueenAnt?

The rst QueenAnt that is instantiated

What happens to any QueenAnt instance that is instantiated after the rst one?

Its health is reduced to 0 upon taking its rst action

<table width="299">

 <tbody>

  <tr>

   <td width="96">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="96">QueenAnt</td>

   <td width="90">7</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

However, with great power comes great responsibility. The QueenAnt is governed by three special rules:

If the queen ever has its health reduced to 0, the ants lose. You will need to override

Ant.reduce_health in QueenAnt and call ants_lose() in that case in order to signal to the simulator that the game is over. (The ants also still lose if any bee reaches the end of a tunnel.)

There can be only one queen. A second queen cannot be constructed. To check if an Ant can be constructed, we use the Ant.construct() class method to either construct an Ant if possible, or return None if not. You will need to override Ant.construct as a class method of QueenAnt in order to add this check. To keep track of whether a queen has already been created, you can use an instance variable added to the current GameState .

The queen cannot be removed. Attempts to remove the queen should have no e ect (but should not cause an error). You will need to override Ant.remove_from in QueenAnt to enforce this condition.

python3 ok -q 12

<h2>Extra Credit (2 pt)</h2>

During O     ce Hours and Project Parties, the sta will prioritize helping students with <a href="https://oh.cs61a.org/">required</a> <a href="https://oh.cs61a.org/">questions.</a> <a href="https://oh.cs61a.org/">We</a> <a href="https://oh.cs61a.org/">will</a> <a href="https://oh.cs61a.org/">not</a> <a href="https://oh.cs61a.org/">be</a><a href="https://oh.cs61a.org/"> o</a> <a href="https://oh.cs61a.org/">ering</a> <a href="https://oh.cs61a.org/">help</a> <a href="https://oh.cs61a.org/">with</a> <a href="https://oh.cs61a.org/">this</a> <a href="https://oh.cs61a.org/">question</a> <a href="https://oh.cs61a.org/">unless</a> <a href="https://oh.cs61a.org/">the</a> <a href="https://oh.cs61a.org/">queue (https://oh.cs61a.org/)</a> <a href="https://oh.cs61a.org/">is</a> <a href="https://oh.cs61a.org/">empty.</a>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q EC -u

Implement two nal thrower ants that do zero damage, but instead apply a temporary “status” on the action method of a Bee instance that they throw_at . This “status” lasts for a certain number of turns, after which it ceases to take e ect.

We will be implementing two new ants that inherit from ThrowerAnt .

SlowThrower throws sticky syrup at a bee, slowing it for 3 turns. When a bee is slowed, it can only move on turns when gamestate.time is even, and can do nothing otherwise.

If a bee is hit by syrup while it is already slowed, it is slowed for an additional 3 turns.

ScaryThrower intimidates a nearby bee, causing it to back away instead of advancing. (If

the bee is already right next to the Hive and cannot go back further, it should not move. To check if a bee is next to the Hive, you might nd the is_hive instance attribute of Place s useful). Bees remain scared until they have tried to back away twice. Bees cannot try to back away if they are slowed and gamestate.time is odd.

Once a bee has been scared once, it can’t be scared ever again.

<table width="689">

 <tbody>

  <tr>

   <td width="494">Class</td>

   <td width="86">FoodCost</td>

   <td width="109">InitialHealth</td>

  </tr>

  <tr>

   <td width="494">SlowThrower</td>

   <td width="86">4</td>

   <td width="109">1</td>

  </tr>

  <tr>

   <td width="494">ScaryThrower</td>

   <td width="86">6</td>

   <td width="109">1</td>

  </tr>

 </tbody>

</table>

In order to complete the implementations of these two ants, you will need to set their class attributes appropriately and implement the slow and scare methods on Bee , which apply their respective statuses on a particular bee. You may also have to edit some other methods of Bee .

You can run some provided tests, but they are not exhaustive: python3 ok -q EC

Make sure to test your code! Your code should be able to apply multiple statuses on a target; each new status applies to the current (possibly previously a ected) action method of the bee.

<h1>Optional Problems</h1>

<h2>Optional Problem 1</h2>

During O     ce Hours and Project Parties, the sta will prioritize helping students with <a href="https://oh.cs61a.org/">required</a> <a href="https://oh.cs61a.org/">questions.</a> <a href="https://oh.cs61a.org/">We</a> <a href="https://oh.cs61a.org/">will</a> <a href="https://oh.cs61a.org/">not</a> <a href="https://oh.cs61a.org/">be</a><a href="https://oh.cs61a.org/"> o</a> <a href="https://oh.cs61a.org/">ering</a> <a href="https://oh.cs61a.org/">help</a> <a href="https://oh.cs61a.org/">with</a> <a href="https://oh.cs61a.org/">this</a> <a href="https://oh.cs61a.org/">question</a> <a href="https://oh.cs61a.org/">unless</a> <a href="https://oh.cs61a.org/">the</a> <a href="https://oh.cs61a.org/">queue (https://oh.cs61a.org/)</a> <a href="https://oh.cs61a.org/">is</a> <a href="https://oh.cs61a.org/">empty.</a>

Before writing any code, read the instructions and test your understanding of the problem: python3 ok -q optional1 -u

Implement the NinjaAnt , which damages all Bee s that pass by, but can never be stung.

<table width="299">

 <tbody>

  <tr>

   <td width="96">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="96">NinjaAnt</td>

   <td width="90">5</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

A NinjaAnt does not block the path of a Bee that ies by. To implement this behavior, rst modify the Ant class to include a new class attribute blocks_path that is set to True , then override the value of blocks_path to False in the NinjaAnt class.

Second, modify the Bee ‘s method blocked to return False if either there is no Ant in the

Bee ‘s place or if there is an Ant , but its blocks_path attribute is False . Now Bee s will just y past NinjaAnt s.

Finally, we want to make the NinjaAnt damage all Bee s that y past. Implement the action method in NinjaAnt to reduce the health of all Bee s in the same place as the NinjaAnt by its damage attribute. Similar to the FireAnt , you must iterate over a potentially changing list of bees.

After writing code, test your implementation: python3 ok -q optional1

For a challenge, try to win a game using only HarvesterAnt and NinjaAnt .

<h2>Optional Problem 2</h2>

During O     ce Hours and Project Parties, the sta will prioritize helping students with <a href="https://oh.cs61a.org/">required</a> <a href="https://oh.cs61a.org/">questions.</a> <a href="https://oh.cs61a.org/">We</a> <a href="https://oh.cs61a.org/">will</a> <a href="https://oh.cs61a.org/">not</a> <a href="https://oh.cs61a.org/">be</a><a href="https://oh.cs61a.org/"> o</a> <a href="https://oh.cs61a.org/">ering</a> <a href="https://oh.cs61a.org/">help</a> <a href="https://oh.cs61a.org/">with</a> <a href="https://oh.cs61a.org/">this</a> <a href="https://oh.cs61a.org/">question</a> <a href="https://oh.cs61a.org/">unless</a> <a href="https://oh.cs61a.org/">the</a> <a href="https://oh.cs61a.org/">queue (https://oh.cs61a.org/)</a> <a href="https://oh.cs61a.org/">is</a> <a href="https://oh.cs61a.org/">empty.</a>

We’ve been developing this ant for a long time in secret. It’s so dangerous that we had to lock it in the super hidden CS61A underground vault, but we nally think it is ready to go out on the eld. In this problem, you’ll be implementing the nal ant — LaserAnt , a ThrowerAnt with a twist.

<table width="475">

 <tbody>

  <tr>

   <td width="272">Class</td>

   <td width="90">Food Cost</td>

   <td width="113">Initial Health</td>

  </tr>

  <tr>

   <td width="272">LaserAnt</td>

   <td width="90">10</td>

   <td width="113">1</td>

  </tr>

 </tbody>

</table>

The LaserAnt shoots out a powerful laser, damaging all that dare to stand in its path. Both

Bee s and Ant s, of all types, are at risk of being damaged by LaserAnt . When a LaserAnt takes its action, it will damage all Insect s in its place (excluding itself, but including its container if it has one) and the Place s in front of it, excluding the Hive .

If that were it, LaserAnt would be too powerful for us to contain. The LaserAnt has a base damage of 2 . But, LaserAnt ‘s laser comes with some quirks. The laser is weakened by 0.25 each place it travels away from LaserAnt ‘s place. Additionally, LaserAnt has limited battery.

Each time LaserAnt actually damages an Insect its laser’s total damage goes down by

0.0625 (1/16). If LaserAnt ‘s damage becomes negative due to these restrictions, it simply does 0 damage instead.

In order to complete the implementation of this ultimate ant, read through the LaserAnt class, set the class attributes appropriately, and implement the following two functions: insects_in_front is an instance method, called by the action method, that returns a

dictionary where each key is an Insect and each corresponding value is the distance

(in places) that that Insect is away from LaserAnt . The dictionary should include all Insects on the same place or in front of the LaserAnt , excluding LaserAnt itself. calculate_damage is an instance method that takes in distance , the distance that an

insect is away from the LaserAnt instance. It returns the damage that the LaserAnt instance should a      ict based on:

The distance away from the LaserAnt instance that an Insect is.

The number of Insects that this LaserAnt has damaged, stored in the insects_shot instance attribute.

In addition to implementing the methods above, you may need to modify, add, or use class or instance attributes in the LaserAnt class as needed.

You can run the provided test, but it is not exhaustive: python3 ok -q optional2

Make sure to test your code!


