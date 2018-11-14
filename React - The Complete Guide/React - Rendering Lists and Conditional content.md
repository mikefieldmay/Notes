# Rendering lists and Conditional Content

## Conditional rendering

With React, in order to render content we do the following:
```
{ this.state.showPersons ? 
    <div>
    <Person 
        name={this.state.persons[0].name} 
        age={this.state.persons[0].age}
        click={this.switchNameHandler.bind(this, "Max!")} 
        changed={this.nameChangedHandler}/>
    <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Eating</Person>
    <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
    </div> : null
}
```
We wrap the that conditional content in a div, and then we essentially write a ternary if fi statement that returns either the content or null. 
React only renders the elements when the condition is true.
You can also render the content in a slightly different way. As the application grows and conditions become nested it can become confusing. Everything inside the render function gets executed when react re-renders the component. 
```
...
const person = null;
 if(this.state.showPersons) {
      persons = (
        <div>
          <Person 
            name={this.state.persons[0].name} 
            age={this.state.persons[0].age}
            click={this.switchNameHandler.bind(this, "Max!")} 
            changed={this.nameChangedHandler}/>
          <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Eating</Person>
          <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
        </div>
      )
    } // we can use an if statement before the return to declare some jsx. 

    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <button style={style} onClick={this.togglePersonsHandler} >Switch Name</button> 
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        {persons} // we then interpolate the value of persons into the return
      </div>
    );
  }
...
```

We can use this to keep the template clean and it's the preferred way of outputting content in React

## Outputting lists

We normally create the jsx for a list by mapping an array into jsx elements as below:
```
{this.state.persons.map((person) => {
    return <Person 
            name={person.name} 
            age={person.age}
            click={this.switchNameHandler.bind(this, person.name)} 
            changed={this.nameChangedHandler}/>
    })}
```
As it's all javascript anyway, this is the common method and works perfectly fine.

## Lists and state
We can add or delete items from lists in state using the below method:
```
deletePersonHandler = (personIndex) => {
    const persons = this.state.persons;
    persons.splice(personIndex, 1);
    this.setState({persons})
  }
```
This isn't actually that great though and can cause issues.
Because objects and arrays are reference types, when we get persons, we are actually referencing the original state. We then go on to mutate the state. A good practice is to create a copy of your array. When you change the array you are changing the copy, not the original.
```
deletePersonHandler = (personIndex) => {
    const persons = this.state.persons.slice();
    persons.splice(personIndex, 1);
    this.setState({persons})
  }
```
`slice()` without arguments returns a copy of an array.
You can also do: 
```
deletePersonHandler = (personIndex) => {
    const persons = [...this.state.persons];
    persons.splice(personIndex, 1);
    this.setState({persons})
  }
```
You should always update state in an immutable fashion. 

## Lists and keys

The key prop is an important property we should add when rendering lists of data. The key property helps you update the data efficiently. React has the virtual DOM where it compares what it would render with the previously rendered DOM. For lists it needs to find out which elements have changed. By default it'll just re-render the whole lists and for long lists it is very inefficient. That's why React has a key element, so it only needs to render the elements that changed not the whole list. Keys should be unique.
```
state = {
    persons: [
      { name: "Max", age: 28 },
      { name: "Mike", age: 28 },
      { name: "Jeff", age: 29 }
    ],
    showPersons: false
  } 

if(this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return <Person 
                    name={person.name} 
                    age={person.age}
                    click={() => {this.deletePersonHandler(index)}}
                    change={this.nameChangeHandler}
                    key={person.id}
                    />
          })}
        </div>
      )
    }
```

## Updating list elements:

```
nameChangedHandler = (event, id) => {

    const personIndex = this.state.persons.findIndex((person) => { 
      return person.id === id;
    }) // we find the index of the person

    const person = {...this.state.persons[personIndex]};

    person.name = event.target.value;

    const persons = [...this.state.persons];
    persons[personIndex] = person;

    this.setState({persons});
  }
``` 
