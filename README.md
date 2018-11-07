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
Normally, this would come from an API. duh. But for this one, we have `public/houses.json` file that looks like this:
```json
[
    {
        "id": 1,
        "address": "12 Foo Bar, Geneva",
        "country": "Switzerland",
        "description": "A Victorian property... blah",
        "price": 9000000,
        "photo": "277667"
    },
    {
        "id": 2,
        "address": "12 Minecraft Road, Bern",
        "country": "The Netherlands",
        "description": "A couple of chunks worth!",
        "price": 5000000,
        "photo": "462358"
    }
]
```

But our `main-page/index.js` file will now look like this with the "API" part done.

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './main-page.css';
import Header from './header';

class App extends Component {
  fetchHouse = () => {
    fetch('/houses.json')
    .then(rsp => rsp.json())
    .then(allHouses => {
      this.allHouses = allHouses;
      this.determineFeaturedHouse();
    });
  }

  determineFeaturedHouse = () => {
    if (this.allHouses) {
      const randomIndex = Math.floor(Math.random() * this.allHouses.length);
      const featuredHouse = this.allHouses[randomIndex];
      this.setState({ featuredHouse });
    }
  }

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

### State
* "State" is private data for the component.
* `setState` triggers re-render of the component
* Smart, async, optimised
* State is used in `render()`. Everything you put in the state should be used in the `render()` method.
  * If you have data that doesn't need rendering, just store it in a private property. Just like `allHouses` / `this.allHouses`.
* Read by accessing the state property
* Written to by using `setState`. Always use this! Since states trigger re-render of the component.
* State updates are merged.

One way to initialise state is to use a constructor. Take this example for `main-page/index.js`
```js
// ...
class App extends Component {

  constructor(props) {
    super(props);
    this.state = {}; // You can init some specific stuff here
  }

  fetchHouse = () => {
      // ...
  }
// ...
```

Other way to do is to use property initialisers. This is the shorter and cleaner way to do it.
```js
// ...
class App extends Component {

  state = {};

  fetchHouse = () => {
      // ...
  }
// ...
```

### Lifecycle Methods
 
 We call the `fetchHouses()` in the `componentDidMount()` method.

`main-page/index.js` would be something like:
 ```js
 // ...
 class App extends Component {

  state = {};

  componentDidMount() {
    this.fetchHouse();
  }
// ...
 ```

 FYI, `render()` is a Lifecycle Method. It is required!

#### Mounting
```
constructor()
render()
componentDidMount()
```

#### Updating
 ```
 getDerivedStateFromProps()
 shouldComponentUpdate()
 render()
 componentDidUpdate()
 ```

 #### Unmounting
 ```
 componentWillUnmount()
 ```

 #### Error Boundary
 ```
 componentDidCatch() 
 ```

 ### Error Boundaries
Each component can load another component. If something happens to the child component; with "Error Boundaries" we can handle errors more gracefully.

Take this for example:
```js
componentDidCatch(error, info) {
    this.setState({ hasError: true });
    log(error, info);
}

render() {
    if (this.state.hasError) {
        return <h1>Whoops! Sorry!</h1>
    }
    return <div></div>; //normal UI
}
```

### Nesting Components
Create a new file inside `main-page` directory. 
File: `main-page/featured-house.js`

```js
import React from 'react'; // If you have "React Snippets" installed, type: "imr" to insert this line automatically

// Then type: "sfc" to insert this "Stateless"
const FeaturedHouse = (props) => {
    if (props.house) return (
        <div>
            <div className="row featuredHouse">
                <h3 className="col-md-12 text-center">
                    Featured House
                </h3>
            </div>
            <House house={props.house} />
        </div>
    )
    return (<div>No featured house at this time</div>);
}
export default FeaturedHouse;
```

Note that we do not have a `House` component yet (`<House house={props.house} />`)

Let's create a new directory inside the `root` directory called `house` (`house`).
Inside it, let's put `house.css`
```css
.price {
    font-size: larger;
    color: red;
}
```

Then add `house/index.js`:
```js
import React, { Component } from 'react'; // Type: "imrc"  for some magic...
import './house.css';

// Type: "cc"
class house extends Component {
    state = {};
    render() { 
        const house = this.props.house;
        return (
        <div>
            <div className="col-md-12">
                Country: {house.country}<br/>
                Address: {house.address}<br/>
                Price: <span className="price">$ {house.price}</span><br/>
                Description: {house.description}<br/>
                <img src={`https://images.pexels.com/photos/${house.photo}/pexels-photo-${house.photo}.jpeg?w=600&h=400`} alt="Photo of house" /><br/>
            </div>
        </div>
        );
    }
}
 
export default house;
```

Then in `main-page/featured-house.js` just add:
```js
import House from '../house';
```
That should bring in `house` in to the component.

And in the `main-page/index.js` we bring in the `featuredHouse` by importing `FeaturedHouse`.

Make sure to update the `render()` method to use the new `FeaturedHouse` component. Note that we pass in the `house` prop to it.
```js
import FeaturedHouse from './featured-house';
// ...
  render() {
    return (
      <div className="container">
        <Header subtitle="Providing houses all over the world. From a prop."/>
        <FeaturedHouse house={this.state.featuredHouse}/>
      </div>
    );
  }
```


### Binding Component Props
Let's create a country filter. 
Tutorial goes around by filtering all countries in the JSON file. But I decided to just write them explicitly.

Add this in `main-page/index.js`
```js
  determineUniqueCountries = () => {
    // const countries = this.allHouses
    //   ? Array.from(new Set(this.allHouses.map(h => h.country)))
    //   : [];
    // countries.unshift(null);
    const countries = [
      '',
      'Switzerland',
      'The Netherlands'
    ];
    this.setState({ countries });
  }
```

The inside `fetchHouse()`, call it:
```js
// ...
this.determineFeaturedHouse();
this.determineUniqueCountries();
// ...
```

Next is to create a HouseFilter.

Create file `main-page/house-filter.js` with the following contents:
```js
import React, { Component } from 'react';

class HouseFilter extends Component {
    state = {};

    onSearchChange = (e) => {
        const country = e.target.value;
    }

    render() { 
        const search = this.state.search;
        const countries = this.props.countries || [];

        return (
            <div>
                Look for you dream house in country: 
                <select value={search} onChange={this.onSearchChange}>
                    {countries.map((c) => <option key={c} value={c}>{c}</option>)}
                </select>
            </div>
        );
    }
}
 
export default HouseFilter;
```

Then in `main-page/index.js` use it:
```js
import HouseFilter from './house-filter';
// ...

  render() {
    return (
      <div className="container">
        <Header subtitle="Providing houses all over the world. From a prop."/>
        // Use here!!!
        <HouseFilter countries={this.state.countries}/>
        <FeaturedHouse house={this.state.featuredHouse}/>
      </div>
    );
  }
// ...
```

#### Passing Functions to Child Components

##### Functions as Props

Take for example we have this:
```js
filter = (country) => {
    // ...
}

render() {
    <HouseFilter filterHouses={this.filter} />
}
```

In the root component, we receive the function:
```js
onSearchChange = (arg) => {
    this.props.filterHouses(country);
}
```

The `arg` above is basically a function.

The `filterHouses()`  method looks like:
```js
filterHouses = (country) => {
    const filteredHouses = this.allHouses.filter((h) => h.country === country);
    this.setState({ filteredHouses });
    this.setState({ country });
}
```

Then in the `render()` method, I just add it to the `HouseFilter` as a second parameter. Like this:
```js
render() {
return (
    <div className="container">
    <Header subtitle="Providing houses all over the world. From a prop."/>
    <HouseFilter countries={this.state.countries} filterHouses={this.filterHouses}/>
    <FeaturedHouse house={this.state.featuredHouse}/>
    </div>
);
}
```

Then update `main-page/house-filter.js`:
```js
onSearchChange = (e) => {
    const country = e.target.value;
    this.props.filterHouses(country);
}
```

#### Varying Render Output with Component Variables
Read the title again. That says it all.

It is basically hiding / showing components in the `render()` depending on variable states.

#### Form Controls
This section shows you how to do form controls. It deals with value and event binding.
Unlike VueJS where you use `v-bind`, in React you have to this by "hand". See for more info: https://reactjs.org/docs/forms.html

For example see `value={this.state.value} onChange={this.handleChange}` below:
```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

## Augmenting Features and Tooling
To be continued...
