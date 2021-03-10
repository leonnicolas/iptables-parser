# iptables-parser

Parse lines generated by `iptables-save`.
This parser is inspired by [Ben Johnson's SQL Parser](https://github.com/benbjohnson/sql-parser).

## Description

This parser parses lines returned from `iptables-save` and returns a Line or an Error.
A Line can be a Rule, Comment, Default (default rule) or Header,
all of them being structs.

### Match Extensions

iptables has a lot of match extensions.
Only a few are implemented.
If one is not implemented, the parses returns an error for that line.

### Target Extensions

Just like in [Match Extensions](#Match-Extension), not all of the target extensions are implemented. 

## Example

```go
package main

import (
	"fmt"
	"log"
	"os/exec"
	"strings"

	ipp "github.com/leonnicolas/iptables_parser"
)

func main() {
	o, err := exec.Command("iptables-save").Output()
	if err != nil {
		log.Fatal(err.Error())
	}
	p := ipp.NewParser(strings.NewReader(string(o)))
	for l, err := p.Parse(); err != ipp.ErrEOF; l, err = p.Parse() {
		if err != nil {
			fmt.Println(err.Error())
		} else {
			fmt.Println(l)
		}
	}
}
```
