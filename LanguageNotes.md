## Slices
These will dynamically grow as needed
```go
capacity:= 5
myslice := make([] string, capacity)

for i:=0 ; i <100 ; i++ {
  myslice = append(myslice, i)
}
```
###subslices
low range is inclusive, and upper range is exclusive
my slice has indices from 0..99

lopping off the first and last elements of myslice would be 
```go
mySlice[1,99]
```
###Sorting
Ues the built in "sort" library
These are "in place" sorts of the slice
* sort.Ints([]int)
* sort.Strings([]string)
 
For soorting nonstandard/structs, 
Example Code
```go
type kv struct {
  ch   rune
  freq int
}

pairs := make([]kv, 0, len(runeMap))
for ch, freq := range runeMap {
  pairs = append(pairs, kv{ch, freq})
}

// Sort pairs by frequency in descending order
sort.Slice(pairs, func(i, j int) bool {
  return pairs[i].freq > pairs[j].freq
})
```

