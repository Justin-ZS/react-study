### [ReactFiberBeginWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberBeginWork.js)


#### [updateClassComponent](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberBeginWork.js#L428)
1. go different branch: `mount`, `resume`, `update`
2. call some hooks, such as: `componentWillReceiveProps`, `shouldComponentUpdate`, ...
3. return `child` fiber of `workInProgress`
```
In 
  current: Fiber | null
  workInProgress: Fiber
  Component: any
  nextProps
  renderExpirationTime: ExpirationTime
Body
  instance = workInProgress.stateNode;
  if_fiber_is_unmount
    constructClassInstance(...)
    mountClassInstance(...)
    shouldUpdate = true
  if_in_a_resume
    shouldUpdate = resumeMountClassInstance(...)
  else // update
    shouldUpdate = updateClassInstance(...) // componentWillReceiveProps | shouldComponentUpdate
Out
  finishClassComponent(...)
```

#### [finishClassComponent](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberBeginWork.js#L501)
1. `update refs` even if shouldComponentUpdate returns `false`
2. call `render` and get new children when shouldComponentUpdate returns `true`
3. `reconcile` returned next children
4. return `child` fiber of `workInProgress`
```
In
  current: Fiber | null
  workInProgress: Fiber
  Component: any
  shouldUpdate: boolean  // return from shouldComponentUpdate
  hasContext: boolean
  renderExpirationTime: ExpirationTime
Body
  if_ref_exist
    workInProgress.effectTag |= Ref; // Schedule a Ref effect

  if_shouldUpdate_is_false_or_capture_an_error
    > bailoutOnAlreadyFinishedWork(...) // return

  instance = workInProgress.stateNode;
  if_no_error
    nextChildren = instance.render() // hook

  workInProgress.effectTag |= PerformedWork // React DevTools reads this flag
  reconcileChildren(...nextChildren...)
  workInProgress.memoizedState = instance.state;
Out
  workInProgress.child
```

#### [beginWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberBeginWork.js#L1510)
```
In
  current: Fiber | null
  workInProgress: Fiber
  renderExpirationTime: ExpirationTime
Body
```