### **1. The `stack()` Function in Pandas**

The `stack()` function is used to pivot the columns of a DataFrame into rows. It is especially useful for reshaping data from **wide format** to **long format**, making it easier to analyze or visualize.

---

#### **Syntax**
```python
DataFrame.stack(level=-1, dropna=True)
```

#### **Parameters**
- **`level` (default = -1):**
  - Specifies the column level(s) to stack. Works with MultiIndex columns.
  - Default (`-1`) stacks the innermost column level.
  - Accepts single integer or list of integers for multiple levels.
  
- **`dropna` (default = `True`):**
  - If `True`, removes missing values (`NaN`) from the stacked result.
  - If `False`, retains rows with `NaN`.

#### **Returns**
- A `Series` (if stacking single-level columns) or a `DataFrame` (if stacking multiple levels).

---

#### **Examples**

##### **1.1 Single-Level Columns**
```python
import pandas as pd

# Example DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': [7, 8, 9]
}, index=['Row1', 'Row2', 'Row3'])

print("Original DataFrame:")
print(df)

# Apply stack()
stacked = df.stack()
print("\nStacked DataFrame:")
print(stacked)
```

**Output:**
```
Original DataFrame:
       A  B  C
Row1   1  4  7
Row2   2  5  8
Row3   3  6  9

Stacked DataFrame:
Row1  A    1
      B    4
      C    7
Row2  A    2
      B    5
      C    8
Row3  A    3
      B    6
      C    9
dtype: int64
```

**Explanation:**
- The columns (`A`, `B`, `C`) are pivoted into rows.
- The resulting Series has a **MultiIndex**, with:
  - The first level being the row index (`Row1`, `Row2`, `Row3`).
  - The second level being the column labels (`A`, `B`, `C`).

---

##### **1.2 Multi-Level Columns**
```python
# Multi-Level Columns Example
df = pd.DataFrame({
    ('Group1', 'A'): [1, 2],
    ('Group1', 'B'): [3, 4],
    ('Group2', 'C'): [5, 6]
}, index=['Row1', 'Row2'])

print("Original DataFrame with Multi-Level Columns:")
print(df)

# Apply stack()
stacked = df.stack()
print("\nStacked DataFrame:")
print(stacked)
```

**Output:**
```
Original DataFrame with Multi-Level Columns:
       Group1      Group2
            A  B       C
Row1       1  3       5
Row2       2  4       6

Stacked DataFrame:
              A  B
Row1 Group1   1  3
     Group2   5
Row2 Group1   2  4
     Group2   6
```

**Explanation:**
- The innermost level of the column MultiIndex (`A`, `B`, `C`) is stacked by default.

##### **1.3 Using the `level` Parameter**
```python
# Stack only the outermost column level
stacked_level_0 = df.stack(level=0)
print("\nStacked by Level 0:")
print(stacked_level_0)
```

**Output:**
```
Stacked by Level 0:
        A  B
Row1 Group1   1  3
     Group2   5
Row2 Group1   2  4
     Group2   6
```

---

##### **1.4 Using the `dropna` Parameter**
```python
# DataFrame with Missing Values
df = pd.DataFrame({
    'A': [1, None, 3],
    'B': [4, 5, None],
    'C': [None, 8, 9]
}, index=['Row1', 'Row2', 'Row3'])

# Apply stack() with dropna=True
stacked = df.stack()
print("\nStacked with dropna=True:")
print(stacked)

# Apply stack() with dropna=False
stacked_with_nan = df.stack(dropna=False)
print("\nStacked with dropna=False:")
print(stacked_with_nan)
```

**Output:**
```
Stacked with dropna=True:
Row1  A    1.0
      B    4.0
Row2  B    5.0
      C    8.0
Row3  A    3.0
      C    9.0
dtype: float64

Stacked with dropna=False:
Row1  A    1.0
      B    4.0
      C    NaN
Row2  A    NaN
      B    5.0
      C    8.0
Row3  A    3.0
      B    NaN
      C    9.0
dtype: float64
```

---

### **2. The `select_dtypes()` Function in Pandas**

The `select_dtypes()` function is used to filter a DataFrame based on the **data types of its columns**.

---

#### **Syntax**
```python
DataFrame.select_dtypes(include=None, exclude=None)
```

#### **Parameters**
- **`include`:**
  - A single dtype or list of dtypes to include.
  - Examples: `'number'`, `'object'`, `'float64'`, etc.
  
- **`exclude`:**
  - A single dtype or list of dtypes to exclude.
  - Cannot be used simultaneously with `include`.

#### **Returns**
- A DataFrame containing only the columns that match the specified data types.

---

#### **Examples**

##### **2.1 Include Numeric Columns**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': ['x', 'y', 'z'],
    'C': [4.5, 5.5, 6.5],
    'D': [True, False, True]
})

numeric_df = df.select_dtypes(include='number')
print("Numeric Columns:")
print(numeric_df)
```

**Output:**
```
Numeric Columns:
   A    C
0  1  4.5
1  2  5.5
2  3  6.5
```

---

##### **2.2 Include and Exclude Specific Types**
```python
# Include only object columns
object_df = df.select_dtypes(include='object')
print("\nObject Columns:")
print(object_df)

# Exclude boolean columns
non_boolean_df = df.select_dtypes(exclude='bool')
print("\nNon-Boolean Columns:")
print(non_boolean_df)
```

**Output:**
```
Object Columns:
   B
0  x
1  y
2  z

Non-Boolean Columns:
   A  B    C
0  1  x  4.5
1  2  y  5.5
2  3  z  6.5
```

---

##### **2.3 Multiple Types**
```python
# Include numeric and boolean columns
numeric_and_bool = df.select_dtypes(include=['number', 'bool'])
print("\nNumeric and Boolean Columns:")
print(numeric_and_bool)
```

**Output:**
```
Numeric and Boolean Columns:
   A    C      D
0  1  4.5   True
1  2  5.5  False
2  3  6.5   True
```

---

### **Key Differences Between `stack()` and `select_dtypes()`**
| Feature              | `stack()`                              | `select_dtypes()`                          |
|----------------------|----------------------------------------|-------------------------------------------|
| Purpose              | Reshape columns into rows.            | Filter columns by data type.              |
| Output               | Series or DataFrame (stacked format). | DataFrame with selected columns.          |
| Handles Data Types   | Not directly. Must pre-filter.         | Filters columns directly by type.         |
| Common Use Case      | Reshaping for analysis or visualization. | Cleaning data by selecting specific types.|



The `.astype()` method in the pandas library is used to cast a pandas object (such as a Series or DataFrame) to a specified data type. It is often used to convert data types for efficient processing or to ensure compatibility with specific operations.

Here’s an overview with examples:

---

### 1. **Basic Usage**
Convert a column or Series to a specific data type.

```python
import pandas as pd

data = {'A': ['1', '2', '3'], 'B': [0.5, 1.5, 2.5]}
df = pd.DataFrame(data)

# Convert column 'A' to integer
df['A'] = df['A'].astype(int)

print(df)
print(df.dtypes)
```

**Output:**
```
   A    B
0  1  0.5
1  2  1.5
2  3  2.5
A    int64
B    float64
dtype: object
```

---

### 2. **Convert to String**
Convert numerical columns to strings.

```python
df['B'] = df['B'].astype(str)
print(df)
print(df.dtypes)
```

**Output:**
```
   A    B
0  1  0.5
1  2  1.5
2  3  2.5
A     int64
B    object
dtype: object
```

---

### 3. **Convert to Float**
Convert a column to a float type.

```python
data = {'A': ['1', '2', '3']}
df = pd.DataFrame(data)

# Convert to float
df['A'] = df['A'].astype(float)

print(df)
print(df.dtypes)
```

**Output:**
```
     A
0  1.0
1  2.0
2  3.0
A    float64
dtype: object
```

---

### 4. **Convert to Categorical**
Converting columns to categorical type to save memory and speed up operations.

```python
data = {'A': ['low', 'medium', 'high']}
df = pd.DataFrame(data)

# Convert to categorical
df['A'] = df['A'].astype('category')

print(df)
print(df.dtypes)
```

**Output:**
```
        A
0     low
1  medium
2    high
A    category
dtype: object
```

---

### 5. **Convert with Multiple Types in DataFrame**
Convert specific columns to specific data types.

```python
data = {'A': ['1', '2', '3'], 'B': [0.5, 1.5, 2.5]}
df = pd.DataFrame(data)

# Convert multiple columns
df = df.astype({'A': 'int', 'B': 'float'})

print(df)
print(df.dtypes)
```

**Output:**
```
   A    B
0  1  0.5
1  2  1.5
2  3  2.5
A      int64
B    float64
dtype: object
```

---

### 6. **Handling Invalid Conversions**
Use `errors='ignore'` or `errors='coerce'` to handle invalid conversions.

#### `errors='coerce'`
Invalid parsing will result in `NaN`.

```python
data = {'A': ['1', 'two', '3']}
df = pd.DataFrame(data)

# Attempt conversion, invalid values become NaN
df['A'] = df['A'].astype(float, errors='coerce')

print(df)
```

**Output:**
```
     A
0  1.0
1  NaN
2  3.0
```

#### `errors='ignore'`
Ignore invalid conversions and leave the column unchanged.

```python
df['A'] = df['A'].astype(int, errors='ignore')
print(df)
```

**Output:**
The column remains unchanged.

---

### 7. **Convert to Datetime**
Convert a column to `datetime`.

```python
data = {'A': ['2023-01-01', '2023-02-01', '2023-03-01']}
df = pd.DataFrame(data)

# Convert to datetime
df['A'] = df['A'].astype('datetime64[ns]')

print(df)
print(df.dtypes)
```

**Output:**
```
           A
0 2023-01-01
1 2023-02-01
2 2023-03-01
A    datetime64[ns]
dtype: object
```

---

### 8. **Convert to Boolean**
Convert a column to a boolean type.

```python
data = {'A': [1, 0, 1]}
df = pd.DataFrame(data)

# Convert to boolean
df['A'] = df['A'].astype(bool)

print(df)
print(df.dtypes)
```

**Output:**
```
       A
0   True
1  False
2   True
A    bool
dtype: object
```

---
