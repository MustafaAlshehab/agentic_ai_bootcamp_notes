# Python Week 1 — Strings, Slicing, and Data Structures (Lists, Tuples, Sets)

## TL;DR
This session recapped Python basics (variables, data types, operators), then went deep on **strings**: indexing, forward/backward access, slicing with a non-inclusive end, and step slicing. The instructor covered **immutability**, string methods, concatenation, and a practical “encrypted location” puzzle using slicing. The second half introduced **data structures**—lists, tuples, and sets—including nested access, mutability vs immutability, typecasting, list `extend`/`append`, and set operations (union, intersection, difference, symmetric difference). **Dictionaries** and more list topics were deferred to the next class.

## Topics Covered
- Session logistics (rename in Zoom, use Q&A not chat, attendance drop noted)
- Quick recap: Python as OOP/open-source language; variables, keywords, data types, operators from prior session
- Advice on learning Python (practice, AI-generated exercises, Codecademy/YouTube supplements)
- **Strings**: characters, forward index (0-based), backward index (-1-based)
- **Indexing & slicing** in Google Colab (`a[1]`, `a[1:4]`, omitted start/end, step `a[2:13:2]`)
- **`print` vs bare expression** (quotes visibility); `len`, `type`
- **String concatenation** (`+`); embedding spaces in concatenation
- **Immutability** of strings (`TypeError` on item assignment)
- **String methods**: `lower`, `upper`, `strip`, `lstrip`, `rstrip`
- **Encrypted message** exercise (interleaved characters → area + city via stride-2 slices)
- **Data structures overview**: list, tuple, set, dictionary (dict later)
- **Lists**: syntax, mutability, heterogeneous elements, nested lists, indexing like strings
- **Break**; file sharing (Google Drive week 1, final files via SharePoint / Nazia & Aparna)
- **Nested list access** (`list2[4][1]`); mutability demo (change physics → chemistry)
- **Exercise**: slice `"mathematics"` to get `"math"` without assignment
- **Typecasting**: `int()`, `float()`, `str()`; chained conversion for quoted floats
- **Tuples**: round brackets, immutable, single-element tuple requires trailing comma
- **Lists**: `+` merge, `.extend()`, `.append()`
- **Sets**: deduplication via `set()`, no index access; Venn-style `union`, `intersection`, `difference`, `symmetric_difference`
- Wrap-up: homework, Thursday doubt-clearing class, ML/DL vs GenAI career note

## Key Concepts Explained

### Variables and basic data types
- What it is: Variables are temporary storage in Python memory (e.g. `a = 10`). Core types covered: **int**, **float**, **string**, **date**, **bool**.
- Why it matters: Type determines behavior (e.g. `5 + 10` → 15 vs `"5" + "10"` → `"510"`).
- Emphasis: Bootcamp covers Python in ~8 hours across four sessions—extra self-study is needed for intermediate fluency.

### String indexing (forward and backward)
- What it is: Each character sits at an index; **forward indexing starts at 0**; **backward indexing starts at -1** (last character).
- Why it matters: Foundation for slicing and extracting substrings.
- Caveats: Instructor noted backward indexing has no special “use case” in week one—it’s just how Python works. For `"Hello, World"` (11 chars), second letter `e` is `a[1]` or `a[-10]`.

### Slicing (`start:stop` and `start:stop:step`)
- What it is: `a[start:stop]` returns multiple characters; **stop index is non-inclusive**. Omitted start defaults to 0; omitted stop goes to end (`a[6:]` for `"World"`). Step: `a[2:13:2]` takes every 2nd character in range.
- Why it matters: Core pattern for substring extraction and the encrypted-message puzzle.
- Emphasis: If you want through index 10 inclusive, use stop **11** because 10 is excluded. Practice step slicing on your own strings.

### `print`, `type`, and `len`
- What it is: `print()` displays values without quotes on strings; bare `a[6]` in Colab may show quotes. `type(x)` returns datatype; `len(x)` returns length.
- Why it matters: Debugging and validating slices (e.g. `len(a[0:5]) == 5` confirms no space in `"Hello"`).
- Caveat: `type(print(a))` is **`NoneType`** because `print` returns `None`, not the string’s type.

### String concatenation
- What it is: `+` joins strings (`"Hello" + "World"`). With ints, `+` adds numerically.
- Why it matters: Building full names, messages, and later combining sliced parts (`a[0:8] + a[15:]` → `"Welcome class"`).
- Emphasis: `first + last` omits space—use `first + " " + last` or a middle variable.

### Immutable vs mutable
- What it is: **Immutable** = cannot change in place (strings). **Mutable** = can change elements (lists).
- Why it matters: Explains `a[1] = 'e'` failing on strings but `list2[4][1] = 'chemistry'` working on lists.
- Error to expect: `TypeError: 'str' object does not support item assignment`.

### String methods (`lower`, `upper`, `strip`, `lstrip`, `rstrip`)
- What it is: `a.lower()`, `a.upper()` change case; `strip`/`lstrip`/`rstrip` remove leading/trailing whitespace (demo: length 27 → 11 after strip).
- Why it matters: Cleaning user input and normalizing text before processing.

### Encrypted interleaved message (stride slicing)
- What it is: Message like `BMaUNMDBRaAI` encodes two words by alternating characters (Bandra + Mumbai). Decode with `message[0::2]` (first word) and `message[1::2]` (second word)—empty start/end means beginning through end with step 2.
- Why it matters: Instructor framed this as a **real use case** for string slicing after earlier “no use cases” comments in week one basics.

### Lists
- What it is: Ordered, **mutable** sequences in **square brackets** `[]`; elements can mix types; list inside list = **nested list**.
- Why it matters: Store one record (age, location, marks) per candidate instead of millions of separate variables.
- Access: Same indexing/slicing as strings (`a[1:4]`). Nested: `list2[4][1]` for physics in example list `[India, Delhi, 5, 10, [mathematics, physics]]`.

### Tuples
- What it is: Like lists but in **round brackets** `()` (or comma-separated without brackets); **immutable**.
- Why it matters: Data that should not change (e.g. passport-like fields—instructor’s illustrative example).
- Critical gotcha: **Single-element tuple requires a trailing comma**: `(5,)` is tuple; `(5)` is int. One-element with only brackets is still int, not tuple.

### Typecasting
- What it is: `int()`, `float()`, `str()` convert between types; e.g. `int("5") + int("6")` → 11 vs `"5" + "6"` → `"56"`.
- Why it matters: Arithmetic on numeric strings; defensive pattern: convert to **float first** when parsing numeric strings to reduce errors.
- Caveat: `int("5.5")` fails—use `int(float("5.5"))` for quoted decimal strings.

### List combine: `+`, `extend`, `append`
- What it is: `list1 + list2` **concatenates** full lists (not element-wise sum). `.extend(list2)` merges elements in place like `+`. `.append(list2)` adds **entire second list as one nested element**.
- Why it matters: Choosing merge vs append avoids subtle bugs when building collections.

### Sets
- What it is: **Curly braces** `{}` or `set(iterable)`; stores **unique** elements only; **no indexing** (`b[2]` fails).
- Why it matters: Deduplication (`b = set(a)` from a list with duplicates). Set ops mirror Venn diagrams.
- Operations: `.union()`, `.intersection()`, `.difference()` (A minus B), `.symmetric_difference()` (elements in A or B but not both—equivalent to union minus intersection).

### Four data structures (preview)
- What it is: **list**, **tuple**, **set**, **dictionary**—dict covered next session; dict concepts tie to future **Pandas/NumPy** work per instructor.
- Why it matters: Reduces variable sprawl and memory; dict especially for keyed data at scale.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Python | OOP, open source; scripting and general programming |
| Google Colab | Live coding; Shift+Enter to run cells; markdown headers (`#`, `##`) for notebook sections |
| `print()` | Output; does not add quotes around strings |
| `len()` | Length of string, slice, or list |
| `type()` | Returns datatype; not for wrapping `print()` |
| `int()`, `float()`, `str()` | Typecasting |
| `def` / `return` | Mentioned as coming later for functions |
| String methods | `.lower()`, `.upper()`, `.strip()`, `.lstrip()`, `.rstrip()` |
| Lists | `[]`, indexing, slicing, `+`, `.extend()`, `.append()` |
| Tuples | `()`, trailing comma for singleton |
| Sets | `set()`, `{}`, `.union()`, `.intersection()`, `.difference()`, `.symmetric_difference()` |
| Pandas / NumPy | Named as future topics using dict-like concepts |
| Discord | Q&A, ping instructor; tag Nazia/Aparna for SharePoint files |
| Google Drive (AGI 3 folder) | Temporary week 1 uploads during class |
| SharePoint | Final course materials via Nazia & Aparna |
| Codecademy dashboard | Session recordings under bootcamp details |
| Grok / ChatGPT | Generate practice questions without answers |
| Instructor YouTube playlist | “End-to-end Python” for supplemental hours |
| Codecademy Python course | Recommended extra practice |

## Code & Examples

### Basic string and indexing
```python
a = "Hello, World"
print(a)        # Hello, World
print(a[1])     # e
print(a[-10])   # e (same as a[1])
print(a[6])     # W
print(a[-5])    # W
```

### Slicing (non-inclusive end)
```python
print(a[1:4])      # ell
print(a[-10:-7])   # ell
print(a[0:5])      # Hello (no space)
print(len(a[0:5])) # 5
print(a[6:])       # World (through end)
print(a[6:9])      # Wor
# a[6:11] may work but instructor prefers a[6:] for "till end"
print(a[-5:])      # World via negative start
print(a[:5])       # Hello — omitted start = 0
```

### Step slicing
```python
# Example string: "Welcome to the class" (indices as taught in class)
print(a[2:13:2])   # LOE T H (illustrative on their board string)
print(a[4:18:3])   # O  EL (second classroom example)

# Puzzle: "Welcome class" from welcome... string
print(a[0:8] + a[15:])  # Welcome class
```

### Encrypted location (Bandra, Mumbai)
```python
encrypted = "BMaUNMDBRaAI"  # transcript spelling; verify against assignment
area = encrypted[0::2]    # Bandra
city = encrypted[1::2]    # Mumbai
print(area, city)
```

### Immutability
```python
a = "H3llo"
# a[1] = 'e'  # TypeError: 'str' object does not support item assignment
```

### String methods and strip
```python
a = "   Hello world   "  # padded with spaces in demo
print(a.lower())
print(a.upper())
print(len(a))           # 27 in demo
print(len(a.strip()))   # 11 after strip
# lstrip / rstrip for leading / trailing only
```

### Concatenation
```python
first_name = "Elon"
last_name = "Musk"
print(first_name + last_name)           # ElonMusk
print(first_name + " " + last_name)   # Elon Musk
```

### List basics and nested access
```python
a = [5, 10, 20, 30, 40]
print(len(a))       # 5
print(type(a[0]))   # int

list2 = ["India", "Delhi", 5, 10, ["mathematics", "physics"]]
print(len(list2))              # 5
print(list2[2:4])              # [5, 10]
print(list2[4][1])             # physics
print(list2[4][0][0:4])        # math — slice string inside nested list
list2[4][1] = "chemistry"      # mutable
```

### Typecasting
```python
a, b = "5", "6"
print(a + b)              # 56
print(int(a) + int(b))    # 11

a, b = "5", "6.5"
print(float(a) + float(b))  # 11.5

# int("5.5")  # fails
print(int(float("5.5")))    # 5

a = 5
print(str(a))             # string type
```

### Tuple vs list
```python
a = (5, 10, 20, 30, 40)
print(type(a))   # tuple
a = (5,)         # single-element tuple — comma required
# a = (5)        # int, not tuple

# Immutable
# tuple version of list2[4][1] = "chemistry"  # TypeError
```

### List merge, extend, append
```python
list1 = [1, 2, 3, 4]
list2 = [5, 6, 7, 8]
print(list1 + list2)           # [1,2,3,4,5,6,7,8]
list1.extend(list2)            # merges in place (like +)
list1.append(list2)            # adds [5,6,7,8] as single element
```

### Sets
```python
a = [1,2,3,4,5,5,2,3,4,1,2,3]  # duplicates
print(len(a))                  # 15
b = set(a)
print(b)                       # {1,2,3,4,5}
# print(b[2])  # fails — no indexing

A = {1,2,3,4,5,6}
B = {2,4,6,8,10}
print(A.union(B))                    # all unique combined
print(A.intersection(B))             # {2,4,6}
print(A.difference(B))               # {1,3,5}
print(A.symmetric_difference(B))     # {1,3,5,8,10}
```

### Notebook markdown (Colab)
```markdown
## String operations
```
Use `#` / `##` for heading sizes in markdown cells between code blocks.

## Q&A / Discussion Points
- **Reverse indexing use case?** No special use case in week one; forward and backward both work for access.
- **Quotes when printing vs not?** Both are strings; `print` omits quote display. Use `type()` on the value, not `type(print(...))`.
- **Slice end index 10 vs 11?** Stop is non-inclusive—use one past the last desired index.
- **`a[-5:-1]` missing final char?** Use `a[-5:]` with empty end to include through the end.
- **NaN / null / undefined in Python?** `NaN` discussed briefly in context of null numeric columns; instructor said they would return to it (student: Vicky).
- **What changes in mutable data—type or value?** Instructor: you can change values or effectively change what’s stored; focus was on mutating list elements.
- **Recordings location?** Codecademy dashboard → bootcamp details → recordings.
- **Tuple inside list / list inside tuple?** Allowed by Python but not recommended for beginners due to access complexity.
- **Tuples changeable?** No—immutable.
- **ML/DL required for GenAI?** Not mandatory for the bootcamp path; recommended for career (analogy: GenAI = sentences, ML/DL = words). Resources to be shared later; not deep in this program.

## Action Items & Assignments
- [ ] Practice Python daily through the bootcamp—don’t expect mastery from ~8 hours of lecture alone
- [ ] After each day, prompt **Grok** or **ChatGPT** with topics covered and ask for **~10 practice questions without answers**; solve first, then check
- [ ] Use instructor’s YouTube **end-to-end Python** playlist and/or **Codecademy Python** course for extra depth
- [ ] Practice string slicing, especially **step/jump** slices, on custom strings
- [ ] Work through encrypted-message-style problems using `[start:stop:step]`
- [ ] Review Colab notebooks from **Google Drive week 1**; final copies will appear on **SharePoint** (contact Nazia/Aparna on Discord if missing)
- [ ] Don’t wait until Saturday’s class—study and practice during the week
- [ ] **Thursday**: doubt-clearing session (or career/off-topic if no doubts)
- [ ] Ask week questions on **Discord** (ping instructor directly)

## Open Questions / Unclear Spots
- Exact encrypted string spelling in materials may differ from whiteboard (`BMaUNMDBRaAI` in transcript)—confirm against shared notebook.
- **Date** data type was listed in recap but not demonstrated this session.
- **Dictionaries**, fuller **list** utilities, and **loops/iterations** (stated at session start as planned topics) were largely deferred to the next class.
- Student question about **null-like sentinels** in Python (vs JS `undefined` / SQL `NULL`) was acknowledged but not fully answered—marked for later.
- Instructor briefly searched for `.merge` on lists; correct methods taught were **`extend`** and **`append`** (not `merge`).
