✅ Goal of the Program:
It’s a fun terminal-based game that:

Scrapes quotes from http://quotes.toscrape.com

Picks one random quote

Asks the user to guess the author

Gives hints after wrong guesses

Ends when the user guesses right or runs out of attempts

🧠 Full Code Explanation:
python
Copy
Edit
import requests
from bs4 import BeautifulSoup
from time import sleep
from random import choice
📦 requests: To send HTTP requests to the quote website.

🥣 BeautifulSoup: To parse HTML content and extract data.

⏱️ sleep: To pause scraping between pages (polite scraping).

🎲 choice: To randomly select one quote.

1. Scrape all quotes:
python
Copy
Edit
all_quotes = []
base_url = "http://quotes.toscrape.com/"
url = "/page/1"
all_quotes: List to store all quote dictionaries.

base_url: Static part of the site URL.

url: Tracks current page being scraped.

2. Loop through all pages:
python
Copy
Edit
while url:
    res = requests.get(f"{base_url}{url}")
    print(f"Now Scraping {base_url}{url}")
    soup = BeautifulSoup(res.text, "html.parser")
Downloads each page of quotes.

Parses the page using html.parser.

3. Extract quote details:
python
Copy
Edit
    quotes = soup.find_all(class_="quote")

    for quote in quotes:
        all_quotes.append({
            "text": quote.find(class_="text").get_text(),
            "author": quote.find(class_="author").get_text(),
            "bio-link": quote.find("a")["href"]
        })
From each quote block, it extracts:

The quote text

The author’s name

The link to author’s bio page

Each quote is stored as a dictionary in the all_quotes list.

4. Move to next page (if exists):
python
Copy
Edit
    next_btn = soup.find(class_="next")
    url = next_btn.find("a")["href"] if next_btn else None
    sleep(2)
Checks for a Next button and updates url accordingly.

sleep(2) slows down the request to avoid overloading the site.

5. Start the Game:
python
Copy
Edit
quote = choice(all_quotes)
remaining_guesses = 4
print("Here's a quote:  ")
print(quote["text"])
Picks a random quote.

Shows it to the user.

6. User Guess Loop:
python
Copy
Edit
guess = ''
while guess.lower() != quote["author"].lower() and remaining_guesses > 0:
    guess = input(f"Who said this quote? Guesses remaining {remaining_guesses}")
User is prompted to guess the author.

It runs until:

User guesses correctly OR

Runs out of attempts.

7. Check Guess and Give Hints:
python
Copy
Edit
    if guess == quote["author"]:
        print("CONGRATULATIONS!!! YOU GOT IT RIGHT")
        break
✅ If the guess is correct, it congratulates the user and exits the loop.

8. Wrong Guesses → Show Hints:
python
Copy
Edit
    remaining_guesses -= 1
Deducts one guess.

🧠 Hint 1: After 1 wrong guess
python
Copy
Edit
    if remaining_guesses == 3:
        res = requests.get(f"{base_url}{quote['bio-link']}")
        soup = BeautifulSoup(res.text, "html.parser")
        birth_date = soup.find(class_="author-born-date").get_text()
        birth_place = soup.find(class_="author-born-location").get_text()
        print(f"Here's a hint: The author was born on {birth_date}{birth_place}")
Goes to the author’s bio page.

Extracts and shows:

Birth date

Birth place

🧠 Hint 2: After 2 wrong guesses
python
Copy
Edit
    elif remaining_guesses == 2:
        print(f"Here's a hint: The author's first name starts with: {quote['author'][0]}")
Gives the first initial of the author’s name.

🧠 Hint 3: After 3 wrong guesses
python
Copy
Edit
    elif remaining_guesses == 1:
        last_initial = quote["author"].split(" ")[1][0]
        print(f"Here's a hint: The author's last name starts with: {last_initial}")
Gives the last name’s first letter

❌ Game Over:
python
Copy
Edit
    else:
        print(f"Sorry, you ran out of guesses. The answer was {quote['author']}")
Reveals the answer after 4 incorrect attempts.

✅ Example Output
pgsql
Copy
Edit
Here's a quote:
“Be yourself; everyone else is already taken.”
Who said this quote? Guesses remaining 4
> Steve Jobs
Here's a hint: The author was born on October 16, 1854in Dublin, Ireland
Guesses remaining 3
> William Shakespeare
Here's a hint: The author's first name starts with: O
Guesses remaining 2
> Oscar Wilde
🎉 CONGRATULATIONS!!! YOU GOT IT RIGHT
