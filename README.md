Python programming

Task 1: Dice Rolling Game Write a program that simulates rolling a pair of dice. Each time the program runs, itshould randomly generate two numbers between 1 and 6 (inclusive), representingthe result of each die. The program should then display the results and ask if theuser would like to roll again. Modify the program so the user can specify how manydice they want to roll. Add a feature that keeps track of how many times the userhas rolled the dice during the session. This will require a counter that incrementseach time the dice are rolled.
import random

def roll_dice(num_dice):
    """Rolls `num_dice` dice and returns a list of results."""
    return [random.randint(1, 6) for _ in range(num_dice)]

def main():
    print("🎲 Welcome to the Dice Rolling Game!")
    roll_count = 0

    while True:
        try:
            num_dice = int(input("How many dice would you like to roll? "))
            if num_dice <= 0:
                print("Please enter a positive number.")
                continue
        except ValueError:
            print("Invalid input. Please enter a number.")
            continue

        results = roll_dice(num_dice)
        roll_count += 1

        print(f"Roll #{roll_count} Results: ", end="")
        print(" | ".join(f"Die {i+1}: {res}" for i, res in enumerate(results)))

        choice = input("Would you like to roll again? (yes/no): ").strip().lower()
        if choice not in ('yes', 'y'):
            print(f"🎉 You rolled the dice {roll_count} time(s). Thanks for playing!")
            break

if __name__ == "__main__":
    main()

Task 2 Code
Write a program to create a simple To-Do List application. The program allows users
to add tasks, view the current list of tasks, and remove tasks once they are
completed. Add an option in the menu that allows users to mark tasks as
completed instead of removing them. Completed tasks can be displayed separately
or removed from the list. Implement functionality that saves the list to a file and
loads it when the program starts. This way, users can maintain their to-do list across
different sessions. Allow users to categorize tasks (e.g., Work, Personal) and filter
the list based on these categories. This adds an organizational element to the to-do
list.
import json
import os

FILE_NAME = "todo_simple.json"

def load_tasks():
    if os.path.exists(FILE_NAME):
        with open(FILE_NAME, "r") as f:
            return json.load(f)
    return []

def save_tasks(tasks):
    with open(FILE_NAME, "w") as f:
        json.dump(tasks, f, indent=4)

def show_tasks(tasks):
    if not tasks:
        print("No tasks found.")
    else:
        for i, task in enumerate(tasks, 1):
            status = "✅" if task["done"] else "❌"
            print(f"{i}. [{status}] {task['title']}")

def add_task(tasks):
    title = input("Enter task: ")
    tasks.append({"title": title, "done": False})
    print("Task added.")

def complete_task(tasks):
    show_tasks(tasks)
    try:
        num = int(input("Task number to mark as completed: "))
        if 1 <= num <= len(tasks):
            tasks[num-1]["done"] = True
            print("Task marked as completed.")
        else:
            print("Invalid number.")
    except ValueError:
        print("Please enter a number.")

def delete_task(tasks):
    show_tasks(tasks)
    try:
        num = int(input("Task number to delete: "))
        if 1 <= num <= len(tasks):
            removed = tasks.pop(num-1)
            print(f"Deleted: {removed['title']}")
        else:
            print("Invalid number.")
    except ValueError:
        print("Please enter a number.")

def main():
    tasks = load_tasks()
    while True:
        print("\n--- To-Do List ---")
        print("1. View tasks")
        print("2. Add task")
        print("3. Mark task as completed")
        print("4. Delete task")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            show_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            complete_task(tasks)
        elif choice == "4":
            delete_task(tasks)
        elif choice == "5":
            save_tasks(tasks)
            print("Goodbye! Tasks saved.")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
