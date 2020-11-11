# 🔱 Conditions – A parser of a simple conditions specification language

[![CircleCI](https://badgen.net/circleci/github/JulienBreux/conditions/master)](https://circleci.com/gh/JulienBreux/conditions)
[![Go Report Card](https://goreportcard.com/badge/github.com/JulienBreux/conditions)](https://goreportcard.com/report/github.com/JulienBreux/conditions)
[![GoDoc](https://godoc.org/github.com/JulienBreux/conditions?status.svg)](http://godoc.org/github.com/JulienBreux/conditions)
[![GitHub tag](https://img.shields.io/github/tag/JulienBreux/conditions.svg)](Tag)
[![Go version](https://img.shields.io/badge/go-v1.15-blue)](https://golang.org/dl/#stable)

This package offers a parser of a simple conditions specification language (reduced set of arithmetic/logical operations).
The package is mainly created for Flow-Based Programming components that require configuration to perform some operations
on the data received from multiple input ports.
But it can be used wherever you need externally define some logical conditions on the internal variables.

Additional credits for this package go to:

- [Handwritten Parsers & Lexers in Go](http://blog.gopheracademy.com/advent-2014/parsers-lexers/) by Ben Johnson
- [InfluxML package from InfluxDB repository](https://github.com/influxdb/influxdb/tree/master/influxql)
- [Zhuojie Zhou](https://github.com/zhouzhuojie/conditions)

## Usage example

```go
package main

import (
    "fmt"
    "strings"

    "github.com/julienbreux/conditions"
)

func main() {
    // Our condition to check
    s := `({foo} > 0.45) AND ({bar} == "ON" OR {baz} IN ["ACTIVE", "CLEAR"])`

    // Parse the condition language and get expression
    p := conditions.NewParser(strings.NewReader(s))
    expr, err := p.Parse()
    if err != nil {
        // ...
    }

    // Evaluate expression passing data for $vars
    data := map[string]interface{}{"foo": 0.12, "bar": "OFF", "baz": "ACTIVE"}
    r, err := conditions.Evaluate(expr, data)
    if err != nil {
        // ...
    }

    // r is false
    fmt.Println("Evaluation result:", r)
}
```

## Credit

Forked from [https://github.com/zhouzhuojie/conditions](https://github.com/zhouzhuojie/conditions)

The main differences are

- Usage of go modules in go `1.15`.
- Allow variables with `.` in name.
