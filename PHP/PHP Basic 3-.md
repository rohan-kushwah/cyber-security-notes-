# Loop in Programming

  

A **loop** is a programming structure that repeats a block of code as long as a specified condition is true. It helps automate repetitive tasks without having to write the same code multiple times.

  

---

  

## 🔄 **Types of Loops (Common in Most Languages)**

  

### 1. **`for` loop**

Used when you know in advance how many times you want to run a block of code.

  

```python

for i in range(5):

    print("Hello")  # Runs 5 times

```

  

### 2. **`while` loop**

Runs as long as a condition is true.

  

```python

count = 0

while count < 5:

    print("Hi")

    count += 1

```

  

### 3. **`do...while` loop** *(in some languages like C, JavaScript)*

Runs the code **at least once**, then checks the condition.

  

```javascript

let i = 0;

do {

    console.log("Hey");

    i++;

} while (i < 5);

```

  

---

  

## 🧠 Why Use Loops?

  

- Automate repeated tasks

- Iterate over items in a list or array

- Perform actions until a condition is met

  

---

  

# Loop Examples

  

## 🔁 **FOR Loop Examples**

  

### ✅ Python

```python

# Print numbers from 1 to 5

for i in range(1, 6):

    print(i)

  

# Print each item in a list

fruits = ["apple", "banana", "cherry"]

for fruit in fruits:

    print(fruit)

```

  

### ✅ JavaScript

```javascript

// Print numbers from 0 to 4

for (let i = 0; i < 5; i++) {

    console.log(i);

}

  

// Loop through an array

let colors = ["red", "blue", "green"];

for (let color of colors) {

    console.log(color);

}

```

  

### ✅ C

```c

// Print numbers from 1 to 5

for (int i = 1; i <= 5; i++) {

    printf("%d\n", i);

}

  

// Sum of first 5 numbers

int sum = 0;

for (int i = 1; i <= 5; i++) {

    sum += i;

}

printf("Sum: %d\n", sum);

```

  

## 🔁 **WHILE Loop Examples**

  

### ✅ Python

```python

# Print numbers 1 to 5

i = 1

while i <= 5:

    print(i)

    i += 1

  

# Print even numbers less than 10

i = 2

while i < 10:

    print(i)

    i += 2

```

  

### ✅ JavaScript

```javascript

// Countdown from 5 to 1

let i = 5;

while (i > 0) {

    console.log(i);

    i--;

}

  

// Print numbers < 10 that are divisible by 3

let j = 0;

while (j < 10) {

    if (j % 3 === 0) {

        console.log(j);

    }

    j++;

}

```

  

### ✅ C

```c

// Print numbers from 1 to 3

int i = 1;

while (i <= 3) {

    printf("%d\n", i);

    i++;

}

  

// Double value until it reaches 100

int val = 2;

while (val < 100) {

    printf("%d\n", val);

    val *= 2;

}

```

  

## 🔁 **DO...WHILE Loop Examples**

  

### ✅ JavaScript

```javascript

// Print "Hello" at least once

let i = 0;

do {

    console.log("Hello");

    i++;

} while (i < 3);

  

// Run a loop until a random number is 5

let num;

do {

    num = Math.floor(Math.random() * 10);

    console.log(num);

} while (num !== 5);

```

  

### ✅ C

```c

// Print 1 to 3 using do...while

int i = 1;

do {

    printf("%d\n", i);

    i++;

} while (i <= 3);

  

// Ensure at least one execution

int num = 0;

do {

    printf("Executed\n");

} while (num != 0);  // Will still run once

```

  

---

  

# Difference Between All Loops

  

| Loop Type   | Condition Checked | Runs At Least Once? | Best Used When...                                |

|-------------|-------------------|----------------------|---------------------------------------------------|

| `for`       | Before each loop  | No                   | You know how many times to run                   |

| `while`     | Before each loop  | No                   | You don't know how many times in advance         |

| `do...while`| After each loop   | **Yes**              | You want to **guarantee** at least one execution |

| `foreach`   | Implicit (collection) | Yes (if not empty) | You want to loop over items in a collection      |

  

---

  

# Break and Continue

  

## 🔹 `break` Statement

Stops the loop entirely.

  

```python

for i in range(5):

    if i == 3:

        break

    print(i)

```

  

## 🔹 `continue` Statement

Skips the current iteration.

  

```python

for i in range(5):

    if i == 3:

        continue

    print(i)

```

  

---

  

# PHP Array Pointers

  

```php

$colors = ["red", "green", "blue", "yellow"];

  

echo current($colors);  // red

next($colors);

echo current($colors);  // green

next($colors);

echo key($colors);      // 2

echo current($colors);  // blue

prev($colors);

echo current($colors);  // green

end($colors);

echo current($colors);  // yellow

reset($colors);

echo current($colors);  // red

```

  

---

  

# What is a Function?

  

```php

function greet($name) {

    echo "Hello, $name!";

}

greet("Alice");  // Output: Hello, Alice!

```

  

## With Return Value

  

```php

function add($a, $b) {

    return $a + $b;

}

$result = add(5, 3);

echo $result;  // Output: 8

```


A **`foreach` loop** is a type of loop designed specifically to iterate over **all elements** in a collection like an array, list, or dictionary — without needing to use an index or counter. It makes looping over items simpler and cleaner.

---

## What is a `foreach` Loop?

- It automatically goes through **each element** in the collection.
    
- You don’t have to manage the loop counter or worry about the collection length.
    
- Commonly used when you just want to **process every item** in a collection.
    

---

## Examples of `foreach` Loop in Different Languages

### ✅ PHP

php

CopyEdit

`$fruits = ["apple", "banana", "cherry"];  foreach ($fruits as $fruit) {     echo $fruit . "\\n"; }`

### ✅ Python (using `for` which works like `foreach`)

python

CopyEdit

`fruits = ["apple", "banana", "cherry"]  for fruit in fruits:     print(fruit)`

### ✅ JavaScript (`for...of` is like `foreach`)

javascript

CopyEdit

`const fruits = ["apple", "banana", "cherry"];  for (const fruit of fruits) {     console.log(fruit); }`

### ✅ C#

csharp

CopyEdit

`string[] fruits = { "apple", "banana", "cherry" };  foreach (string fruit in fruits) {     Console.WriteLine(fruit); }`
