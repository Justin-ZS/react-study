### [describeFiber](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactCurrentFiber.js#L29)
> `Fiber` is reimplementation of the `stack`, specialized for React components. You can think of a single fiber as a `virtual stack frame`.  

this function will collect stack info from `fiber`
```
In
  fiber: Fiber
Body
  describeComponentFrame(...)
Out
  fiber info(string)
```


### [getStackByFiberInDevAndProd](https://github.com/facebook/react/blob/v16.6.3/packages/react-reconciler/src/ReactCurrentFiber.js#L51)
this function will traverse the whole fiber stack.(from bottom to top)
```
In
  workInProgress: Fiber
Body
  node = workInProgress;
  while(node)
    info += describeFiber(node);
    node = node.return;
Out 
  info
```