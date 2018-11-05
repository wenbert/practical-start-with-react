# practical-start-with-react
A Practical Start with React from Pluralsight

## Course Overview 
- Done

## Should You React?
- Done

## Getting Ready
- Done

## Structuring the Application

### Modules

#### Export and import
module.js
```js
class component {
    doSomething() {
    }
}
export component;
```

anotherModule.js
```js
import {component} from "./modulex"
component.doSomething();
```

#### Export default
module.js
```js
class component {
    doSomething() {
    }
}
export default component;
```

anotherModule.js
```js
import comp from "./module";
comp.doSomething();
```
No need for `{}`.

#### Mixing Export default
module.js
```js
class component {
    doSomething() {
    }
}
const x = 10;
export default component, x;
```

anotherModule.js
```js
import comp, {x} from "./module";
comp.doSomething();
console.log(x);
```