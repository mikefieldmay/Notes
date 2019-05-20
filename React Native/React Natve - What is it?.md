## What is React Native?

"Learn once, write everywhere"
The key thing with React Native is always using the same language, but with some platform specific adjustments. Learn React Native once and then adjust it for specific platforms. Building real mobile apps using javascript and React Native.

A React Native app is not a web app running in a mobile device. It's no a WebApp hosted by a web view. JS + ReactNative App compiled to Native code. 
Behind the scenes we have a React + React Native App. React Native gives us some other componentns (not HTML) that can be compiled to Native code. At it's core it gives us a DOM that replaces the web dom for building Native applications.

Rendering the content:

React for the Web: <div> | <input>
Native Component (Android): android.viewNative | EditText
Native Component (iOS): UIView | UITextField
React Native: <View> | <TextInput> (Under the hood it knows the translations for Android and iOs)

We're not using JSX elements which are Dom elements, we will use Native components instead.

## The Javascript Side:

UI is compiled to components exposed by React Native. The Logic is written by us in javascript. It stays as Javascript and Native creates an environment (Thread) where it can run. It gives us a bridge from javascript to our native code.

### React Native App Limitations

React Native apps are hardwork. We write javascript and it runs everywhere?

"Learn once, Write Everywhere". We have very little Cross-Platfor Styling of Components. We will have to do all the styling manually. We only get a basic set of Pre-Built components. Tools to create Responsiveness, but no Responsiveness out of the box.

It does create a real Native app though which far outways the negatives.

React Native is a "Fast Moving Target":
- New Versions every month. 
- Breaking changes do happen
- High dependency on Third-Party Packages that also change
- Bugs

Still an amazing library. There are tools to get over all these challenges.