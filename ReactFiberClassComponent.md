### [ReactFiberClassComponent](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js)

```
{
  isMounted: boolean,
  enqueueSetState(inst, payload, callback)
    get_some_values_to_do_further_update
    update = createUpdate(expirationTime);

    flushPassiveEffects()
    enqueueUpdate(fiber, update)
    scheduleWork(fiber, expirationTime)
  enqueueReplaceState(inst, payload, callback)
  enqueueForceUpdate(inst, callback)
}
```


####  [classComponentUpdater](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L187)

#### [adoptClassInstance](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L502)
link `fiber` with `instance`.
```
In 
  workInProgress: Fiber
  instance: any
Body
  instance.updater = classComponentUpdater
  workInProgress.stateNode = instance
```

#### [constructClassInstance](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L512)
Construct component instance by `ctor`.
```
In
  workInProgress: Fiber
  ctor: any
  props: any
Body
  context = readContext_from_ctor
    || getMaskedContext_from_workInProgress_and_ctor
    || emptyContextObject
  instance = new ctor(props, context)
  state = workInProgress.memoizedState = instance.state
  adoptClassInstance(workInProgress, instance)
Out
  instance
```

#### [applyDerivedStateFromProps](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L148)
shallow merge `state` with `derived state` of `props`  
update `fiber's memoizedState`
```
In
  workInProgress: Fiber
  ctor: any
  getDerivedStateFromProps: (props: any, state: any) => any
Body
  prevState = workInProgress.memoizedState
  partialState = getDerivedStateFromProps(nextProps, prevState)
  if_partialState_exist
    workInProgress.memoizedState = shallowMerge(prevState, partialState)
  updateQueue = workInProgress.updateQueue;
  if_updateQueue_exist
    updateQueue.baseState = memoizedState;
```

#### [mountClassInstance](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L725)
mount class `instance`  
set `instance` properties and call related hooks
```
In
  workInProgress: Fiber
  ctor: any
  newProps: any
  renderExpirationTime: ExpirationTime
Body
  instance = workInProgress.stateNode
  update_some_properties_of_instance

  if_workInProgress.updateQueue_exist
    processUpdateQueue(...)
    instance.state = workInProgress.memoizedState

  if_ctor.getDerivedStateFromProps_exist
    call_applyDerivedStateFromProps
    instance.state = workInProgress.memoizedState

  if_instance.componentWillMount_exist
    callComponentWillMount(...) // hook
    call_processUpdateQueue
    instance.state = workInProgress.memoizedState
  
  if_instance.componentDidMount_exist
    workInProgress.effectTag |= Update
```

#### [updateClassInstance](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberClassComponent.js#L980)
just like `mountClassInstance`
```
In
  current: Fiber
  workInProgress: Fiber
  ctor: any
  newProps: any
  renderExpirationTime: ExpirationTime
Body
  instance = workInProgress.stateNode
  callComponentWillReceiveProps(...) // hook

  oldProps = workInProgress.memoizedProps;
  newState = oldState = workInProgress.memoizedState;

  if_updateQueue_exist
    processUpdateQueue(...)
    newState = workInProgress.memoizedState;

  shouldUpdate = checkShouldComponentUpdate(...) // hook
  if_shouldUpdate
    instance.componentWillUpdate(...) // hook
    if_instance.componentDidUpdate_exist
      workInProgress.effectTag |= Update
  else
    workInProgress.memoizedProps = newProps;
    workInProgress.memoizedState = newState;
  
  instance.props = newProps;
  instance.state = newState;
  instance.context = nextContext;
Out
  shouldUpdate
```
