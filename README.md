/*Virtual-Gully-Cricket-Application by Akanksha*/
/*The project “GULLY CRICKET APPLICATION” provides an interactive platform to the individuals to capture the essence of a simple game of cricket comprising two innings, each consisting of six balls. This project revolves around those school days when we used books to play cricket to pass time during boring lectures. The project “Gully Cricket Application” is developed with the same nostalgia where we would create a similar cricket game using the concepts of object-oriented programming of C++, which we learned throughout the journey of our course .*/
/*Implementation

*1. There should be two teams: TeamA and TeamB*
>-  Each team will have 4 players.
>-   The players for each team will be selected by the user from the given pool of 11 players. You can assign the names to those 11 players yourself.
>-   The sequence in which the players are selected for each team will decide the batting and bowling sequence for that team. For example, if Team A has Virat, Rohit, Dhawan, and Rahul; Virat will be the first player to bat or ball.

*2. There should be two innings* 
>-   Each innings will be of 6 balls.
>-   In each innings, only one bowler from the bowling team will bowl all the 6 deliveries and at a time one batsman from the batting team will bat until he is declared out.
>-   The first player from the bowling team will be always selected to bowl first and the first player from the batting team will be always selected to bat first.
>-   When a batsman is OUT, the next batsman from the batting team in sequence will start batting.
    
*3. There should be a toss functionality*
>-   The team who wins the toss will decide to either bat or bowl first
      
*4. In each delivery, possible runs can be scored from 0 to 6*

*5. OUT criteria:* 
>-   If a batsman scores 0 runs he will be declared OUT and the next batsman in sequence will start batting
      
*6. Match End criteria* 
>-    If runs scored by TeamA is greater than the opponent TeamB, then TeamA will win the game or vice-versa.
>-    If runs scored by TeamA and TeamB are the same then the match will draw.
*/
#include <bits/stdc++.h>
using namespace std;

class cric
{

public:
    string name;
    int id;
    int runsScored;
    int ballsPlayed;
    int ballsBowled;
    int runsGiven;
    int wicketsTaken;

    // Constructor for cric class
    cric()
    {
        runsScored = 0;
        ballsPlayed = 0;
        ballsBowled = 0;
        runsGiven = 0;
        wicketsTaken = 0;
    }
};

class group
{
public:
    string name;
    int totalRunsScored;
    int wicketsLost;
    int totalBallsBowled;
    vector<cric> players;
    
    // Constructor for group class
    group()
    {
        totalRunsScored = 0;
        wicketsLost = 0;
        totalBallsBowled = 0;
    }
};

class Game
{

public:
    Game();
    int playersPerTeam;
    int maxBalls;
    int totalPlayers;
    string players[22];

    bool isFirstInnings;
    group teamA, teamB;
    group *battingTeam, *bowlingTeam;
    cric *batsman, *bowler;

    void welcome();
    void showAllPlayers();
    int takeIntegerInput();
    void selectPlayers();
    bool validateSelectedPlayer(int);
    void showTeamPlayers();
    void toss();
    void tossChoice(group);
    void startFirstInnings();
    void initializePlayers();
    void playInnings();
    void bat();
    bool validateInningsScore();
    void showGameScorecard();
    void startSecondInnings();
    void showMatchSummary();
};

void Game::welcome()
{

    cout << "----------------------------------------------" << endl;
    cout << "|============== GULLY-CRICKET ===============|" << endl;
    cout << "|                                            |" << endl;
    cout << "|   Welcome to Akanksha's GULLY CRICKET..!   |" << endl;
    cout << "----------------------------------------------" << endl;

    cout << endl
         << endl;
    cout << "----------------------------------------------------" << endl;
    cout << "|================== Instructions ==================|" << endl;
    cout << "----------------------------------------------------" << endl;
    cout << "|                                                  |" << endl;
    cout << "| 1. Create 2 teams (Team-A and Team-B with 4      |" << endl;
    cout << "|    players each) from a given pool of 22 players.|" << endl;
    cout << "| 2. Lead the toss and decide the choice of play.  |" << endl;
    cout << "| 3. Each innings will be of 6 balls.              |" << endl;
    cout << "| 4. This is a 1 over match and each over          |" << endl;
    cout << "|  contains 6 balls                                |" << endl;
    cout << "----------------------------------------------------" << endl;
}

Game::Game()
{

    playersPerTeam = 4;
    maxBalls = 6;
    totalPlayers = 22;

    players[0] = "Virat";
    players[1] = "Rohit";
    players[2] = "Dhawan";
    players[3] = "Pant";
    players[4] = "Karthik";
    players[5] = "KLRahul";
    players[6] = "Jadeja";
    players[7] = "Hardik";
    players[8] = "Bumrah";
    players[9] = "BKumar";
    players[10] = "Mdhoni";
    players[11] = "Rahul";
    players[12] = "Sourav";
    players[13] = "Ravishankar";
    players[14] = "Sachin";
    players[15] = "Kapil";
    players[16] = "Anil";
    players[17] = "Zaheer";
    players[18] = "Harbhajan";
    players[19] = "Yuvraj";
    players[20] = "Imran";
    players[21] = "Brian";
    isFirstInnings = false;
    teamA.name = "Team-A";
    teamB.name = "Team-B";
}

void Game::showAllPlayers()
{

    cout << endl;
    cout << "---------------------------------------" << endl;
    cout << "|========== Pool of Players ==========|" << endl;
    cout << "---------------------------------------" << endl;
    cout << endl;

    for (int i = 0; i < totalPlayers; i++)
    {
        cout << "\t\t[" << i << "] " << players[i] << endl;
    }
}

int Game::takeIntegerInput()
{
    int n;

    while (!(cin >> n))
    {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Invalid input! Please try again with valid input: ";
    } 
    return n;
}

bool Game::validateSelectedPlayer(int index)
{
    int n;
    vector<cric> players;

    players = teamA.players;
    n = players.size();
    for (int i = 0; i < n; i++)
    {
        if (players[i].id == index)
        {
            return false;
        }
    }

    players = teamB.players;
    n = players.size();
    for (int i = 0; i < n; i++)
    {
        if (players[i].id == index)
        {
            return false;
        }
    }
    return true;
}

void Game::selectPlayers()
{

    cout << endl;
    cout << "------------------------------------------------" << endl;
    cout << "|========== Create Team-A and Team-B ==========|" << endl;
    cout << "------------------------------------------------" << endl;

    for (int i = 0; i < playersPerTeam; i++)
    {

    // Add player to team A
    teamASelection:
        cout << endl << "Select player " << i + 1 << " of Team A (-: only numbers :-) - ";
        int playerIndexTeamA = takeIntegerInput();

        if (playerIndexTeamA < 0 || playerIndexTeamA > 22)
        {
            cout << endl
                 << "Please select from the given players" << endl;
            goto teamASelection;
        }
        else if (!validateSelectedPlayer(playerIndexTeamA))
        {
            cout << endl << "Player has been already selected. Please select other player" << endl;
            goto teamASelection;
        }
        else
        {
            cric teamAPlayer;
            teamAPlayer.id = playerIndexTeamA;
            teamAPlayer.name = players[playerIndexTeamA];
            teamA.players.push_back(teamAPlayer);
        }

    // Add player to team B
    teamBSelection:
        cout << endl << "Select player " << i + 1 << " of Team B (-: only numbers :-) -";
        int playerIndexTeamB = takeIntegerInput();

        if (playerIndexTeamB < 0 || playerIndexTeamB > 22)
        {
            cout << endl << "Please select from the given players" << endl;
            goto teamBSelection;
        }
        else if (!validateSelectedPlayer(playerIndexTeamB))
        {
            cout << endl << "Player has been already selected. Please select other player" << endl;
            goto teamBSelection;
        }
        else
        {
            cric teamBPlayer;
            teamBPlayer.name = players[playerIndexTeamB];
            teamB.players.push_back(teamBPlayer);
        }
    }
}

void Game::showTeamPlayers()
{

    vector<cric> teamAPlayers = teamA.players;
    vector<cric> teamBPlayers = teamB.players;

    cout << endl << endl;
    cout << "--------------------------\t\t--------------------------" << endl;
    cout << "|=======  Team-A  =======|\t\t|=======  Team-B  =======|" << endl;
    cout << "--------------------------\t\t--------------------------" << endl;

    for (int i = 0; i < playersPerTeam; i++)
    {
        cout << "|\t" << "[" << i << "] " << teamAPlayers[i].name << "\t |"
             << "\t\t" << "|\t" << "[" << i << "] " << teamBPlayers[i].name << "\t |" << endl;
    }
    cout << "--------------------------\t\t--------------------------" << endl << endl;
}

void Game::toss()
{

    cout << endl;
    cout << "-----------------------------------" << endl;
    cout << "|========== Let's Toss  ==========|" << endl;
    cout << "-----------------------------------" << endl << endl;

    cout << "Tossing the coin..." << endl << endl;

    srand(time(NULL));
    int randomValue = rand() % 2; // 0 or 1

    switch (randomValue)
    {
    case 0:
        cout << "Team-A won's the toss!!!!!!!!" << endl << endl;
        tossChoice(teamA);
        break;
    case 1:
        cout << "Team-B won's the toss!!!!!!!!" << endl << endl;
        tossChoice(teamB);
        break;
    }
}

void Game::tossChoice(group tossWinnerTeam)
{

    cout << "Enter 1 to bat or 2 to bowl first. " << endl << "1. Bat" << endl
         << "2. Bowl " << endl;

    int tossInput = takeIntegerInput();

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    switch (tossInput)
    {
    case 1:
        cout << endl << tossWinnerTeam.name << " won the toss and elected to bat first" << endl
             << endl;

        if (tossWinnerTeam.name.compare("Team-A") == 0)
        { // if Team-A is the toss winner
            battingTeam = &teamA;
            bowlingTeam = &teamB;
        }
        else
        { // else Team-B is the toss winner
            battingTeam = &teamB;
            bowlingTeam = &teamA;
        }

        break;
    case 2:
        cout << endl
             << tossWinnerTeam.name << " won the toss and choose to bowl first" << endl
             << endl;

        if (tossWinnerTeam.name.compare("Team-A") == 0)
        { // if Team-A is the toss winner
            bowlingTeam = &teamA;
            battingTeam = &teamB;
        }
        else
        { // else Team-B is the toss winner
            bowlingTeam = &teamB;
            battingTeam = &teamA;
        }

        break;
    default:
        cout << endl
             << "Invalid input. Enter a valid input Please try again:" << endl
             << endl;
        tossChoice(tossWinnerTeam);
        break;
    }
}

void Game::startFirstInnings()
{
    cout << "\t\t ||| FIRST INNINGS STARTS ||| " << endl
         << endl;

    isFirstInnings = true;

    initializePlayers();
    playInnings();
}

void Game::startSecondInnings()
{
    cout << "\t\t ||| SECOND INNINGS STARTS ||| " << endl
         << endl;

    isFirstInnings = false;

    // Swap battingTeam and bowlingTeam pointers
    group tempTeam = *battingTeam;
    *battingTeam = *bowlingTeam;
    *bowlingTeam = tempTeam;

    // Select the batsman and the bowler for 2nd Innings
    initializePlayers();
    playInnings();
}

void Game::initializePlayers()
{
    // Choose batsman and bowler: Initialize *batsman and *bowler
    batsman = &battingTeam->players[0];
    bowler = &bowlingTeam->players[0];

    cout << battingTeam->name << " - " << batsman->name << " is batting " << endl;
    cout << bowlingTeam->name << " - " << bowler->name << " is bowling " << endl
         << endl;
}

void Game::playInnings()
{
    for (int i = 0; i < maxBalls; i++)
    {
        cout << "Press Enter to bowl...";
        getchar();
        cout << "Bowling.............." << endl;

        bat();

        if (!validateInningsScore())
        {
            break;
        }
    }
}

void Game::bat()
{
    srand(time(NULL));
    int runsScored = rand() % 6; // u increse this count

    // Update batting team and batsman score
    batsman->runsScored = batsman->runsScored + runsScored;
    battingTeam->totalRunsScored = battingTeam->totalRunsScored + runsScored;
    batsman->ballsPlayed = batsman->ballsPlayed + 1;

    // Update bowling team and bowler score
    bowler->ballsBowled = bowler->ballsBowled + 1;
    bowlingTeam->totalBallsBowled = bowlingTeam->totalBallsBowled + 1;
    bowler->runsGiven = bowler->runsGiven + runsScored;

    if (runsScored != 0)
    { // if runsScored = 1, 2, 3, 4, 5, or 6
        cout << endl
             << bowler->name << " to " << batsman->name << " " << runsScored << " runs!" << endl
             << endl;
        showGameScorecard();
    }
    else
    { // else runScored = 0 and the batsman is â€˜OUTâ€™
        cout << endl
             << bowler->name << " to " << batsman->name << " OUT!" << endl
             << endl;

        battingTeam->wicketsLost = battingTeam->wicketsLost + 1;
        bowler->wicketsTaken = bowler->wicketsTaken + 1;

        showGameScorecard();

        int nextPlayerIndex = battingTeam->wicketsLost;
        batsman = &battingTeam->players[nextPlayerIndex];
    }
}

bool Game::validateInningsScore()
{

    if (isFirstInnings)
    {
        if (battingTeam->wicketsLost == playersPerTeam || bowlingTeam->totalBallsBowled == maxBalls)
        {
            cout << "\t\t ||| FIRST INNINGS ENDS ||| " << endl << endl;

            cout << battingTeam->name << " " << battingTeam->totalRunsScored << " - "
                 << battingTeam->wicketsLost << " (" << bowlingTeam->totalBallsBowled
                 << ")" << endl;

            cout << bowlingTeam->name << " needs " << battingTeam->totalRunsScored + 1
                 << " runs to win the match" << endl << endl;

            return false;
        }
    }
    else
    { // Else 2nd innings

        if (battingTeam->totalRunsScored > bowlingTeam->totalRunsScored)
        { // Case1: If batting team score > bowling team score
            cout << battingTeam->name << " WON THE MATCH" << endl
                 << endl;
            return false;
            // Case2: Batting team is all OUT OR Bowling team is done bowling
        }
        else if (battingTeam->wicketsLost == playersPerTeam || bowlingTeam->totalBallsBowled == maxBalls)
        {
            if (battingTeam->totalRunsScored < bowlingTeam->totalRunsScored)
            {
                cout << bowlingTeam->name << " WON THE MATCH" << endl
                     << endl;
            }
            else
            {
                cout << "MATCH DRAW" << endl
                     << endl;
            }
            return false;
        }
    }

    return true;
}

int main()
{
    Game game;
    game.welcome();

    cout << "\nPress Enter to continue";
    getchar();

    game.showAllPlayers();

    cout << "\nPress Enter to continue";
    getchar();

    game.selectPlayers();
    game.showTeamPlayers();

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "\nPress Enter to toss";
    getchar();

    game.toss();
    game.startFirstInnings();
    game.startSecondInnings();
    game.showMatchSummary();

    return 0;
}

void Game::showGameScorecard()
{

    cout << "--------------------------------------------------------------------------" << endl;

    cout << "\t" << battingTeam->name << "  " << battingTeam->totalRunsScored << " - "
         << battingTeam->wicketsLost << " (" << bowlingTeam->totalBallsBowled << ") | " << batsman->name
         << " " << batsman->runsScored << " (" << batsman->ballsPlayed << ") \t" << bowler->name << " "
         << bowler->ballsBowled << " - " << bowler->runsGiven << " - " << bowler->wicketsTaken << "\t" << endl;

    cout << "--------------------------------------------------------------------------" << endl
         << endl;
}

void Game::showMatchSummary()
{

    cout << "\t\t ||| MATCH ENDS ||| " << endl
         << endl;

    cout << battingTeam->name << " " << battingTeam->totalRunsScored << "-" << battingTeam->wicketsLost << " (" << bowlingTeam->totalBallsBowled << ")" << endl;

    cout << "==========================================" << endl;
    cout << "| PLAYER \t BATTING \t BOWLING |" << endl;

    for (int j = 0; j < playersPerTeam; j++)
    {
        cric cric = battingTeam->players[j];
        cout << "|----------------------------------------|" << endl;
        cout << "| "
             << "[" << j << "] " << cric.name << "  \t "
             << cric.runsScored << "(" << cric.ballsPlayed << ") \t\t "
             << cric.ballsBowled << "-" << cric.runsGiven << "-"
             << cric.wicketsTaken << "\t |" << endl;
    }
    cout << "==========================================" << endl << endl;

    cout << bowlingTeam->name << " " << bowlingTeam->totalRunsScored << "-" << bowlingTeam->wicketsLost << " (" << battingTeam->totalBallsBowled << ")" << endl;

    cout << "==========================================" << endl;
    cout << "| PLAYER \t BATTING \t BOWLING |" << endl;

    for (int i = 0; i < playersPerTeam; i++)
    {
        cric cric = bowlingTeam->players[i];
        cout << "|----------------------------------------|" << endl;
        cout << "| "
             << "[" << i << "] " << cric.name << "  \t "
             << cric.runsScored << "(" << cric.ballsPlayed << ") \t\t "
             << cric.ballsBowled << "-" << cric.runsGiven << "-"
             << cric.wicketsTaken << "\t |" << endl;
    }
    cout << "==========================================" << endl << endl;
}
