### [ReactFiberCompleteWork](https://github.com/facebook/react/blob/master/packages/react-reconciler/src/ReactFiberCompleteWork.js)

#### [completeWork](https://github.com/facebook/react/blob/master/packages/react-reconciler/src/ReactFiberCompleteWork.js#L539)
```
In
  current: Fiber | null
  workInProgress: Fiber
  renderExpirationTime: ExpirationTime  
Body
  newProps = workInProgress.pendingProps;
  switch(workInProgress.tag)
    IndeterminateComponent: 
```