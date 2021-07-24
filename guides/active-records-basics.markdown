---
title: Active Records Basics
layout: guide
toc: true
checklist:
- How to use Active Record models to manipulate data stored in a relational database.
---

## 1 Creating Active Record Models

To create Active Record models, use the `New` method from `activerecord` package
and you're good to go:

```go
var Product = activerecord.New("product", func(r *activerecord.R) {
    r.AttrString("name"),
})
```

The schema above declares a table with two columns: id and name. Each row of this table
represents a certain product with these two parameters. Thus, you would be able to write
code like the following:

```go
p := Product.New()
p.AssignAttribute("name", "Some Book")

fmt.Println(p) // #<Product name: "Some Book">
```
