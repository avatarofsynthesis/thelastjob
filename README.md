###############################################
# Code Nation Develop Python
# RPG Game Assignment 2021
#
# team-green
# Marian Krchlik
# Tim Stokes
# David Lynch
###############################################

# Import Modules
import os
from random import randint
from time import sleep

# Clear screen buffer for current operating system
def clear_screen():
    try:
        os.system('cls||clear')  # Clear terminal
    except:  # stops any error
        return 0

# Wrapper for input statement to except integer only
def inputint(question): 
    try:
        return int(input(question))
    except:  # stops any error with alphabetical letters from being entered
        return 0

# status constants
ALIVE = "alive"
NOT_ALIVE = "not alive"

player_detective_name = ""
player_detective_status = ALIVE
player_detective_weapons = []
player_detective_skills = []
player_detective_case_solved = False

def player_detective_change_status(status):
    global player_detective_status
    player_detective_status = status

def player_detective_weapon_add(weapon):
    global player_detective_weapons
    player_detective_weapons.append(weapon)

def player_detective_init_game():
    global player_detective_status
    global player_detective_weapons
    global player_detective_skills
    global player_detective_case_solved

    player_detective_status = ALIVE
    player_detective_weapons.clear()
    player_detective_skills.clear()
    player_detective_case_solved = False


# Function: start_game
def start_game():
    #####################################
    # Start of the game                 #
    #####################################
    global player_detective_name 
    global player_detective_status 
    global player_detective_weapons 
    global player_detective_skills 
    global player_detective_case_solved

    game_in_progress = True

    try:
        # Game play loop
        while game_in_progress:
            player_detective_init_game()

            # add default weapons
            player_detective_weapon_add("Gun")
            player_detective_weapon_add("Hammer")
            player_detective_weapon_add("Pizza")
            player_detective_weapon_add("Torch")

            # set the scene
            scene_setup()

            # visit each room in the game

            # Monster Room
            monster_room()

            # Skeleton Room
            if player_detective_status == ALIVE:
                skeleton_room()

            # Ghost Room
            if player_detective_status == ALIVE:
                if not ghost_room():
                   player_detective_status = NOT_ALIVE

            # Treasure Chest Room
            if player_detective_status == ALIVE:
                treasure_chest_room()

            # Report end of game status
            if player_detective_status == ALIVE:
                print(f"\nWell done {player_detective_name} you managed to live until another day.")

                if player_detective_case_solved == True:
                    print("You successfully solved the case")
                else:
                    print("You failed to solve the case")
            else:
                print("Sorry you failed to solve the case")

            sleep(1)

            # Ask if the player wants to play the game again
            play_again = input("\nDo you want to respawn and play again (y for yes - any other key for no)\n>>> ")

            if play_again.lower() != "y":
                game_in_progress = False
                print("\n\nThank you for playing!")

    except:
        print("\nWARNING: An error occured in the game\nReport it to the developer")
        return 0

# scene set area - introduction to game
def scene_setup():
    #####################################
    # Scene Setup Room                  #
    #####################################
    global player_detective_name

    clear_screen()

    wait = 3 # No of seconds to display screen
    
    show_main_title()
    print("\nCopyright 2021 team-green\nWelcome to our exciting text-based detective adventure game\nThe Last Job")

    if len(player_detective_name) == 0:
        player_detective_name = get_player_name("Dirk Gently") # homage to Douglas Adam's "Dirk Gently's Holistic Detective Agency"
    
    clear_screen()
    show_detective_office()
    print(f"\nDetective {player_detective_name} gets a phone to solve a case at an old house\n")
    sleep(wait)

    clear_screen()
    show_house()
    print(
        f"\nThe detective walks up to the front door of the house. He sees the door is ajar. He opens the door and enters the house.\n")
    sleep(wait)

    clear_screen()
    show_house_enterance()
    print(f"\nDetective {player_detective_name} has a look round looking for clues to solve the case")

    sleep(wait)


def monster_room():
    #####################################
    # Monster Room                      #
    #####################################
    global player_detective_name
    global player_detective_status

    os.system('cls||clear')  # Clear terminal
    
    print(f"\nHello {player_detective_name} and welcome to the room of monsters\n\n")

    print("\nYou approach the door but cannot open it. A puzzle pops up that you need to solve before you can leave this room")

    puzzle_solved = monster_room_puzzle()

    if puzzle_solved:
        if player_detective_status == ALIVE:
            monster_room_skills()
            print("You now enter the next room")

    sleep(1)

def skeleton_room():
    #####################################
    # Skeleton Room                     #
    #####################################
    global player_detective_status

    print("""  
                                                                                                                                  ||   ||    ||    ||                  
                        /|========================================================================================             ||                      ||              
                       / |=__==____======___¬__========____======== ==    |         /      /       \    /      |             ||                          ||            
                      /  |      __    __    __    __         |        |           |      ________________ /  |  |          ||  |                        |  ||          
                  /  /   |   __|..|__|..|__|..|__|..|__      |           |          |    ||------------||     |           ||  |                          |  ||          
                    /|   |  || |  |  |  |  |  |  |  |  ||          |         |        |  || --------   ||      |         ||  |  ||||               ||||   |  ||        
                  //  |  | _||_|__|__|__|__|__|__|__|__|__||                 |          |||  --------  ||    |          ||  | ||--- ||           ||--  ||     ||        
               /  //     | |___|__|__|__|__|__|__|__|____|     |                 |       || ---------- ||     |         ||   ||----   ||       ||----    ||   ||        
              /  /    |  |  ||_|  |__|  |__|  |__|  |__||            |     |           \ || ---------  ||    |          ||   ||-------||       || ----   ||   ||        
               //    |   |     |..|  |..|  |..|  |..|                           |     /  || ---------- ||     |          ||   ||----||           ||----||    ||        
         /    / |  |   | |                                    |                       \  || ---------- ||    | | |        ||    ||||               ||||     ||---      
             / |     |   |____--_¬__--____----_______ ---_-----___¬_---__---¬__-----------*---------------------           ||                              ||--      
           //    |    | /----*----------------------*------------------* --------------*------------*-------------          || |         |-  |         |  ||----        
       === /  |      | /--*- ---------*--------*-------------*-------------------*--------------------------                 ||  |      ||-- ||      |   ||-------      
       |||||   |  |  / --*-----------*----------*-----------  -             -*----------*------------------*------           ||   |      |-- |      |    ||-----------  
       |||||     |  /---------*----------*----------*-------         |||             ------------*----------------            ||                        ||----------    
       |||||  | |  / -------*---------*----------*------*---     __|\/\          *-        ----*-----------------              ||   ||           ||    ||-----------    
       |||||     |/---  ---------*-----------*--------- *      /  / / \ \         -------       *----                           ||  |||||||||||||||-  ||----------      
       |||||     /--*--------*---------**-------------*---     / / |    /      ----          --                ----             ||    |||||||||||--   || ------------  
       |||||    / -------**--------**--------  --***---     //     \           --------------                                    ||                  ||--------------  
       |||||   /-----*--------------------*-----------    //       //        -- -------*-----                                     ||                ||--------------    
       |||||  /---------------**----*------------*----            //                                                                ||            ||----------          
       ||||| /-----------------------  ---------------------                       (SKELETON ROOM)                                    ||  || || ||---------------      
       |||||/---------------**----------------**--------                                                                                --------------------            
\n\nYou've entered the spooky Skeleton Room
\nThis room is different because you smell a death here. Now I know why! There is a skeleton on the floor. 
\nAs a detective I feel that it will be right to check the skeleton for more clues.""")

    print("\nOHHH THIS SKELETON IS DISGUSTING AND SMELLY!!!")

    key_found = False
    life_force = 100

    # Loop through number of player attempts to find the skeleton key

    for attempts in range(1, 6):
        
        x = 6 - attempts
 
        # Breaks the loop if lifeforce is 0 or below 0
        if life_force <= 0:  
            print("\nThe sheer effort of this 'last job' defeats your heart. It explodes in your chest and you die as you collapse onto the floor to rot. Your lifeforce is 0%. RIP!")
            break

        # User input (1 - 6) to search different skeleton body parts
        choice = inputint(
            "\nYou reach over the old smelly skeleton and nearly puke at the stench. Select 1-6 to search the skeleton >>> \n\n1 - Right Leg\n2 - Left Leg\n3 - Right Arm\n4 - Left Arm\n5 - Skull\n6 - Ribs\n\n>>> ")
        
  
        # Right Leg - wrong answer reduces lifeforce by 25%
        if choice == 1 and life_force > 0: 
            print("You have selected the right leg. A cloud of mould spores emanate from the leg engulfing you. You cough and wheeze for 5 minutes. Please search again")
            life_force -= 25
            if life_force > 0: 
                print(f"\nYour lifeforce is now {life_force}%. You have {x} attempts left to get the key")

        # Left Leg - wrong answer reduces lifeforce by 25%
        elif choice == 2 and life_force > 0: 
            print("You have selected the left leg. A puff of smoke rises from the leg and it stings your eyes. You cannot see for 3 minutes. Please search again")
            life_force -= 25
            if life_force > 0: 
                print(f"\nYour lifeforce is now {life_force}%. You have {x} attempts left to get the key")

        # Right Arm - correct answer rewards you with the skeleton key 
        elif choice == 3 and life_force > 0:
            print(f"""
            Your mouldy skeleton searching skills are wizard! You have found the key. It is a mystery skeleton key ...\n
                    | | | |
                    | |     | |_________________________
                | |  |||          |       |          |
                | |  |||      __----____---___----__|
                    | |     | |
                    | | | |
                """)

            print(f"\nYou also feel your lifeforce returning slowly to 100%")
            sleep(2)
            key_found = True
            break

        # Left Arm - wrong answer reduces lifeforce by 25%   
        elif choice == 4 and life_force > 0:
            print(f"You have selected the left arm. Suddenly the left hand of the skeleton cuts open your wrist. As you rip the hand off the skeleton you feel unwell as blood oozes out. Please search again")
            life_force -= 25
            if life_force > 0: 
                print(f"\nYour lifeforce is now {life_force}%. You have {x} attempts left to get the key")
            
        # Skull - wrong answer reduces lifeforce by 25%
        elif choice == 5 and life_force > 0:
            print(f"You have selected the skull. A large carnivorous beetle pops out of the left eye socket and bites you on the arm. The pain hurts. Please search again")
            life_force -= 25
            if life_force > 0: 
                print(f"\nYour lifeforce is now {life_force}%. You have {x} attempts left to get the key")
            
        # Ribs - wrong answer reduces lifeforce by 25%
        elif choice == 6 and life_force > 0:
            print(f"You have selected the dry ribs. As you search through the ribs, a great big spider pops out and bites you on your hand. You almost faint. Please search again")
            life_force -= 25
            if life_force > 0: 
                print(f"\nYour lifeforce is now {life_force}%. You have {x} attempts left to get the key")

        # Catch user error input, for example if they don't enter a number
        else: 
            if x > 0:
                print(f"\nYou have typed incorrectly. Please try again and type carefully this time! Remember that you have only {x} attempts left to get the key")
            else:
                print("You imbecile. You are dead. Bye, bye!")
                break

    # Returns dead detective status to the main code
    if not key_found:
        player_detective_status = NOT_ALIVE


def ghost_room():
    #####################################
    # Ghost Room                        #
    #####################################

    print("""\n
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@&    %@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@#           ..    ..........,@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@/                ......,/##&@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@%             ......,,,,,@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@             .......,,@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@#        %@@@@@@@@@@@@@@,   /@    ..@%.......,,@@@@@@@@@%     .(@@@@@@@@@@
        @@@@..... .,.    .@@@@@@@@@@@(.@@@@@&....@@@&......,,@@@@@@@.        ..,@@@@@@@@
        @@@......,,...     &@@@@@@@@&..@@@%......&@@@@@,...,,@@@@@@.      ......,@@@@@@@
        @@@,,,,,%.........  /@@@@@@%...............%@@......,,@@@@.    .....,,,,,,@@@@@@
        @@@@@@@@@@...........@@@@@@&...@@@@@&&@@@@@(........,,@@@.  .......,,*,,,,,@@@@@
        @@@@@@@@@@............&@@@@@%....@@@@@@@@@@........,,%@@& .........,***,,,,,@@@@
        @@@@@@@@@&...............@@@........&&%.........***&@@@/...........,*****,,**@@@
        @@@@@@@@@..,.......,,...........................*@@@@@......,,,....,,*@@@@@@@@@@
        @@@@@@@@@.,,,,,,*,,,..............................,........,,,,,,..,,*@@@@@@@@@@
        @@@@@@@@@**,,***,,.........................................,,,,*,,,,,*(@@@@@@@@@
        @@@@@@@@@%***%,,,,.............................,............,,,***,,,**@@@@@@@@@
        @@@@@@@@@@@@@@@*,,,,...........................,,,,....,,,,,,,,*******%@@@@@@@@@
        @@@@@@@@@@@@@@@@,,,,,........,..............,..,,,,,,,,,,,,,,,*@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@,,,,,,,,,,,,,,...............,,,,,,,,,,,,,,,**@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@&,,,,,,,,**,,,........,,,,,,,,,,,,,*,,,,,,,,**@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@**,,***,,,,....,..,,,,,,,,,,,,,,,****,,,,,,*%@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@*&@@,,,,...,,,,,,,,,,,,,,,,,,,,***/******/@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@&,,,,,,,,,,,,,,,,,,,,,,,,*@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@,,,,,,,,,,,,,,,,,/#*****@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@,,,,,,,,,,,,,,,,,,**@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@#,,,,,,,,,,,,,,,,/////@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@(,,,,,,,,,,,,,,,///////*///%##,**&@@@@@@@@@**//////@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@,,,,,,,,,,,,,,,////////////////**#@@@@@**//&@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@**,,,,/////,,,//////////////////****////@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@&*/////////////////////////////////%@@@@@@@@@@
        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@&/////////@@@@@@@@@@@//////@@@@@@@@@@@@@""")

    print("""\n\nYou see a scary looking ghost rising out of the skeleton. The ghost hovers two feet in front of you and opens its mouth to speak to you.""")

    # The ghost asks you to guess its age. You have eight attempts. It's age can be anything from 1 to 100

    count = 1

    # Generate random age of the ghost

    age = randint(1, 100) 

    # Loop through each attempt at guessing the age of ghost. 
    
    while count < 9:
 
        # Set x to display number of guesses player has left
        x = 8 - count 
        
        # User input to the ghost's question - input you guess
        answer = inputint("\nI DIED BETWEEN AGE 1 AND 100. CAN YOU GUESS MY AGE? (1 to 100):")

        # Guessed age too low - tells player number of guesses they have left 
        if answer < age and answer >= 1:
            print(f"\nToo low! Please guess higher. You have {x} guesses left")

        # Guessed age correctly - the ghost gives you a random skeleton code to go with the skeleton key
        elif answer == age:
            code = randint(100000, 999999)
            print(f"""\nYou have correctly guessed my age as {answer} when I died here. As you have guessed correctly here is your skeleton code. It is {code} and you may now pass me to the next room""")
            return(True)

        # Guessed age too high - tells player number of guesses they have left 
        elif answer > age and answer <= 100:
            print(f"\nToo high! Please guess lower. You have {x} guesses left")

        # Guessed age incorrectly. After eight attempts the ghost attacks you and you die. 
        elif answer != age and count == 8:
            print("\nThe ghost attacks you and it's ectoplasm burns through you deeply. You die an agonising death as the goo eats you.")
            break

        # Input is not a valid number - prompt user to try again and reset count so they have another go
        else:
            print(f"\nYou have entered an incorrect age. Please enter a number from 1 to 100! You still have {x} guesses left")
            count -= 1

         # Increment counter for next loop so it runs from 1 to 10
        count += 1



def treasure_chest_room():
    #####################################
    # Treasure Chest Room               #
    #####################################
    global player_detective_name
    global player_detective_skills
    global player_detective_case_solved
    global player_detective_status

    show_treasure_chest()

    #look for clues in the chest
    
    print(
        f"\n\n{player_detective_name} enters the room, looks around and sees an old wooden treasure chest.")
    print("You walk over to the chest, reach into your pocket and get the key you found on the skeleton.")
    print("You put the key into the keyhole of the chest and lift up the lid.")

    # find the note from the skill
    if "read invisible ink" in player_detective_skills:
        print("\nYou pick up a note from inside the chest, it has no writing on it")
        print("but with your ability to read invisible ink the text slowly appears")

    elif "x-ray vision" in player_detective_skills:
        print("\nYou pick up a small metal box. It appears there is no way to open the box")
        print("but with your x-ray vision ability you read the text on a note inside the box")

    elif "time travel" in player_detective_skills:
        print("""\nYou see an old newspaper from 25 years ago. The headlines are about the case your
working on. You read it and discover the case was unsolved because the a note which had a clue
to solve the case was thrown onto the fire before it could be read""")
        print("But with your time travel ability you travel back in time to see what was written on the note")

    else:
        print("You find the note lying at the bottom of the chest")

    sleep(2)
    print("\nThe note reads:-\n  LOOK IN THE GOLDEN MIRROR - YOU WON'T SEE THE CULPRIT\n")

    print("\nTo find the golden mirror you must solve a puzzle.")
    print("There are three domino pieces - You must add together ALL numbers on the dominoes")

    # Create 3 domino's to show the player to correctly sum the values
    domino = list()

    found = False
    while not found:
        dom = [randint(0, 6), randint(0, 6)]

        if dom not in domino:
            domino.append(dom)

        if len(domino) == 3:
            found = True

    dots = sum_domino(domino)

    show_treasure_chest_domino(domino)

    # Players choice
    choice = inputint("Enter your answer\n>>> ")

    if choice == dots:
        show_well_done_message()
        print(
            "You found the golden mirror- You look into the mirror and see no reflection!\nTurns out your a vampire")
        
        show_golden_mirror()
        sleep(3)
        player_detective_case_solved = True
    else:
        "Sorry you failed to find the mirror and you turn into a ghost"
        player_detective_case_solved = False
        player_detective_status = NOT_ALIVE


def show_treasure_chest_domino(domino):
    print(f"""\n---------\t---------\t---------
| {domino[0][0]} | {domino[0][1]} |\t| {domino[1][0]} | {domino[1][1]} |\t| {domino[2][0]} | {domino[2][1]} |
---------\t---------\t---------""")


def sum_domino(domino):
    d = 0
    for a, b in domino:
        d += a + b

    return d


def monster_room_puzzle():
    global player_detective_name
    global player_detective_status

    #####################################
    #A puzzle to enter the monster room #
    #####################################

    print("\nPlease choose the word that CANNOT be spelled out from the top row of the keys of a standard UK QWERTY keyboard")
    
    print("\nFail and you cannot enter the room, good luck          Top Keys - [Q][W][E][R][T][Y][U][I][O][P]\n")
    
    enter_room = False

    top_row_words = ["typewriter", "type", "report", "were", "poor", "tree", "qwerty", "upper", "tripe",
                     "quiet", "quote", "tyre", "porter", "port", "reporter", "troop", "trooper",
                     "true", "prettier", "property", "pewter", "repute", "yeti", "wort", "pouter"]

    other_row_words = ["word", "point", "kite", "python", "computer", "throat", "fort", "dice", "code",
                       "loop", "iterate", "water", "apple", "banana", "ice", "chair", "twist"]

    valid_word = []

    #  Find the two words from the top row
    for i in range(2):
        num = randint(0, len(top_row_words)-1)
        word = top_row_words[num]
        valid_word.append(word)
        top_row_words.remove(word)

    # Get a random word from 'other row words'
    num = randint(0, len(other_row_words)-1)
    none_top_row_word = other_row_words[num]

    # generate random order for the words <33 first, 34-66 second, 67> last
    num = randint(1, 100)

    if num <= 33:
        print(
            f"(1) {none_top_row_word}  (2) {valid_word[0]}  (3) {valid_word[1]}")
        answer = 1
    elif num <= 66:
        print(
            f"(1) {valid_word[0]}  (2) {none_top_row_word}  (3) {valid_word[1]}")
        answer = 2
    else:
        print(
            f"(1) {valid_word[0]}  (2) {valid_word[1]}  (3) {none_top_row_word}")
        answer = 3

    # player choice
    while True:
        choice = inputint(
            "\nEnter the number of your choice 1 or 2 or 3\n>>> ")
        if choice >= 1 and choice <= 3:
            break

    if choice == answer:
        enter_room = True
    else:
        enter_room = False

    if enter_room:  # see if can light up the room
        print("\n\nWell Done\nYou've successfully entered the room")
        sleep(0.5)
        torch_available = monster_room_torch_puzzle()

        if torch_available:
            show_well_done_message()
        else:
            print("A monster attacks you and you are eaten")
            player_detective_status = NOT_ALIVE

    else:
        print("You failed to enter the room")
        player_detective_status = NOT_ALIVE

    if player_detective_status == ALIVE:
        monster_room_weapon()

    return player_detective_status == ALIVE


def monster_room_torch_puzzle():
    #####################################
    # Light up monster room             #
    #####################################

    global player_detective_weapons

    print("\nThis room is dark and you hear a growling noise coming from the back of the room.")
    print("\nYou feel around the wall and find the light switch. You try to flip it on but the room stays dark")
    print ("\nYou feel a sense of intense dread as you know that the light switch doesn't work and the room stubbornly remains dark.")

    torch_available = False

    if "Torch" in player_detective_weapons:
        torch_available = True

    if torch_available:
        print("\nYou must solve a puzzle to turn on your torch and light up the room")

        print("\nTo turn on the torch you must enter a Palindrome")
        print("Any text or number that can be read forward and backword. Must be more than 1 letter\n")

        # get player palindrome
        choice = input("Enter your palindrome\n>>> ")

        if len(choice) > 1:
            if choice == choice[::-1]:
                return True  # able to light room

    print("You failed to enter a valid palindrome")

    return False  # not light up room


def monster_room_weapon():
    #####################################
    # Get past monster with weapon      #
    #####################################
    global player_detective_status

    show_monster()

    sleep(2)
    show_monster_weapon()

    print("You must pass the monster to get to the next room")
    print("You have some weapons to use:\n\tGun - to shoot dead the monster\n\tHammer - to scare the monster\n\tPizza - to feed the monster")

    print(f"\nChoose your weapon (1) Gun  (2) Hammer  (3) Pizza  (4) Run away scared")

    # player choice
    while True:
        choice = inputint(
            "\nEnter the number of your choice 1 or 2 or 3 or 4\n>>> ")
        if choice >= 1 and choice <= 4:
            break

    if choice == 1:
        print("\nYou chose to shoot the monster and run past it\n")
    elif choice == 2:
        print("\nYou have scared the monster with your hammer and it ran away\n")
    elif choice == 3:
        print("\nYou fed the monster and make friends, the monster lets you pass\n")
    else:
        print("\nYou are very scared of the monster and run away\n")
        player_detective_status = NOT_ALIVE

    sleep(3.5)


def monster_room_skills():
    #####################################
    # Get skills to solve case          #
    #####################################
    global player_detective_skills

    show_cabinet()

    print("Before leaving the room, you see a cabinet with 3 drawers that contain skills which you can use to solve the case in another room.")
    print("Top drawer 1, Middle drawer 2, Bottom drawer 3")
    sleep(0.5)

    # player choice
    while True:
        choice = inputint(
            "\nEnter the drawer of your choice 1 or 2 or 3\n>>> ")
        if choice >= 1 and choice <= 3:
            break

    if choice == 1:
        print("\nYou have a skill: To read invisible ink\n")
        player_detective_skills.append("read invisible ink")
    elif choice == 2:
        print("\nYou have a skill: X-Ray vision\n")
        player_detective_skills.append("x-ray vision")
    elif choice == 3:
        print("\nYou have a skill: Travel back anywhere in time for 1 minute\n")
        player_detective_skills.append("time travel")

    sleep(0.5)
    print("...", end="")
    sleep(0.5)
    print("...", end="")
    sleep(0.5)
    print("...", end="")
    sleep(0.5)
    print("...\n")

def get_player_name(default_name):
    #####################################
    # Player enters their name          #
    #####################################
    player_name = input(f"\nPlease enter your detective's name or press 'Enter' key to remain as Detective {default_name}\n>>> ")

    if len(player_name.strip()) != 0:
        return player_name.strip()
    else:
        return default_name

def show_well_done_message():
    print("""
    ***    **    *** ************  ***               ***              ******            *******     ***         *** ************
    **     **     ** ************  ***               ***              *********        ***   ***    *****       *** ************
    **     **     ** ***           ***               ***              ***     ***     ***     ***   ***  **     *** ***
     **    **    **  ***           ***               ***              ***      ***    ***      ***  ***  **     *** ***
     **    **    **  ***********   ***               ***              ***      ***    ***      ***  ***  **     *** ************
     **    **    **  ***********   ***               ***              ***      ***    ***      ***  ***   **    *** ************
     **    **    **  ***           ***               ***              ***      ***    ***      ***  ***    **   *** ***
     **    **    **  ***           ***               ***              ***    ****      ***    ***   ***     **  *** ***
      ***  **  ***   ************  ***************   *************    *********         ***  ***    ***       ***** ************
       *********     ************  ***************   *************    ******             ******     ***         *** ************
    """)
    sleep(1)


def show_main_title():
    print(f"""
|_____|      |----|---        ________   |               |-----|   ____     _________
| ||  |      |  || |    |    | ||    | ||| |-----|       | ||  |  |  | |    | ||   ||
|     |      |     |____|__| |     |||   | |   || |      |   |||  | || |    |   ||  |
|   |||      | ||  |  ||| |  | ||    |-----|   || |------|     |--|    |----|       |
       ------|     |------|--|
   __           _______        ________     __________             ----
  |  |         |  __   |      |  ______|   |___    ___|          -------------
  |  |        |  |   |  |     |  |_____        |  |
__|  |________|  |___|  |_____|______  |_______|  |______________________________
  |  |____    |  |   |  | -    _____|  |---    |  |---                           \\
\ |_______|-  |__|   |__|--   |________| ---   |__|---             --    ---------\\
 \  ========    ===    ===      =========        ===              ----             \\
\ \  ====        ==========           ====        ===           ---------           \\
 \ \  ====       ===    ===      =====              ===            ------            \\
 |\ \   =====       =======        =========     ==========            ---            \\
 | \ \              -------                              __     _________     _________\\
 |  \ \       ----            ------------------        |  |   |   ___   |   |   ___   |\\
 |   \ \          ----                                  |  |   |  |   |  |   |  |___|  | \\
 | | |\ \      ----  ------                             |  |   |  |   |  |   |   ___  |   \\
 | | | \ \       -----                             __   |  |   |  |   |  |   |  |   |  |   \\
   | |  \ \             ---      -----------      |  |__|  |   |  |___|  |   |  |___|  |
     |   \ \                                      |________|   |________ |   |________ |
     | |  \ \                                      ==========    ==========    ==========
       |  |\ \                                     ===  =====     ===   ===     ===    ===
       |  | \ \                                          ======    ====   ===     ==========
          |  \ \                                          ======      ========      =========
          |   ===============================================================================""")


def show_detective_office():
    print(f"""  _________________________________________________________________________
   /        \      /   \     \      \                     -----       -----
   / ___--__________--___________-___            ----             ----
 / |________--____________--________|    /          ---   -                ---
  \ |  ,   , ||    ,  ,  , ||,  , ,  |    \      ----   ---  --       ---
   /|   ,    ||  , ,,  ,   ||  ,,    |      \                  ----       ----
  / | ,   ,  ||   ,    , , || ,   ,, |    /           |=|                         --
    |   , ,  ||   ,  , ,   ||  ,     |   /    ---    |   |    |=|     ----
    | ,, ,   ||    ,     , ||    ,   |               |   |   |   |
    |   ,    ||   ,   , ,  ||  ,    ,|    |  ---     |||||   |   |   ---      ----
  \ |  ,  ,  ||  ,  , ,    ||   ,  , |   |       |\  |||||   |||||  \\
   \|    ,   ||      ,     ||  ,,    |   /\      | \_________-----___\    ---
    |  , , , ||   ,     ,  ||     ,, |    |       \|__________________|
    |      , ||       ,    ||  ,,    |   /                                      ----
  \ ----------------------------------        ---        -----
 /  _________________________________________                        ------
 /                                                   ----        ----
       RING      RING        RING                                            ----
             ________________      RING     ---                ---
   !RING!  / \--------------/ \\
          [[ ]]  ||   ||   [[ ]]      RING          ---      |---------------|
                <<<< >>>>                                  |------------------|
     ----      <(1)(2)(3)>        ----      --            | ------------------|    --
      ________<<(4)(5)(6)>>_______________________---____ |-------------------|
     | \      <<(7)(8)(9)>>          ________            | ------------------|
      \ \      <<<<<>>>>>>         --\ 43$£$4 \  --     |-------------------|
      |\ \                            \ $%^&^& \        | -----------------|   --
      | \ \      --            ---     \________\  -    |------------------|
      |  \ \        _________  -                        | ----------------|
      || |\ \       \ %$%$£$£\         ---       --    **| ----------------|
      |  | \ \   --  \ f$gg$gg\     -        -       ***| -----------------|     --
      | ||  \ \       \________\     ---    -     ****/  ------------------/
      | ||   \ \_______________________________ ***** / -----------------/
    --------- \ |______----___ -----______---__  ***/  ------------------/            """)


def show_house():
    print(f"""   x           *****    *****        ******  **   *****              x       x
     x           x                       *****          x                x
                       x       ____--_______
                              /^^^^^^^^^^^^^\                    ***  ***
       x        x            /^^^^^^^^^^^^^^^\              ***    *** ****
           x          x     /____-----___--___\          ***  ***    ***
                              | | | ,,, | | |         _**___       ***
                              | | |(*.*)| | |         ||||||                  x
   x      _________--_________| | |-----| | |___--____|____|______
         /^^^^^^^^^^^^^^^^^^^^| | | | | | | |-^^^^^^^^--------^^^^^\     x
        /^^^^^^^^-----^^^^^^^^| | | | | | | |-^^^^^^^^^------^^^^^^^\\
       /^^^^^^^^^^---^^^^^^^^^^---^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^\\
      /^^^^^^^^^^^^^^^^^^^^^______--____--______^^^^^^^^^^^----^^^^^^^\          x
      |______----_____--___|---|----------|----|___--_______--____--__|
        |   |   | |  |    |    0__________0     ||   | |  |   |   |
        |   ||  | |  |   ||     ||/|  |  ||     |    | |  |   ||  |
        |   |  || |  |    |   | || |  |  || |  ||    | |  |   |   |
     ---|  ||   |_|__|    ||  | || |  |* || |   |    |_|__|   |   |
  ------|   |   |    ||   |     ||/___|__||    ||    |    ||  |   |---      \/
 --  -- |   |   |    |    |     |_________|     |    |    |   |   |------  \/\/
 ------ |||||||||||||||||||||||||_________|||||||||||||||||||||||||------ \/\/\/ --
--   -----   ----   -----------  . .  . .   ----------   \/   ---------- \/\/\/\/ ---
---- \/   -------^----------\*/ .- .  . . .  .    -----------     \*/    - \/\/ ---
   --------  \*/    ----------.. ..  .  .  .. .-\/-------------- ^ -----------  --   """)


def show_house_enterance():
    print(f"""
                  =__=====____======___¬__========___¬___======== == /
              /   |   ||||     /            \  /    |||        ||   |/
           /     /||   ||     /            /          |        |    |/
              / / |  ||        \   ____________---  -||             |/
           /   /  |    |         ||------------||    |       \/     |/
        /    //   ||   |   \     ||            ||    |       \      |/
         /   /||  |    |    \    ||            ||   |        /      |/
       /  /     | ||             ||*-          ||         |         |/
       //   /||   |        /     ||            ||        |       |  |/
  /    /   //||   |    \   \     ||            ||       |         | |/
      /   // ||   |____/_¬__\____||----_______ ||___¬___¬_||__¬__|__|/--
    //   //  ||   /----      -----------------*---------------*---------
=== /   //   ||  /--   ---------*--------*-----------*----------------
|||||   ||   || / -  --*---*------------------*----------*------------
|||||   ||   ||/ --*-----------*----------*-----------  -----*------
|||||   ||*  ||---------*----------*----------*-------------------
|||||   ||  / -------*---------*----------*------*-----------*-----
|||||   || /---  ---------*-----------*---------   ---*---------
|||||   ||/--*--------*---------**-------------***----------
|||||   |/ -------**--------**--------  --***-------------------
|||||   /-----*--------------------*----------------------
|||||  /---------------**----*------------*----------
||||| /-----------------------  ------------------------
|||||/---------------**----------------**--------
                                                                         """)


def show_monster_weapon():
    print(f"""________________________________________________________________________________________________________________      
    | ------------------------            ----------------                 --------------                        ----    |      
    | ----------------------------               --------------    ------------               ----------------           |      
    | ____________________________--------------------____________________-------------------------______________________|      
    ||-------------                                                                                     -   ------------||--    
    ||__________________________---------__________________---------------__________________________-_____       -------||---  
    ||---                                                                                                        -------||---  
    ||--                                                                                                             ---||---  
    ||--                                                                                                              --||---  
    ||-   __________________                                                                                           -|| ---  
    ||   /______________/__/|                                                                                           ||---                                                                  
    ||   |            / |  ||---                           __         ||---                                             ||--
    ||   |            \ |  ||---                      __ ``  /         || ---                                           ||---  
    ||   |____________/_|__|/---                __ ``     * /           ||   ___-------------___                        || ---  
    ||          |||||-----              ___  ```  ---(0) ` /            ||--   _______________ ---_____________====__   || --  
    ||          |||||----          __ ``  *  .. `.. --*   /             |     |______________ |      -------------   |- || ---  
    ||          | / |--       _ ``   --   *   ..  .. - ` /            ||   -- | ---  --- --   |    __________________|- ||---  
    ||          | \ |--       - _ -- (0)   --   (0).. * /          | ||      |---------------|    _________________|-   ||---  
    ||          | / |--       - _   *     ¬  `  .. `   /          |    |--   |_______________|      /-----              || ---  
    ||          | \ |--         - _     *  ``  ..   ../          |       |     ----    --       ----                    || ---  
    ||          | / |--          - _* --  (0)   *    /          |         | |_____-||- ____----                         || ---  
    ||          | \ |--              - --    --  .. /          |           |  ||    \    || ----                        ||---  
    ||          | / |--              -  _  * --`   /          |   (*)     |     ||_____||---                           -||---  
    ||          | \ |--                  - __ --  /          |----      |-----                                        --||----  
    ||        --| / |---                     - __/           |----       |------                                    ----|| ----
    ||--    ----| \ |----                                 ---|__________|------                                    -----|| ---  
    ||--       ------                 ---------------        ---------------                                    --------||----  
    ||---     ( HAMMER )                 ( PIZZA )             ( GUN )                                          --------||----  
    ||-----                                                                                                   ----------||---  
    ||_____________________---------__________________-------------__________________________--------------_____________||----  
    |          |        |                                                       ------    |        |                     |-----
    |          |  [  ]  |        ----             ----- [       ]                         |  [  ]  |   ---------         |------
     |         |________|                               [  [ ]  ]          ----           |________|                    |------
     |       ----                                       [       ]                                       ------          |-----  
     |          -------              ----------------                           ----------                    ----      |----  
     |     --------          ----                             -------------                     -----------             |----  
      |________________________________________________________________________________________________________________|---                    
        """)


def show_cabinet():
    print("""
    =========================
    | {                   } |
    | {         o         } |
    |-----------------------|
    | {                   } |
    | {         o         } |
    |-----------------------|
    | {                   } |
    | {         o         } |
    =========================
      ||                 ||
      ||                 ||
      --                 --
    """)
    sleep(0.4)


def show_treasure_chest():
    print("""
     /=============================\\
    /_______________________________\\
    |                                |
    |              <O>               |
    |                                |
    |                                |
    |                                |
    |                                |
    L--------------------------------|
           [Treasure Chest]
    """)
    sleep(1)


def show_monster():
    print(f"""                                                                                                                        
                                                          (*(                                               .          
                                                    .,(#,*/*/*,#(*.                                .  .   .    .        
                                               **     ,%#/, ,/##,     //                              .  .  ..   .      
                                             ,,#. .. ../##,.,(#/.. .. ,#,.                           . ..    ..        
                                          ,#.#/*..,,.*/*#/*.*/%*(/.,,..,/#.#,                             ..            
                                         #*.*   ..,(, ,//, . ,/(* *(,...  *.,(                
                                        ,(,#**..(/* ,,*(%(/./(#(**, */( .**%*/,                                        
                                       % .,.(*,,./,.#//%%(#,#/%#//#.,/.,,*/.,. &       .                                
              * *        .,.,,/,,*..  # . ,,,//,*(*..*..(%/*/%(..* .*/*.(/,., . #  . *,*/,,.*.        * *              
               ,,       ...*/*(,/(/(  ./****.**.*/**,/%#&#*,*#&#%(,,*/*.**.,*,,*. .(/(/,(/(*....      ,*                
                 (,/,/*(# # # . ,/*(   #,..*, .(**///*,/*#*./%//,**//*/(,.**,.,#   (*/, . / % #/*/**,(                  
                      /   **. ,**.(#  #(#(,(//..(/(//.,,*%,.,#*,,./*#*(,.*//,(#/(. #/ ,*, .,/   *                      
                          .&/.,..(.  ,*%,,,*/*/**,. /**//(/*/(//**( .,**/*/**,,#*,.  (..,.(&.                          
                         (,  . (&    #/,(&&@/@@@@##*.,..,/,,*(*....*#%@&@@#@&&#*/#.   %( .  ./                          
              ..        % ,,..*/    ..&//(#%@@@@@@@&,.,,,((.((,...*&@@@@@@@%#(//&,     /*. ,, #        ..              
                      .( ,,.(.(      %(&#&&*@@@@@@@@#*,(# */* ((,,(@@@@@@@@*&%#&(#      (,#..../.                      
                      ..,%,./%.      ,@(*,**,*,,/&#%%/*,,,,,,,,,*/%&#%(***,*,,*(@.      .#(.,%*..                      
                 #(((%%(.%#*,.%     ..#@&(((//,,,..##%%.(/.../(.%%##,.,,*(/(//%&#.      %.,*#%,/%%((/#                  
              %.......,/(/%/*.,,((.(%#%&((*%**&%%@%**....,...,....*,#&&%%*/%*/(@%(%(.((,..//%///,,.. ...(              
            /,..,*(/#(%%&@&%(//(&(. .,,//#%%(#%%,/*/(*  ..,#,.,  ,(**/,#&(/%%(*/...../&(//(%&@&%%##((*,..*/            
   ,/(     ,..,/,%@#%%((/*((*,/(&%/%&*.. **(#(%%(//.,.,....(....,.,,//(##(#(/*.,.*&%/#&#*.*(/*/((#%#@%,*.,.*    .(/,    
  ,,,.*    #.*, ./.*#*,,(/,(%%#%/@%@*(#,.#@@%&@&&(#**,, .(**/(..,,**#(@@@&%@@%..((*@%@/%(%%(.*(,,*#*./  ** #.   /.,,*  
     .,.  /*,,    ******.,#(.(@@&#(%&,&%#(((%#(%&///*(,..,(#(,..,(*/((&%(#%(((#%%,&#/#&@@(./#,.*,****  . ,,*/  .,. ...  
      .*##*(,     .,*.#*,#%/.#. #/%##&.%@##/#/&@##/,/(,,.*,@,*..,(/,*##@&*%*##@%.&(#%(% .(,*##,,(.*,.     ,(,##*.      
                  .,,.*.#/..   . ,/%(%(,%%/,&&&&(#/.(#** *&&&/.*,((.*#(%&&&,/&%,(#(%(,     . /%.*.,,.         .        
                ././,,. (        #/.(*#%.(%%#((/*,/##/,,,&((#&.,*/##/,*/(/#&%(.&#*/./#        ( .,.(,/.                
                  **/,.,*.        .(**(%&,/&#(*//((**#/.#%%&%%%./#**((((*(%&(,&%(,*(.        .*,.,***                  
                   *((/.. #       .***/%#@#,###/#/(((( /#&(*(&%( (##//%(###.#&#%/,**        (.../#/*                    
                     &.&/,.%      .,*##%#@%%%..,*...//.../#,((.../(. .*,..&%%@#%#%**       %.,/%,&                      
                        *&&(,%     . . .&*,%%/#%%(,,    .**.,*.    ,,/%&#/%%,*%.....     %,(&@/                        
                        (/*/(*(#        (.,(/*.*..       /,.,(        ./.*//,.(        #(*(/*//                        
                        (,##&((.        .  /.*.          (, ,(           *./  .         ((@##,(                        
                          .,%(*    ,       .             *   *             .       .    .(%,                            
                       *.*#(                             .   .                             (#,,*                        
                       ,/..                              .                                  .,/*.                                  """)

    print("The torch lights up the room and you see a scary monster")

def show_golden_mirror():
    print(f"""
                                                    .*(*.                                                          
                                                  ###%%&&&@                                                    
                                               /#/,...*((%&&@@&                                                    
                                             ,(,,...,/(#%&&@@@#                                                    
                                            *%(/(*/((%%&&&&@@&                                                    
                                             (*&@@@@@%@&&&&&@@@@%#@%     ..                                          
                                   %&&%@@%@#/@&@@@@@&&%&@@@@@@%,@@(@@@@@@@@&                                        
                                   @&%&@&@@%&#&&%%&&&&@@@@&&&&&@@#@@@@@@&@@@%(                                      
                                 &&@(@&@/@@@@@@%@@@@(/*#&%@@@@@@@@&&&(@&@%@@#@&                                    
                                .*%@&%@@@@@&@@@@%@%#((#%&&@@&@@&@&%.&&@@@@@@%&@&                                    
               (#/#%&            #//%@%&%%@@@@@&@@&%#*****,*(%#%&&%&@@@&&&@@@@@@&         *&%%#%&&.                  
            #(/@@%,            /(&**//(,,&@&&@&*,.....   .    ...,(&&%&@#@&@@&&&#@(           &%&#%&&                
          %/%@@%%,      .(%&&%&#%&@#@&@&%@@%%%%%.....            .#/#&&%@@@@&&%#%%@@%%###     #&&&%&%&@              
          %,@@%&&#(   .#%%*,*(#%#(&&%@@(@%@@&%....  .                (@%%@&@@&&#%(#*%&@#%%%%%&/@&&&@%%&%@            
           %/@@&&##((/%%%#((%&%@&%&#&&@&@@@@@&@&.. ..                 .@&@@%&@&%##(%&@@@&%##%%#&@&&&&%&%%%&&          You found the golden mirror
          %*@@%%&#%%/((/&#//#@@&%#&%@&@@&*,.,,..                    ........../#%(@&@@&@@&*%&%&@&&%@@#@#&&%@%          
          (#@&&&%#(&%(/*#&&&&@@&%#@@%@&%,..... .                 . .......,,,,,/&&%@@%@@%%%&@%&@&%@@&&@(&##&&          
          &%@@&&#&##&@&/#@(%%&&#&@@&(@#......                ..........,,,,,,,,,*#%&%&&%/#&@/#&&&#@@%@&/&#/@#         You look into the mirror and see no reflection!
          %&@@%@%&%%&%%#*/%%&@@@@%#%@#......               ..........,,,,,,,,,,,,*%#&&&&&@%(//#&&@&&@@*@%&&@            
          %&@@&&&%&%%@@@%#%@(&@@&%@&&.....             ............,,,,,,,,,,,,,,,*%#@@&&(,**#%@%@@@@#@&(&@%            
           %&@@%&&&&&@@@@@&/&@&@&*&&,....            ............,,,,,,,,,,,,,,,,,,(*%@#(**%&@&@@&@&#@&*&@&             Turns out your a vampire ,but how is possible ?
            %&@@&&&&&%&@@%(@&(%&&%%@*...     . ................,,,,,,,,,,,,,,,,,,,*#(%(/*(@@@@@%&&&/@%/@@&              
             (%@@&&&&&&&&@&*##@&#&%&# .......................,,,,,,,,,,,,,,,,,,,,*/(*//%@#&%&@@@&,&&(#&@&        ,,,                                                       ,,,,,,
               #&@@%&&@@@@%&%%%&&%@&&...................,,,,,,,,,,,,,,,,,,,,,*****#((&@&&&&&@@@@/&%/#&@/            GOLDEN MIRROR IS MAGICAL AND SHOW YOUR PART OF YOUR PAST      
                #%&@@&&@@@%#/&&@@&%@@#..............,,,,,,,,,,,,,,.,,,,,,,,*******##@@@@%&&&@@&&&*(@&@                  
                 ,%&&@@&&&&..,(&@&@&&&..........,,,,,,,,,,,,,..,.,,,,,,,*********/#%@@@/&##%@@@(#&&&&                 CASTLE ,KING,MAGE  
                   %&%&@@@@@&%@@@@@&&%,....,,,,,,,,,,,,,,,...,,,,,,,,***********///(@@%@&#&@@(%#&&&&                    
                    %&&&@@%&%&(&&@/@&,,,,,,,,,,,,,,,,,,..,,,,,,,,,**************////(#@@(%&@@@%&&&&*                    
                    ,%&&@@@&@@@%(&&@,,,,,,,,,,,,,,,,..,,,,,,,,,***************///****(&##&&@@@%&&&&     Now I Know !!! I remember Long time ago in the dark vampire age King Arthur and his best Mage    
                     #%&&@&/%&**#%@&(,,,,,,,,,,,,,,,,,,,,,,*****************/////***/..,#&&@#@@&&&%     was hunting me through  the whole kingdom ,              
                     .%&&@%%/,*#/(&&#*,,,,,,,,,,,,,,,,,,,,*****************///******/@&%&&&&@@&&@@%                    
                      %%@&@%@%&#@@&*&%,,,,,,,,,,,,,,,,,****************//////******#(@%#&&@%#@%@%@&     but I hide in a small cave in the hills and waited and waited for the      
                      #%@@@@&@%#(&@@@&(,,.,,,,,,,,,******************///////******((&&#(&&@@@@@@&@/     time when will it will be safe to leave my hiding place,              
                      .%@@@@&&&&&&&@&&@(.,,,,,,,,,,****************///////*******(/(//%&@@@@@@@@&%    
                        %@@@&#(//%&%&&&&#.,,,,,,*****************//////*/*******(##/((#&&&&@@@%%       but this centuries in the cave has had an impact on me and I forgot my past,                
                          .&/&@%&#&@@%&@&%,,,,,*************///////////*******,*&%#@&##%&@&%#    but time past so quickly in  the world  and I never thought about who Iam ,what is my purpose in this world.
                            /#&(%@&##&@@&&&%*,,**********///////////*********/#&@%&&#&%%%%%#@                              
                            %%%%#/(&@@%&@@@&&&/*********///////////********((&@(#&@@%##&&&&@@            
                           &%%%%&@&%&@@#&@@@@@&&%/(#&%%(////////*//#%/#*//&@@%%#&@@%#&&@@%&&@@                        Time will show  
                          ,#%%%%&@&&&#,#(#%#(&@@&&&&@%%%(&#&#@&%&/&@%%(@##/%%&/%#%%&@@@@@@@&@@%                    
                         /####%&,(#&@&##@%((%&#%#%&&&&#&#((#(&*&&&@@&#&@@&%%#%%&#/@%%(%%%@@@@@@@                        
           /(*.        /###&#%&&&&&&%#%&&%#%((@&&#(#(%&/&&&%#&@@@&@@@&&%&((#&&&%%(/@&%%%#%#%%&&@@&       #%%&&%      
        *&/*(&@&.   #/%(%%&&&&&%%&%%&%(@&#%%##(@@%&#%&#%%%##%&%&&@@@&&&#&##&&&%&%##&@&&&%&&&%%%%%&%@@(  @%((%&@@#      
        #((#%%%&@&%(*#%&&&&&&%%%%%%&%%&&&%%#%###@&%&%%%%#%%@%%&&@@&&%%%##%&@&%&&%##%@&&%#&&&&&&&&&%%&@@@&@@&@&&##      
         %@@&&&&%(/%&&&&&%&@@@@@%%&&%%@@&    .#%&&,    %%%%%%%&@@&(    *%%&      *%&@&&&&@&@@@@@@@&&&%%@@&&&%%%        
            *(((*%%&&&%@@@@#@%@@&@  %%&        %&      %%%%%%&&%&&       &         %/  &/@((&@@@@@*@@ &&%&              
                %%  .#&&@%( (%&@(%   *                                                  #%%%   /@*&%/                  
   .,,.......................................................................................,,,.......
   ,,,,,..........,,,,,****,,,,,,,,,**,*,,,,,,,,...................,.,,,,,,,,,,,,.......,,,,,,,****,,,........
.,,,,,,,.....,,,,,,,,,,**,,,,,,,,,,,,,,,,,,,,,,,.................,,,,,,,,,,,,,,,...........,,,,,,,,,,,,,,,,......
   ,,,,,..,,,,,,,,,,,,,,*,,,,,,,,,,*,,,,,,,,,,,,,,,,...........,,,,,,,,,,,,****,,,,,,,,,,,,,,,,,,.,..,..,,,,,,,.....
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,**/(#%##(/*,,,,,,,,,,,,,,,,,,,,,,,,**/#%%%%%##/****,,,,,,,,,.............,,,,,,,...                  
  ,,,,,,,,,,,,,.,,,,,,,,,,,**,**,**//(///(////(*,,,,,*,,,,,,,,****/##(((((((((((((/////**,..................,,,,,,,.,,,,
,,,,,,,,,........,,,,,,,,,*,************************,************//******************,,,.....................,,,.,,,,
,,,,,,,,,........,,,,,,,,,,,*,**************************************,,,,,,,,,,,,,,,,..........................,,,,,,,,,,
  ,,,,,,........,,,,,,,,,,,,,,,*,*,,,,,,,,*,*,*,***************,,,,,,,,.......................................,,,,,,,
,,,,,,,,,.......,,,,,,,,,,,,,,,,,,,,,,,,,,,,*,*************,,,,,,...............................................,,,,,,
  ,,,,,,......,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,,.....................................................,,,,
  ,,,,,.......,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,................... .. . ..  .... .........................,,,,,,,
,,,,,,,......,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,.,.................                 .   .........................,,,,
  ,,,,,,..,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,**/*,.....................   ..*/,,******.,,,,,,.......................,,
  ,,,,,,,.,,,,,,,,,,,,,,,,,,,*(##%%%%%%%%%%%%%########((((*//(((##((**#//(#(/#**//#/(##((((#((#(/,,................,,,,,
*,,,,,,,,,,,,,,,,,,****(#%%%%%%%%%%%%%%%%##(#%######%%##%%%%%%####(####((#((((((/(((#(((((((((((((#%##*,............,,
*,,,,,,,,,,,,,**//#%%%%%%%%##%%#(((//****/*////((/((#(/##############((#######(((((((((((((((((((((###%%%(,,,,......,,,,
 ,,,,,,,,,,**/((((((((((((###########(((#(#((###((########%###########################################%%%%%#*,,,,,..,
  ,,,,,,,,/((##%%%%###(((((################################(##%########%%#####%%#%#################%%%%&%%%%%(*,,,,.,,
 ,,,,,,,,*(##&&&@@&%#**,,,,,,,..****,,,.,,**,*,,,,,,,,,,*******,,,,....,,**//*,. ...,///**,,,*/#((((###%%%%%%%(*,,,.,,,,
 ,,,,,,,,*/#%&&&@@@&%/*,,,,,,....,,...   ....,..         ......           ...        .,.  ..,,******/(&&#####%(*,,....,,
  ,,,,,,,,//(%&&&@@@@@@(,,*,,. ......      ...              .              .          .   ..,,,,,,,*/@@@####%#/,,,....
,,,,,,,.,,,*((%%&&@@@@@@@@@/.      ...      .               .              .               .,&@*,%@@@&&%###%(*,,......,,
  ,,,.....,,/((#%%@@@@@@@@@@,..    @@*.,....#.              .              %      ..,&    ..*@@@@@@@@&&&##(/*,,.......,
,,,,.......,,*(((#%%@@@@@@@@&,..   @@@@@@&&&&&.             *          .../%%%%%&&%%&(   ..,@@&@@@@&&&#(///,.........
  ,,........,,*((#(#%&@@@@@@@@,..  @@@@@@&&&&&&&%/**,,,,,,/&&&#(/*****(%%%%%%%%%%%%%%, ..,#&%%%&&@&&#((((*,..........,,,
  ,,.........,,*####((#&@@@@@@@,.. @@@@@@&&&&&&&&&&&&&&&&&&&&&&&&&&&%%%%%%%##(%#####% ..,&####%%%%((/*/*,,............
,,,,..........,,*(#%##((#%&@@@@@@,.%@@@@&&&&&&&&&&&&&&&&&&&&&&&&&&%&%&%%%%%%%#%##%##...#////((##(/****,*,,...........
,,,,..........,,,*##%####((#&&&&&&&&@@@&&&&&&&&&&&&&&&&&&&&%&&&&&&%%%##%%%############%%(///(((/**/*,/(,,..........
   ,.........,,,,,,/%%######(((&%&%%%@@@&&&&&&&&&&&&&&&&&%&%&%%%%%%%%%%%#############**,*(/((/*//(*(#/,,............                 World, I'm back again!  
  ,,........,,,,,,,,*%######(((((####(#@@&&&&&&&&&&&&&%&%%%%%%%%%%%%%###########%%&##(((((////((((#(*,...............
,,,,......,.,,,,,,,,,*#%#%######(((((((((#(##&&&&&&&&%%%%%%%%%%%%%%%%%###%%%##((((((((((///((((((#/,..............
  ,,........,,.,,,,,,,,*%#%#########(((((/////////((/,,.#,,,*,,,**#(((((((((((((((/(/*/((((#(((%(*,...................
 ,,,,.......,,,,,,,,,,,,*(%%#%#######(#((((/(////////////(/((((((((((/(((/////,***,((((######%%*,.....................
    ,,,....,,,.,,,,,,,,,,,*(%%########(((((((//(///(////*///////((/(*///**/**,//((#########%%(,,...................
   ,,,,,,.....,.,,,,,,,,,,,,*(#####((//****/*/(((((((((((((((/(((((///((((((((######(####%%#,,.................
     ,,,.,..,....,,,,,,,,,,,,,*(####((/**/*#**/((#/*(#(((/((((((###((#####(##(##(###%#%%#(*,,.......................
    ,,,,,.,.....,,,,,,,,,,,,,,,,*(###(/*(////(((####(#############%######(#######%%%%(*,,,,,..............
         ,,......,,,,,,,,,,,,,,,,,,*((((%##################################%%%%%%#/*,,,.,................
             .......,,,,,,,,,,,,,,,,,,,,*/((#(###%%%%%%%#%###%%#%%%%%%%%%%%%#(/*,,,,..,................
                  ..,,,,,,,,,,,,,,,,,,,,,,,,,,,,********////*/**********,,,,,,,..,..,,............
                ...,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,..,,...,.,,,...................
                     ......,...,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,...,,..............
                               ...,..,,.,,..,,,.,,,,,,,,,,,,,,,,,,,,,,,,,,,,..,..,........                                                        
""")
    
#
#
# Start of game
#
start_game()


