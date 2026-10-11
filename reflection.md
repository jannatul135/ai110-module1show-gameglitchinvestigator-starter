# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- - When I first ran it, the game loaded fine with Normal difficulty and 8 attempts. I guessed 1 and it told me "Go LOWER!", which made no sense. I switched to Easy (range 1 to 20) and the debug panel showed a secret of 98. After switching to Hard, the banner said "Attempts left: -3". That's when I realized the hints, the range, and the attempts counter were all broken. 
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

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
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
