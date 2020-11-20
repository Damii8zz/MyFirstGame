// # MyFirstGame
// First programming game I did in a class exam.
/****************************************************************************
*						FINAL BATTLE
*
*					Student name: Damià Fillat Fernández
* 
*****************************************************************************/



#include <stdio.h>		// Required for function(s): printf(); scanf_s(); getchar(); 
#include <stdlib.h>		// Required for function(s): rand(); srand()
#include <time.h>		// Required for function(s): time();

void DrawTitle(void);
void DrawGargole(void);
void DrawCrazyRat(void);
void DrawSkeleton(void);
void DrawPlayerEnemyLife(int pLife, int eLife);

int main()
{
	// Game variables declaration
	int playerLife = 50;
	int playerAttack = 0;
	int playerAction = 0;
	int playerEscape = 0;
	int enemyLife = 0;
	int enemyType = -1;
	int enemyAttack = 0;
	int totalGold = 0;
	int enemyGold = 0;
	char startKey = ' ';
    char playerName[32] = { 0 };

	// Basic game logic
	
	// Random seed based on current time.
	
	srand(time(NULL));

	DrawTitle(); 	// Draws main title
    
    //  Ask user for player name and store it in playerName 

	printf("\nIntroduce your player name: ");
	scanf_s("%s", playerName, 32);

	printf("\nSo your player name is %s! Interesting...\n\n", playerName);

	printf("You are lost in the woods, the most horrible creatures are chasing you...\n");
	printf("Something is approaching, you can feel it, it's time to do your best!\n");

	printf("\nIntroduce X to exit or any other key to START: ");
	scanf_s(" %c", &startKey, 1);

	// Check if X or any other key has been pressed
	if (startKey == 'X')
	{
		printf("\n\nBye!");
	}
	while (startKey != 'X')
	{
		// At this point starts a game LOOP
		playerEscape = 0;		// Initialize playerEscape for the loop

		// Get a random enemy type (Enemy type could a number between 0 and 2)
		
		enemyType = (rand() % 3);

		// Depending on enemyType, draw the right enemy
		// display the right message and set enemyLife 
		// enemyType = 0  -->  enemyLife random between [20..30]
		// enemyType = 1  -->  enemyLife random between [40..60]
		// enemyType = 2  -->  enemyLife random between [80..120];

		if (enemyType == 0) 
		{
			DrawGargole();
			
			while (enemyLife < 20)
			{
				enemyLife = rand() % 31;
			}

			DrawPlayerEnemyLife(playerLife, enemyLife);
		}
		else if (enemyType == 1)
		{
			DrawCrazyRat();
			
			while (enemyLife < 40)
			{
				enemyLife = rand() % 61;
			}

			DrawPlayerEnemyLife(playerLife, enemyLife);
		}
		else if (enemyType == 2)
		{
			DrawSkeleton();

			while (enemyLife < 80)
			{
				enemyLife = rand() % 121;
			}

			DrawPlayerEnemyLife(playerLife, enemyLife);
		}
		

		printf("\n\nPress ENTER to START BATTLE! \n");
		system("pause");

		// At this point starts a battle LOOP 
		// Loop while player and enemy are not dead and player did not escape

		while ((playerLife > 0) && (enemyLife > 0) && (playerEscape == 0))
		{
			// Print available actions and read player selection 
							// 1 - Attack
							// 2 - Defend
							// 3 - Escape

			printf("\nI'ts your turn. Those are the actions you can do:\n\n (1) - Attack\n (2) - Defend\n (3) - Escape\n\nChoose your action:");
			scanf_s("%i", &playerAction);

			// Define playerAttack, random value between [6..20] 
			while (playerAttack < 6)
			{
				playerAttack = rand() % 21;
			}
			

			// Define enemy attack power (enemyAttack), it depends on enemyType 
			// enemyType = 0  -->  enemyAttack random between [2..10]
			// enemyType = 1  -->  enemyAttack random between [4..16]
			// enemyType = 2  -->  enemyAttack random between [6..22]

			if (enemyType == 0)
			{
				while (enemyAttack < 2)
				{
					enemyAttack = rand() % 11;
				}
			}
			else if (enemyType == 1)
			{
				while (enemyAttack < 4)
				{
					enemyAttack = rand() % 17;
				}
			}
			else if (enemyType == 2)
			{
				while (enemyAttack < 6)
				{
					enemyAttack = rand() % 23;
				}
			}

			// Depending on choosen action, different things happen
			
			if (playerAction == 1)			// Attack
			{
				// Implement attack logic 

				// playerAttack and enemyAttack are compared, 
				// if playerAttack is greater (or equal), enemy receives de full hit (all playerAttack damage) and player only receives enemyAttack/2 damage.
				// if enemyAttack is greater, player receives de full hit and enemy only receives playerAttack/2 damage.

				if (playerAttack >= enemyAttack )
				{
					enemyAttack = enemyAttack / 2;
					playerLife = playerLife - enemyAttack;
					enemyLife = enemyLife - playerAttack;
				}
				else if (playerAttack < enemyAttack)
				{
					playerAttack = playerAttack / 2;
					playerLife = playerLife - enemyAttack;
					enemyLife = enemyLife - playerAttack;
				}
				DrawPlayerEnemyLife(playerLife, enemyLife);
			}
			else if (playerAction == 2)		// Defend
			{
				// Implement defend logic

				// enemy receives no damage, player receives enemyAttack / 2 damage but also recovers +2 life points

				playerAttack = 0;

				enemyAttack = enemyAttack / 2; 

				playerLife = playerLife - enemyAttack + 2;

				DrawPlayerEnemyLife(playerLife, enemyLife);
			}
			else if (playerAction == 3)		// Escape
			{
				// Implement escape logic 

				// player has a 50% probability to escape:
				playerEscape = rand() % 2;

				
				if (playerEscape == 0)
				{
					// Print on screen player/enemy attacks (only if player didn't escape)

					printf("\nPlayer Attack: %i", playerAttack);
					printf("\nEnemy Attack: %i", enemyAttack);

					printf("\nYou couldn't scape, you recieve full enemy damage!\n");
					DrawPlayerEnemyLife(playerLife, enemyLife);
				}
				else if (playerEscape == 1)
				{
					printf("You could escape! The luck is with you today!");
					enemyAttack = 0;
					playerAttack = 0;
				}
				
				// if escape, player receives no damage, if not, player receives full damage 
			}
			else
			{
				printf("The number you introduced doesn't correspond to an action. The monster kills you for being dumb!");
				playerLife = 0;

			}
		}



		
		// Check player/enemy life to see if player died or enemy died
		if (playerLife <= 0)
		{
			printf("\nOuch! You died... YOU DIED!!!\n");

			printf("\nGAME OVER...\n");
		}
		else if (enemyLife <= 0)
		{
			printf("GREAT! You beat the monster... YOU BEAT THE MONSTER!!!\n");

			printf("\nCONGRATULATIONS...\n");

			// Enemy dropped gold logic
			enemyGold = 100 + enemyType * 200 + rand() % 100;
			printf("\nMonster drop a bag with money... You get +%i of Gold!\n", enemyGold);

			// Random life potion
			int lifePotion = rand() % 30 + 10;

			printf("Monster also drop a life potion... You get +%i of Life\n", lifePotion);

			totalGold += enemyGold;
			playerLife += lifePotion;

			printf("\nTOTAL GOLD: %i\n", totalGold);
		}

		system("pause");

		// At this point, if player or enemy are not dead and player didn't escape, 
		// return to battle LOOP...

		printf("\nPress X to exit or any other key to fight another ENEMY: ");
		scanf_s(" %c", &startKey, 1);

		// Reset player life for next game
		if (playerLife <= 0) playerLife = 50;

		// NOTE: At this point, if X is pressed finish, if not, get a new FIGHT!

		printf("\nPLAYER TOTAL GOLD: %i\n", totalGold);

		printf("\nGAME OVER\n");
		
		system("cls");
	}
		
	return 0;
}

//--------------------------------------------------------------------
// Functions definition
//--------------------------------------------------------------------

void DrawTitle(void)
{
	printf("\n");
	printf("            787288778   42    87     87     882      20               \n");
	printf("          78887 78887 7885  2887    887   7228887  7888               \n");
	printf("           280         882   8807   887  88   788   480               \n");
	printf("           788722      482   88  97 887  88   588   784               \n");
	printf("           788  7      487   887  75887  88 77788   289               \n");
	printf("           788         082   887    887  882   88   784               \n");
	printf("           7884        8887  8887   888  881  7885  488  797          \n");
	printf("            77         77    77     77   77    77   727227            \n");
	printf("     784887      484     752820770   774078 97   8        477888 82   \n");
	printf("   7888 78887  7228887 707 28 7888 25  80 7887 488      2888  4888    \n");
	printf("    288   12  88    88     88      7  289      788       88           \n");
	printf("    788 780   88   288     88         784       88       884727       \n");
	printf("    788  4887 88777788     88         284       88       887          \n");
	printf("    788   784 885   88     88   72    284   77  88       88           \n");
	printf("   7888   45  880   888    888729     7888742   887 725  0884 7787    \n");
	printf("     77215    72    27      722         712     222557    722155      \n");
	printf("\n");
}

void DrawGargole(void)
{
	printf("\n");
	printf("                 .;                    ;,.                  \n");
	printf("               ##-,.xX              =x-.=+#,                \n");
	printf("                  =#X -=           x ,XX  .                 \n");
	printf("         =-===       X- +X-;--;,+x, #       -===-           \n");
	printf("      =X-      ##    #X  .      .  .#    =#=      #X        \n");
	printf("     =  .= -+,   ,; x  +- -X; +x  +- ;= +    += .=   +      \n");
	printf("   #+ ++-=.#x .#;  -  #; * .###  * #-  ,  X+ -#=.=-X. #x    \n");
	printf("   #=X#+  -#  x-##  .xX-   +#+#    #x=  =#+=- +x   ##+x#    \n");
	printf("       #X;X  #x  Xx  .-=++x    =x+x=-   #, .#+ -x;#,        \n");
	printf("        ,#+ xx   ;   ###++=    -=+###-  -    #. ##          \n");
	printf("         Xx++X#x x#  ###+  x###   ###, -#.,X#+++X           \n");
	printf("                ###x =###.####X#--###  ###;                 \n");
	printf("                   #- .X###;+X;+###=  X=                    \n");
	printf("               x+   #+  ;###xxX##+   #.  -+                 \n");
	printf("              #  -= +,+x   -+++   -X;=- x  ##               \n");
	printf("               ##  . #  ##x    -X#+ +- . =#,                \n");
	printf("                 #x  #     #+=#=    ##  #X                  \n");
	printf("                  +##      ;#;+      +##                    \n");
	printf("                             X;-X          ++++xxxXx        \n");
	printf("                              ,# .xx    xX  ##X,    #-      \n");
	printf("                                x#+ +XX= =#+   +X   #=      \n");
	printf("                                   Xxx+xX        ###        \n");
	printf("\n");
}

void DrawCrazyRat(void)
{
	printf("\n");
	printf("                x+++x  ++++  +x==x.                         \n");
	printf("  =#. #X      +#  x+;+x;...+x-=X. ##      x#. ##            \n");
	printf("  X# +X #=    ##  ,#,         #-  x#     # +x x#    -;;;    \n");
	printf("  =#; .=X#=     +-=;          ,=-=,    ,X#+,.-+= =x+.;###,  \n");
	printf(" .++#=-#, ;X-.   # XX=+;  ,==+# #,  .,X=  #=-; .=  ;#x;,=   \n");
	printf("     ###x.  ;+x+## *  =+;;++  * +#+x+-   +##= =#  =#+       \n");
	printf("         #x     -#x ,=,    .=; +#=     =#,   =, =#X         \n");
	printf("          =#;=    =X+,      .+X+    =;#X   ;x   =#          \n");
	printf("           ##-     ,=##x=;x##=-     ,##    #=  +#           \n");
	printf("            #-    +=;=##,###---x.   .#    #   ##            \n");
	printf("            #=  ;X  =+=.+#-;+=  x-  -#   #,  ;#+            \n");
	printf("           #,     =X.         xx      #x,+  ,#x             \n");
	printf("           #,     -            ,     .##   ;#+              \n");
	printf("         ,#,.                          X+ ,##               \n");
	printf("         ,#  .                    .    +#x#.                \n");
	printf("         ;#+.,                    ., ;.x#-                  \n");
	printf("          =#,-;  .    ,     ,   , .=;x#=                    \n");
	printf("          =##X-=+. ;..,. , ,; . .,-X+##+                    \n");
	printf("        ## =-x##x-;x-,===,;x+.=;;+##X;= =#                  \n");
	printf("        =##  #-.##################;.#. x##                  \n");
	printf("          Xx#++x#. .=======-==-  #X++#xX.                   \n");
	printf("\n");
}

void DrawSkeleton(void)
{
	printf("\n");
	printf("                      ;;,;;                       \n");
	printf("                    x+  #  +x                     \n");
	printf("                  #-    .=   -#                   \n");
	printf("                 -+      x.   +-                  \n");
	printf("                 #     ;   ,   #                  \n");
	printf("                 # .;       ,. #                  \n");
	printf("                 ## -.      - ##                  \n");
	printf("    ..           + x###   ###x +                  \n");
	printf("   #. ==        # +####   ####+ #                 \n");
	printf("   .+ . ---   -;# .x#X  -  X#x. #--               \n");
	printf("    +x,;.  --+ =#x.   .###.   .x#+ =,.            \n");
	printf("     ##,.-.  - xXXX X       X XXX+  X##XXX        \n");
	printf("    -+x#+.,-   x#x= #,# ###.# -x##  =xxX####=     \n");
	printf("  -#=..=##-.;;  ,#X =#######= X##   #++++xxX##-   \n");
	printf("  ##+-++=x#X,.-.  +# +=#-#-+ x##  +#+=++++xX###   \n");
	printf("    ##x+===X#x.,-   -       =+  ,##+==+++x##X     \n");
	printf("     .#x+=+=+##=.;-  ,#;   -+ ++#x+=++++x##.      \n");
	printf("       ##x====x#X;.-,  -X+--#xx+=+=+=x+###        \n");
	printf("      = .X#++===X#x.,-   X#x+++=+==++x##. =       \n");
	printf("    +;    ###x===+##=.,-   ##+===++x###    -==    \n");
	printf("  .++-  -,  -#X+++=x##-.-,  ;##+x+X#-  ;; .   #   \n");
	printf("  #  +  #     #X+++++##x,,-   +#XX#     #  #,.#   \n");
	printf("   +   =       #x+++++###x,,-   ##      #, X   #  \n");
	printf(" . #;,=# .     ,#xxxX#;  =#=.-,  ;       .-#   #  \n");
	printf(" ##xXX####      #==+X  -.+=+X,.-.  -       #.  #  \n");
	printf("  #+=+x##       #x-=- #x=XX x#+.,-  ;=-+-=-##x #  \n");
	printf("  #x==-+#       ##Xx#X.;.,.x####- -+++==-=,;####  \n");
	printf("  ##+X=+X##    #X====X#;,;#x-+X##X+=x##Xx##- +##  \n");
	printf("  #####xx###   #+-+XX==+++Xx+Xx=#+=#X+x===+#X X   \n");
	printf("    #######.  #XX+-+x+++++x++++XX##.X#X+x==X#;.#  \n");
	printf("      ----   ##+xXxx======+X#xX+=+## ;###=x#X-=-  \n");
	printf("              ##XxxXX#X#X#XX=++x##=    X####  +#  \n");
	printf("             ;#XXX#########X###XX#         +x##.  \n");
	printf("            .#xx+x#,        ##++=x#         --    \n");
	printf("            #=-=X#-          ##+xx##              \n");
	printf("            #-;=##            ##=--#              \n");
	printf("           ,##x+X##X#-        ##+-=#              \n");
	printf("          #;X####x;, #       ##xxx#-              \n");
	printf("          #         -X    XxX###xx#-+             \n");
	printf("          .=     +###     # .  -#+#X #            \n");
	printf("            ###XxxxX##    ;+         #            \n");
	printf("             #x=====x#X    ####X;--==             \n");
	printf("            -##Xx+==x##   ##x++xxX##              \n");
	printf("          .#==xXX+=###-  ##x=X###X##..            \n");
	printf("         ##-;--+x##xx    ##x=++==-,;x##           \n");
	printf("          #######         ###x===;,,  +#x         \n");
	printf("                           .;######xxx###         \n");
	printf("                                 ++++++           \n");
	printf("\n");
}


void DrawPlayerEnemyLife(int pLife, int eLife)
{

	printf("PLAYER LIFE:");

	for (int counter = 0; counter < pLife; counter++)
	{
		printf("O");
	}
	printf(" [%i]\n", pLife);
	
	printf("ENEMY LIFE:");
	for (int counter = 0; counter < eLife; counter++)
	{
		printf("X");
	} 
	printf(" [%i]\n", eLife);
}
