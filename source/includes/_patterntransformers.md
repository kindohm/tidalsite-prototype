# Pattern Transformers

Pattern transformers are functions that take a pattern as input and transform it into a new pattern.

## Beat Rotation

You specify beat rotation by using the <code>&lt;~</code> and <code>~&gt;</code>
characters. One rotates a pattern to the left, and the other rotates it to the
right.

Simply specify a number, then the direction.

Parameter | Type | Description | Min Value | Max Value
--------- | ---- | ----------- | --------- | ---------
Time | decimal | The amount of time, in cycles to shift. | 0 | None

> Definition

```haskell
(<~) :: Time -> Pattern a -> Pattern a
(~>) :: Time -> Pattern a -> Pattern a
```

> Every 4th cycle, rotate to the left by 0.25 cycle

```haskell
d1 $ every 4 (0.25 <~) $ sound (density 2 "bd sn kurt")
```

> Every 3rd cycle, rotate to the right by 5.75 cycle

```haskell
d1 $ every 3 (5.75 ~>) $ sound (density 2 "bd sn kurt")
```

> Rotate to the right by 10 cycles

```haskell
d1 $ (10 ~>) $ sound (density 2 "bd sn kurt")
```


## brak

brak will take a pattern and kind of turn it into a breakbeat.

> Definition

```haskell
brak :: Pattern a -> Pattern a
```

> Example:

```haskell
d1 $ brak $ sound "bd sn kurt drum"
```


## degrade

Randomly removes events from a pattern 50% of the time.

The shorthand syntax for degrade is a question mark: ?. Using ? will allow you
to randomly remove events from a portion of a pattern.

You can also use ? to randomly remove events from entire sub-patterns.

> Definition

```haskell
degrade :: Pattern a -> Pattern a
```

> Example

```haskell
d1 $ degrade $ sound "[[[feel:5*8,feel*3] feel:3*8], feel*4]/2"
```

> Using the ? shorthand syntax:

```haskell
d1 $ slow 2 $ sound "bd ~ sn bd ~ bd? [sn bd?] ~"
```

> Using ? on only a sub-pattern:

```haskell
d1 $ slow 2 $ sound "[[[feel:5*8,feel*3] feel:3*8]?, feel*4]"
```

## degradeBy

Randomly removes events from a pattern based on a percentage you specify.

> Definition

```haskell
degradeBy :: Double -> Pattern a -> Pattern a
```

Similar to `degrade`, `degradeBy` allows you to control the percentage of events that
are removed.

> To remove events 90% of the time:

```haskell
d1 $ degradeBy 0.9 $ sound "[[[feel:5*8,feel*3] feel:3*8], feel*4]/2"
```

Parameter | Type | Description | Min Value | Max Value
--------- | ---- | ----------- | --------- | ---------
Percentage | decimal | The percentage of random events to remove. | 0 | 1


## density

x

## first

x

## iter

x

## jux (and juxBy)

x

## palindrome

x

## rev

x

## slow

x

## slowspread

x

## smash

x

## slowspread

x

## trunc

x

## zoom

x
