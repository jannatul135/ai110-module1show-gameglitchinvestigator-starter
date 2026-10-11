# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- **Game purpose:** A Streamlit number-guessing game where the player has a limited number of attempts to find a secret number, with Higher/Lower hints after each guess.
- **Bugs I found:** The hint messages were swapped ("Go LOWER!" for a low guess), the secret was converted to a string on even attempts (so comparisons were alphabetical), the secret ignored the difficulty range, the attempts counter was off by one, and the game state didn't reset when difficulty changed.
- **Fixes I applied:** I moved `check_guess` into `logic_utils.py` and put the hint messages on the correct branches, then removed the string conversion of the secret in `app.py`. I used Claude Code to make the changes and write tests, and I reviewed each diff before accepting it. I left the difficulty range, attempts counter, and state-reset bugs documented in `reflection.md` instead of fixing them.

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. I start a new game on Normal difficulty (range 1 to 100, 8 attempts).
2. I enter a guess of 1 and the game shows "Go HIGHER!".
3. I enter a guess of 55 and the game shows "Go LOWER!".
4. I enter a guess of 40 and the game shows "Go HIGHER!", so the secret is between 41 and 54.
5. The hints now stay correct on both odd and even attempts, including the very first guess after loading the page.
6. A correct guess ends the game with a win message.
7. When I run out of attempts, the game shows "Game over" and asks me to start a new game.

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

## 🧪 Test Results

```
$ python -m pytest
======================== test session starts ========================
collected 5 items

tests/test_game_logic.py .....                                [100%]

========================= 5 passed in 0.04s =========================
```
