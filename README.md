# made with love by Rakshit
import random
import urllib.request
import io

def load_names_from_url(url):
    """Download and return list of names from the CMU male names dataset."""
    try:
        response = urllib.request.urlopen(url)
        names_text = io.TextIOWrapper(response, encoding='utf-8').read()
        names = [line.strip() for line in names_text.splitlines() if line.strip() and not line.startswith('#')]
        return [name for name in names if name.isalpha()]  # Filter to alphabetic names only
    except Exception as e:
        print(f"Error loading names: {e}")
        return []

def generate_names(starting_letter, length, all_names):
    """Filter and randomly select names matching criteria."""
    matching_names = [name for name in all_names if name.lower().startswith(starting_letter.lower()) and len(name) == length]
    if not matching_names:
        return []  # No matches found
    return random.sample(matching_names, min(5, len(matching_names)))  # Up to 5 unique random picks

# Load dataset (CMU common male names, ~2940 entries)
url = "https://www.cs.cmu.edu/Groups/AI/util/areas/nlp/corpora/names/male.txt"
print("Loading real baby names dataset...")
all_names = load_names_from_url(url)
print(f"Loaded {len(all_names)} names from source.[web:17]")

# Get user input
letter = input("Enter the starting letter: ").strip()
try:
    length = int(input("Enter the desired name length: ").strip())
    if length < 2:
        print("Name length must be at least 2.")
    else:
        suggestions = generate_names(letter, length, all_names)
        if suggestions:
            print(f"\nReal name suggestions starting with '{letter}' (length {length}):")
            for i, name in enumerate(suggestions, 1):
                print(f"{i}. {name}")
        else:
            print(f"No real names found starting with '{letter}' of exact length {length}. Try another combo!")
except ValueError:
    print("Enter a valid number for length.")
