# Week 2: Dictionaries, Control Flow, Loops, and Functions

## TL;DR
This fast-track session introduced Python **dictionaries** (key–value pairs), then moved through **if/elif/else**, **for** loops over lists/strings/dicts, and **functions** (`def`, `return` vs `print`). Hands-on drills included nested dict/list indexing, even/odd checks, factorial, sum of natural numbers, and palindrome detection. The instructor stressed this curriculum targets **data science/AI** (Pandas, NumPy) basics—not a full Python developer or competitive-programming path.

## Topics Covered
- Quick recap: lists (mutable), tuples (immutable), sets (unique elements)
- Dictionaries: syntax, access, update, delete, `.keys()`, `.values()`, `.get()`, `.update()`
- Nested dictionary exercise (employee records)
- Bootcamp scope vs full Python developer roadmap (HackerRank, LeetCode, OOP, DSA)
- `if` / `elif` / `else`, indentation, colons
- `input()` and `int()` type casting
- Even/odd with modulo (`%`)
- `for` loops and iteration over lists, strings, dictionaries
- `dict.items()` for key–value pairs
- Functions: `def`, arguments, `return` vs `print`
- `range()` and factorial implementation
- Sum of natural numbers (loop-based)
- Palindrome check (two-pointer loop and slice shortcut)
- Jupyter vs `.py` workflows; session feedback process

## Key Concepts Explained

### Dictionary (key–value pair)
- A mapping like a real dictionary: each **key** maps to a **value**; written with `{}` and comma-separated pairs.
- **Why it matters:** Core for data work; heavily used in **Pandas** and everyday Python scripting in this bootcamp.
- **Caveats:** Keys must be immutable/hashable types; **values** can change. Accessing a missing key with `d[key]` raises **KeyError**; assignment `d[new_key] = value` **creates** a new key (no error). Instructor emphasized keys are immutable and values are mutable.

### Dictionary operations
- `len(d)`, `d[key]`, `d[key] = value`, `del d[key]` (permanent delete; no built-in undo).
- `.keys()`, `.values()`, `.get(key)`, `.update({...})` or assign to update values.
- **Instructor note:** Two ways to read values—`d[key]` and `d.get(key)`—both exist; no single “right” style for all cases.

### Comments (`#`)
- Lines starting with `#` are not executed; use for notes or to disable code.

### Bootcamp Python scope vs Python developer path
- **Python developer:** weeks of fundamentals, OOP, DSA, `map`/`filter`/`lambda`, platforms like HackerRank/LeetCode—**not** this bootcamp’s goal.
- **Data science / AI track:** basics (lists, tuples, sets, dicts, loops, functions) then **Pandas, NumPy**, etc.
- **Instructor emphasis:** What’s taught here is “as much as required” for the domain, not exhaustive Python.

### Conditional statements (`if` / `elif` / `else`)
- Run one branch based on truth of conditions; `elif` is Python’s form of “else if” (not `else if`).
- **Why it matters:** Foundation for all control flow before loops and functions.
- **Caveats:** After a true branch runs, remaining conditions are skipped. Party-age example can be simplified with `or` for two “no party” cases.

### Indentation
- Blocks under `if`, `elif`, `else`, `for`, etc. must be indented (spaces/tab); colon `:` ends the header line.
- Python auto-indents on Enter after a colon in notebooks.

### `input()` and type casting
- `input()` returns a **string** by default; comparing to numbers needs `int(input(...))` or similar.
- **Gotcha:** `if "14" < 18` causes **TypeError** (str vs int).

### Modulo for even/odd
- `n % 2 == 0` → even; else odd. `%` is remainder, not “percentage.”

### `for` loop iteration
- `for i in sequence:` assigns each element to temporary variable `i` and runs the body until the sequence is exhausted.
- Works on **lists** (elements), **strings** (characters), **dicts** (keys by default).
- **Dictionary iteration:** `for k, v in d.items():` for pairs; `.keys()` / `.values()` optional.

### Nested indexing (dict + list)
- Employee example: `employee_data[104]` returns a **list**; profession at index `2` → `employee_data[104][2]`.
- Chaining: age `employee_data[103][1]`; second subject `employee_data[105][1]`; second letter of first subject `employee_data[101][0][1]`.

### Functions (`def`)
- Named reusable blocks: `def name(args):` body; call with `name(arg)`.
- **Why it matters:** Avoids repeating the same multi-line logic.
- **`return` vs `print`:** `print` only shows output in the console (OK while developing); **`return`** passes a value back to the caller—preferred for reusable/production-style functions.

### `range()`
- `range(start, stop)` iterates from `start` up to **stop − 1** (stop exclusive).
- Example: `range(1, n+1)` for 1 through `n` when computing factorial or sums.

### Factorial (iterative)
- Initialize accumulator (e.g. `fact = 1`), multiply through `1..n` in a loop, `return` result.
- **Instructor note:** `fact = 1` is set once; updates happen inside the loop, not by re-running line 2 each iteration.

### Palindrome
- Compare characters from both ends inward; or shortcut: `s == s[::-1]` → boolean / derived message.
- **Interview tip:** Work through examples on paper (e.g. `madam`) before coding.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Python 3 | Core language for the session |
| Google Colab / Jupyter (`.ipynb`) | Line-by-line execution; preferred while learning/debugging |
| `.py` files / other IDEs | Run full script at once; switch after notebook code is stable |
| Codecademy Pro Python 3 | Mentioned as optional resource; instructor hadn’t fully reviewed it |
| `len()`, `del`, `.keys()`, `.values()`, `.get()`, `.update()` | Dictionary utilities |
| `input()`, `int()` | Interactive input and casting |
| `%` (modulo) | Remainder; even/odd |
| `for`, `range()` | Iteration |
| `def`, `return`, `print` | Functions |
| `//` | Floor division (e.g. `len(s)//2` for palindrome half-iterations) |
| Pandas / NumPy | Named as next-step libraries for this career path |
| HackerRank, LeetCode, Data Lemur | Competitive / interview prep (out of scope for bootcamp) |
| Discord (Nazia, Aparna) | Anonymous negative session feedback |
| poi.com | Instructor recommendation to generate ~10 practice problems (describe today’s topics; ask for questions only, no answers)—likely **Poe.com** or similar AI chat [unclear: exact URL] |

## Code & Examples

### Basic dictionary (country → currency)

```python
d1 = {"India": "INR", "USA": "USD", "Hong Kong": "HKD"}
len(d1)           # 3
d1["India"]       # 'INR'
d1["Japan"]       # KeyError — key missing
d1["Japan"] = "yen"   # adds key
d1["India"] = "INR"   # updates value (no output if last expr in cell)
del d1["Delhi"]   # remove key (after mistaken entry)
d1.keys()
d1.values()
d1.get("India")
d1.update({"China": "RMB"})  # or d1["China"] = "RMB"
```

### Employee profession (nested list in dict)

```python
# employee_data pasted in class — structure: id -> [name, age, profession, ...]
employee_data[104][2]           # profession for id 104
employee_data.get(104)[2]       # same via .get
```

### Practice indexing (answers given in class)

```python
employee_data[103][1]    # age of 103 → 31
employee_data[105][1]    # second subject of 105 → 'Chem'
employee_data[101][0][1] # second letter of first subject of 101 → 'H'
```

### if / elif / else (party ages)

```python
age = 24  # or int(input("Enter age: "))
if age < 18:
    print("No party")
elif age > 60:
    print("No party")
else:
    print("Party")
```

### Even / odd

```python
n = int(input("Enter a number: "))
if n % 2 == 0:
    print("even number")
else:
    print("odd number")
```

### for loop — list and string

```python
list_1 = [10, 20, 30, 40, 50]
for i in list_1:
    print(i)

str1 = "Hong Kong"
for i in str1:
    print(i)
```

### for loop — dictionary

```python
for i in dict1:
    print(i)  # keys only

for i, j in dict1.items():
    print(f"Key is {i} and value is {j}")

# ages only (values are lists; age at index 1)
for i in dict1.values():
    print(i[1])
```

### Function — even/odd (with return)

```python
def evenorodd(n):
    if n % 2 == 0:
        return "even number"
    else:
        return "odd number"

evenorodd(102)
```

### Factorial

```python
def fact_num(n):
    fact = 1
    for i in range(1, n + 1):
        fact = fact * i
    return fact

fact_num(5)  # 120
```

### Sum of natural numbers (adapt factorial loop)

```python
def sum_natural(n):
    s = 0
    for i in range(1, n + 1):
        s = s + i
    return s
```

### Palindrome — loop version (approximate from transcript)

```python
def is_palindrome(s):
    j = len(s)
    for i in range(j // 2):
        if s[i] != s[j - 1 - i]:
            return "not a palindrome"
    return "palindrome"
```

### Palindrome — shortcut

```python
def is_palindrome(s):
    return "palindrome" if s == s[::-1] else "not a palindrome"
# or return s == s[::-1] for True/False
```

## Q&A / Discussion Points
- **Python book for beginners?** No specific book; may share YouTube channels (mostly Indian creators) on Discord; Codecademy Pro Python 3 suggested by students.
- **Dictionary like an object / used in Pandas?** Yes—dicts are central; also normal Python; Pandas builds on these ideas.
- **Can dicts be used outside Pandas?** Yes; bootcamp teaches core structures for general Python + DS stack.
- **Why `del d1["key"]` not `d1.del()`?** `del` is a **keyword** / statement, not a dict method.
- **Can keys be numbers?** Yes—keys and values can be various types (keys must be hashable).
- **Retrieve deleted key after `del`?** No, unless you saved it elsewhere first.
- **Other ways to access dict values besides `d[k]` and `.get()`?** Instructor only highlighted those two; student asked about more—suggested exploring separately.
- **`elif` vs nested `if`?** `elif` is standard in Python; `else if` is invalid (unlike Java); readability is a common reason to prefer `elif` chains.
- **Undo in notebook?** Cmd+Z (macOS) / Ctrl+Z (Windows).
- **When to leave Colab/Jupyter for `.py`?** When you can run the full pipeline comfortably; still start complex ML work in notebooks for line-by-line debugging.
- **Doubt-clearing sessions “not insightful”?** Expected variance; give **specific** anonymous feedback via Nazia/Aparna, not only poll votes.

## Action Items & Assignments
- [ ] Practice dictionary notebook exercises (employee dict indexing); instructor will share code.
- [ ] Implement even/odd, factorial, sum of natural numbers, palindrome in your own Colab/notebook with correct indentation.
- [ ] Use **poi.com** (per instructor) to request ~10 practice problems on today’s topics (loops, functions, etc.) **without** solutions, then solve them.
- [ ] Vote in the session poll if not done.
- [ ] If the session wasn’t useful, message **Nazia** or **Aparna** on Discord with date (28 March) and specific reasons.
- [ ] Attend doubt-clearing sessions and ask questions if pace is too fast.
- [ ] Advanced students: send answers in **private** chat so beginners can try first.

## Open Questions / Unclear Spots
- Instructor said **sets are immutable** in recap; in standard Python, **set** objects are **mutable** (only **frozenset** is immutable)—likely conflation with “unique elements” or tuple comparison [unclear: exact intent].
- **`poi.com`** for practice problem generation—exact site/tool not confirmed [unclear: poi.com vs poe.com].
- Transcript typo: instructor once said factorial of 5 “output should be **20**” while explaining logic; worked example correctly gives **120**.
- Whether `else if` (two words) works in Python was left as “might work, I don’t know”—in CPython it does **not**; use `elif`.
