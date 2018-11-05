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

### Boostrap 4
`npm install boostrap@4`

Then in `index.js` we have something like:
```js
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
```

## Understanding Components
* Component basics
* Kinds of components
* Props
* State
* Lifecycle methods
* Building components

### Creating a simple component

header.js
```js
import React from 'react';
import logo from './GloboLogo.png';

const Header = () => (
    <header className="row">
        <div className="col-md-5">
            <img src={logo} className="logo" alt="logo"/>
        </div>
        <div className="col-md-7 mt-5 subtitle">
            Providing houses world wide
        </div>
    </header>
);

export default Header;

```

index.js
```js
import React, { Component } from 'react';
import './main-page.css';
import Header from './header';

class App extends Component {
  render() {
    return (
      <div className="container">
        <Header/>
      </div>
    );
  }
}

export default App;
```

### Class and Function Components
"Function" components: Just render things. Small.
This would be:
```js
const Header = () => (
    // ...
);
```

"Class" components used for everything else...

### Props
All components can accept props. 
Props are arguments that are passed from the outside into the component.

```js
import React from 'react';
import logo from './GloboLogo.png';

const Header = (props) => (
    <header className="row">
        <div className="col-md-5">
            <img src={logo} className="logo" alt="logo"/>
        </div>
        <div className="col-md-7 mt-5 subtitle">
            {props.subtitle}
        </div>
    </header>
);

export default Header;
```

Now using it in index.js would be something like:
```js
import React, { Component } from 'react';
import './main-page.css';
import Header from './header';

class App extends Component {
  render() {
    return (
      <div className="container">
        <Header subtitle="Providing houses all over the world. From a prop."/>
      </div>
    );
  }
}

export default App;
```

### Fetching Data
Normally, this would come from an API. duh.
TO BE CONTINUED...