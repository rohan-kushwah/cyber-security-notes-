### **Asterisk (`*`)**

- The asterisk matches **zero or more** occurrences of the preceding character or group.
- Example: `grep 'ab*' file.txt`
    - This matches lines that contain "a" followed by zero or more "b"s (e.g., "a", "ab", "abb", "abbb", etc.).

### 2. **Period (`.`)**

- The period matches **any single character** except a newline.
- Example: `grep 'a.b' file.txt`
    - This matches lines that contain "a" followed by any single character and then "b" (e.g., "aab", "acb", etc.).

### 3. **Caret (`^`)**

- The caret matches the **beginning** of a line.
- Example: `grep '^start' file.txt`
    - This matches lines that begin with the word "start".

### 4. **Dollar Sign (`$`)**

- The dollar sign matches the **end** of a line.
- Example: `grep 'end$' file.txt`
    - This matches lines that end with the word "end".

### 5. **Square Brackets (`[]`)**

- Square brackets are used to match **any single character** from the set or range inside the brackets.
- Example: `grep '[aeiou]' file.txt`
    - This matches lines that contain any vowel (a, e, i, o, or u).
- You can also specify a range: `grep '[a-z]' file.txt`
    - This matches lines containing any lowercase letter.

### 6. **Caret inside square brackets (`[^]`)**

- A caret inside square brackets negates the character set, matching **any character that is not** in the set.
- Example: `grep '[^0-9]' file.txt`
    - This matches lines that contain any character that is not a digit.

### 7. **Pipe (`|`)**

- The pipe symbol is used to represent **alternation** (OR), matching either one pattern or another.
- Example: `grep 'cat|dog' file.txt`
    - This matches lines that contain either "cat" or "dog".

### 8. **Parentheses (`()`) and Backslashes (`\`) for Grouping**

- Parentheses are used to group patterns together, but since parentheses are special characters in regular expressions, they must be escaped with a backslash (`\`).
- Example: `grep '\(cat\|dog\)' file.txt`
    - This matches lines containing either "cat" or "dog".

### 9. **Question Mark (`?`)**

- The question mark matches **zero or one** occurrence of the preceding character or group.
- Example: `grep 'colou?r' file.txt`
    - This matches both "color" and "colour" (where the "u" is optional).

### 10. **Plus (`+`)**

- The plus symbol matches **one or more** occurrences of the preceding character or group.
- Example: `grep 'a+b+' file.txt`
    - This matches lines with one or more "a"s followed by one or more "b"s (e.g., "ab", "aab", "aaab", etc.).

### Usage Example with Wildcards:

bash

`grep 'ab*' file.txt     # Matches "a" followed by zero or more "b"s grep 'a.b' file.txt     # Matches "a", any single character, then "b" grep 'cat|dog' file.txt # Matches "cat" or "dog" grep '^[A-Za-z]' file.txt # Matches lines starting with a letter grep '.*end$' file.txt   # Matches lines ending with "end"`

### Notes:

- If you're using extended regular expressions (with the `-E` flag), some characters like `+`, `?`, `|`, `()` will not need to be escaped (e.g., `grep -E 'a+b+'`).
- If you're using basic regular expressions (default mode without `-E`), you must escape certain characters like `+`, `?`, `|`, `()`.

These wildcards provide flexibility and allow you to search for complex patterns in text files with ease.

  
