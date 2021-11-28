---
title: Active Records Basics
description: |
    This guide is an introduction to Active Record. After reading this guide
    you'll know:
layout: guide
toc: true
checklist:
- How to use Active Record models to manipulate data stored in a relational database.
---

# 1. [Creating Active Record Models](#1-creating-active-record-models)

To create Active Record models, use the `New` method from `activerecord` package
and you're good to go:

```go
Product := activerecord.New("product")
```

Suppose that the `products` table was created using an SQL statements like following:
```sql
CREATE TABLE products (
  id    INTEGER    NOT NULL auto_increment,
  name  VARCHAR,
  PRIMARY KEY (id)
);
```

The schema above declares a table with two columns: id and name. Each row of this table
represents a certain product with these two parameters. Thus, you would be able to write
code like the following:

```go
p := Product.New()
p.AssignAttribute("name", "Some Book")

fmt.Println(p) // #<Product name: "Some Book">
```

<hr>

# 2. [Reading and Writing Data](#2-reading-and-writing-data)

## 2.1. [Create](#21-create)

Active Record objects can be created from hash or have their attributes manually set
after creation.

For example, using a `Create` a new instance of the model will be created and inserted into
the database.
```go
user = User.Create(activesupport.Hash{"name": "Eric", "occupation": "psychiatrist"})
```

Using `New` method, a new instance of the model will be created without being saved.
```go
user := User.New()
user.AssignAttribute("name", "Eric")
user.AssignAttribute("occupation", "psychiatrist")
```

## 2.2. [Read](#22-read)

Active Record provides API for accessing data within a database. Below there are few
examples of different approaches accessing data.

```go
// return a collection of all users.
users := User.All()
```

```go
// return a user with name "Eric".
user := User.Where("name", "Eric")
```

```go
// return a user with id = 1.
user := User.Find(1)
```

## 2.3. [Update](#23-update)

TBD.

## 2.4. [Delete](#24-delete)

Once retrieved, an Active Record instance can be removed from a database.

```go
user := User.FindBy("name", "Eric")
user.Delete()
```

Collection of records can be destroyed using `DestroyAll` method.
```go
user := User.Where("occupation", "psychiatrist")
user.DeleteAll()
```
