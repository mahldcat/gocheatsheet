# Notes
## Channels

### Send and Receive
```go
ch := make(chan int, 1)
ch <- 42

value := <-ch // Receives the value 42 from the channel
fmt.Println(value) // Outputs: 42
```
### One Way
```go
//One Way channel (transmits data)
func Transmit(send chan<- string){
}
//One Way channel (receives only)
func Receive(recv <-chan string){
}
```
## WaitGroups, Mutex and Semaphore
Waitgroup & Mutex should be passed by pointer to external functions

### Mutex
```go
var mu sync.Mutex

//in the function itself:
mu.Lock()
mu.Unlock()
```

### Waitgroup
```go 
var wg sync.WaitGroup
wg.Add(1) for each worker function

//in the worker function:
wg.Done() //in the calling code
defer wg.Done() is a common practice
```
### Semaphore
This can be done with channels && 

# Sample channel with WaitGroups
```go
package main

import (
	"fmt"
	"log"
	"math/rand"
	"sync"
	"time"
)

func worker(path string, wg *sync.WaitGroup, content chan string) {
	defer wg.Done()
	content <- SimulateContentRequest(path)
}

func main() {
	rand.Seed(time.Now().UnixNano())

	workCt := 5
	var wg sync.WaitGroup

	commChan := make(chan string)

	for i := 0; i < workCt; i++ {
		wg.Add(1)

		path := fmt.Sprintf("/path/to/resource/%d", i)
		log.Printf("Starting worker to fetch data from path %s\n", path)
		go worker(path, &wg, commChan)
	}

	log.Println("Workers made, waiting to complete")

	go func() {
		wg.Wait()
		close(commChan)
	}() //anon function?

	// Receiving from the content channel
	for data := range commChan {
		fmt.Println(data)
	}
}

/**********support simulation functions ****************/
func getRand() int {
	return rand.Intn(10) + 1
}

func SimulateContentRequest(path string) string {
	//this simulates a slow API request
	delay := getRand()
	log.Printf("Worker '%s' [delay%d sec]\n", path, delay)

	time.Sleep(time.Duration(delay) * time.Second)
	return fmt.Sprintf("Simulated Content, Delay:%d sec, for path:%s", delay, path)
}

```
