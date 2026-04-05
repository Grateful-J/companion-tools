# Button History

Track previously pressed buttons and style them differently in Companion.

## 1) Create the custom variable

Create a custom variable (example: `playedTracks`).

- Set **Startup Value** from text (`"T"`) to an empty object (`{}`).
- Set **Current Value** to an empty object (`{}`) as well.


```js
{}
```
<img width="1335" height="945" alt="Screenshot 2026-04-05 at 3 29 27 PM" src="https://github.com/user-attachments/assets/d6be26c0-5fae-4821-a4d6-35106034aab7" />

## 2) Save button presses to history

At the end of your action chain, add:

- `internal: Custom Variable: Set with Expression`
- Select the custom variable you created in the previous step.
- Use this expression to store the button key as `true`.

```js
page = $(this:page);
row = $(this:row);
col = $(this:column);

key = `internal:b_text_${page}_${row}_${col}`;
track = getVariable(key);

obj = $(custom:playedTracks) || {};
obj[track] = true;
obj 
```
<img width="1335" height="945" alt="Screenshot 2026-04-05 at 3 46 57 PM" src="https://github.com/user-attachments/assets/f043e4a6-f750-423d-b914-45101c8257c1" />

## 3) Format pressed buttons with feedbacks

Add:

- `internal: Variable: Check boolean expression`

This runs a true/false check. If the button key exists in your stored object, it applies the formatting you choose.

Use this expression:

```js
page = $(this:page);
row = $(this:row);
col = $(this:column);

key = `internal:b_text_${page}_${row}_${col}`;
track = getVariable(key);

obj = $(custom:playedTracks) || {};

obj[track] === true
```

Then choose the cell formatting you want (for example, a gray background to indicate a track has already been played).
<img width="1335" height="945" alt="Screenshot 2026-04-05 at 3 47 25 PM" src="https://github.com/user-attachments/assets/e18921d0-b5fc-4740-8b1b-b8b89b83aa40" />

> TODO: Check whether this can prevent re-toggling an accidentally pressed track and instead advance to the next cell.

## 4) Clear history (cleanup)

Using the same logic as above, create an action:

- `internal: Custom Variable: Set with Expression`

Use it to reset the object and wipe button history.

If Companion restarts, history is lost unless the variable is persisted.  
To keep values across reloads, enable **Persist value** on the custom variable.

Persisted values must be cleared manually using the cleanup method above.
