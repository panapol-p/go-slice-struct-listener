# go-slice-struct-listener

A Go library to listen slice of struct (when feed new data then output show id was added , id was updated or id was deleted)
<hr>



## Usage <a id="usage"></a>
```go
package main

import (
	"fmt"

	listener "github.com/panapol-p/go-slice-struct-listener"
)

type FeedData struct {
	ID    string `listener:"id"`
	Name  string
	Score float32
}

func main() {
	fs := []FeedData{
		{ID: "1", Name: "Bob", Score: 98.50},
		{ID: "2", Name: "Joe", Score: 92.50},
	}

	l := listener.NewListener[FeedData]()

	// set callback func if you need
	f := func(e []listener.Events[FeedData]) {
		fmt.Println("[callback func]", "receive new event!!", e)
	}
	l.SetCallback(f)

	events := l.AddNewValue(fs)
	fmt.Println(events) // [{1 added {1 Bob 98.5}} {2 added {2 Joe 92.5}}]

	fs = []FeedData{
		{ID: "1", Name: "Bob", Score: 96.50},
		{ID: "2", Name: "Joe", Score: 92.50},
		{ID: "3", Name: "Micky", Score: 89.70},
	}
	events = l.AddNewValue(fs)
	fmt.Println(events) // [{1 updated {1 Bob 96.5}} {3 added {3 Micky 89.7}}]

	fs = []FeedData{
		{ID: "1", Name: "Bob", Score: 96.50},
	}
	events = l.AddNewValue(fs)
	fmt.Println(events) // [{2 deleted {  0}} {3 deleted {  0}}]
}
```

## License <a id="license"></a>
Distributed under the MIT License. See [license](LICENSE) for more information.

## Contributing <a id="contributing"></a>
Contributions are welcome! Feel free to check our [open issues](https://github.com/panapol-p/go-slice-struct-listener/issues).