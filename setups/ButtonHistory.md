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

_______________________________

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



#### Troubleshooting
If you are having difficuties setting this up, a good check is to go into your Custom Variables tab and check to see if your entries are actually making it into your variable.

You should be able to press a button and see your JSON object update with `button name: true`

<img width="1823" height="945" alt="Screenshot 2026-04-05 at 4 28 10 PM" src="https://github.com/user-attachments/assets/78849d85-c224-4976-8a88-4e266fa70414" />

_______________________________


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

___________________________________________

## 4) Clear history (cleanup)

Using the same logic as above, create an action:

- `internal: Custom Variable: Set with Expression`

Use it to reset the object and wipe button history.

If Companion restarts, history is lost unless the variable is persisted.  
To keep values across reloads, enable **Persist value** on the custom variable.

Persisted values must be cleared manually using the cleanup method above.

___________________________
## 5. Advanced

### Creating local variables to utilize the history in other logic

Instead of copying the boolean check each time, you create a local variable named `hasHistory` for example

we will input our boolean check logic like we did in the previous step

``` 
page = $(this:page);
row = $(this:row);
col = $(this:column);

key = `internal:b_text_${page}_${row}_${col}`;
track = getVariable(key);

obj = $(custom:playedTracks) || {};
obj[track] = true;
obj 
```

Now we can do a simple check across other actions or formatting

Back on the feedback tab we can do a simple check:
does our button have button history?
`$(local:hasHistory) === true`

this does the same logic as before but is easier to read and trouble shoot.

Now we can go back to actions page and build and IF action
`internal: Logic: If statement`

#### When true
`$(local:hasHistory) === true`

#### Then

<insert your logic here for when the button has been already pressed>
you could skip over, you could have it move to the next cue in line, or whatever suits your workflow

#### Else
<insert your normal behavior>
This is normal button activity that triggers when the key is not in our button history
