---
title: Action Controller Overview
layout: guide
toc: true
checklist:
- How to follow the flow of a request through a controller.
---

# 1. [Actions](#1-actions)

When the controller receives the request, the routing determines which controller
and action to run, then creates a new context and executes the necessary method.

```go
ProductController := actioncontroller.New(func(c *actioncontroller.C) {

    // Index action is rendered as a `products { }` query in the resulting
    // GraphQL scheama. Additionally, the input parameters could be overridden
    // with `Permit` call.
    c.Index(func(ctx *actioncontroller.Context) actioncontroller.Result {
        products := activesupport.Return(Product.All().ToA())

        // Action should return implementation of a `Result`,
        // in case of GraphQL, this is always `ContentResult`.
        return actionview.ContentResult(authors)
    })
})
```

As an example, if a user calls query `query { products { id, name } }` in your
application to list all available products, Active Graph will execute an action
`Index` of the `ProductController`.


# 2. [Filters](#2-filters)

```go
func RequireLogin(ctx *actioncontroller.Context) actioncontroller.Result {
    if !loggedIn(ctx) {
        return actionview.ContentResult(errors.New("not logged in"))
    }
    return nil
}


AdminController := actioncontroller.New(func(c *actioncontroller.C) {
    c.BeforeAction(RequireLogin)
})
```
