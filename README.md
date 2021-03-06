# DTree

Tool for working with semi-structured data like json or xml

### Usage

Reading json file:
```go
fmt.Println("\nReading json file:\n")
var tree dtree.JsonHandler
if err := tree.ReadFile("start.conf"); err != nil {
		panic(err)
}

fmt.Printf("File \"%v\" exists and was read.\n\n", tree.FileName)

/* Output: 
 File "start.conf" exists and was read.
*/

if err := tree.Decode(); err == nil {
		fmt.Println("tree.Value:", tree.Value)
}

/* Output: 
 tree.Value: map[root:map[Map:map[a:0 b:1 c:2 i0:3] Arr:[a b c 0 1 2]]]
*/
```

Decoding string:
```go
fmt.Println("\nReading json string:\n")
tree.Content = []byte(`{"Other" : {"a" : 0, "b" : 1, "c" : 2, "i0" : 3}}`)
if err := tree.Decode(); err == nil {
		fmt.Println("tree.Value:", tree.Value)
}

/* Output: 
 tree.Value: map[Other:map[a:0 b:1 c:2 i0:3]]
*/
```	

Getting value, used path, remained path and error when path is right:
```go
result := tree.Get("Other.a")

/* Result: 
 result.Value: 0
 result.UsedPath: "Other.a"
 result.RestPath: ""
 result.Error: <nil>
*/
```
Getting value, used path, remained path and error when path is wrong:
```go
result = tree.Get("Other.d")

/* Result: 
 result.Value: <nil>
 result.UsedPath: "Other"
 result.RestPath: "d"
 result.Error: Map has no element with key "d"!
*/
```
Setting values:
```go
result = tree.Set("Other.d", tree.NewValue(`"abc"`))

/* Result: 
 result.Value: abc
 result.UsedPath: "Other.d"
 result.RestPath: ""
 result.Error: <nil>
*/

result = tree.Set("NewArr.2.1", tree.NewValue(`"new_value"`))

/* Result: 
 result.Value: new_value
 result.UsedPath: "NewArr.2.1"
 result.RestPath: ""
 result.Error: <nil>
*/
	
result = tree.Set("NewArr.2.+", tree.NewValue(`"plus_value"`))

/* Result: 
 result.Value: plus_value
 result.UsedPath: "NewArr.2.+"
 result.RestPath: ""
 result.Error: <nil>
*/
```
Final tree:
```go
fmt.Printf(`
 Final tree.Value: "%v"
`, tree.Value )

/* Output: 
 Final tree.Value: map[Other:map[a:0 b:1 c:2 i0:3 d:abc] NewArr:[<nil> <nil> [<nil> new_value plus_value]]]
*/
```