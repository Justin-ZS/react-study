#### completeUnitOfWork(https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberScheduler.js#L939)
1. merge all `effects` in `workInProgress` into higher `returnFiber`
2. yield out `siblingFiber`, deliver it to `beginWork`
3. call self with `returnFiber`. (like `recursion`, but implement with `loop`)
```
In
  workInProgress: Fiber
Body
  while (true)
    current = workInProgress.alternate;
    returnFiber = workInProgress.return;
    siblingFiber = workInProgress.sibling;
      if_this_fiber_completed
        nextUnitOfWork = completeWork(...)
        resetChildExpirationTime(...)
        if_nextUnitOfWork_exist
          > nextUnitOfWork
        merge_effects_into_workInProgress
        if_siblingFiber_exist
          > siblingFiber               // more work in returnFiber, deliver to beginWork
        if_returnFiber_exist
          workInProgress = returnFiber // complete the returnFiber
          continue                     // recursion
        > null                    // We've reached the root.
```

#### [performUnitOfWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberScheduler.js#L1125)
```
In
  workInProgress: Fiber
Body
  current = workInProgress.alternate;

  next = beginWork(...)
  workInProgress.memoizedProps = workInProgress.pendingProps;

  if_next_is_null // we've reached the leaf.
    next = completeUnitOfWork(workInProgress)
Out
  next: Fiber
```

#### [workLoop](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberScheduler.js#L1125)
1. like `Event Loop` in JS
```
In
  isYieldy
Body
  while (nextUnitOfWork !== null)
    nextUnitOfWork = performUnitOfWork(nextUnitOfWork)
```