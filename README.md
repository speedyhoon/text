# text/template
Dynamically load sub templates with {{template .MyVariable}}. Forked from Go's built in library "text/template" version 1.9.2.

[![Build Status](https://travis-ci.org/speedyhoon/text.svg?branch=master)](https://travis-ci.org/speedyhoon/text)
[![go report card](https://goreportcard.com/badge/github.com/speedyhoon/text)](https://goreportcard.com/report/github.com/speedyhoon/text)

```
import (
	"github.com/speedyhoon/text/template"
)
```

## Example
```
package main

import (
	"log"
	"os"
	"github.com/speedyhoon/text/template"
)

func main() {
	const tpl = `
<!doctype html>
<html>
	<head>
		<title>{{.Title}}</title>
	</head>
	<body>
		<p>Template name: {{.SubTemplate}}
		{{template "foo" .}}
		{{template .SubTemplate .}}
	</body>
</html>
{{define "foo"}}<div>Template data: {{.Data}}</div>{{end}}`

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

	t, err := template.New("page").Parse(tpl)
	if err != nil {
		log.Fatal(err)
	}

	err = t.Execute(os.Stdout, data)
	if err != nil {
		log.Fatal(err)
	}
}
```

## Output
```
<!doctype html>
<html>
        <head>
                <title>My page</title>
        </head>
        <body>
                <p>Template name: foo
                <div>Template data: Four, Five, Six.</div>
                <div>Template data: Four, Five, Six.</div>
        </body>
</html>
```
