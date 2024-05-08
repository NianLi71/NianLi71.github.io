---
layout: post
title:  "Optimistic Locking"
date:   2024-05-08 16:00:00 +0800
categories: CS
---

https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBContext.VersionSupport.html
https://stackoverflow.com/questions/129329/optimistic-vs-pessimistic-locking

### When to use Optimistic locking
Retrieve an item before update, for example:
1. read 
2. change value
3. update

all three steps in separate operation


### DynamoDB atmoic operation
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html

> DynamoDB provides four operations for basic create, read, update, and delete (CRUD) functionality. All these operations are atomic.
> PutItem — Create an item.
> GetItem — Read an item.
> UpdateItem — Update an item.
> DeleteItem — Delete an item.

对于一些简单update操作, 可以使用in place update 来避免使用Optimistic locking