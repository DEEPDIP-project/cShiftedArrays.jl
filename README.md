# ShiftedArrays

[![Build Status](https://travis-ci.org/piever/ShiftedArrays.jl.svg?branch=master)](https://travis-ci.org/piever/ShiftedArrays.jl)

[![codecov.io](http://codecov.io/github/piever/ShiftedArrays.jl/coverage.svg?branch=master)](http://codecov.io/github/piever/ShiftedArrays.jl?branch=master)

Implementation of shifted arrays.

## Shifted Arrays

A `ShiftedArray` is a lazy view of an Array, shifted on its first indexing dimension by a constant value `n`.

```julia
julia> v = reshape(1:16, 4, 4)
4×4 Base.ReshapedArray{Int64,2,UnitRange{Int64},Tuple{}}:
 1  5   9  13
 2  6  10  14
 3  7  11  15
 4  8  12  16

julia> s = ShiftedArray(v, 2)
4×4 ShiftedArrays.ShiftedArray{Int64,2,Base.ReshapedArray{Int64,2,UnitRange{Int64},Tuple{}}}:
 3         7         11         15       
 4         8         12         16       
  missing   missing    missing    missing
  missing   missing    missing    missing
```

The parent Array as well as the amount of shifting can be recovered with `parent` and `index` shift respectively.

```julia
julia> parent(s)
4×4 Base.ReshapedArray{Int64,2,UnitRange{Int64},Tuple{}}:
 1  5   9  13
 2  6  10  14
 3  7  11  15
 4  8  12  16

julia> indexshift(s)
2
```

Use `copy` to collect the shifted data into an `Array`:

```julia
julia> copy(s)
4×4 Array{Union{Int64, Missings.Missing},2}:
 3         7         11         15       
 4         8         12         16       
  missing   missing    missing    missing
  missing   missing    missing    missing
```

## Shifting the data

Using the `ShiftedArray` type, this package provides two operations for lazily shifting vectors: `lag` and `lead`.

```julia
julia> v = [1, 3, 5, 4];

julia> lag(v)
4-element ShiftedArrays.ShiftedArray{Int64,1,Array{Int64,1}}:
  missing
 1       
 3       
 5       

julia> v .- lag(v) # compute difference from previous element without unnecessary allocations
4-element Array{Any,1}:
   missing
  2       
  2       
 -1       

julia> s = lag(v, 2) # shift by more than one element
4-element ShiftedArrays.ShiftedArray{Int64,1,Array{Int64,1}}:
  missing
  missing
 1       
 3
```

`lead` is the analogous of `lag` but shifts in the opposite direction:

```julia
julia> v = [1, 3, 5, 4];

julia> lead(v)
4-element ShiftedArrays.ShiftedArray{Int64,1,Array{Int64,1}}:
 3       
 5       
 4       
  missing
```
