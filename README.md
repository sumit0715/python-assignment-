# Text-Based Adventure Game
# Player navigates rooms, makes choices, and collects items to win.

# Define rooms as a dictionary of dictionaries
rooms = {
    'start': {
        'description': 'You are in a dark forest. Paths lead north to a cave and east to a river.',
        'choices': {
            'north': 'cave',
            'east': 'river'
        },
        'item': None
    },
    'cave': {
        'description': 'You enter a cave. A shiny key lies on the ground. You can go south back to the forest.',
        'choices': {
            'south': 'start',
            'take key': 'cave'  # Stay in cave but take item
        },
        'item': 'key'
    },
    'river': {
        'description': 'You reach a river. A boat is here, but you need a key to unlock it. Go west back to the forest.',
        'choices': {
            'west': 'start',
            'unlock boat': 'river'  # Check for key
        },
        'item': None
    }
}

# Global inventory
inventory = []

# Function to display room and get choice
def play_room(room_name):
    room = rooms[room_name]
    print(f"\n{room['description']}")
    
    if room['item'] and room['item'] not in inventory:
        print(f"You see a {room['item']} here.")
    
    print("Choices:")
    for choice in room['choices']:
        print(f"- {choice}")
    
    choice = input("What do you do? ").strip().lower()
    
    if choice in room['choices']:
        if choice == 'take key' and room['item'] == 'key':
            inventory.append('key')
            print("You took the key!")
        elif choice == 'unlock boat':
            if 'key' in inventory:
                print("You unlocked the boat and escaped with treasure! You win!")
                return 'win'
            else:
                print("You need a key to unlock the boat.")
        return room['choices'][choice]
    else:
        print("Invalid choice. Try again.")
        return room_name

# Main game loop
def main():
    current_room = 'start'
    print("Welcome to the Adventure Game! Find the treasure by exploring.")
    
    while True:
        current_room = play_room(current_room)
        if current_room == 'win':
            break
        # Add a lose condition if stuck (e.g., after 10 moves), but keep simple for now

if __name__ == "__main__":
    main()
