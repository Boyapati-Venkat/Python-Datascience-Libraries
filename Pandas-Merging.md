In **pandas**, merging (or joining) combines two DataFrames based on a common column or index. The type of merge determines how the rows from both DataFrames are included in the result.

---

### **Types of Merges**

1. **Inner Merge**:
   - Includes only the rows with matching keys in both DataFrames.
   - Intersection of the keys.

2. **Left Merge**:
   - Includes all rows from the left DataFrame and matches from the right DataFrame.
   - If no match is found in the right DataFrame, `NaN` is used.

3. **Right Merge**:
   - Includes all rows from the right DataFrame and matches from the left DataFrame.
   - If no match is found in the left DataFrame, `NaN` is used.

4. **Full Merge**:
   - Includes all rows from both DataFrames.
   - Rows without a match in either DataFrame are filled with `NaN`.

---

### **Syntax**
```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None)
```

### **Parameters**
- **`left` and `right`:** DataFrames to merge.
- **`how`:** Type of merge (`'inner'`, `'left'`, `'right'`, `'outer'`).
- **`on`:** Column(s) to join on (must be common to both DataFrames).
- **`left_on` and `right_on`:** Columns to join on in the left and right DataFrames if they are different.

---

### **Examples**

#### **Sample DataFrames**
```python
import pandas as pd

# First DataFrame
df1 = pd.DataFrame({
    'ID': [1, 2, 3],
    'Name': ['Alice', 'Bob', 'Charlie']
})

# Second DataFrame
df2 = pd.DataFrame({
    'ID': [2, 3, 4],
    'Score': [85, 90, 88]
})
```

#### **1. Inner Merge**
Keeps only the rows with matching `ID` in both DataFrames.
```python
result = pd.merge(df1, df2, how='inner', on='ID')
print(result)
```

**Output:**
```
   ID   Name  Score
0   2    Bob     85
1   3 Charlie     90
```

---

#### **2. Left Merge**
Keeps all rows from `df1` and matches rows from `df2`. Non-matching rows in `df2` are filled with `NaN`.
```python
result = pd.merge(df1, df2, how='left', on='ID')
print(result)
```

**Output:**
```
   ID     Name  Score
0   1    Alice    NaN
1   2      Bob   85.0
2   3  Charlie   90.0
```

---

#### **3. Right Merge**
Keeps all rows from `df2` and matches rows from `df1`. Non-matching rows in `df1` are filled with `NaN`.
```python
result = pd.merge(df1, df2, how='right', on='ID')
print(result)
```

**Output:**
```
   ID     Name  Score
0   2      Bob   85.0
1   3  Charlie   90.0
2   4      NaN   88.0
```

---

#### **4. Full (Outer) Merge**
Keeps all rows from both DataFrames. Non-matching rows in either DataFrame are filled with `NaN`.
```python
result = pd.merge(df1, df2, how='outer', on='ID')
print(result)
```

**Output:**
```
   ID     Name  Score
0   1    Alice    NaN
1   2      Bob   85.0
2   3  Charlie   90.0
3   4      NaN   88.0
```

---

### **Merging on Different Column Names**
If the key columns have different names in the DataFrames, use `left_on` and `right_on`.

```python
df1 = pd.DataFrame({
    'UserID': [1, 2, 3],
    'Name': ['Alice', 'Bob', 'Charlie']
})

df2 = pd.DataFrame({
    'ID': [2, 3, 4],
    'Score': [85, 90, 88]
})

result = pd.merge(df1, df2, how='inner', left_on='UserID', right_on='ID')
print(result)
```

**Output:**
```
   UserID     Name  ID  Score
0       2      Bob   2     85
1       3  Charlie   3     90
```

---

### **Use Cases**
- **Inner Merge:** When you want only the rows with matching data.
- **Left Merge:** When you want all data from the left table, regardless of matches.
- **Right Merge:** When you want all data from the right table, regardless of matches.
- **Full Merge:** When you want a comprehensive dataset that includes all rows from both tables.

Let me know if you'd like further examples or explanations!
