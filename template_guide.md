# Basic Go Templating for HTML

This guide explains how to use Go's `html/template` package to inject dynamic data (like names and dates) into your HTML certificate templates.

## Basic Syntax
Go templates use double curly braces `{{ }}` to denote "actions."

### Accessing Data (`.`)
The dot `.` represents the current data object passed to the template. If you pass a "Person" struct, `{{.Name}}` will display the name.

```html
<h1>Certificate of Completion</h1>
<p>This is awarded to:</p>
<h2>{{.RecipientName}}</h2>

```

### Conditionals (`if`)

You can show or hide parts of the certificate based on data.

```html
{{if .Honors}}
    <div class="medal">Graduated with Honors</div>
{{else}}
    <div class="spacer"></div>
{{end}}

```

---

## 5. Learning More

Go templating is very powerful. For advanced techniques like loops (`range`), custom functions, or template nesting, refer to the official documentation:

* **[Official Go html/template Documentation](https://pkg.go.dev/html/template)**
