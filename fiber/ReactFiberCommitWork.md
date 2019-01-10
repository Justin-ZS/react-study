### [commitWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberCommitWork.js#L1033)
1. commit coming `effect` according to different tag.
2. handle `Update` effect.
```
In
  current: Fiber | null
  finishedWork: Fiber
Body
  switch(finishedWork.tag)
    FunctionComponent:
    ForwardRef:
    MemoComponent:
    SimpleMemoComponent:
      > commitHookEffectList(...)
    HostComponent:
      > commitUpdate(...)
    HostText:
      > commitTextUpdate(...)
    ClassComponent:
    HostRoot:
    Profiler:
    IncompleteClassComponent:
      >
```

### [commitLifeCycles](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberCommitWork.js#L351)
1. handle some hooks after commit complete.
```
In
  finishedRoot: FiberRoot
  current: Fiber | null
  finishedWork: Fiber
  committedExpirationTime: ExpirationTime
Body
  switch(finishedWork.tag)
    FunctionComponent:
    ForwardRef:
    SimpleMemoComponent:
      commitHookEffectList(...)
    ClassComponent:
      instance = finishedWork.stateNode
      if_current_exist
        instance.componentDidUpdate(...) // hook
      else
        instance.componentDidMount() // hook

      updateQueue = finishedWork.updateQueue
      if_updateQueue_exist
        commitUpdateQueue(...)  // ?
    HostComponent:
      commitMount(...)
    ...
```