---
title: Tutorial
description: Some tutorial!
layout: ../../layouts/MainLayout.astro
---

# Using ElectroDB With Existing Data

When using ElectroDB with an existing table and/or data model, there are a few configurations you may need to make to your ElectroDB model. Read the sections below to see if any of the following cases fits your particular needs.

Whenever using ElectroDB with existing tables/data, it is best to use the [Query Option](#query-options) `ignoreOwnership`. ElectroDB leaves some meta-data on items to help ensure data queried and returned from DynamoDB does not leak between entities. Because your data was not made by ElectroDB, these checks could impede your ability to return data.

```typescript
// when building params
.params({ignoreOwnership: true})
// when querying the table
.go({ignoreOwnership: true})
// when using pagination
.page(null, {ignoreOwnership: true})
```

**Your existing index fields have values with mixed case:**

DynamoDB is case-sensitive, and ElectroDB will lowercase key values by default. In the case where you modeled your data with uppercase, or did not apply case modifications, ElectroDB can be configured to match this behavior. Checkout the second on [Index Casing](#index-casing) to read more.

**You have index field names that match attribute names:**

With Single Table Design, it is encouraged to give index fields a generic name, like `pk`, `sk`, `gsi1pk`, etc. In reality, it is also common for tables to have index fields that are named after the domain itself, like `accountId`, `organizationId`, etc.

ElectroDB tries to abstract away your when working with DynamoDB, so instead of defining `pk` or `sk` in your model's attributes, you define them as indexes and map other attributes onto those fields as a composite. Using separate item fields for keys, then for the actual attributes you use in your application, you can leverage more advanced modeling techniques in DynamoDB.

If your existing table uses non-generic fields that also function as attributes, checkout the section [Attributes as Indexes](#attributes-as-indexes) to learn more about how ElectroDB handles these types of indexes.