# Accurat Coding Styleguide

## Preamble

**The following rules are a general styleguide, but you should always prefer the project style if you have to work in an existant project. To state it differently: _Project style is more important than Accurat style._**

The following rules assume the knowledge of ES6-7-8 and its terminology.
If any of the words in the index of [this handbook](https://www.freecodecamp.org/news/the-react-handbook-b71c27b0a795/) (especially the first 4 sections) sound new to you, you should read something about it to fully understand the document in you hands.
For a quick reminder on some of the ES6 terminology, have a look at [http://es6-features.org/].

## JavaScript

###### Names
Be very careful when naming files, variables or functions: it's one of the most important things in software development.

###### File Naming
 - Name the file exactly as the main exported variable (the most important exported). Use CamelCase, avoid dash-casing. (open problem: `*-utils` files)
      WHY? Because it's easier to rename using the same convention for variables and files.

###### Variables Naming
 - Try to maintain the same name for the same entity or structure throughout the entire file.
 - The bigger is the scope in which a variable is used, the more explanative its name should be. You can use `d` if it's a one-line function, but in a 100 lines function you should explain a little more what that variable contains, such as: `filteredUniqueDatasetGroupedByCountry`.
 - A variable should be named with a noun, never with a verb.
     WHY? A verb conveys the meaning of an action and it will be read by another developer as a function, while a noun is semantically associated with a value. BAD examples: `let getElements = [1, 2, 3]`; GOOD examples: `let elements = [1, 2, 3]` or `function createElements() { return [1, 2, 3] }`.
 - Prefer to name a variable containing a boolean to begin with prefix `has*` of `is*` like `const isVisible = condition1 && condition2`, or `const hasChildren = children.length > 0`. Or `isOpen` instead of `open`, use `open` for the action which sets `isOpen = true`.
     WHY? More readable, and can be read as plain english: `if (isOpen) { close() }`
 - For a variable containing a count (integer) prefer a name ending with `*Count`. This is not so obvious, since anyone would be tempted to assign `const elements = 5`
     WHY? It's way more explicit to use `const elementsCount = 5`, since `elements` could be an array of elements.
 - Variables should NEVER be named `data`, please prefer `dataset` for collections and `datum` for a single row / element.
     WHY? The word `data` express no plurality or singularity, and gives absolutely no hints about the content. For example use: `productsDataset` `filteredTimelineDataset` and so on
     NB: This should ONLY be used for real data, as in Accurat's Data. Not for configuration objects or information, for which other names should be used.

###### Functions Naming
  - A function should be named with a verb, never with a noun.
      WHY? A verb conveys the meaning of an action and it will be read by another developer as a function, while a noun is semantically associated with a value. Wrong examples: `function elements() { return [1, 2, 3] }` Good examples: `let elements = [1, 2, 3]` or `function createElements() { return [1, 2, 3] }`.
  - After reading the rule above, you will surely be tempted to call all you functions starting with `get`: BAD examples are `getElements`, `getProducts`, `getValue`. Try to avoid this as much as possible to make your code readable! Please favor more detailed verbs, such as `create`, `fetch`, `compute`, `elaborate`, `generate`, `randomize`, etc. The cited examples could be rewritten as GOOD examples `createElements`, `fetchProducts`, `computeValue`.

###### Modules
 - Keep your imports in order! A neat and tidy block of imports keeps the file ordered. (Alphabetical ordering is too much)
 - Do not ever `export default` (unless is somehow needed like in `pages` folder in Next.js) but ALWAYS export named function, const, class or component
     WHY? It helps naming coherence (you import the exact name of what you export and it increase readability), and it helps tree shaking.

###### Other
 - When you access more nested properties, do it at once with [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) and never repeat it more times.
BAD: `this.props.dataset[0]` and then `this.props.color` and then `this.props.asd.foo.bar`
GOOD: `const { dataset, color, asd } = this.props; const bar = asd.foo.bar; const first = dataset[0]`
    WHY? Who reads the code can immediately see what is extracted from the class/object to be used in the method.

## React Components
  - A function returning elements (JSX) is a component. Recognize a component, and don't call it as a function!
      BAD: `<div>{getElement()}</div>`. GOOD: `<div><Element /></div>`.
      WHY? React offers all kinds of optimization if you use components, but if you don't it can't recognise it.
  - For the same reason avoid putting JSX in a variable, but favor a small component.
  - Function components should use the named function style (`function Card() {}`) instead of the anonymous function style (`const Card = () => {}`). Even if you are using a HOC: `const Card = observer(function Card() {})`.
      WHY? Having a named function helps when debugging, especially with the React Devtools.
  - Listeners props on new components should always be called with the `on`: `onClick`, `onHover`, `onPress`. Don't call them `clickListener` or similar alternative names, think of the developer that will be using that component or reading it being used.
      WHY? Makes the code obvious.
  - If you create a function returning a function to be used as listener, please call it in a way that makes the reader understand it: `build*`, `make*`, `create*`. Example: `buildSelect = (family) => (event) => { select(family) }`
      WHY? To differentiate listeners from listeners builders, which return functions to be used as listeners.
  - Icon components should end with `*Icon`, and similar.
      WHY? Think about the name in the context and scope it will be used: reading `<Search>` is not useful, while reading `<SearchIcon>` makes it immediately clear.
  - Do not mix Function Components with Class Components in the same project. Use the one your project was started with.
    WHY? Codebase coherence.
  - Class components should have ALL their JSX contained in the `render()` method. No other method returning JSX (= render method) should exist, such as `renderSection()` or similar. If you spot one, that will be a code smell and you should suggest it to be refactored.
      WHY? So that everyone knows where to look. If you feel the need for a render method, that should be a new component! (It also improves performance, because it has logic on whether to re-render itself)

## State, MobX and Mobx State Tree
  - The _root_ MobX state should have no actions and no views, only properties containing other models.
      WHY? This separates concerns in an otherwise irreversible way. If you create an action in the global state, you will regret it. (I did! – @caesarsol)
  - The models should never be nested more than one level. Always keep models at the root level.
    WHY? It keeps the project maintainable as you could easily end up with some unmanageable 5 level deep models. It’s easier to access the model from the root, with `const { timeline } = getRoot(self)` in MST, and `const { timeline } = this.props.state` in React
  - Action names should be grammatical verbes.
      WHY? Actions perform a modification, and return nothing.
  - Views names should be grammatical names. BAD: `get filterValues() { … }` GOOD: `get filteredValues() { … }`
      WHY? They represent values.
  - Computed Values names should not start with `get`, because they will be used as if they are concrete values.
      BAD: `get getUserList() { … }`
      GOOD: `get userList() { … }`
      WHY? Getters should be indistinguishable from state properties, so that you can later refactor your code and switch between them without the need to update the components using them. Moreover, they are automatically cached/updated so it makes no sense to use a verb inside, since they do not "perform" any action.
  - If MST (Mobx State Tree) is interrogating some APIs, each model should not do more than 1 API request. There should be one `load()` action which makes the request and saves the request in a property of the model. The data computation should be done in the getters or on server-side.
    WHY? The raw API data can be logged more easily, future server models changes can be reflected more easily, and the data flow is more manageable.
  - If MST is interrogating some APIs, make sure to call the `load()` method from a react component’s `constructor` or `componentDidMount`, not from an `afterCreate()` of the state.
      WHY? It leads to less issues if you have multiple routes and some state shared between those
  - If MST is interrogating some APIs, make sure to create a `reset()` action alongside the `load()` action. It should be called in the react component in the `constructor`, before the `load()` call. A reset action looks like this:
      ```js
      reset() {
        applySnapshot(self, {})
      },
      ```
      WHY? It leads to less issues if you have multiple routes, for example a login page.
  - In the models with some array data, avoid using `.find()` in the getters if the arrays are possibly big (>1000). Use indexed objects instead. Here is a simple example:
      ```js
      get datasetById() {
        return keyBy(self.dataset, 'id')
      },
      ```
      Then use: `self.datasetById[id]` instead of `self.dataset.find(x => x.id === id)`
      WHY? It could lead to some performance issues in the long run, as the data magnitude increases.


## TypeScript
  - Any defined **type** should be `TitleCased`
  - When defining types for something that is not a type try to use the same name (even if Vito thinks that it should be illegal (cit.))
     WHY? The TypeScript compiler can discern this information for itself and having `FooType` and `FooModel` is just noisy when you master the difference between what is a type and what is not.

## TypeScript and React

  - For [Function Components](https://medium.com/@ethan_ikt/react-stateless-functional-component-with-typescript-ce5043466011) always use the type `React.FunctionComponent<Props>`, don’t try to rewrite the type.
      WHY? Don’t try to type `children` in a correct way: it’s hard and it can change between releases of @types/react.
  - Always declare `type Props` and `type State` of a component in a standalone type (not inline) and with the simplest name possible:
```ts
// BAD
class MyComponent extends React.Component<{
 myProp: number
}> { … }

// GOOD
type Props = { myProp: number }
class MyComponent extends React.Component<Props> { … }
```

## Folders
TODO (what components go into components, which into ui, the lib folder (!!!), the locale folder, the constants file, API file)

## CSS
TODO

## Commit naming
TODO
- https://chris.beams.io/posts/git-commit/
- emojis

## Workflow

 - Prettier: config your editor to format on save
 - Eslint: config your editor to show warnings and errors, and never ignore them!
 - Editor: Ensure Newline at end of file
 - Editor: Remove trailing spaces, everywhere
 - Git: when merging, always create a merge commit (= don't use the fast-forward `ff`). [here](https://git-scm.com/docs/git-config#Documentation/git-config.txt-mergeff) is an useful config for that.
 - Git: when pulling, always pull with rebase (`git pull --rebase`) and not with merge (the default). [here](https://git-scm.com/docs/git-config#Documentation/git-config.txt-branchautoSetupRebase) is an useful config for that.
 - Git: configure your [`merge.conflictStyle`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-mergeconflictStyle) as `diff3`
 - `main` as most updated branch, for production systems use the branch `production` or deploy tags `production-2022-01-01-10-12-23`
 - Push your branch ASAP, even if it's not ready for merge. Every end of day for example.
 - OSX: show dotfiles and hidden files command: `defaults write com.apple.finder AppleShowAllFiles -bool true`
