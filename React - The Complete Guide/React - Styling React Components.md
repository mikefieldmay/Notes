## Styling React Components

We can style React components in the render function. This means that our styles are not global. Whikst this approach is good, unfortunately it doesn't let us apply pseudo-selectors to the component. 
```
render() {
    const style = {
      backgroundColor:' white',
      font: 'inherit',
      border: '1px solid blue',
      padding: '8px',
      cursor: 'pointer'
    }
```

Alternatively we can write our styles in a css file, but this means our settings become global.

## Setting styles dynamically

Because everything is javascript, we can adjust styles that are in the render function. We can easily add changes in our existing if statement:
```
  if(this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return <Person 
                    name={person.name} 
                    age={person.age}
                    click={() => {this.deletePersonHandler(index)}}
                    changed={(event) => this.nameChangedHandler(event, person.id)}
                    key={person.id}
                    />
          })}
        </div>
      )
      style.backgroundColor = 'red'; // style is an object with properties.
    }
```
We can also dynamically add styles froma  css file:
```
const classes = [];
    if(this.state.persons.length <= 2) {
      classes.push('red');
    }
    if(this.state.persons.length <= 1) {
      classes.push('bold');
    }
...
<p className={classes.join(' ')}>
    To get started, edit <code>src/App.js</code> and save to reload.
</p>

```

## Adding and using Radium

Radium is a popular package for React that allows us to use inline-styles with pseudo selectors and media queries.
In order to use Radium, we simply wrap our application in a radium component. Radium is a higher level component.
```
import Radium from 'radium';
...
export default Radium(App);
```
### Pseudo-selectors
We can now start using extra features that Radium gives us. We can use it with both class based and function based components. 
```
style = {
    ...
    ':hover': {
            backgroundColor: 'lightgreen',
            color: 'black'
        }
}

if condition {
    style[':hover'] = {
        backgroundColor: 'salmon',
        color: 'black'
      }
}
```
### Media-queries
We can add media queris as part of the style object and Raduim parses these in order to make them work.

```
const person = (props) => { // a function tends to be lowercase whilst a class is uppercase
    const style = {
        '@media (min-width: 500px)': {
            width: '450px'
        }
    };

    return (
        <div className="Person" style={style}>
            <p onClick={props.click}>I'm {props.name} and I am {props.age} years old </p>
            <p>{props.children}</p>
            <input type="text" onChange={props.changed} value={props.name}/>
        </div>
    )
}
```

We also need to wrap our entire app in a `<StyleRoot>` tag that we import from Radium. This allows us to use it's advanced features.

## Enabling and using CSS Modules

We can use a feature called CSS modules to scope our CSS to each component. In order to do so we can run `npm run eject` in order to get access to get access to the configuration. This gives us access tot he scripts folder and the config folder. We now have access to the Webpack config file in `config/webpack.config.dev` and `config/webpack.config.prod`. 
```
// webpack-config.dev
... 
test: /\.css$/,
            use: [
              require.resolve('style-loader'),
              {
                loader: require.resolve('css-loader'), // css loader parses and converts css
                options: {
                  importLoaders: 1,
                  modules: true, // allows us to use modules
                  localIdentName: '[name]__[local]__[hash:base64:5]' // gives classes unique names [name] = css class name, [local] scopes it to a component, [hash:base64:5] = unique hash so you don't accidentally overwrite clases
                },
              },
...
```

Our styles in our css files are now scoped to our components. We now need to change the way we import the css:
```
import Classes from'./App.css';
```
It essentially transforms our css into a javascript object that we can access in our code. The css loader transforms our class name into a unique one that is specific to each component. We can add pseudo-selectors in the usual CSS way. Media queries are also imported normally.
