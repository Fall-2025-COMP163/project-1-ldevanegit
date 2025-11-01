[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/JTXl4WMa)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21381116&assignment_repo_type=AssignmentRepo)
# COMP 163 - Project 1: Character Creator & Chronicles
# 🎯 Project Overview

Build a text-based RPG character creation and story progression system that demonstrates mastery of functions and file I/O operations.

# Required Functions 
Complete these functions in project1_starter.py:

create_character(name, character_class) - Create new character

calculate_stats(character_class, level) - Calculate character stats

save_character(character, filename) - Save character to file

load_character(filename) - Load character from file

display_character(character) - Display character info

level_up(character) - Increase character level

# 🎭 Character Classes
Implement these character classes with unique stat distributions:


Warrior: High strength, low magic, high health

Mage: Low strength, high magic, medium health

Rogue: Medium strength, medium magic, low health

Cleric: Medium strength, high magic, high health

# 📁 Required File Format
Your save_character() function must create files in this exact format:

Character Name: [name]

Class: [class]

Level: [level]

Strength: [strength]

Magic: [magic]

Health: [health]

Gold: [gold]


# Run specific test file
python -m pytest tests/test_character_creation.py -v

# Test your main program
python project1_starter.py

GitHub Testing:

After pushing your code, check the Actions tab to see automated test results:

✅ Green checkmarks = tests passed
❌ Red X's = tests failed (click to see details)

# ⚠️ Important Notes
Protected Files

DO NOT MODIFY files in the tests/ directory

DO NOT MODIFY files in the .github/ directory

Modifying protected files will result in automatic academic integrity violation

# AI Usage Policy

✅ Allowed: AI assistance for implementation, debugging, learning

📝 Required: Document AI usage in code comments

🎯 Must be able to explain: Every line of code during interview

# 📝 Submission Checklist

 All required functions implemented
 
 Code passes all automated tests
 
 README updated with your documentation
 
 Interview scheduled and completed
 
 AI usage documented in code comments

# 🏆 Grading

Implementation (70%): Function correctness, file operations, error handling

Interview (30%): Code explanation and live coding challenge
#MYCODEBELOW  I DIDN'T KNOW IF YOU WANTED TO REPLACE THE ABOVE OR LEAVE IT
"""
COMP 163 - Project 1: Character Creator & Saving/Loading
Name: Lemanuel Devane
Date: 10-30
AI Usage: ChatGPT (GPT-5) helped with organization, finding syntax errors as well as creating dictionary for loading characters and developing code for main program in case a file with information was not given.
"""

import random
#was part of fun one i did for the shell of this program
import os


# calculate_stats function
def calculate_stats(class_name, level):
    """
    Calculates base stats based on class and level.
    Each level adds +5 to all stats.
    """
    class_name = class_name.capitalize()
    #incase a class name is given but isn't capitalized
    base_stats = {
        "Warrior": (15, 8, 70),
        "Mage": (7, 20, 90),
        "Rogue": (12, 10, 110),
        "Cleric": (10, 21, 100)
    }

    if class_name in base_stats:
        strength, magic, health = base_stats[class_name]
    else:
        strength, magic, health = (10, 10, 100)

    # Add +5 to each stat for every level up
    strength += (level - 1) * 5
    magic += (level - 1) * 5
    health += (level - 1) * 5

    return strength, magic, health


# create_character function
def create_character(name, class_name):
    """
    Creates a character with the given class.
    Returns: character dictionary or None if invalid.
    """
    class_name = class_name.capitalize()
    valid_classes = ["Warrior", "Mage", "Rogue", "Cleric"]

    if class_name not in valid_classes:
        return None
#default level
    level = 1
    strength, magic, health = calculate_stats(class_name, level)

    character = {
        "name": name,
        "class": class_name,
        "level": level,
        "strength": strength,
        "magic": magic,
        "health": health,
        "gold": 100
    }

    return character


# display_character function
def display_character(character):
    """Prints formatted character sheet."""
    print("\n=== CHARACTER SHEET ===")
    print(f"Name: {character['name']}")
    print(f"Class: {character['class']}")
    print(f"Level: {character['level']}")
    print(f"Strength: {character['strength']}")
    print(f"Magic: {character['magic']}")
    print(f"Health: {character['health']}")
    print(f"Gold: {character['gold']}")
    print("\n")


# save_character function
#Chatgpt5assistance with dir_name to help gracefully deal with nonexistent files
def save_character(character, filename):
    """
    Saves the character to a text file.
    Returns True if successful, False if filename is empty.
    """
    if filename == "":
        print("No filename entered. Character not saved.")
        return False
    
    #Chatgpt5
    dir_name = os.path.dirname(filename)
    if dir_name != "" and not os.path.exists(dir_name):
        print(f"Directory '{dir_name}' does not exist. Character not saved.")
        return False
    with open(filename, "w") as file:
            file.write(f"Character Name: {character['name']}\n")
            file.write(f"Class: {character['class']}\n")
            file.write(f"Level: {character['level']}\n")
            file.write(f"Strength: {character['strength']}\n")
            file.write(f"Magic: {character['magic']}\n")
            file.write(f"Health: {character['health']}\n")
            file.write(f"Gold: {character['gold']}\n")
    return True


# load_character function
def load_character(filename):
    """
    Loads character from text file.
    Returns character dictionary if file exists, None otherwise.
    """
    if not os.path.exists(filename):
        print("File not found. Please check the name and try again.")
        return None
    else:
        with open(filename, "r") as file:
            lines = file.readlines()

        data = {}
        for line in lines:
            parts = line.strip().split(": ")
            if len(parts) == 2:
                key = parts[0]
                value = parts[1]
                data[key] = value

        character = {
            "name": data["Character Name"],
            "class": data["Class"],
            "level": int(data["Level"]),
            "strength": int(data["Strength"]),
            "magic": int(data["Magic"]),
            "health": int(data["Health"]),
            "gold": int(data["Gold"])
        }
        return character


# level_up function
def level_up(character):
    """Increases level and recalculates stats."""
    character["level"] += 1
    strength, magic, health = calculate_stats(character["class"], character["level"])
    character["strength"] = strength
    character["magic"] = magic
    character["health"] = health
    print(f"\n{character['name']} has leveled up to Level {character['level']}!\n")


# MAIN PROGRAM
if __name__ == "__main__":
    print("=== SOLO LEVELING CHARACTER CREATOR ===\n")
    name = input("Enter your character's name: ")
    class_name = input("Enter your class (Warrior, Mage, Rogue, Cleric): ").capitalize()

    # Create character
    char = create_character(name, class_name)
    if char is None:
        print("Invalid class. Please restart and enter a valid one.")
        exit()

    display_character(char)

    # Main loop
    running = True
    while running:
        print("Options:")
        print("1. Level Up")
        print("2. Save Character")
        print("3. Load Character")
        print("4. Display Character")
        print("5. Exit")
        choice = input("Choose an option (1-5): ")

        if choice == "1":
            confirm = input("Level up? (y/n): ").lower()
            if confirm == "y":
                level_up(char)
                display_character(char)
        elif choice == "2":
            filename = input("Enter filename to save (e.g., my_char.txt): ")
            success = save_character(char, filename)
            if success:
                print("Character saved successfully!\n")
        elif choice == "3":
            filename = input("Enter filename to load: ")
            loaded = load_character(filename)
            if loaded is not None:
                char = loaded
                print("Character loaded!\n")
                display_character(char)
        elif choice == "4":
            display_character(char)
        elif choice == "5":
            print("Goodbye, Hunter.")
            running = False
        else:
            print("Invalid option. Try again.\n")
