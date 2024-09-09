from fastapi import FastAPI
import random

app = FastAPI()

# Sample list of Pokémon names
pokemon_list = [
    "Bulbasaur", "Charmander", "Squirtle", "Pikachu", "Eevee", "Snorlax", "Jigglypuff", "Meowth", "Psyduck",
    "Machamp", "Gengar", "Lapras", "Ditto", "Articuno", "Zapdos", "Moltres", "Dratini", "Mewtwo", "Mew", 
    "Chikorita", "Cyndaquil", "Totodile", "Lugia", "Ho-Oh", "Celebi", "Torchic", "Mudkip", "Treecko", "Latias", 
    "Latios", "Groudon", "Kyogre", "Rayquaza", "Deoxys", "Lucario", "Darkrai", "Zoroark", "Zygarde", "Solgaleo", 
    "Lunala", "Zacian", "Zamazenta"
]

# Sample list of trainer names
trainer_list = [
    "Ash", "Misty", "Brock", "Gary", "Serena", "Dawn", "May", "Iris", "Cynthia", "Steven", "Red", "Blue", "Lance"
]

# Sample list of attack names
attack_list = [
    "Thunderbolt", "Flamethrower", "Hydro Pump", "Hyper Beam", "Solar Beam", "Earthquake", "Psychic", 
    "Dragon Claw", "Ice Beam", "Shadow Ball", "Iron Tail", "Focus Punch", "Rock Slide", "Aerial Ace"
]

# Sample list of items trainers can use
item_list = [
    {"name": "Potion", "effect": "heal", "amount": 30},
    {"name": "Super Potion", "effect": "heal", "amount": 50},
    {"name": "Attack Boost", "effect": "boost_attack", "amount": 20},
    {"name": "Defense Boost", "effect": "boost_defense", "amount": 20}
]

# Function to create a random trainer with Pokémon and items
def generate_trainer():
    trainer_name = random.choice(trainer_list)
    trainer_level = random.randint(10, 100)
    pokemon_team = []
    
    # Each trainer gets 5 random Pokémon with HP, Attack, and Defense values
    for pokemon in random.sample(pokemon_list, 5):
        hp = random.randint(50, 150)
        attack = random.randint(30, 120)
        defense = random.randint(30, 100)
        pokemon_team.append({
            "name": pokemon,
            "hp": hp,
            "attack": attack,
            "defense": defense
        })
    
    # Each trainer gets 3 random items from the item list
    items = random.sample(item_list, 3)
    
    return {
        "trainer_name": trainer_name,
        "level": trainer_level,
        "pokemon": pokemon_team,
        "items": items
    }

# Function to apply an item during the battle
def use_item(trainer, pokemon):
    if trainer["items"]:
        item = random.choice(trainer["items"])
        trainer["items"].remove(item)  # Remove the used item from the trainer's items
        
        if item["effect"] == "heal":
            pokemon["hp"] += item["amount"]
            pokemon["hp"] = min(pokemon["hp"], 150)  # HP cannot exceed 150
            return f'{trainer["trainer_name"]} used {item["name"]} on {pokemon["name"]}, restoring {item["amount"]} HP!'
        
        if item["effect"] == "boost_attack":
            pokemon["attack"] += item["amount"]
            return f'{trainer["trainer_name"]} used {item["name"]}, boosting {pokemon["name"]}\'s attack by {item["amount"]}!'
        
        if item["effect"] == "boost_defense":
            pokemon["defense"] += item["amount"]
            return f'{trainer["trainer_name"]} used {item["name"]}, boosting {pokemon["name"]}\'s defense by {item["amount"]}!'
    return None

# Function to simulate a battle between two trainers with the ability to switch Pokémon and use items
def battle(trainer1, trainer2):
    battle_log = []
    trainer1_wins = 0
    trainer2_wins = 0

    # Remaining Pokémon list for each trainer
    trainer1_pokemon_remaining = trainer1["pokemon"].copy()
    trainer2_pokemon_remaining = trainer2["pokemon"].copy()

    for round_number in range(5):
        if not trainer1_pokemon_remaining or not trainer2_pokemon_remaining:
            break  # If a trainer runs out of Pokémon, stop the battle

        # Trainer 1 and Trainer 2 switch Pokémon at the start of each round
        pokemon1 = random.choice(trainer1_pokemon_remaining)
        pokemon2 = random.choice(trainer2_pokemon_remaining)

        # Each trainer has a chance to use an item
        item_log1 = use_item(trainer1, pokemon1)
        item_log2 = use_item(trainer2, pokemon2)

        if item_log1:
            battle_log.append(item_log1)
        if item_log2:
            battle_log.append(item_log2)

        # Trainer yells the Pokémon's name and attack
        attack1 = random.choice(attack_list)
        attack2 = random.choice(attack_list)

        trainer1_yell = f'{trainer1["trainer_name"]} yells: "{pokemon1["name"]}, use {attack1}!"'
        trainer2_yell = f'{trainer2["trainer_name"]} yells: "{pokemon2["name"]}, use {attack2}!"'

        battle_log.append(trainer1_yell)
        battle_log.append(trainer2_yell)

        # Calculate the damage considering attack and defense
        damage_to_pokemon2 = max(pokemon1["attack"] - pokemon2["defense"], 0)
        damage_to_pokemon1 = max(pokemon2["attack"] - pokemon1["defense"], 0)

        if damage_to_pokemon2 > damage_to_pokemon1:
            winner = f'{pokemon1["name"]}\'s {attack1} was more effective! {trainer1["trainer_name"]}\'s {pokemon1["name"]} wins!'
            trainer1_wins += 1
            trainer2_pokemon_remaining.remove(pokemon2)  # Remove fainted Pokémon from Trainer 2's list
        elif damage_to_pokemon1 > damage_to_pokemon2:
            winner = f'{pokemon2["name"]}\'s {attack2} was more effective! {trainer2["trainer_name"]}\'s {pokemon2["name"]} wins!'
            trainer2_wins += 1
            trainer1_pokemon_remaining.remove(pokemon1)  # Remove fainted Pokémon from Trainer 1's list
        else:
            winner = f'Both {pokemon1["name"]} and {pokemon2["name"]} were equally strong! It\'s a tie!'
        
        battle_log.append(winner)

    # Determine the overall winner based on Pokémon victories
    if trainer1_wins > trainer2_wins:
        overall_winner = f'{trainer1["trainer_name"]} wins the battle!'
    elif trainer2_wins > trainer1_wins:
        overall_winner = f'{trainer2["trainer_name"]} wins the battle!'
    else:
        overall_winner = 'The battle is a tie!'

    # Adding a funny description to the battle outcome
    funny_description = random.choice([
        "This was the most dramatic battle since Pikachu forgot how to use Thunderbolt.",
        "The crowd was speechless. Well, except for that one guy who kept shouting 'I choose you!'",
        "It was a fierce showdown, but it seems one trainer brought more snacks to share, influencing the outcome.",
        "Rumor has it, the Pokémon are still arguing over who really won.",
        "Both trainers had their moments, but only one remembered to give their Pokémon a pep talk!"
    ])

    return {"battle_log": battle_log, "overall_winner": overall_winner, "funny_description": funny_description}

@app.get("/battle")
def start_battle():
    # Generate two random trainers
    trainer1 = generate_trainer()
    trainer2 = generate_trainer()

    # Simulate the battle
    battle_result = battle(trainer1, trainer2)
    
    return {
        "trainer1": trainer1,
        "trainer2": trainer2,
        "battle_result": battle_result
    }
