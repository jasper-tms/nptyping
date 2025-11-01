[![PyPI version](https://img.shields.io/pypi/pyversions/np2typing.svg)](https://img.shields.io/pypi/pyversions/np2typing.svg)
[![Downloads](https://pepy.tech/badge/nptyping/month)](https://pepy.tech/project/nptyping)
[![PyPI version](https://badge.fury.io/py/np2typing.svg)](https://badge.fury.io/py/nptyping)
[![codecov](https://codecov.io/gh/ramonhagenaars/nptyping/branch/master/graph/badge.svg)](https://codecov.io/gh/ramonhagenaars/nptyping)
[![Code style](https://img.shields.io/badge/code%20style-black-black)](https://img.shields.io/badge/code%20style-black-black)


<p align='center'>
  <a href='https://https://pypi.org/project/np2typing/'>
    <img src='https://github.com/jasper-tms/nptyping/raw/main/resources/logo.png' />
  </a> 
</p>

🧊 *Type hints for `NumPy`* <br/>
🐼 *Type hints for `pandas.DataFrame`* <br/>
💡 *Extensive dynamic type checks for dtypes shapes and structures* <br/>
🚀 *[Jump to the Quickstart](https://github.com/jasper-tms/nptyping/blob/main/USERDOCS.md#Quickstart)*

[Ramon Hagenaars' original `nptyping` package](https://github.com/ramonhagenaars/nptyping) is available via `pip install nptyping`. This fork has been updated to work with NumPy version 2 and above, and is available via `pip install np2typing`. The vast majority of the code in this package remains unchanged from Ramon's original work.

Example of a hinted `numpy.ndarray`:

```python
>>> from nptyping import NDArray, Int, Shape

>>> arr: NDArray[Shape["2, 2"], Int]

```

Example of a hinted `pandas.DataFrame`:

```python
>>> from nptyping import DataFrame, Structure as S

>>> df: DataFrame[S["name: Str, x: Float, y: Float"]]

```

### Installation

| Command                           | Description                   |
|:----------------------------------|-------------------------------|
| `pip install np2typing`           | Install the basics            |
| `pip install np2typing[pandas]`   | Install with pandas extension |
| `pip install np2typing[complete]` | Install with all extensions   |

### Instance checking

Example of instance checking:
```python
>>> import numpy as np

>>> isinstance(np.array([[1, 2], [3, 4]]), NDArray[Shape["2, 2"], Int])
True

>>> isinstance(np.array([[1., 2.], [3., 4.]]), NDArray[Shape["2, 2"], Int])
False

>>> isinstance(np.array([1, 2, 3, 4]), NDArray[Shape["2, 2"], Int])
False

```

`nptyping` also provides `assert_isinstance`. In contrast to `assert isinstance(...)`, this won't cause IDEs or MyPy
complaints. Here is an example: 
```python
>>> from nptyping import assert_isinstance

>>> assert_isinstance(np.array([1]), NDArray[Shape["1"], Int])
True

```

### NumPy Structured arrays

You can also express structured arrays using `nptyping.Structure`:
```python
>>> from nptyping import Structure

>>> Structure["name: Str, age: Int"]
Structure['age: Int, name: Str']

```

Here is an example to see it in action:
```python
>>> from typing import Any
>>> import numpy as np
>>> from nptyping import NDArray, Structure

>>> arr = np.array([("Peter", 34)], dtype=[("name", "U10"), ("age", "i4")])
>>> isinstance(arr, NDArray[Any, Structure["name: Str, age: Int"]])
True

```

Subarrays can be expressed with a shape expression between square brackets:
```python
>>> Structure["name: Int[3, 3]"]
Structure['name: Int[3, 3]']

```

### NumPy Record arrays
The recarray is a specialization of a structured array. You can use `RecArray`
to express them.

```python
>>> from nptyping import RecArray

>>> arr = np.array([("Peter", 34)], dtype=[("name", "U10"), ("age", "i4")])
>>> rec_arr = arr.view(np.recarray)
>>> isinstance(rec_arr, RecArray[Any, Structure["name: Str, age: Int"]])
True

```

### Pandas DataFrames
Pandas DataFrames can be expressed with `Structure` also. To make it more concise, you may want to alias `Structure`.
```python
>>> from nptyping import DataFrame, Structure as S

>>> df: DataFrame[S["x: Float, y: Float"]]

```

### More examples

Here is an example of a rich expression that can be done with `nptyping`:
```python
def plan_route(
        locations: NDArray[Shape["[from, to], [x, y]"], Float]
) -> NDArray[Shape["* stops, [x, y]"], Float]:
    ...

```

More examples can be found in the [documentation](https://github.com/jasper-tms/nptyping/blob/main/USERDOCS.md#Examples).

## Documentation

* [User documentation](https://github.com/jasper-tms/nptyping/blob/main/USERDOCS.md) <br/>
The place to go if you are using this library. <br/><br/>
  
* [Release notes](https://github.com/jasper-tms/nptyping/blob/main/HISTORY.md) <br/>
To see what's new, check out the release notes. <br/><br/>

* [Contributing](https://github.com/jasper-tms/nptyping/blob/main/CONTRIBUTING.md) <br/>
If you're interested in developing along, find the guidelines here. <br/><br/>

* [License](https://github.com/jasper-tms/nptyping/blob/main/LICENSE) <br/>
If you want to check out how open source this library is.
