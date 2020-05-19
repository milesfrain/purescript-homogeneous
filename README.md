# purescript-homogeneous

This library exploits the underling representation of the `Record` (`Variant` comming soon ;-)) to provide convenient instances when they are homogeneous.

## `Data.Homogeneous.Record`

The core type is:

```purescript
newtype Homogeneous (row ∷ # Type) a = Homogeneous (Object a)
```

`row` only provides information about the structure of a `Record` (we put a `Unit` as a placeholder for values) but `a` is the underling type of values in it.

Given the above we can provide (by mainly newtype derving from the `Object`) many instances for this type like: `Traversable`, `Foldable`, `Monoid`. There is also an `Applicative` instance (which is isomorphic to the instance from `sized-vectors`) which allows us to do:

```purescript

main :: Effect Unit
main = do
  let
    r = { one: 1, two: 2, three: 3 }

    multiply = pure (_ * 2)

    -- | We put a record in
    o = multiply <*> Homogeneous.Record.homogeneous r

    -- | We get a record back
    r' = Homogeneous.Record.toRecord o

  logShow r'
```

which outputs:

```shell
{ one: 2, three: 6, two: 4 }
```

What is quite nice about this `newtype` approach is that underling machinery for deriving all instances is really simple and efficient :-) Additionally it seems that it can be also complemented by `Homogeneous.Variant` in the future...
