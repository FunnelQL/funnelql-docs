# Funnel Query Language

Funnel Query Language is a language designed to perform queries on the chronological context of activity of a website visitor for the purpose of advanced **Conversion Funnel Optimization**.

A [Javascript API](javascript-api.md) enables to write a funnel data-set and provides several methods to query the data-set using Funnel Query Language.

Funnel Query Language converts to JSON before it is used to query the funnel data-set. The language is designed for easy usage by non-technical users. It is possible to use JSON instead of Funnel Query Language to save additional parsing time in a production environment.

Funnel Query Language provides the following methods for querying the registered funnel data-set.

* Tag (JSON string)
* URL (`URL`)
* Path (`PATH`)
* Tag Count (`COUNT`)
* Date or time (`SINCE`)
* Custom function (`FN`)

#### Tag Query

A tag can be queried using a JSON string `"tag-name"` or using a Regular Expression method `REGEX(/tag-name/i)`.

```mysql
"tag-x" + REGEX(/tag-name-(\d+)?/i) + "tag-y"
```

#### URL Query

An URL can be queried using the method `URL("url")` or using a Regular Expression `URL( REGEX(/tag-name/i) )`.

```mysql
URL("url/to/match") + URL( REGEX(/url\.(html|js|css)/i) )
```

#### Path Query

The `PATH` query enables to add chronological context to a query. The path method starts a sub query that can contain a `tag`, `url`, `count` or `function` query.

The path query provides operators to define the chronological context. The path sequence can be based on URL navigation position or time.

##### Path Query Operators


| Option                         | Description     |
|--------------------------------|-----------------|
| `>`               | Matches in consecutive query should follow previous query's URL position. |
| `>=`               | Matches in consecutive query should match or follow previous query's URL position. |
| `\|>`               | Matches in consecutive query should be on same URL position as previous query's URL position. |
| `>10`                | Matches in consecutive query should be at least 10 URL positions from previous query's URL position. |
| `>10m`                | Matches in consecutive query should be at least 10 minutes from previous query's event position. |
| `<10m`                | Matches in consecutive query should be within 10 minutes from previous query's event position. |


```mysql
PATH( "tag-x" > "tag-y" )
```

The following example matches when `tag-y` follows within 1 minute after `tag-x`.

```mysql
PATH( "tag-x" <1m "tag-y" )
```

The following example matches when the visitor visits an URL containing `cart` or `other-page` within 5 navigations after having visited `product-1.html`.

```mysql
PATH( URL("product-1.html") <5 URL( REGEX(/(cart|other-page)/) ) )
```

#### Since Query

The `SINCE` query enables to add chronological context to a query. The since method starts a sub query that can contain a `tag`, `url`, `count` or `function` query.

The first parameter of the `SINCE` method can contain a time reference, e.g. `1s` (1 second) or `10m` (10 minutes) or a date in format `yyyy-MM-ddThh:mm`.

```mysql
SINCE( 1m, "tag-x" )
```

```mysql
SINCE( 2018-09-02T12:10, ( "tag-x" + "tag-y" ) )
```

#### Count Query

A `COUNT` query enables to query based on number of occurrences of a tag.

The first parameter of the `COUNT` method contains the tag as JSON string or as `REGEX` query. The query supports the operators `>`, `<`, `=`, `<=`, `>=` followed by a number.

```mysql
COUNT( "tag-x" > 1 )
```

#### Function Query

A `FN` query enables to perform a custom query using a javascript function. Optional conditions from a parent query are added as a extra argument.

```mysql
FN( "function-name", "argument-1", "argument-2" )
```