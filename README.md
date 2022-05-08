## `ratio_min()`

### Here, the difflib Python library is improved with one additional function named `.ratio_min()` at line ~549.

---

`.ratio_min(self,m)` is an extension of the difflib's function `.ratio(self)`. Equivalently to `.ratio(self)`, `.ratio_min(self,m)` returns a measure of two sequences' similarity (float in [0,1]).
In addition to .ratio(), it can ignore matched substrings of length less than a given threshold m.

The score is calculated as 2.0\*M_min / T.

Where T is the total number of characters (both sequences), and
M_min is the number of elements matched with every single match having at least m consecutive characters.

Equivalently to `.ratio()`, `.ratio_min()` is applied to the output of SequenceMatcher(None, a, b) where a, b are the two sequences compared.

Note that this is 1 if the sequences are identical, and 0 if
they have no substring of length m or more in common.
`.ratio_min(1)` is equivalent to `.ratio()`.

---

## Basic use example

```
 >>> s = SequenceMatcher(None, "abcd", "bcde")
 >>> s.ratio() # "bcd" matched to "bcd"
 0.75
 >>> s.ratio_min(1) # "bcd" matched to "bcd"
 0.75
 >>> s.ratio_min(2) # "bcd" matched to "bcd"
 0.75
 >>> s.ratio_min(3) # "bcd" matched to "bcd"
 0.75
 >>> s.ratio_min(4) # No match of length 3 or more characters
 0.0
```

## A simple use case

---

### Consider the following use case. We want to compare a given string to a given list of strings.

```
>>> string = 'Animal farm'
>>> strings = ['A nominal farm', 'A minimal farm', 'Animal farm book']
```

The resulting similarity scores between "Animal Farm" and each string in `strings` are:

|              | A nomin.. | A minim.. | Animal .. |
| ------------ | --------- | --------- | --------- |
| ratio()      | 0.800     | **0.880** | 0.815     |
| ratio_min(1) | 0.800     | **0.880** | 0.815     |
| ratio_min(2) | 0.560     | 0.800     | **0.815** |
| ratio_min(3) | 0.560     | 0.800     | **0.815** |

### An alternative approach: starting and ending each string with a space.

This approach can help highlighting "full words".

```
>>> string = ' Animal farm '
>>> strings = [' A nominal farm ', ' A minimal farm ', ' Animal farm book ']
```

|              | A nomin.. | A minim.. | Animal .. |
| ------------ | --------- | --------- | --------- |
| ratio()      | 0.828     | **0.897** | 0.839     |
| ratio_min(1) | 0.828     | **0.897** | 0.839     |
| ratio_min(2) | 0.690     | **0.897** | 0.839     |
| ratio_min(3) | 0.552     | 0.759     | **0.839** |

---

Note that, the original library, named difflib, contains several other functions, here not reported.
