---
title: Active Records Validations
description: |
  This guide teaches you how to validate the state of objects before
  they go into the database using Active Record's validations feature.
layout: guide
toc: true
checklist:
- How to use built-in Active Record validations.
---

# 1. [Validations Overview](#1-validations-overview)

Call `Validates` method in a record builder to enable validation for an attribute:
```go
Product := activerecord.New("product", func(r *activerecord.R) {
    r.ValidatesPresence("name")
})
```

```go
p := Product.New(activesupport.Hash{"name": nil})
p.IsValid() // false
```

As you can see, the `Product` record without `name` attribute is not valid.

# 2. [Validation Helpers](#2-validation-helpers)

A record builder `aciverecord.R` provides several helpful validators, further we will
briefly describe all of them. For more details, please, refere to the API documentation.

## 2.1. [Presence](#21-presence)

This helper validates that value of the specified attribute is not blank. It uses
`activesupport.IsBlank` method to make a validation. There are two ways to enable
validation of the attribute. The first way is to use `Validates` method:

```go
Product := activerecord.New("product", func(r *activerecord.R) {
    r.Validates("name", &activerecord.Presence{})
    r.Validates("category", &activerecord.Presence{AllowNil: true})
})
```

The second way is to use `ValidatePresence` method that accepts multiple attribute names:
```go
Product := activerecord.New("product", func(r *activerecord.R) {
    r.ValidatesPresence("name", "category", "weight")
})
```

The default error message is `can't be blank`.

## 2.2. [Inclusion](#22-inclusion)

This helper validates that value of the specified attribute is available in a slice.

```go
Movie := activerecord.New("movie", func(r *activerecord.R) {
    r.Validates("format", &activerecord.Inclusion{In: activesupport.Strings("mkv", "avi")})
})
```

The default error message is `is not included in the list`.

## 2.3. [Exclusion](#23-exclusion)

This helper validates that value of the specified attribute is not in the slice.

```go
Zone := activerecord.New("Zone", func(r *activerecord.R) {
    r.Validates("name", &activerecord.Exclusion{From: activesupport.Strings("tk", "biz")})
})
```

The default error message is `is reserved`.

## 2.4. [Format](#24-format)

This helper validates that value of the specified attribute match the regular expression.

```go
Ticket := activerecord.New("ticket", func(r *activerecord.R) {
    r.Validates("reference", &activerecord.Format{With: `[A-Z]{3}-[0-9]{4}`})
})
```

Alternatively, you can request that attribute does not match the regular expression
using `Without` parameter of `Format` helper.

```go
Flight := activerecord.New("flight", func(r *activerecord.R) {
    r.Validates("destination", &activerecord.Format{Without: `[0-9]`})
})
```

The default error message is `has invalid format`.

## 2.5. [Length](#25-length)

This helper validates that the attributes' values comply with the specified length
restrictions.

```go
Book := activerecord.New("book", func(r *activerecord.R) {
    r.Validates("title", &activerecord.Length{Maximum: 50})
    r.Validates("abstract", &activerecord.Length{Minimum: 100, Maximum: 200})
})
```

The default error message when value is too short is `is too short (minimum %d characters)`,
when value is too long error with text `is too long (maximum %d characters)`.
