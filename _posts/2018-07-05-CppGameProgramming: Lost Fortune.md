### Lost Fortune Game

* This is simple text based adventure game in c++.

* The algorithm is as follows;

1. Create a constant for the number of gold pieces.
2. Get input from the user for the number of adventurers in the game. Store this in the adventurers variable.
3. Get input from the user for the number
4. Ask the user to enter a number for the amount of adventures killed.
5. Get the number of survivors by subtracting the amount of killed from the total adventures in the game.
6. Get the lastname of the leader.
7. The display the full story of the adventure.


* Constant for the number of gold pieces.

```
const int GOLD_PIECES = 900; 
```

* Input for the number of adventures in the story.

```
cout << "Enter a number: ";
cin >> adventures;
```

* Enter in the number of the adventures killed.

```
cout << "Enter a number, smaller than the first: ";
cin >> killed;
```

* Calculate the number of adventurers killed.

```
survivors = adventures - killed;
```


* Enter in the lastname of leader.

```
cout << "Enter your last name: ";
cin >> leader;
```

* Now display the story.

```
cout << "\nA brave group of " << adventures << " set out on a quest ";
cout << "- in search of the lost treasure of the Ancient Dwarves. ";
cout << "The group was lead by that legendary rouge. " << leader << ".\n";
cout << "\nAlong the way a band of marauding ogres ambushed the party. ";
cout << "All fought bravely under the command of " << leader;
cout << ".and the ogres were defeated. but at a cost.";
cout << "Of the adventures. " << killed << " were vanquished. ";
cout << "leaving just " << survivors << " in the group.\n";
cout << "\nThe party was about to give up all hope. ";
cout << "But while laying the deceased to rest. ";
cout << "They stumbled upon the buried fortune. ";
cout << "So the adventurers spilt " << GOLD_PIECES << " gold pieces. ";                                                
cout << leader << " held on to the extra " << (GOLD_PIECES % survivors);
cout << " pieces to keep things fair of course.\n";
```


