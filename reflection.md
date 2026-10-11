# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- When I first ran it, the game loaded fine with Normal difficulty and 8 attempts. I guessed 1 and it told me "Go LOWER!", which made no sense. I switched to Easy (range 1 to 20) and the debug panel showed a secret of 98. After switching to Hard, the banner said "Attempts left: -3". That's when I realized the hints, the range, and the attempts counter were all broken. 
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
  - Concrete bugs: the hint messages were swapped ("Go LOWER!" for a low guess), the secret was converted to a string on even attempts, the secret ignored the difficulty range, and the attempts counter was off by one.

**AI explanation (Claude Code, @app.py):** I asked Claude Code why guessing below the secret says "Go LOWER!". It explained that in `check_guess` the hint messages are attached to the wrong branches: "Too Low" returns "Go LOWER!" and "Too High" returns "Go HIGHER!". It also found a second bug: on even-numbered attempts the submit block converts the secret to a string, so the comparison becomes alphabetical (e.g. "9" > "50" is True). Because `attempts` starts at 1 and is incremented before the check, the very first guess is already attempt 2, so it uses the string path.

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input Used | Expected Behavior | Actual Behavior | Console Output / Error | Suspected Code Location |
|---|---|---|---|---|
| Guess 1 (Normal, 1 to 100) | "Go HIGHER" | "Go LOWER!" | none | app.py, check_guess (hint messages swapped) |
| First guess on a freshly loaded game, e.g. 9 vs secret 50 | "Too Low" using a numeric comparison | Compared as text, so "9" > "50" returns "Too High" | none | app.py, submit block (secret = str(...) on even attempts) and the except TypeError branch in check_guess |
| Easy difficulty, new game | Secret between 1 and 20 | Secret was 98 | none | app.py, if "secret" not in st.session_state and the New Game button (randint(1, 100)) |
| Fresh Normal game, no guesses yet | "Attempts left: 8" | "Attempts left: 7" | none | app.py, attempts = 1 at startup (New Game sets it to 0) |
| Switched Normal to Hard mid-game | Fresh game with 5 attempts | "Attempts left: -3" | none | app.py, no reset when difficulty changes |

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Claude Code in VS Code (to explain the bug, move and fix the code, and write tests), plus Claude in a chat window for planning and reviewing diffs.
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- One correct suggestion was moving the check_guess logic into logic_utils.py and fixing the reversed HIGHER and LOWER messages. I verified this by running the tests, where all 5 tests passed, and by testing the Streamlit game myself. 
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.
- One suggestion I did not accept as written was when Claude said the tests passed, because running plain pytest on my computer gave me an import error. Instead of assuming the tests were working, I investigated the problem and changed how I ran them. I used python -m pytest and verified my version when all 5 tests passed.



---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
-  I decided a bug was really fixed only after both the automated tests and the actual game behaved correctly. 
  and what it showed you about your code.
  - I ran python -m pytest and all 5 tests passed, including tests that checked guesses above and below the secret number. I also manually tested the game by guessing different numbers and confirmed that the HIGHER and LOWER hints were consistent. I fixed the two bugs required for the assignment and left the other bugs, such as the difficulty range and attempts counter issues, documented instead of expanding the scope of the project.
- Did AI help you design or understand any tests? How?
- AI helped create and explain some of the tests, but I still ran them myself to verify the results.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- I learned that Streamlit reruns the whole Python script from top to bottom whenever the user interacts with something like a button. Session state is used to save values that need to stay the same between those reruns, such as the secret number, attempts, and game history. This helped me understand why changing the difficulty did not automatically create a new secret number. The old value was still stored in st.session_state, so it stayed there until the program specifically changed or reset it.



---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
  - One habit I want to keep using is testing AI-generated fixes myself instead of assuming they are correct. In this project, even when the AI said something worked, I ran pytest and played the game manually to make sure. 
- What is one thing you would do differently next time you work with AI on a coding task?
- Next time, I would give the AI a smaller and more specific problem at a time so its suggestions are easier to review. 
- In one or two sentences, describe how this project changed the way you think about AI generated code.
- This project showed me that AI-generated code can be useful and save time, but I still need to understand, test, and verify the code before using it.