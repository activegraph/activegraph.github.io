---
title: Active Records Associations
description: |
  This guide teaches you how to associate multiple models.
layout: guide
toc: true
checklist:
- How to use Active Records associations.
---

# 1. [Types of Associations](#1-types-of-associations)

## 1.1. [The `BelongsTo` Association](#11-the-blongsto-association)

```go
Owner := activerecord.New("owner", func(r *activerecord.R) {
  r.BelongsTo("target")
})
```

If you want to access the associated record, use the `Association` method:
```go
target := Owner.Association("target")
fmt.Println(target) // Ok(Some(<#Target id: 2, name: "Bill">))
```

## 1.2. [The `HasOne` Association](#12-the-hasone-association)

```go
Owner := activerecord.New("owner", func(r *activerecord.R) {
  r.HasOne("target")
})
```

```go
target := Owner.Association("target")
fmt.Println(target) // Ok(Some(<#Target id: 4, owner_id: 5, name: "John"))
```


## 1.3. [The `HasMany` Association](#13-the-hasmany-association)

```go
Owner := activerecord.New("owner", func(r *activerecord.R) {
  r.HasMany("targets")
})
```

If you want to access the collection of association records, use the `Collection` method:
```go
targets := Owner.Collection("targets")
fmt.Println(targets.First()) // #<Target id: 3, owner_id: 1, name: "Elon">
```
