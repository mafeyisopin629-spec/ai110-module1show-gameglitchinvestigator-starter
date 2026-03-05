# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?
Bug 1 — Hint direction is incorrect

Expected:
If my guess is too low, the game should tell me “Go HIGHER.”

Actual:
When I entered 0.5, the game told me “Go LOWER!”, which is incorrect because the guess is already lower than the secret number.

Steps to reproduce:

Start the game

Enter 0.5

Click Submit Guess

The hint says “Go LOWER!”

Bug 2 — Score becomes negative after invalid input

Expected:
If the player enters an invalid input like "abc", the game should show an error but not change the score.

Actual:
When I entered "abc", the game displayed “That is not a number.”, but the score decreased to -10.

Steps to reproduce:

Start the game

Enter "abc"

Submit guess

The game shows an error but the score becomes negative

Bug 3 — Secret number resets every time a new game starts

Expected:
The secret number should stay consistent during the game until the player presses New Game.

Actual:
Each time I reset the game, the secret number changes, which can confuse players because the target number keeps changing between rounds.

Steps to reproduce:

Start the game

Open Developer Debug Info

Press New Game

The secret number changes each time


- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)? Answer: I used Copilot

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
Answer:
One correct AI suggestion I received was when Copilot explained that the check_guess() function had the hint messages reversed. It pointed out that when the guess was greater than the secret, the code returned "Too High" but the message said "Go HIGHER!", and when the guess was lower than the secret, it returned "Too Low" but the message said "Go LOWER!". This suggestion was correct. I verified it by testing the game in Streamlit with low and high guesses and seeing the wrong hint directions. After I changed the messages, I ran python -m pytest and manually tested the game again with guesses like 99 and 10, and the hints matched correctly.

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
Answer:
One incorrect or misleading AI suggestion was that the logic in logic_utils.py was already the main issue to test immediately, when the file still contained NotImplementedError placeholders and the real app behavior was also tied to code in app.py. At first, this made it seem like I could just run tests after changing one function, but the tests failed because the helper functions had not actually been refactored yet. This suggestion was misleading because it did not fully account for the placeholder starter code and the mismatch between the app and test setup. I verified this by running python -m pytest, seeing the NotImplementedError failures, then searching logic_utils.py and finding that multiple functions still needed to be implemented. After replacing those placeholders with working code and updating the tests to match the return values, the tests passed.


---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
Answer
I verified my repairs in two ways: automated tests and manual testing in the live Streamlit app. First, I ran python -m pytest. The tests failed at first because logic_utils.py still had NotImplementedError placeholders and the tests expected check_guess() to return only a string instead of an outcome-message tuple. I fixed the helper functions, updated the tests, and ran python -m pytest again until all 3 tests passed.

- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  Answer:
  Second, I manually tested the game in Streamlit after making the fixes. I tried invalid input like 0.5, which correctly showed an error message. I tested a high guess like 99, and the game correctly responded with Go LOWER!. I tested a lower guess like 10, and the game correctly responded with Go HIGHER!. I also fixed the bug where the secret number could be converted into a string in app.py, which made the game logic more consistent. These checks confirmed that the repaired code worked both in the tests and in the actual game.

- Did AI help you design or understand any tests? How?
Answer:
Yes, AI helped me understand how the tests were supposed to work. Copilot suggested that the check_guess() function should return two values: the outcome (such as "Win", "Too High", or "Too Low") and a hint message. This helped me realize that the tests needed to unpack the return values using outcome, _message = check_guess(...). AI also suggested adding a test that checks the hint message for high guesses to ensure the hint direction is correct. I verified these suggestions by running python -m pytest and confirming that all tests passed after updating the test cases.
---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
Answer
The secret number kept changing in the original app because Streamlit reruns the entire script every time the user interacts with the page. Each rerun caused the secret number to be regenerated, so the game never kept the same value between guesses.

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
Answer
Streamlit “reruns” mean that whenever a user clicks a button or submits input, Streamlit executes the whole script from top to bottom again. Session state is used to store variables so they persist between these reruns.

- What change did you make that finally gave the game a stable secret number?
Answer
The change that fixed the issue was keeping the secret number inside st.session_state so that it only gets created once and stays the same for the entire game session.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?

  - This could be a testing habit, a prompting strategy, or a way you used Git.
Answer
One habit I want to reuse in future projects is writing and running tests with pytest to verify that code changes actually fix the problem. Testing helped confirm that the logic in the game worked correctly after the fixes.

- What is one thing you would do differently next time you work with AI on a coding task?
Answer
Next time I work with AI on a coding task, I would verify suggestions earlier by running tests or checking the code behavior before assuming the AI solution is correct.

- In one or two sentences, describe how this project changed the way you think about AI generated code.
Answer
This project helped me realize that AI can be a useful debugging assistant, but its suggestions still need to be reviewed and tested carefully before applying them.
