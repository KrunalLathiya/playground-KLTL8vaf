# ReactJS Higher Order Components Tutorial

A higher-order component is a function that takes a component and returns a new component. A higher-order component (HOC) is the advanced technique in React.js for reusing a component logic. Higher-Order Components are not part of the React API. They are the pattern that emerges from React’s compositional nature. The component transforms props into UI, and a higher-order component converts a component into another component. The examples of HOCs are Redux’s connect and Relay’s createContainer.

# Syntax Overview of HOC

## Step 1: Create one React.js project.

```
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

## Step 2: Create one new file inside src folder called HOC.js.

```javascript
// HOC.js

import React, {Component} from 'react';

export default function Hoc(HocComponent){
    return class extends Component{
        render(){
            return (
                <div>
                    <HocComponent></HocComponent>
                </div>

            );
        }
    } 
}
```

Now, include this function into the App.js file.

```javascript
// App.js

import React, { Component } from 'react';
import Hoc from './HOC';

class App extends Component {
  
  render() {
    return (
      <div>
        Higher-Order Component Tutorial
      </div>
    )
  }
}
App = Hoc(App);
export default App;
```

## Explanation

First, we have made one function that is Hoc inside HOC.js file.

That function accepts one argument as a component. In our case that component is App.

```javascript
App = Hoc(App);
```

Now, that component wrapped inside another React component, so we can modify it. Thus, it is the primary application of the <b>Higher-Order Components</b>.

## Example

Let us say; we have one component, which just lists the stocks list and its price.
So, first, we are going to create one <b>StockList.js</b> component.

```javascript
// StockList.js

import React, { Component } from 'react';
import TableRow from './TableRow';

class StockList extends Component {
    constructor(props) {
        super(props);
        this.state = {
          stocks: [
            {
                  name: 'TCS',
                  price: 2500
            },
            {
                name: 'Infosys',
                price: 990
            },
            {
                name: 'HCL',
                price: 800
            }
          ]
        };
      }
      
      tabRow(){
        if(this.state.stocks instanceof Array){
          return this.state.stocks.map(function(object, i){
              return <TableRow obj={object} key={i} />;
          })
        }
      }
      render() {
        return (
            <div className="container">
            <table className="table table-striped">
              <thead>
                <tr>
                  <td>Stock Name</td>
                  <td>Stock Price</td>
                </tr>
              </thead>
              <tbody>
                {this.tabRow()}
              </tbody>
            </table>
        </div>
        );
      }
}
export default StockList;
```
Now, also for making the rows of the table, we are importing one another component called <b>TableRow.js.</b>

```javascript
// TableRow.js

import React, { Component } from 'react';

class TableRow extends Component {
  render() {
    return (
        <tr>
          <td>
            {this.props.obj.name}
          </td>
          <td>
            {this.props.obj.price}
          </td>
        </tr>
    );
  }
}

export default TableRow;
```

Now, we need to include this <b>StockList.js</b> component into the <b>App.js</b> file.

```javascript
// App.js

import React, { Component } from 'react';
import StockList from './StockList';

class App extends Component {
  
  render() {
    return (
      <div>
        <StockList></StockList>
      </div>
    )
  }
}

export default App;
```
Now, save the file and go to http://localhost:3000/. You will find one Stock Listing Table.

Okay, now this is the first scenario. The second situation is somewhat same as the first scenario. In this, we are listing user details. So, create one file called <b>UserList.js</b> inside <b>src</b> folder.

```javascript
// UserList.js

import React, { Component } from 'react';
import TableRow from './TableRow';

class UserList extends Component {
    constructor(props) {
        super(props);
        this.state = {
          users: [
            {
                id: 1,
                name: 'Krunal'
                  
            },
            {
                id: 2,
                name: 'Ankit'
            },
            {
                id: 3,
                name: 'Rushabh'
            }
          ]
        };
      }
      
      tabRow(){
        if(this.state.users instanceof Array){
          return this.state.users.map(function(object, i){
              return <TableRow obj={object} key={i} />;
          })
        }
      }
      render() {
        return (
            <div className="container">
            <table className="table table-striped">
              <thead>
                <tr>
                  <td>ID</td>
                  <td>Name</td>
                </tr>
              </thead>
              <tbody>
                {this.tabRow()}
              </tbody>
            </table>
        </div>
        );
      }
}
export default UserList;
```

## Explanation

In both the cases, we are doing the same thing. Just display the stocks and users properties id and name. So, here what we can do is that make one <b>Higher-Order Component</b> and pass both the components as an argument when needed.

Make one file called <b>HOC.js</b> inside <b>src</b> directory.

```javascript
// HOC.js

import React, {Component} from 'react';

export default function Hoc(HocComponent, data){
    return class extends Component{
        constructor(props) {
            super(props);
            this.state = {
                data: data
            };
        }
        
        render(){
            return (
                <HocComponent data={this.state.data} {...this.props} />
            );
        }
    } 
}
```

Above file exports one default function, which returns a modified Component. This function takes two arguments.

1. Component
2. Data

```javascript
// StockList.js

import React, { Component } from 'react';
import TableRow from './TableRow';

class StockList extends Component {
    constructor(props) {
        super(props);
      }
      
      tabRow(){
        if(this.props.data instanceof Array){
          return this.props.data.map(function(object, i){
              return <TableRow obj={object} key={i} />;
          })
        }
      }
      render() {
        return (
            <div className="container">
            <table className="table table-striped">
              <thead>
                <tr>
                  <td>ID</td>
                  <td>Name</td>
                </tr>
              </thead>
              <tbody>
                {this.tabRow()}
              </tbody>
            </table>
        </div>
        );
      }
}
export default StockList;
```

Also, <b>UserList.js</b> file looks like this.

```javascript
// UserList.js

import React, { Component } from 'react';
import TableRow from './TableRow';

class UserList extends Component {
    constructor(props) {
        super(props);
      }
      
      tabRow(){
        if(this.props.data instanceof Array){
          return this.props.data.map(function(object, i){
              return <TableRow obj={object} key={i} />;
          })
        }
      }
      render() {
        return (
            <div className="container">
            <table className="table table-striped">
              <thead>
                <tr>
                  <td>ID</td>
                  <td>Name</td>
                </tr>
              </thead>
              <tbody>
                {this.tabRow()}
              </tbody>
            </table>
        </div>
        );
      }
}
export default UserList;
```

Now, In <b>App.js</b> file, we are calling that <b>higher-order component function</b>.

```javascript
// App.js

import React, { Component } from 'react';
import StockList from './StockList';
import UserList from './UserList';
import Hoc from './HOC';

const StocksData = [
  {
      id: 1,
      name: 'TCS'
        
  },
  {
      id: 2,
      name: 'Infosys'
  },
  {
      id: 3,
      name: 'Reliance'
  }
];
const UsersData = [
  {
      id: 1,
      name: 'Krunal'
        
  },
  {
      id: 2,
      name: 'Ankit'
  },
  {
      id: 3,
      name: 'Rushabh'
  }
];

const Stocks = Hoc(
  StockList,
  StocksData
);

const Users = Hoc(
  UserList,
  UsersData
);


class App extends Component {
  
  render() {
    return (
      <div>
        <Users></Users>
      </div>
    )
  }
}

export default App;
```
In above, you can also write <Stocks></Stocks> in render() function. At that time, Stocks component will be rendered.

## Conclusion

The primary use of <b>Higher-Order Component</b> is to enhance the reusability of particular components in multiple modules or components. We can also comprise various components to get improved components. Most of the third party libraries are using this feature to write another cool library.

@[ReactJS Higher Order Components Tutorial]({"stubs": ["src/app/app.jsx", "src/main.js", "src/app/HOC.js", "src/app/StockList.js", "src/app/UserList.js"], "command": "./run.sh"})