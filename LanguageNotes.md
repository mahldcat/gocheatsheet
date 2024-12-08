## Slices
These will dynamically grow as needed
```go
capacity:= 5
myslice := make([] string, capacity)

for i:=0 ; i <100 ; i++ {
  myslice = append(myslice, i)
}
```
subslices
low range is inclusive, and upper range is exclusive
my slice has indices from 0..99

lopping off the first and last elements of myslice would be 
```go
mySlice[1,99]
```

