# Chapter 5 Record-like data structures

`namedtuple` from colletions

```python
from collections import namedtuple
>>> Coordinate = namedtuple('Coordinate', 'lat long')
>>> issubclass(Coordinate, tuple)
True
>>> moscow = Coordinate(55.756, 37.617)
>>> moscow
Coordinate(lat=55.756, long=37.617)  1
>>> moscow == Coordinate(lat=55.756, long=37.617)  2
True
```

`typing.NamedTuple` from typing

```python
>>> import typing
>>> Coordinate = typing.NamedTuple('Coordinate', [('lat', float), ('long', float)])
>>> issubclass(Coordinate, tuple)
True
>>> Coordinate.__annotations__
{'lat': <class 'float'>, 'long': <class 'float'>}
```

`@dataclass` decoration from dataclasses

```py
from dataclasses import dataclass

@dataclass(frozen=True)
class Coordinate:

    lat: float
    long: float

    def __str__(self):
        ns = 'N' if self.lat >= 0 else 'S'
        we = 'E' if self.long >= 0 else 'W'
        return f'{abs(self.lat):.1f}°{ns}, {abs(self.long):.1f}°{we}'
```
> By default, @dataclass produces mutable classes. But the decorator accepts several keyword arguments to configure the class, including frozen—shown in above example

## Comparism between "namedtuple", "NamedTuple" and "dataclass"

| | namedtuple             | NamedTuple | dataclass |
|------------------------|------------|-----------|-----|
| mutable instances      | NO         | NO        | YES |
| class statement syntax | NO         | YES       | YES |


## Convert to dict

Use `_asdict` for `namedtuple`
```py
>>> from collections import namedtuple
>>> Coordinate = namedtuple('Coordinate', 'lat long')
>>> moscow = Coordinate(55.756, 37.617)
>>> moscow._asdict()
{'lat': 55.756, 'long': 37.617}
```

Use module level function `asdict` from `dataclasses.asdict`

```py
from dataclasses import dataclass, asdict
@dataclass
class Point:
     x: int
     y: int

>>> p = Point(10, 20)
>>> asdict(p) == {'x': 10, 'y': 20}
{'x': 10, 'y': 20}
```

## Mutable default value issue

```py
@dataclass
class ClubMember:
    name: str
    guests: list = []
```

The `list = []` will raise 
> ValueError: mutable default <class 'list'> for field guests is not allowed: use default_factory

`default_factory` will solve the problem

```py
@dataclass
class ClubMember:
    name: str
    guests: list = field(default_factory=list)
```

# Chapter 6. Object References, Mutability, and Recycling



