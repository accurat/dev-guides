# Getting Started @ Accurat /||||/|

This collection of resources is meant as a starter, to get familiar with concepts and terminology.

## Junior Developers

**JavaScript**

Theory:
  - This list of concepts is a fundamental basis for any JS development (the last chapter, "Closures", is a bit more complex and can be clarified later): https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript
  - Everything about Array and its prototype methods, very important:
    - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array
    - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#instance_methods
  
**EcmaScript 6, 7, 8 (= "modern" features of JavaScript)**

Understand each one of these concepts, their meaning and purpose:
  - Variable Scoping
    - Function-scoped variables (old style) `var`
    - Block-scoped variables `let`
    - Block-scoped constants `const`
  - Arrow Functions `const f = (x, y) => x * y`
  - Extended Parameter Handling: 
    - Default `const f = (x = 0, y = 1) => x * y`
    - Rest `const f = (...w) => w[0] * w[1]`
    - Spread `Math.max(...[1, 2, 3])`
  - Template literals `` `Welcome, ${name}!` ``
  - Destructuring Assignment: 
    - Arrays `const [a, b, c] = [1, 2, 3]`
    - Objects `const { a, b } = {a: 'ciao', b: 'pippo' }`
    - Function Parameters `const f = ({ a }, [b]) => a * b; f({ a: 2 }, [3])`
  - Spread operator: 
    - Arrays `const a = [b, ...c]`
    - Objects `const a = { b, ...c }`
    - Function Parameters `f(a, ...b)`
  - Modules: 
    - Named import `import { a } from "b"`
    - Default import `import x from "y"`
    - Named export `export const a = 5` / `const a = 5; export { a }`
    - Default export `const x = 5; export default x`
  - Classes / OOP features (Object Oriented Programming)
  - Promises and async/await

Resources where to find these:
  - ES6 Interactive Guide: https://es6-interactive-guide.netlify.app/
  - ES6 Features (examples): http://es6-features.org/
  - Exploring ES6 (very detailed book, to be used for specific doubts): https://exploringjs.com/es6/

A lot of other advanced concepts are in ES6+, not important on a first instance, but interesting to go further.

Note: ES6 is adding the most features, the subsequent updates (ES7, ES8, ES2020, ES2021, ES2022) are adding less but focused features.
    
**React**

Theory: https://reactjs.org/docs/hello-world.html
  - As a starter, get familiar with chapters 1, 2, 3, 4
  - About the state and lifecycle, instead of chapter 5 start with:
    - https://reactjs.org/docs/hooks-state.html
    - https://reactjs.org/docs/hooks-effect.html
  - Very important, to understand React's "philosophy": chapter 12 "Thinking in React"
  - When everything is very clear, start chapters 6, 7, 8, 9, 10, 11

**D3**

This is a quick collection of incremental steps to learn how to build a dataviz:
  - https://bost.ocks.org/mike/bar/ (basic HTML, automatic DOM generated HTML)
  - https://bost.ocks.org/mike/bar/2/ (basic SVG, importing CSV, generated SVG)
  - https://bost.ocks.org/mike/bar/3/ (Ordinal scales, Axes, Margins)
  - The Data Join: https://github.com/d3/d3-selection#selection_join
  - Interactions: https://github.com/d3/d3-selection#handling-events
  - Transitions: https://bost.ocks.org/mike/transition/

**React with D3**

  - D3 and React @ Accurat: article by Ilaria Venturini https://docs.google.com/document/d/1xGfHYnqVr8YRqgr0LLGfSutCvWVR9mOWIQ6epfdRDyA/edit
  - React and D3, interactive guide by Amelia Wattenberger: https://wattenberger.com/blog/react-and-d3


## Mid / Senior Developers

### Frontend

**Accurat Styleguide**

  - Beta version: https://docs.google.com/document/d/183C-72hu8hVUBzpLn0UwhmTFWFG_T6WbCWTZ9L5qdsY/edit?ts=5caf5d4b&pli=1

**Advanced JS concepts**

  - All of the aforementioned topics described as advanced (obviously)
  - Collection of advanced JS articles: http://superherojs.com/
  - Clean Code principles applied to JS: https://github.com/ryanmcdermott/clean-code-javascript

**MobX**

  - Just to understand its concept of reactivity
  - https://mobx.js.org/getting-started.html
  - https://www.mendix.com/blog/making-react-reactive-pursuit-high-performing-easily-maintainable-react-apps/
  - https://github.com/mobxjs/mobx
  - https://egghead.io/courses/manage-complex-state-in-react-apps-with-mobx (If you age comfortable with video lessons)

**mobx-state-tree** [a.k.a. MST]

  - This is what we actually use in Accurat projects
  - https://github.com/mobxjs/mobx-state-tree
  - https://medium.com/react-native-training/state-management-with-mobx-state-tree-373f9f2dc68a
  - https://codeburst.io/the-curious-case-of-mobx-state-tree-7b4e22d461f
  - https://medium.com/free-code-camp/how-to-build-a-state-based-router-using-react-and-mobx-state-tree-e91b2b8e8d79

**Testing**

  - We don't currently have testing enforced in our projects, we do it case-by-case, but it's something we must be ready to do, and we plan to think about.
  - Jest: https://jestjs.io/
  - Testing React: https://medium.com/javascript-scene/unit-testing-react-components-aeda9a44aae2
  - Testing React with Enzyme: https://airbnb.io/enzyme/
  - Testing React and MST, Strategy: https://medium.com/dazn-tech/testing-mobx-state-tree-c588f4bfc430

**TypeScript**

  - Almost a default choice for every Accurat project
  - Quickstart: https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
  - The official Handbook: https://www.typescriptlang.org/docs/handbook/basic-types.html

**Svelte**

  - This is a very interesting project we often use for light editorial projects.
  - Website: https://svelte.dev/
  - Philosophy: https://svelte.dev/blog/frameworks-without-the-framework
  - Quickstart: https://svelte.dev/blog/the-easiest-way-to-get-started 
  - Very thorough tutorial: https://svelte.dev/tutorial/basics

### Backend

**Node.js + TypeScript + Express**
  - TODO
  
**Python**
  - TODO
  
### Data Analysis

**R**
  - TODO
