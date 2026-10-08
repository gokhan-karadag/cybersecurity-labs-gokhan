# Gokhan Karadag | CyberChef: From Beginner to Advanced

> Practical training handbook • From first steps to SOC and DFIR investigations • 67 guided recipes

## How to use this book
CyberChef is a browser-based data transformation workbench. **Input** is the data, **Operations** are tools, **Recipe** is the ordered sequence of transformations, and **Output** is the result. Think of a recipe as a cooking process: your data is the ingredient and each operation is a preparation step.

**Safety:** Do not paste live credentials, access tokens, confidential customer information, or suspicious executable files into untrusted online services. Use authorized, isolated environments. Decoding does not make content safe.

## Interface map
```text
OPERATIONS (search) -> RECIPE (ordered steps) -> INPUT -> OUTPUT
```

## Your first ten minutes
1. Open https://gchq.github.io/CyberChef/.
2. Enter `U09D` in Input.
3. Search for `From Base64` and drag it into Recipe.
4. Confirm the Output reads `SOC`.
5. Add `To Hex` after it. The resulting bytes are `53 4f 43` (spacing depends on settings).
6. Remove the second step, change an option, and observe the difference.
7. Save a recipe for later use.

## Core concepts
**Encoding** changes representation, not secrecy. **Encryption** protects confidentiality with a key. **Hashing** produces a one-way digest. **Compression** reduces storage size. **Parsing** separates structured fields. **Obfuscation** makes content harder to read but does not guarantee security.

## Recipe catalog

### Recipe 01 — To Base64

**Purpose:** Encode text as Base64; this is reversible encoding, not encryption.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Base64** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Merhaba SOC
```

**Expected output / observation:**
```text
TWVyaGFiYSBTT0M=
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 02 — From Base64

**Purpose:** Decode Base64 data to recover original bytes or text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Base64** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
TWVyaGFiYSBTT0M=
```

**Expected output / observation:**
```text
Merhaba SOC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 03 — To Hex

**Purpose:** Display bytes as hexadecimal pairs.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Hex** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
SOC
```

**Expected output / observation:**
```text
53 4f 43
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 04 — From Hex

**Purpose:** Convert hexadecimal byte representations back to text or binary.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Hex** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
53 4f 43
```

**Expected output / observation:**
```text
SOC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 05 — To Binary

**Purpose:** Represent data using binary digits.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Binary** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
A
```

**Expected output / observation:**
```text
01000001
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 06 — From Binary

**Purpose:** Interpret binary digits as bytes.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Binary** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
01000001
```

**Expected output / observation:**
```text
A
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 07 — To Decimal

**Purpose:** Represent byte values as decimal numbers.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Decimal** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
A
```

**Expected output / observation:**
```text
65
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 08 — From Decimal

**Purpose:** Interpret decimal byte values.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Decimal** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
83 79 67
```

**Expected output / observation:**
```text
SOC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 09 — To Charcode

**Purpose:** Convert characters into numerical code points.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Charcode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
ABC
```

**Expected output / observation:**
```text
65 66 67
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 10 — From Charcode

**Purpose:** Convert numerical character codes back into characters.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Charcode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
72 105
```

**Expected output / observation:**
```text
Hi
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 11 — URL Encode

**Purpose:** Percent-encode characters for safe use in URL components.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **URL Encode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello world
```

**Expected output / observation:**
```text
hello%20world
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 12 — URL Decode

**Purpose:** Decode percent-encoded URL characters.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **URL Decode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello%20world
```

**Expected output / observation:**
```text
hello world
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 13 — HTML Entity Encode

**Purpose:** Encode characters as HTML entities.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **HTML Entity Encode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
<tag>
```

**Expected output / observation:**
```text
&lt;tag&gt;
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 14 — HTML Entity Decode

**Purpose:** Decode HTML entities to their characters.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **HTML Entity Decode** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
&lt;tag&gt;
```

**Expected output / observation:**
```text
<tag>
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 15 — ROT13

**Purpose:** Rotate Latin letters by thirteen positions.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **ROT13** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
uryyb
```

**Expected output / observation:**
```text
hello
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 16 — ROT47

**Purpose:** Apply ROT47 substitution to printable ASCII characters.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **ROT47** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
96==@
```

**Expected output / observation:**
```text
hello
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 17 — Reverse Text

**Purpose:** Reverse character order in a string.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Reverse Text** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
SOC
```

**Expected output / observation:**
```text
COS
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 18 — To Morse Code

**Purpose:** Encode text as Morse symbols.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Morse Code** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
SOS
```

**Expected output / observation:**
```text
... --- ...
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 19 — From Morse Code

**Purpose:** Decode Morse symbols into readable text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Morse Code** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
... --- ...
```

**Expected output / observation:**
```text
SOS
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 20 — From Base32

**Purpose:** Decode Base32-encoded data.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Base32** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
JBSWY3DP
```

**Expected output / observation:**
```text
Hello
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 21 — Find & Replace

**Purpose:** Search for a string or pattern and replace matching text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Find & Replace** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
error ERROR error
```

**Expected output / observation:**
```text
warning WARNING warning
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 22 — Split

**Purpose:** Split input into separate items using a delimiter.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Split** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
alice,bob,carol
```

**Expected output / observation:**
```text
alice | bob | carol
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 23 — Join

**Purpose:** Join separate items using a delimiter.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Join** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
red
blue
```

**Expected output / observation:**
```text
red,blue
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 24 — Trim

**Purpose:** Remove unwanted whitespace from the edges of text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Trim** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
  SOC  
```

**Expected output / observation:**
```text
SOC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 25 — To Lower case

**Purpose:** Convert alphabetic characters to lowercase.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Lower case** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
ALERT
```

**Expected output / observation:**
```text
alert
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 26 — To Upper case

**Purpose:** Convert alphabetic characters to uppercase.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Upper case** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
soc
```

**Expected output / observation:**
```text
SOC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 27 — Remove whitespace

**Purpose:** Remove whitespace according to the selected options.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Remove whitespace** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
a b c
```

**Expected output / observation:**
```text
abc
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 28 — Sort

**Purpose:** Sort lines or other items.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Sort** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
z
a
m
```

**Expected output / observation:**
```text
a
m
z
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 29 — Unique

**Purpose:** Remove repeated items while considering ordering and case.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Unique** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
a
a
b
```

**Expected output / observation:**
```text
a
b
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 30 — Count occurrences

**Purpose:** Count occurrences of text or patterns.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Count occurrences** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
error error ok
```

**Expected output / observation:**
```text
error: 2
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 31 — Regular expression

**Purpose:** Match, extract, or replace data using a regular expression.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Regular expression** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
User: alice; IP: 192.0.2.10
```

**Expected output / observation:**
```text
192.0.2.10
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 32 — Extract URLs

**Purpose:** Extract URLs from unstructured text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Extract URLs** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Visit https://example.org/a
```

**Expected output / observation:**
```text
https://example.org/a
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 33 — Extract IP addresses

**Purpose:** Extract IPv4 or IPv6 addresses from text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Extract IP addresses** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Source 192.0.2.10 contacted 198.51.100.8
```

**Expected output / observation:**
```text
192.0.2.10, 198.51.100.8
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 34 — Extract email addresses

**Purpose:** Extract email addresses from text.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Extract email addresses** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Contact analyst@example.org
```

**Expected output / observation:**
```text
analyst@example.org
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 35 — JSON Beautify

**Purpose:** Format JSON with indentation for easier inspection.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **JSON Beautify** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
{"a":1,"b":2}
```

**Expected output / observation:**
```text
Çok satırlı girintili JSON
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 36 — JSON Minify

**Purpose:** Remove unnecessary JSON whitespace while preserving structure.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **JSON Minify** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
{ "a": 1 }
```

**Expected output / observation:**
```text
{"a":1}
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 37 — CSV to JSON

**Purpose:** Convert CSV records into JSON structures.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **CSV to JSON** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
name,score
Ada,95
```

**Expected output / observation:**
```text
[{"name":"Ada","score":"95"}]
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 38 — XML Beautify

**Purpose:** Indent and format XML documents.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **XML Beautify** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
<a><b>1</b></a>
```

**Expected output / observation:**
```text
Girintili XML
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 39 — MD5

**Purpose:** Calculate an MD5 digest; do not use MD5 for modern security assurances.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **MD5** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello
```

**Expected output / observation:**
```text
5d41402abc4b2a76b9719d911017c592
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 40 — SHA2 / SHA-256

**Purpose:** Calculate a SHA-2 digest such as SHA-256.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **SHA2 / SHA-256** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello
```

**Expected output / observation:**
```text
2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 41 — SHA1

**Purpose:** Calculate a SHA-1 digest; SHA-1 is unsuitable for collision-resistant security use.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **SHA1** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello
```

**Expected output / observation:**
```text
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 42 — HMAC

**Purpose:** Calculate a keyed message authentication code.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **HMAC** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello; key=demo
```

**Expected output / observation:**
```text
Anahtara bağlı doğrulama etiketi
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 43 — XOR

**Purpose:** Apply a bitwise XOR operation with a supplied key.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **XOR** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
ABC; key=1
```

**Expected output / observation:**
```text
@CB
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 44 — XOR Brute Force

**Purpose:** Try candidate XOR keys against data to investigate simple obfuscation.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **XOR Brute Force** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
XOR ile maskelenmiş örnek metin
```

**Expected output / observation:**
```text
Olası anahtar/çıktı adayları
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 45 — AES Encrypt

**Purpose:** Encrypt bytes with AES using an explicit key, mode, and IV or nonce as required.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **AES Encrypt** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello; known key and IV
```

**Expected output / observation:**
```text
Şifreli bayt dizisi
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 46 — AES Decrypt

**Purpose:** Decrypt AES ciphertext using the matching parameters.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **AES Decrypt** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
AES ciphertext + matching key
```

**Expected output / observation:**
```text
hello (doğru parametrelerle)
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 47 — Generate Random Bytes

**Purpose:** Generate random bytes for safe test examples.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Generate Random Bytes** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
16 byte isteği
```

**Expected output / observation:**
```text
16 rastgele bayt
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 48 — Entropy

**Purpose:** Measure the entropy of a byte sequence; high entropy alone does not prove encryption.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Entropy** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
AAAAAA ve rastgele bayt
```

**Expected output / observation:**
```text
Düşük ve yüksek entropi farkı
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 49 — To Hexdump

**Purpose:** Display data as a structured hexadecimal dump.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Hexdump** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
ABC
```

**Expected output / observation:**
```text
00000000  41 42 43
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 50 — From Hexdump

**Purpose:** Reconstruct bytes from a hexadecimal dump.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Hexdump** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
00000000  41 42 43
```

**Expected output / observation:**
```text
ABC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 51 — Gzip Compress

**Purpose:** Compress data with gzip.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Gzip Compress** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hello hello hello
```

**Expected output / observation:**
```text
Gzip ikili çıktı
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 52 — Gzip Decompress

**Purpose:** Decompress gzip data.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Gzip Decompress** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Geçerli gzip verisi
```

**Expected output / observation:**
```text
Özgün metin
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 53 — Parse IP address

**Purpose:** Inspect the structure of an IP address.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Parse IP address** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
192.0.2.10
```

**Expected output / observation:**
```text
Adres bilgisi ve gösterimi
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 54 — CIDR Range

**Purpose:** Calculate or examine an address range from CIDR notation.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **CIDR Range** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
192.0.2.0/24
```

**Expected output / observation:**
```text
192.0.2.0–192.0.2.255
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 55 — Defang URL

**Purpose:** Defang a URL to reduce accidental clicking.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Defang URL** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
https://example.org/a
```

**Expected output / observation:**
```text
hxxps://example[.]org/a
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 56 — Refang URL

**Purpose:** Refang a URL for controlled analysis; treat it as potentially dangerous.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Refang URL** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
hxxps://example[.]org/a
```

**Expected output / observation:**
```text
https://example.org/a
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 57 — From Unix Timestamp

**Purpose:** Convert a Unix timestamp into a human-readable date.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **From Unix Timestamp** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
0
```

**Expected output / observation:**
```text
1970-01-01 00:00:00 UTC
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 58 — To Unix Timestamp

**Purpose:** Convert a date into a Unix timestamp.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **To Unix Timestamp** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
1970-01-01 00:00:00 UTC
```

**Expected output / observation:**
```text
0
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 59 — Parse User Agent

**Purpose:** Parse browser and client details from a user-agent string.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Parse User Agent** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Mozilla/5.0 ...
```

**Expected output / observation:**
```text
Tarayıcı/OS tahminleri
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 60 — Parse URL

**Purpose:** Break a URL into scheme, host, path, query, and other components.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Parse URL** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
https://example.org:443/a?x=1
```

**Expected output / observation:**
```text
Scheme=https; host=example.org; path=/a; query=x=1
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 61 — Decode JWT

**Purpose:** Decode JWT segments to inspect claims; decoding does not verify the signature.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Decode JWT** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
eyJ...eyJ...signature
```

**Expected output / observation:**
```text
Header ve payload alanları
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 62 — Parse X.509 certificate

**Purpose:** Inspect fields in an X.509 certificate.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Parse X.509 certificate** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
PEM sertifika metni
```

**Expected output / observation:**
```text
Subject, issuer, validity
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 63 — Magic

**Purpose:** Suggest possible decoding operations from the input.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Magic** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
TWVyaGFiYQ==
```

**Expected output / observation:**
```text
Base64 olasılığı ve önerilen işlem
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 64 — Fork

**Purpose:** Apply a recipe separately to multiple segments.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Fork** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
U09D
VEVTVA==
```

**Expected output / observation:**
```text
SOC
TEST
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 65 — Merge

**Purpose:** Merge separate processing branches into a combined result.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Merge** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
Fork sonrası parçalar
```

**Expected output / observation:**
```text
Tek bir çıktı akışı
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 66 — Subsection

**Purpose:** Apply operations to a selected subsection of input.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Subsection** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
ID=123; ID=456
```

**Expected output / observation:**
```text
123 ve 456 üzerinde işlem
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

### Recipe 67 — Register

**Purpose:** Store and reuse captured values inside a recipe.

**Step-by-step procedure:**
1. Clear the existing Recipe to avoid accidental extra transformations.
2. Copy the sample input below into the Input pane, or load a suitable authorized test file.
3. Search Operations for **Register** and drag it into the Recipe pane.
4. Inspect and configure operation settings. Pay attention to delimiters, alphabets, encodings, keys, modes, and byte representations when relevant.
5. Compare Output with the sample observation. If it differs, adjust one parameter at a time and retry.
6. Disable or remove the operation and re-enable it to understand exactly what it changes.

**Sample input:**
```text
user=alice&token=demo
```

**Expected output / observation:**
```text
Yakalanan alanları sonraki adımda kullanma
```

**Practical exercise:** Modify one option or input character. Explain why the result changes and how you would validate it.

**SOC application:** Use the transformation to investigate authorized logs, artifacts, network indicators, or suspicious text. Record the input type, selected options, and output in your case notes.

## Multi-step SOC labs

### Lab A — Decode a suspicious URL
1. Paste a harmless percent-encoded URL into Input.
2. Add **URL Decode**.
3. Add **Parse URL** to inspect the host and path.
4. If needed, use **Defang URL** before sharing the indicator in a report.
5. Confirm the decoded destination without visiting it.

### Lab B — Unwrap layered encoding
1. Start with `VTJWR2RHVjRQVDA9` (a deliberately layered example).
2. Apply **From Base64** and inspect the output.
3. If the result is another Base64 string, apply **From Base64** again.
4. Stop when you reach meaningful plaintext; never assume the number of layers.

### Lab C — Investigate a possible encoded PowerShell fragment
1. Copy only the authorized command-line text into Input.
2. Identify the encoding first; PowerShell `-EncodedCommand` commonly uses UTF-16LE before Base64.
3. Apply **From Base64** then decode the resulting bytes using the appropriate character encoding.
4. Inspect the resulting script as text; do not execute it.
5. Preserve evidence and document every transformation.

## Common troubleshooting
- Garbled characters usually indicate the wrong character encoding or binary data being interpreted as text.
- An invalid Base64 error may indicate incorrect alphabet, padding, or a false positive.
- Hex and decimal conversions require correct separators and byte boundaries.
- Hashes are not decrypted; compare candidate values by hashing them instead.
- AES decryption requires the correct key, mode, IV or nonce, padding, and authentication tag when applicable.
- A decoded JWT payload is not proof of authenticity; signature verification is separate.

## Four-week study plan
**Week 1:** Recipes 1–20; understand representation and reversible encoding.
**Week 2:** Recipes 21–38; clean, extract, and parse data.
**Week 3:** Recipes 39–52; understand hashing, cryptography, and bytes.
**Week 4:** Recipes 53–67; investigate network artifacts and build chained recipes.

## Knowledge check
1. What is the difference between encoding and encryption?
2. Why does Base64 decoding sometimes produce unreadable text?
3. What parameters are needed to decrypt AES correctly?
4. How can you inspect a suspicious URL without visiting it?
5. Why does decoding a JWT not validate its signature?
6. How can you reproduce and document a CyberChef recipe for another analyst?

## Reference
Official CyberChef: https://gchq.github.io/CyberChef/
Source material: translated and reorganized from the provided Turkish CyberChef training notes.
