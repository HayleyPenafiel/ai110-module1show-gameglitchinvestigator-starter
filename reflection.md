# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?
- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
  
  Answer:
  1. Bug 1: "New Game" button does not reset the game.
    - Expected: Clicking the "New Game" button should restart the game by generating a new random number and resetting the score/attempts.
    - Actual: The game does not reset when the button is clicked, and the previous state continues.

  2. Bug 2: Incorrect hint logic.
    - Expected: The hint should correctly indicate whether the guess should be higher or lower based on the target number.
    - Actual: The hint always says "lower," even when the guess is already at the minimum value (1), which is not possible given the game’s range.

  3. Bug 3: Game over triggers too early.
    - Expected: The "Game Over" message and correct answer should only appear after the final attempt is used.
    - Actual: The game ends one attempt early (on the second-to-last attempt instead of the last one).

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).


Answer:
I used Claude and ChatGPT

Bug 1: New Game does not fully reset

    One correct suggestion the AI gave me for Bug 1 was to refactor the New Game reset logic into a helper function called `new_game_state(low, high)` inside `logic_utils.py`. This was helpful because it separated the game logic from the UI code and ensured that all parts of the game state (attempts, score, status, and history) were reset properly. I verified this by reviewing the updated code and running the Streamlit app. After the fix, clicking "New Game" correctly reset the game and allowed me to play again.

    One incorrect or misleading suggestion the AI gave me was to set `attempts` to `0` in the New Game reset logic. While this might be part of fixing a different bug related to attempt counting, it did not match the current initialization logic in the app, which starts attempts at `1`. I recognized that this would mix two separate bugs together, so I chose not to apply that change at this stage. I verified this by comparing it to the existing code and ensuring consistency.
  
Bug 2: Incorrect hint direction

    The AI suggested updating the check_guess logic so that guesses above the secret return a lower hint and guesses below return a higher hint. This was correct because the original logic was reversed. I verified this by running the game and checking that the hints matched my guesses.

    One issue was that the AI initially did not fully apply the fix and left the incorrect logic in place. I noticed this by reviewing the code and testing the game, and then corrected the logic manually.

Bug 3: Game ends too early

    The AI suggested changing the attempt logic so the game starts with 0 attempts instead of 1. This was correct because the original code caused an off-by-one error and ended the game one turn early. I verified this by reviewing the attempt logic and manually testing the game.

    One misleading suggestion was moving the attempt increment so it only happened after a valid guess. I did not use that change because every submit should count as an attempt. I verified that by comparing the game behavior and keeping the increment at the top of the submit block. 

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

Bug 1: New Game does not fully reset

    To verify the fix, I tested the game manually in the browser using Streamlit. I played a round, then clicked the "New Game" button and checked that the score reset to 0, the guess history cleared, and the game was playable again. I also confirmed that the secret number followed the selected difficulty range.

    AI helped suggest creating a pytest test for the reset logic, which helped me understand what conditions should be verified, even though I mainly relied on manual testing to confirm the fix.

Bug 2: Incorrect hint direction

    I tested this fix manually by running the game in Streamlit. I compared my guesses with the secret shown in the debug panel and confirmed that the hints were now correct (too high → go lower, too low → go higher). This showed that the bug was fixed. 

Bug 3: Game ends too early

    I verified this fix manually by running the game in Streamlit and counting the number of guesses allowed. After the fix, the game ended exactly on the final allowed attempt instead of one turn early. AI helped me understand that the bug came from the attempt counter starting at 1 instead of 0.
---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

  Streamlit reruns the entire script every time the user interacts with the app. Session state is used to store values like attempts, score, and status so they don’t reset on each rerun. Without session state, the game would restart every time the user submits something.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

  One habit I want to reuse is adding small tests or manually checking behavior after each fix instead of changing too many things at once.

  Next time, I would be more careful with AI suggestions and review them before accepting, since some changes introduced new issues.

  This project showed me that AI can help speed things up, but it’s not always correct, so I still need to understand and verify the code myself. 
