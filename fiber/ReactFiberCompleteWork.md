### [ReactFiberCompleteWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberCompleteWork.js)

#### [completeWork](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactFiberCompleteWork.js#L540)
```
In
  current: Fiber | null
  workInProgress: Fiber
  renderExpirationTime: ExpirationTime  
Body
  newProps = workInProgress.pendingProps;
  switch(workInProgress.tag)
    ClassComponent: 
```
