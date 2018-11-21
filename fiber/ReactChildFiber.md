### placeSingleChild
1. `tag with `placement` for single child case.
```
In
  newFiber: Fiber
Body
  if_newFiber.alternate_is_null
    newFiber.effectTag = Placement;
Out
  newFiber

```

### [reconcileChildFibers](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactChildFiber.js#L1218)
> Reconciliation is an `algorithm` React uses to diff one tree with another to determine `which parts need to be changed`.

1. the `returnFiber` has been reconciled
2. create fiber(s) for `newChild`
3. collect `effects` according to the `newChild` fibers(s)
```
In
  returnFiber: Fiber
  currentFirstChild: Fiber | null
  newChild: any
  expirationTime: ExpirationTime
Body
  if_newChild_is_Object
    switch(newChild.$$typeof)
      REACT_ELEMENT_TYPE: > placeSingleChild(reconcileSingleElement(...))
      REACT_PORTAL_TYPE:  > placeSingleChild(reconcileSinglePortal(...))
  if_newChild_is_number_or_string
    > placeSingleChild(reconcileSingleTextNode(...))
  if_newChild_is_array
    > reconcileChildrenArray(...)
  if_newChild_is_iterable
    > reconcileChildrenIterator(...)
Out
  Fiber | null
```