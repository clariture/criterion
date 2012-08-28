# Query language

describe sql where-conditions in a language similar to the [mongo query language](http://www.mongodb.org/display/DOCS/Advanced+Queries)

### Install

    npm install query-language

### Usage

make query from object

```coffeescript
query = new Query {x: 7, y: 'foo'}

query.sql()
# 'x = ? AND y = ?'

query.params()
# [7, 'foo']
```

make query from string

```coffeescript
query = new Query 'x = ? AND y = ?', 6, 'bar'

query.sql()
# 'x = ? AND y = ?'
query.params()
# [6, 'bar']
```

combine queries

```coffeescript
query1 = new Query {x: 7, y: 'foo'}
query2 = new Query 'z = ?', true

query1.and(query2).sql()
# 'x = ? AND y = ? AND z = ?'
query1.and(query2).params()
# [7, 'foo', true]

query2.or(query1).sql()
# '(z = ?) OR (x = ? AND y = ?)'
query2.or(query1).params()
# [true, 7, 'foo']
```

negate queries

```coffeescript
query = new Query {x: 7, y: 'foo'}
query.negate().sql()
# 'NOT (x = ? AND y = ?)'
query.negate().params()
# [7, 'foo', true]
```

### Possible arguments to `new Query`

find where `x = 7` and `y = 'foo'`

```coffeescript
{x: 7, y: 'foo'}
# or
'x = ? AND y = ?', 7, 'foo'
```

find where `x` is in `[1, 2, 3]`

```coffeescript
{x: [1, 2, 3]}
```

find where `x` is not in `[1, 2, 3]`

```coffeescript
{x: {$nin: [1, 2, 3]}}
```

find where `x != 3`

```coffeescript
{x: {$ne: 3}}
# or
'x != ?', 3
```

find where `x < 3` and `y <= 4`

```coffeescript
{x: {$lt: 3}, y: {$lte: 4}}
# or
'x < ? AND y <= ?', 3, 4
```

find where `x > 3` and `y >= 4`

```coffeescript
{x: {$gt: 3}, y: {$gte: 4}}
# or
'x > ? AND y >= ?', 3, 4
```

find where not (`x > 3` and `y >= 4`)

```coffeescript
{$not: {x: {$gt: 3}, y: {$gte: 4}}}
# or
'NOT (x > ? AND y >= ?)', 3, 4
```

find where `x < NOW()`

```coffeescript
{x: {$lt: {$sql: 'NOW()'}}}
#or
'x < NOW()'
```

find where `x < 7` or `y < 7`

```coffeescript
{$or: [{x: {$lt: 7}}, {y: {$lt: 7}}]}
# or
'x < ? OR y < ?', 7, 7
```

find where not (`x < 7` or `y < 7`)

```coffeescript
{$nor: [{x: {$lt: 7}}, {y: {$lt: 7}}]}
# or
'NOT (x < ? OR y < ?)', 7, 7
```

find where `x < 7` and `x > 10`

```coffeescript
{$and: [{x: {$lt: 7}}, {x: {$gt: 10}}]}
# or
'x < ? AND x > ?', 7, 10
```

find where not (`x < 7` and `x > 10`)

```coffeescript
{$nand: [{x: {$lt: 7}}, {x: {$gt: 10}}]}
# or
'NOT (x < ? AND x > ?)', 7, 10
```

find where `x` is `null`

```coffeescript
{x: {$null: true}}
# or
'x IS NULL'
```

find where `x` is not `null`

```coffeescript
{x: {$null: false}}
# or
'x IS NOT NULL'
```

The query language is designed to be intuitive and consistent.
You should be able to infer all the other possible combinations.

### License: MIT
