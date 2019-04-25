## Modern CSS

### Getting the up to date info

CSS is  split into modules. Different working groups work on different modules and you can view whether the most up to date version is a reccomendation or a draft. You probably wouldn't go incredibly deep unless you were a browser developer. 

### CSS Variables

Variables allow us to define a variable on the `:root` selector. we can then access these cariables using var(--selector-name, backup-color). You use variables most commonly for colors or for units. Variables are great, because if we define them once then update them, they are updated in every location. 

### Vendor prefixes

Different browsers implement new features at different speeds. We can use vendor prefixes to allow certain features to be used by different browsers e.g
```
.container {
  display: -webkit-box; // works in older safari
  display: -ms-flexbox; // works in internet explorer and edge
  display: -webkit-flex; // works in newer safari
  display: flex;
}
```
these values are not needed to be understood by other browsers. They don't use flex because if something becomes standardised, this could cause issues across early adopter implementations. It allows you to safely implement a feature that works in older browsers. They are really great as they allow us to implement newer features ahead of time, or to provide support to older browsers. It is recommended to use all versions of display flex when using flexbox.

### Which prefixes should you use

There is a tool you can us called auto-prefixer. It's a tool that automatically scans your code and then adds the prefixes you need. You can add this to the webpack build process.

### Detecting Browser with @supports

Some features simply aren't featured in some browsers. You can use css @supports to check if it is supported or not. When you add a support query, the code in the bracket will only run if the feature is supported.
```
@supports (display: grid) {
  .container {
    display: grid
  }
}
```
You can define your fallback first, and then use your next-gen feature if it's available afterwards.

### Polyfills

A polyfill is a javascript package which enables certain CSS features in Browsers which would not support it otherwise. Sometimes there is no prefix we can use and we don't want to implement a fallback. For some features polyfills are an option. Users have to download the polyfill which will impact the performance of your page. 

### Eliminating cross browser inconsistencies

Another problem we may face are cross browser inconsistencies. Different browsers implement different defaults (e.g box-sizing, fonts etc). Some defaults are annoying and we want to override them. You can use things like Reset-Styling in order to set the defaults across all browsers. You can also add this in yourself. You may not want to add a library as it will inflate your code bundle. Normalize css may reset a lot of styles that we may never even use. 

### Choosing class names correctly

Do
- use `kebab-case` because CSS is case-insensitive
- Name by feature `.page-title` 

Don't
- use `camelCase` because CSS is case-insensitive
- name by style `title-blue`

BEM block element modifier is a convention that causes us to name our styles in a uniform and consistent way.
`.BLOCK__element--modifier` e.g `.menu-main__item--size-big` or `.button--success`

### Vanilla CSS vs CSS frameworks

Vanilla CSS is CSS code we write with the base features. We write all the styles and layouts on our own. Foundation and Bootstrap 4 are component framworks. We can choose from a rich suite of pre-styled components and utility features/classes. Utility frameworks like tailwind CSS let you build your ownstyles and layoutswith the help of utility features and classes. 
Vanilla CSS:
- Full control
- No unnecessary code
- Name classes as you like
- Build everything from scratch
- Danger of 'bad code'
Component Frameworks
- Rapid Development
- Follow best practices
- No need to be an expert
- No or little control
- Unnecessary Overhead code
- All websites look the same
Utility Framework
- Faster development
- Follow best practices
- No expert knowledge needed
- Little Control
- Unecessary Overhead code