# Module Documentation

## Module Database.AnyDB

#### `Query`

``` purescript
newtype Query a
  = Query String
```


#### `Connection`

``` purescript
data Connection :: *
```


#### `DB`

``` purescript
data DB :: !
```


#### `ConnectionString`

``` purescript
type ConnectionString = String
```


#### `ConnectionInfo`

``` purescript
type ConnectionInfo = { password :: String, user :: String, port :: Number, db :: String, host :: String }
```


#### `mkConnectionString`

``` purescript
mkConnectionString :: ConnectionInfo -> ConnectionString
```


#### `connect`

``` purescript
connect :: forall eff. ConnectionInfo -> Aff (db :: DB | eff) Connection
```

Makes a connection to the database.

#### `execute`

``` purescript
execute :: forall eff a. Query a -> [SqlValue] -> Connection -> Aff (db :: DB | eff) Unit
```

Runs a query and returns nothing.

#### `execute_`

``` purescript
execute_ :: forall eff a. Query a -> Connection -> Aff (db :: DB | eff) Unit
```

Runs a query and returns nothing

#### `query`

``` purescript
query :: forall eff a. (IsForeign a) => Query a -> [SqlValue] -> Connection -> Aff (db :: DB | eff) [F a]
```

Runs a query and returns all results.

#### `query_`

``` purescript
query_ :: forall eff a. (IsForeign a) => Query a -> Connection -> Aff (db :: DB | eff) [a]
```

Just like `query` but does not make any param replacement

#### `queryOne`

``` purescript
queryOne :: forall eff a. (IsForeign a) => Query a -> [SqlValue] -> Connection -> Aff (db :: DB | eff) (Maybe a)
```

Runs a query and returns the first row, if any

#### `queryOne_`

``` purescript
queryOne_ :: forall eff a. (IsForeign a) => Query a -> Connection -> Aff (db :: DB | eff) (Maybe a)
```

Just like `queryOne` but does not make any param replacement

#### `queryValue`

``` purescript
queryValue :: forall eff a. (IsForeign a) => Query a -> [SqlValue] -> Connection -> Aff (db :: DB | eff) (Maybe a)
```

Runs a query and returns a single value, if any.

#### `queryValue_`

``` purescript
queryValue_ :: forall eff a. (IsForeign a) => Query a -> Connection -> Aff (db :: DB | eff) (Maybe a)
```

Just like `queryValue` but does not make any param replacement

#### `withConnection`

``` purescript
withConnection :: forall eff a. ConnectionInfo -> (Connection -> Aff (db :: DB | eff) a) -> Aff (db :: DB | eff) a
```

Connects to the database, calls the provided function with the connection
and returns the results.

#### `close`

``` purescript
close :: forall eff. Connection -> Eff (db :: DB | eff) Unit
```



## Module Database.AnyDB.Pool


#### `Pool`

``` purescript
data Pool :: *
```


#### `createPool`

``` purescript
createPool :: forall eff. ConnectionInfo -> Eff (db :: DB | eff) Pool
```

Create a connection pool.

#### `createPoolFromString`

``` purescript
createPoolFromString :: forall eff. ConnectionString -> Eff (db :: DB | eff) Pool
```

Create a connection pool. Remember to call `closePool`.

#### `closePool`

``` purescript
closePool :: forall eff. Pool -> Eff (db :: DB | eff) Unit
```

Close the connection pool.

#### `withPool`

``` purescript
withPool :: forall eff a. Pool -> (Connection -> Aff (db :: DB | eff) a) -> Aff (db :: DB | eff) a
```

Run a database action with a connection from the specified `Pool`.


## Module Database.AnyDB.SqlValue

#### `SqlValue`

``` purescript
data SqlValue :: *
```


#### `IsSqlValue`

``` purescript
class IsSqlValue a where
  toSql :: a -> SqlValue
```


#### `isSqlValueString`

``` purescript
instance isSqlValueString :: IsSqlValue String
```


#### `isSqlValueNumber`

``` purescript
instance isSqlValueNumber :: IsSqlValue Number
```


#### `isSqlValueInt`

``` purescript
instance isSqlValueInt :: IsSqlValue Int
```


#### `isSqlValueMaybe`

``` purescript
instance isSqlValueMaybe :: (IsSqlValue a) => IsSqlValue (Maybe a)
```



## Module Database.AnyDB.Transaction

#### `Transaction`

``` purescript
data Transaction :: *
```


#### `withTransaction`

``` purescript
withTransaction :: forall eff a. (Connection -> Aff (db :: DB | eff) a) -> Connection -> Aff (db :: DB | eff) a
```



## Module Database.AnyDB.Util

#### `finally`

``` purescript
finally :: forall e a. Aff e a -> Aff e Unit -> Aff e a
```

Compute `aff1`, followed by `aff2` regardless of whether `aff1` terminated successfully.



