# Log
**Contributors: Eric Young, Yuqi You, Jiashu Zhang**
### Introduction
<p>Everybody likes games because they can help you relax from a tough day of study and
work. Therefore, we decided to create our own fun little game to reach this goal. We
had a bunch of ideas to begin with and finally stopped at the Minesweeper game which
is one of the most traditional games in video game history. Isn't it true that this is the
first-ever video game that you have played? We had a great time playing it and learned
techniques that could help us win the game quicker and better. However, we can not
compete with our friends and this made the game much less fun. An idea was born!
This time we dive into the step-by-step implementation detail and make it an online
competitive game for our Android users so they can have more fun in the Minesweeper
game with their families and friends.</p>

<p>In our Minesweeper game, the user will be presented with a home screen with three
buttons, "Start Game," "Leaderboard," and "settings." By clicking Start Game, a 5 * 5
grid or a 10 * 10 grid minesweeper game will be generated based on the level of
difficulty. There are two leaderboards (easy and hard and they will be stored externally
in Firebase Realtime Database [1]) displaying the ranked shortest times and nicknames
of users in this app; In the settings, users can adjust the difficulty of the current game as
well as setting different themes of the background(default or dark mode). Once the user
starts the game, on the top left there is a flag label that’s the same amount as the
number of mines and can be used by the user to mark the number of mines left; On the
top right is a time to keep track of the user record. The game is lost as the user clicked
on the cell with mine without flagging it, and wins when the user successfully clicks on
all the cells without mines. Users can submit their record to our global leaderboard once
he/she wins the game. Also at any stage of the game, the player can restart the game if
they feel like he/she has encountered a problem in solving the puzzle.</p>

### Design Process and Intended Goals
<p>After visualizing the core functionalities of the app, we moved on to the game design
stage. We started by defining what are the key terms of our app: game board, score,
mines, flag, counts, buttons, cells, setting, difficulty, timer, customized field, leaderboard,
and home screen of course. Then, we worked as a team to refactor some of the terms
as they belong to one area of the functionality. For example, the Game class has many
items such as a game board, score, mines, flags, and so on. By refactoring, we made
our first draft of the UML Class Diagram [2] which has four classes: Player, Game, Level
n, and Leader board. They have a relationship such that the Player could start as many
games as he or she prefers and the record of the game will both record on his ID as well as on the leaderboard if he or she scores well enough. We initially wished to have a
player class that could keep track of its data such as name, id, best scores, and wins.
We later realized that this adds on unnecessary detail that could complicate the game.
Players might simply wish to have a quick game on their device in their free time just
like they simply open the Minesweeper game on their computer. We abandoned the
player authentication requirement in this case as well as the Player class. Therefore,
users can have their unique in-game id every time they wish to submit their record.
Also, in our first sense, we hope to have a level system that could record different levels
the player is currently at. However, this dramatically decreases the effectiveness and
authority of the leaderboard as we don’t want to be “No.1 in level 4” which is stupid and
meaningless.</p>

<p>Without making the change that’s mentioned above yet, we created several Use Cases
[3] that define three major functionalities: Start Game, Settings, and Leaderboard. In
Start Game, the player could click to open grids, and then after they clicked the flag icon
on the top left, they could flag certain cells to indicate there are mines under cells. There
are certainly winning and losing game states that are triggered by either the player
clicking all cells without mines under them or clicking a cell that has mine under it
without switching to the flag mode. In the end, the player can choose to restart the
game(we later decided the restart button should be always displayed as the player
might wish to terminate their current games if they don’t feel like playing and we respect
the ‘give up’ of the game).In Settings, users can either change the game difficulties from
easy and hard or customize their preferred level layout such as 2 * 8, 3 * 9. However,
this functionality is not ideal after we rigorously tested it out as some default
arrangement of the game like 2 * 8 will hide a great amount of game information such
as the number indicating the mines around a cell, not to mention it further decreases the
effectiveness of our Leaderboard by having plenty of different standards to compare.
We didn’t include it in our development as a result. In the leaderboard, the user could
review the leaderboard of the best players.</p>

<p>Then, we further refine the logic by recreating the Use Cases to have a solid
understanding of the transition between different classes. For example, The user clicks
the flag tool icon on the upper left corner to enter flag mode; The user submits his/her
record to the leaderboard if he/she successfully finishes the game. (Depending on the
difficulty of the game the user completes, the record will be submitted to the “easy
game” database table or the “hard game” database table. Also, if the submitted player
name already exists in the table, the record will be updated; otherwise, a new record is
created.); The user switches between the “easy” leaderboard and the “hard”
leaderboard with the navigation buttons at the bottom of the leaderboard page.
Our user cases give us a general idea of how to get started on the implementation part
in the next section.</p>

### Implementation
<p>With the blueprint of the app, we started implementing the functionalities. We adopted a
development practice that resembles agile. The development was separated into 4
checkpoints, which were similar to the sprints in agile. At the beginning of each sprint,
we held a meeting to review our work in the previous sprint and make a plan for the
upcoming sprint. While the plan focuses on the upcoming sprint, we also considered
work after that sprint as we were evaluating all of the features to be implemented (i.e., a
backlog). Nevertheless, it is noticed that the development strategy we used was not
completely the same as agile because our schedule was so hectic that we did not have
time for a retrospective at the end of each sprint. Yet we somehow combined the
retrospective into the meeting at the beginning of each sprint.</p>

<p>In development checkpoint 1, we implemented the main menu and the basic game
functionality because the first is the landing page and the second is the most important
part of the app. For the major part of the game interface, we use RecyclerView [5] as
the grids of cells are a large set of instances with uniform attributes. Meanwhile, we
initially made a score widget and a mine widget. The score widget counts the cell
opened and the mine widget counts the number of bombs that are not flagged.
However, we later realized the problem of this design. Firstly, displaying the score is
meaningless, because every player who successfully finishes the game gets the same
score. Secondly, displaying the number of mines allows the user to flag every grid to
probe for a mine, which challenges the notion of the game itself: the user only relies on
the number displayed on the revealed cells to speculate the location of a mine.
Therefore, the score widget and the mine widget were later replaced by a flag widget
and a timer in development checkpoint 2. Additionally, we made a shell of the
leaderboard. In the leaderboard layout, we use Fragments [6], which enables the switch
between leaderboards for different game difficulties.</p>

<p>In development checkpoint 2, we set up a Firebase Realtime Database and
implemented the CRUD interactions between the database and the app.
It is noticed that the relational database model we built in the design phase was adapted
for the Firebase Realtime Database since Firebase stores data in a JSON tree [7]. We
also encountered a concurrency problem. While we implemented ViewModel and
Repository to separate concerns of activity and data [8], the notifyDataSetChanged()
RecyclerViewAdapter is not invoked in the Repository, the very first place where data
change can occur. Without the layers of ViewModel and Repository, reflecting remote
data to the local (i.e., the RecyclerView) is faster despite the small latency between the
Firebase thread and the main thread. However, the implementation of ViewModel and
Repository greatly increases the latency between retrieving data from Firebase and
loading the data in the RecyclerView. This resulted in a problem that the leaderboard
was blank the first time a user checked it. The user had to leave the leaderboard page
and re-entered it so that the RecyclerView is informed of the arrival of the livedata. This
would lower user experience so we had to fix it. Eventually, we found the solution after
spending enormous time on the problem. While we expected the change of the livedata set during the Firebase listener query would be shortly observed by the RecyclerView,
this did not happen. In order to realize instant notification, we have the livedata invoked
setValue() [9] immediately after the livedata set is loaded so that the change is forced to
be dispatched to active observers at the moment we expect. Although we are not sure
whether this is an elegant approach, this is the most effective solution we could obtain
given the time constraint.</p>

<p>In development checkpoint 3, we finished the activities for both leaderboards and
improved the functionalities of the game, including flagging, time counting, and visual
elements of the cells. The tasks were easier compared to previous checkpoints since
we had become much more familiar with the implementation.</p>

<p>In the development checkpoint 4, we enabled disk persistence of Firebase Realtime
Database to ensure the app can still handle the player records under network failure
[10]. The cache is also helpful to the app performance, one of the two non-functional
requirements we addressed. Even while online, the app uses local cache before it tries
to query the database, hence less burden on the CPU to handle app-database
interactions [12]. For the other non-functional requirement, we added an option to use
the app with a dark theme [11]. With the dark theme, the user’s eyes can be protected
from the strong light of the screen if they use the app in a dark environment. We also
added a few ViewModel and landscape layouts to ensure that the app works upon
screen rotation. Lastly, we performed a few instrumented UI tests with Espresso [13]
and ensured the test cases were passed.</p>

### Suggested Design Improvements
<p>There is a contradiction in the game. Any player can delete the records of other
players in the Leaderboard Fragments. At the same time, after completing the game,
players can input their name at will to overwrite the records of other players. This is
obviously not a reasonable setting, and this problem can be solved by setting the user
login function. Players are supposed to log in to their account before entering the game
and can only change or delete the results connected with their account in the
leaderboard.</p>

<p>In addition, the leaderboard database in firebase also has changeable parts. The
current mode to store players’ results is to pack and store the nickname entered by the
player after the game with the current game result in a new location of the database.
When the player gets better results but uses the same nickname, the old results will be
directly overwritten. However, the fact is that some players can often get many excellent
results, Occupying the top of the list (for example, Wilt Chamberlain occupies five
places in the top ten of the NBA single game scoring record). We can save all the
player's results by adding the player's new result in the time part of each group of player
data, rather than only the player's best result.</p>

<p>As for the difficulty preset of mine sweeping game, our initial idea is to provide users
with three different preset difficulties: Easy - 5 mines in a 5 * 5 grid, Medium - 10 mines in a 10 * 10 grid, and Hard - 40 mines in a 20 * 20 grid, and to provide a customize
mode that allows the user to set the number of mines and the size of the game (the
maximum number of mines can be set according to the size of the gameboard).
However, due to the conflict of time, we deleted the hard and customized modes. It is
impossible to set an effective leaderboard for a custom size game. We can follow the
design principles of the easy and medium modes to make more styles of game boards
and set leaderboards for these fixed shape game boards.</p>

<p>Another part of the game interface that can be improved is that the complete 10 * 10
game boards cannot be displayed in one screen under landscape mode. It would be a
good solution to implement a double-finger zoom function, so that players can reduce or
expand the display of the game no matter if it’s portrait or landscape mode. When this is
achieved, players can get a better experience even if it’s a larger game board.
Regarding the design of preset game modes, market research can be used to
investigate what the most popular game modes are.</p>

<p>There are still options to improve the game content. We can design a new function
for players to play the game more efficiently. According to the rules of the game, when
the player marks a sufficient number of grids around a non-mine grid by using the flag
mode (for example, a grid displays the number 3, and the player has marked three of
the eight grids around it with flags), the player can directly open other unmarked grids
around it by clicking the specific grid. This is a way to speed up the game and improve
the player's experience by reducing inefficient game time. However, players must also
use this function carefully, because if the wrong grid is marked, this method will
automatically open a grid with mine, then the game is over.</p>

### References
[1] [Firebase](https://firebase.google.com/)<br>
[2] [UML Class Diagram](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/uml-class-diagram-tutorial/)<br>
[3] [Use Cases](https://www.wrike.com/blog/what-is-a-use-case/)<br>
[4] [Cache](https://en.wikipedia.org/wiki/Cache_(computing))<br>
[5] [RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview)<br>
[6] [Fragments](https://developer.android.com/guide/fragments)<br>
[7] [Denormalizing Firebase](https://firebase.blog/posts/2013/04/denormalizing-your-data-is-normal)<br>
[8] [App Architecture](https://developer.android.com/jetpack/guide)<br>
[9] [Livedata setValue()](https://developer.android.com/reference/android/arch/lifecycle/LiveData#setValue(T))<br>
[10] [Firebase Realtime Database Offline Capabilities](https://firebase.google.com/docs/database/android/offline-capabilities)<br>
[11] [Night Mode](https://developer.android.com/reference/androidx/appcompat/app/AppCompatDelegate#setDefaultNightMode(int))<br>
[12] [Android Studio Profiler](https://developer.android.com/studio/profile)<br>
[13] [Espresso](https://developer.android.com/training/testing/espresso)