# Tips and tricks

## If $A=0$

```
3 -> RAM_P      # A < 0
:false -> RAM
4 -> RAM_P
:false -> RAM
INV -> A
3 -> RAM_P      # A > 0
:false -> RAM
4 -> RAM_P
:false -> RAM
;true
                # ...
;false
                # ...
```

## If $A=1$

```
# A = A - 1
255 -> B
ADD -> A
    	        # identical to A=0 from this point
3 -> RAM_P      # A < 0
:false -> RAM
4 -> RAM_P
:false -> RAM
INV -> A
3 -> RAM_P      # A > 0
:false -> RAM
4 -> RAM_P
:false -> RAM
;true
                # ...
;false
                # ...
```

## If $A=RAM$

```
# A = A - RAM
0 -> B
ADD -> B        # B = A
RAM -> A
INV -> A        # A = -RAM
ADD -> A
    	        # identical to A=0 from this point
3 -> RAM_P      # A < 0
:false -> RAM
4 -> RAM_P
:false -> RAM
INV -> A
3 -> RAM_P      # A > 0
:false -> RAM
4 -> RAM_P
:false -> RAM
;true
                # ...
;false
                # ...
```

## If $A>B$

```
INV -> A        # -A
ADD -> A        # B - A
3 -> RAM_P      # B - A < 0
:true -> RAM
4 -> RAM_P
:true -> RAM
;false
                # ...
;true
                # ...
```

## Lookup table

```
10 -> RAM_P     # Write LUT
1 -> RAM
11 -> RAM_P
2 -> RAM
12 -> RAM_P
4 -> RAM
13 -> RAM_P
8 -> RAM
14 -> RAM_P
16 -> RAM
15 -> RAM_P
32 -> RAM
4 -> A          # Access LUT at index A
10 -> B         # LUT base pointer
ADD -> RAM_P
RAM -> A
                # ...
0 -> B          # Example: output LUT value
7 -> RAM_P
ADD -> RAM
```

## Integer division

Try making sense of the [LHC implementation](https://github.com/Ryz3D/LHC/blob/ac698c639ea262593f91906a9b4589e2d44960df/src/assembler.cpp#L147). It basically calculates $A = A - B$ until $A$ is negative, the number of subtractions being the result of the division.
