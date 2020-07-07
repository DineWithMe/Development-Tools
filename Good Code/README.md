# Good Code

This section is a mixture of JS and React.

## Pure function

If possible, make pure function, this make reusability easier.

## Modular Function

Create modular function for any code that we use more than once to improve code cleanness and maintainability.

## Use Captial Constant Variable for string constant

dont leave repeated string value without variable, instead create a constant variable(capital letter) them.

benefit:  
-safer to search and replace variable name
-consistency across code, we only need to change the value in variable

## Export at Index File

For every folder, create a index file that import all the exported variable in that folder and export it in that index file.

Benefit:
- encapsulation: you control how to expose your exports to other modules, the change in our export is less likely to affect the import path in other modules.

without index file
```js
// abc/something.js
export const something = ''

//otherFile1.js, otherFile2.js, otherFile3.js...
import { something } from 'abc/something'

```

if we change the name of variable something or the file name, we need to change it in all other files
```js
// abc/thing.js
export const thing = ''

//otherFile1.js, otherFile2.js, otherFile3.js...
import { something } from 'abc/thing'
```

with index file
```js
// abc/something.js
export const something = ''

// abc/index.js
import { something } from './something'

//otherFile1.js, otherFile2.js, otherFile3.js...
import { something } from 'abc'
```
if we change the name of variable something or the file name, we only need to apply the change in index file
```js
// abc/thing.js
export const thng = ''

// abc/index.js
import { thing as something } from './thing' //only need to apply the change here

//otherFile1.js, otherFile2.js, otherFile3.js...
import { something } from 'abc'
```

- easier to import, you import only folder and variable name, not folder,file and variable name, less thing to keep in mind.

without index file
```js
import { something } from 'abc/somethingFile'
```

with index file
```js
import { something } from 'abc'
```

## Use property for Property(also accessor)

Object property is place where many thing can go wrong because of mistype, apply a constant to the accessor can prevent this type of problem.

example:

```js
cosnt NAME='name'

const { [NAME]:username } = user

console.log(username)
```

benefits:  
1. prevent typo, bundler like webpack will warn you the variable is not found.*
2. easier to replace the value, you dont need to replace it everywhere.**

* This is not applicable if we use the wrong constant that exist, however this is logical error, not semantic error. To solve logical error, you need test.

** work great with typescript, constant + typescript make sure it is both correct and you dont need to replace it everywhere in case you change the member name.

## Don't Use the First Index to Check Wether The Array Empty Or Nor

```js
arr = [undefine,1,2,3]
arr[0] // undefined
```

it give us the false idea that the array is empty

## Use Absolute Path

```js
// abc.js
import {someTools} from '././././utils'
```

this is relative path

```js
//abc.js
import {someTools} from 'src/components/Button/ButtonLarge/utils' 
```

benefits:
- less confusion and more reable.
- easier to import.
- you dont need to change the import if you move abc.js to somewhere else.

The only drawback is we cannot go higher than the root absolute path.

Personally I think 1 or 2 level relative path is still acceptable if the file is staple file in that folder.

## Don't Include File Extension when Importing

Bad:  
```js
import {Button} from 'Components/Button.jsx' 
```

good:  
```js
import {Button} from 'Components/Button' 
```

Benefits:
- This allow us to freely change the file extension without breaking the import, for example changing from `js` to `jsx` since bundler like webpack ignore extension.

## Minimal Cases for "default" 

consider this code:

```javascript
const status = 'pass'

if (status === 'failed') {
  discard()
} else {
	sendToCustomer()
}
```

now imagine in future there would be another status = 'retest', we would also send this to customer.

Of course we can modify the code to something like this:

```javascript
const status = 'pass'

if (status === 'pass') {
	sendToCustomer()
} else {
	discard()
}
```

which is much safer, we still discard the retest unit but better than sending possible defect unit to customer.

However imagine this kind of condition is all over the place and you forgot to change all of them.

I think the ideal solution is to have a enum case where it forces us to fill all the case for every item or else throw compilation error.

but currently how this can be done in JS.

## Create More Dotenv File Rather than Environment Cases

consider this code:

```javascript
if (process.env.ENV === 'production') {
  port = 3000
} else if (process.env.ENV === 'development') {
  port = 4000
}
```

now imagine we have another new EVN value "pre-staging", what we need to do is change the code, this is not safe because you most likely have these condition all over the place and you may forgot to change all the code.

a recommended way to do so is, create different dotenv file for each and change our code to rather than we use env var as condition, instead we use it as value:

```javascript
port = process.env.PORT
```

with this, not only we reduce the line of code plus, we also don't need to worry about forget to change code for every environment.

our code is now extremely consistent and case-less(no if-else, less point of failure/bug), and the benefit of this code is, if something not right, we can quickly tell that most likely it is from environment variable but not the code. However if the code is not case-less, then we need also to check the cases, because cases it self is a frequent point of failure.

## Keep the Parameter Name

consider these two piece of codes:

```javascript
const func = ({name, age, weight}) = {
  const name_ = name || ''
  const age_ = age || 0
  const weight = weight || '0kg'
  
  userStatus = {
    maturity: age > 18 ? 'adult' : 'not adult'
    name_,
    age_,
    weight_
  }
}
```

and

```javascript
const func = (politicianData) = {
  const defaultPoliticianData = { name: '', age: 0, weight: '0kg' }
  const { name, age, weight } = {...defaultPoliticianData,...politicianData}
  
  userStatus = {
    maturity: age > 18 ? 'adult' : 'not adult'
    ...politicianData
  }
}

```

the second approach is more flexible and easier because:

1. it retain variable that represent the whole object, this is convenient when we want to console log the whole object or pass the whole object.
2. the parameter name itself provide meaningful data, for example the first approach, we cannot know this data belong to what group of people, but with second approach we know this data belong to a politician but not a gamer.
3. Easier to assign default value without introducing extra variable names.

The drawback of keeping the parameter name is you may forget to make a shallow copy of it.
to solve this, simply discipline yourself to alway include default value
with default value, it encourage you to make shallow copy

## Object or Multiple Parameters

Object here refer to **object literal**.

consider these cases:

```javascript
const floatingPoint1 = (denominator = 0, numerator = 0) => {
  return numerator / denominator
}
```

```javascript
const floatingPoint2 = (fraction) => {
  const defaultFraction = { denominator: 0, numerator: 0 }
  const { denominator, numerator } = {...defaultFraction, ...fraction}
  return numerator / denominator
}
```

First is multiple parameters, second is object as parameter, and also notice how they are called:

```javascript
floatingPoint1(4, 5)
floatingPoint2({ denominator: 4, numerator: 5 })
```

Which one is better? After some brainstorming:

Benefits of object as parameter are:

1. Changing order of parameter doesn't require you to change the code all over the places.
2. Skipping parameter require less cognitive load, you don't need to count how many you skipped.
3. Explicitly hint you what each parameter does with object key, better for readability.

Drawbacks of object as parameter are:

1. Object is reference, we may accidentally change all the object if we don't shallow copy it.
2. Object name need to be correct.
3. Changing object name require all change to be made.
4. Assigning default value is more hassle.

Benefits of multiple parameter:

1. Changing parameters name doesn't require you to change the code all over the places.
2. Assigning default value is easier.

Drawbacks of multiple parameter are:

1. Changing order of parameters require you to change the code all over the places.
2. Skipping parameter has higher cognitive load, you need to count how many you have skipped.
3. Doesn't explicitly hint you what each parameter do, because there is no name when you call it.

So which one is better? Practically speaking, multiple parameter is better for the reason we change parameter name more often than we change the parameter order.

But for case that the parameter is dynamic, best example is `React.Component` class, where the number of parameter can be largely different when you call it and usually involve a lot of parameter, then obviously with multiple parameter cases, arranging parameter is something that is you do frequently because you want to prioritize the important parameter that is unlikely to be skipped and you probably change quite a lot, hence the cognitive load of ordering the parameter is very noticeable.

In such case, then object is more practical, just like why human remember another human not with number but name and why we surfing web by typing website name but not IP address.

Summary: 3-4 options, then multiple parameter, more than that, object.

more on: https://stackoverflow.com/questions/12826977/multiple-arguments-vs-options-object

## Dont Mutate State

consider this:

```js
const num = { a:1 }

const func = obj =>{
  obj.a += 2
  return obj
}

func(num)
```

Obviously the original object value also changed, which is not good because this is a side effect. There is some few way to solve this:

```js
const func = { a } =>{
  a = a + 2
  return { a }
}
```

However this method is not preferable for the reason mentioned in [Keep the Parameter Name](#keep-the-parameter-name)

```js

const func = obj =>{
  const { a } = {...obj}
  a += 2
  return { a }
}

```

With spread syntax we can easily clone the object.
In case you need deep clone, it is available in lodash npm.

## Circular Referencing Solution

imagine such case:

```javascript
const a = {
	val: 'a',
	neighbor: d,
}

const b = {
	val: 'b',
	neighbor: a,
}

const c = {
	val: 'c',
	neighbor: b,
}

const d = {
	val: 'd',
	neighbor: c,
}

console.log('d: neighbor - > ', d.neighbor)
console.log('a: neighbor - > ', a.neighbor)
```

obviously the code wont work, as `d` is not yet defined when `a` try to access it.

A quick solution for this is by using eval():

```javascript
const a = {
	val: 'a',
	neighbor: 'd',
}

const b = {
	val: 'b',
	neighbor: 'a',
}

const c = {
	val: 'c',
	neighbor: 'b',
}

const d = {
	val: 'd',
	neighbor: 'c',
}

console.log('d: neighbor - > ', eval(d.neighbor))
console.log('a: neighbor - > ', eval(a.neighbor))
```

However eval() is [evil](https://stackoverflow.com/questions/86513/why-is-using-the-javascript-eval-function-a-bad-idea)

So a better way is to change the code structure and use property accessor:

```javascript
const neighborhood = {
	a: {
		val: 'a',
		neighbor: 'd',
	},

	b: {
		val: 'b',
		neighbor: 'a',
	},

	c: {
		val: 'c',
		neighbor: 'b',
	},

	d: {
		val: 'd',
		neighbor: 'c',
	},
}
console.log('d: neighbor - > ', neighborhood[neighborhood.d.neighbor])
console.log('a: neighbor - > ', neighborhood[neighborhood.a.neighbor])
```

this solution is more verbose but safer than eval()

## `this` in Object

Consider these tow pieces of code, which is better?

```javascript
var a = {
	one() {
		return 1
	},
	plusOne(num) {
		return this.one() + num
	},
}
```

```javascript
function one() {
	return 1
}
var a = {
	one,
	plusOne(num) {
		return one() + num
	},
}
```

<p align="center">
  <img src='img/thisandobj.png'>
</p>

The answer is the second one is better because we wont shit ourself when we destructure it, in the first case, the `this` become window object after destructuring long story short, don't use `this` keyword in object, if we have to, use class.

## It is better to import scripts in JS then in HTML

1. Scripts and stylesheets get minified and bundled together to avoid extra network requests.
2. Missing files cause compilation errors instead of 404 errors for your users.
3. Result filenames include content hashes so you don’t need to worry about browsers caching their old versions.

in short, going through webpack > without wbpack

https://facebook.github.io/create-react-app/docs/using-the-public-folder

## Circular Import

https://stackoverflow.com/questions/38841469/how-to-fix-this-es6-module-circular-dependency/42704874#42704874

This can be solve if we enforce uni-flow import with atomic design.

example:

layer 1  
layer 2  
layer 3

layer 3 can import from layer 2, layer 1 and generic layer  
layer 2 can import from layer 1 and generic layer  
layer 1 can only import from generic layer

## Use Render Prop Over Higher Order Component

<p align="center">
  <img src='img/unknown.png'>
</p>

Higher order component name is unknown.

https://dev.to/fargrim/react-devtools-and-unknown-components-3ni6  
also  
https://www.richardkotze.com/coding/hoc-vs-render-props-react

## Place restProps in The End of Props (JSX)

```js
const CustomButton = props => {
  return (
    <button
      {...otherProps}
      onClick={
        // do something
      }
    />
  )
}
```

this is not good, this doesnt provide the option for developer to overwrite default onClick callback.

```js
const CustomButton = props => {
  return (
    <button
      onClick={
        // do something
      }
      {...otherProps}
    />
  )
}
```

this is better, this provide the option for developer to define their own onClick callback.

## Always Provide Initial Value For Reduce

<p align="center">
  <img src='img/skipped.png'>
</p>

If initial value is not provided, accumulator will take the first element of array as default value and skip first loop.  
  
This could be an issue, for example we might want to do other calculation in first loop, that also will be skipped, so always include initial value.

## Naming Component, Folder, File

1.Name should be long because:

- more meaningful (informative)
- easier to search / replace
- long name has higher chance to be automatically unique and no collision, hence you wont mistaken it for another component
  
2.Name should be reversed:
  
What is meant by this is, the `set` that entity belong to should be in front of the name, for example SignUpForm should be named as FormSignUp, the first word give you good clue of what this item is.

the same rule also apply to folder and file naming and with extra benefit, this will automatically group similar file/folder together:

FormResetPassword  
FormSignIn  
FormSIgnUp  

The common issue of long name is, the chance of typo is higher, but this is not an issue at all, webpack throw compilation error if it cannot find your component or folder, will throw error.

## Do Not Export Variable in Object

bad:
```js
// obj.ts
const a = 1
const b = 2

export const number = {a, b}
```
this does not enjoy tree shaking provided by bundler like webpack

good:
```js
// notObj.ts
export const a = 1
export const b = 2
```
this enjoy tree shaking

also we can export it like
```js
import * as number from 'notObj'
```
this has the best of both world
