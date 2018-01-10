# text/template
Dynamically load sub templates with {{template .MyVariable}}. Forked from Go's built in library "text/template" version 1.9.2.

[![Build Status](https://travis-ci.org/speedyhoon/text.svg?branch=master)](https://travis-ci.org/speedyhoon/text)
[![go report card](https://goreportcard.com/badge/github.com/speedyhoon/text)](https://goreportcard.com/report/github.com/speedyhoon/text)

## Example
```go
package main

import (
	"log"
	"os"
	"github.com/speedyhoon/text/template"
)

func main() {
	const tpl = `{{.Title}}
Template name: {{.SubTemplate}}
Standard template: {{template "foo" .}}
Dynamic template: {{template .SubTemplate .}}
{{define "foo"}}
	Template data: {{.Data}}
{{end}}`

	data := struct {
		Title string
		Items []string
		SubTemplate string
		Data string
	}{
		Title: "My page",
		SubTemplate: "foo",
		Data: "Four, Five, Six.",
	}

	err := template.Must(template.New("page").Parse(tpl)).Execute(os.Stdout, data)
	if err != nil {
		log.Fatal(err)
	}
}
```

## Output
```
My page
Template name: foo
Standard template:
        Template data: Four, Five, Six.

Dynamic template:
        Template data: Four, Five, Six.

```
