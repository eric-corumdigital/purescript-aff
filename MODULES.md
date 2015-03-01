# Module Documentation

## Module Control.Monad.Aff

#### `Async`

``` purescript
data Async :: # ! -> !
```

An effect constructor which accepts a row of effects. `Async e` 
represents the effect of asynchronous computations which perform effects 
`e` at some indeterminate point in the future.

#### `EffA`

``` purescript
type EffA e1 e2 a = Eff (async :: Async e1 | e2) a
```

The `Eff` type for a computation which has asynchronous effects `e1` and
synchronous effects `e2`.

#### `Aff`

``` purescript
data Aff e1 e2 a
```

A computation with asynchronous effects `e1` and synchronous effects 
`e2`. The computation either errors or produces a value of type `a`.

This is moral equivalent of `ErrorT (ContT Unit (EffA e1 e2)) a`.

The type implements `MonadEff` so it's easy to lift synchronous `Eff` 
computations into this type. As a result, there's basically no reason to
use `Eff` in a program that has some asynchronous computations.

#### `PureAff`

``` purescript
type PureAff a = forall e1 e2. Aff e1 e2 a
```


#### `launchAff`

``` purescript
launchAff :: forall e1 e2 a. Aff e1 e2 a -> EffA e1 e2 Unit
```

Converts the asynchronous effect into a synchronous one. All values and
errors are ignored.

#### `runAff`

``` purescript
runAff :: forall e1 e2 a. (Error -> Eff e1 Unit) -> (a -> Eff e1 Unit) -> Aff e1 e2 a -> EffA e1 e2 Unit
```

Runs the asynchronous effect. You must supply an error callback and a 
success callback.

#### `makeAff`

``` purescript
makeAff :: forall e1 e2 a. ((Error -> Eff e1 Unit) -> (a -> Eff e1 Unit) -> EffA e1 e2 Unit) -> Aff e1 e2 a
```

Creates an asynchronous effect from a function that accepts error and 
success callbacks.

#### `attempt`

``` purescript
attempt :: forall e1 e2 a. Aff e1 e2 a -> Aff e1 e2 (Either Error a)
```

Promotes any error to the value level of the asynchronous monad.

#### `liftEff'`

``` purescript
liftEff' :: forall e1 e2 a. Eff (err :: Exception | e2) a -> Aff e1 e2 (Either Error a)
```

Lifts a synchronous computation and makes explicit any failure from exceptions.

#### `semigroupAff`

``` purescript
instance semigroupAff :: (Semigroup a) => Semigroup (Aff e1 e2 a)
```


#### `monoidAff`

``` purescript
instance monoidAff :: (Monoid a) => Monoid (Aff e1 e2 a)
```


#### `functorAff`

``` purescript
instance functorAff :: Functor (Aff e1 e2)
```


#### `applyAff`

``` purescript
instance applyAff :: Apply (Aff e1 e2)
```


#### `applicativeAff`

``` purescript
instance applicativeAff :: Applicative (Aff e1 e2)
```


#### `bindAff`

``` purescript
instance bindAff :: Bind (Aff e1 e2)
```


#### `monadAff`

``` purescript
instance monadAff :: Monad (Aff e1 e2)
```


#### `monadEffAff`

``` purescript
instance monadEffAff :: MonadEff e2 (Aff e1 e2)
```


#### `monadErrorAff`

``` purescript
instance monadErrorAff :: MonadError Error (Aff e1 e2)
```

Allows users to catch and throw errors on the error channel of the 
asynchronous computation. See documentation in `purescript-transformers`.