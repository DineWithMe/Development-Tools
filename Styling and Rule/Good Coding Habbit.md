# Habit

Habit is actually rules that not able to translate into linter yet.

## Pure function

If possible, make pure function, this make testing easier.

## Modular Function

Create modular function for any code that we use more than once to improve code cleanness and maintainability.

## Export at Index File

For every folder, create a index file that import all the exported functions in the folder and export with that index file. This is make importing much cleaner because we only import one file(index file) for every folder.

## Use Constant

Create constant for any value that we use more than once.

Sure we can use code editor search and replace utility but that is a dangerous thing to do, it is safer and more convenience to just create a constant.

## Object Property Accessor Constant

Object property is place where many thing can go wrong because of mistype, apply a constant to the accessor can prevent this type of problem.

example:

```
cosnt NAME='name'

const { [NAME]:username } = user

console.log(username)
```

this practise has few crucial benefits:
1. It prevent typo (no more out of no where undefined)
2. If you mistype any variable (username or NAME), your linter will throw error, you wont able to compile.

The only mistake is, you may use the wrong constant, but this is not fault of constant it is common error but still to see such error is rare.

With constant we can also easily share it within back end and front end because object is heavily involved in api.

# Don't Use the First Index to Check Wether The Array Empty Or Nor

```
arr = [undefine,1,2,3]
arr[1] // undefined
```

this will give you false idea that the array is empty

## Use Absolute Path

Our file path stay the same, we don't need to worry about the path when we copy the import statement to another file or when we move file.

The only drawback is we cannot go higher than 

## Don't Include File Extension when Importing

This allow us to freely change the file extension without breaking the import, for example changing from `js` to `jsx`

## Minimal Cases for "default"

consider this code:

```javascript
const status = 'pass'

if (status==='failed'){
  discard()
} else {
  sendToCustomer()
}
```

now imagine in future there would be another status = 'retest', we would also send this to customer.

Of course we can modify the code to something like this:

```javascript
const status = 'pass'

if (status==='pass'){
  sendToCustomer()
} else {
  discard()
}
```

which is much safer, we still discard the retest unit but better than sending possible defect unit to customer.

However imagine this kind of condition is all over the place and you forgot to change all of them.

I think the ideal solution is to have a enum case where it forces us to fill all the case for every item or else throw compilation error 

but currently this seem impossible with JS.

## Create More Dotenv File Rather than Environment Cases

consider this code:

```javascript
if (process.env.ENV === 'production'){
  port = 3000
} else if (process.env.ENV === 'development'){
  port = 4000
}
```

now imagine we have another new EVN value "pre-staging", what we need to do is change the code, this is not safe because you most likely have these condition all over the place and you may forgot to change all the code.

a recommended way to do so is, create different dotenv file for each and change our code to rather than we use env var as condition, instead we use it as value:

```javascript
port = process.env.PORT
```

with this, not only we reduce the line of code plus, we also don't need to worry about forget to change code for every environment.

our code is now extremely consistent and caseless(less point of failure/bug), and the benefit of caseless code is, if something not right, we can quickly tell that most likely it is from environment variable but not the code. However if the code is not caseless, then we need also to check the cases, because cases it self is more prone to error.

## Keep the Parameter

consider these two piece of codes:

```javascript
const func = ({name, age, occupation}) = {
  userStatus = {
    maturity: age > 18 ? 'adult' : 'not adult'
    name,
    age,
    occupation
  }
}
```

and

```javascript
const func = (userData) = {
  const { name, age, occupation } = userData
  userStatus = {
    maturity: age > 18 ? 'adult' : 'not adult'
    ...userData
  }
}

```

the second approach is more flexible because it retain variable that represent the whole object, this is convenient when we want to console log the whole object or pass the whole object.

## Object or Non-Object as Parameter

Object here refer to **object literal**.

Consider these two pieces of codes:

```javascript
const floatingPoint = (denominator, numerator) => {
  return numerator/denominator
}
floatingPoint(4,5)
floatingPoint(5,4)
```

```javascript
const floatingPoint = (fraction)=>{
  const { denominator, numerator } = fraction
  return numerator/denominator
}
floatingPoint({ denominator: 4, numerator: 5 })
floatingPoint({ numerator: 5, denominator: 4 })
```

In this case we want to look for floating point of 4/5, if we use the first code, we have higher chance of confusing our self when assigning values to arguments, object eliminate the need to remember the order of parameter.

It also easier to skip some paramerters, we can only assign what we want easily.

The two drawback of object as parameter is:

1. object is reference, we may accidentally change all the object.
2. object name need to be correct.

However non-Object as parameter also has great advantage:

1. you don't need to worry about name
2. assigning default value is easier than object

more on: https://stackoverflow.com/questions/12826977/multiple-arguments-vs-options-object

## Circular Referencing Solution

imagine such case:

```javascript
const a  = {
    val: 'a',
    neighbor: d
}

const b = {
    val: 'b',
    neighbor: a
}


const c = {
    val: 'c',
    neighbor: b
}

const d = {
    val: 'd',
    neighbor: c
}


console.log( 'd: neighbor - > ' , d.neighbor )
console.log( 'a: neighbor - > ' , a.neighbor )
```

obviously the code wont work, as `d` is not yet defined when `a` try to access it.

A quick solution for this is by using eval():

```javascript
const a  = {
    val: 'a',
    neighbor: "d"
}

const b = {
    val: 'b',
    neighbor: "a"
}


const c = {
    val: 'c',
    neighbor: "b"
}

const d = {
    val: 'd',
    neighbor: "c"
}


console.log( 'd: neighbor - > ' , eval(d.neighbor) )
console.log( 'a: neighbor - > ' , eval(a.neighbor) )
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
console.log( 'd: neighbor - > ' , neighborhood[neighborhood.d.neighbor] )
console.log( 'a: neighbor - > ' , neighborhood[neighborhood.a.neighbor] )
```

this solution is more verbose but definitely safer than eval()

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

The answer is the second one is better
![thi and obj](img/thisandobj.png)
because we wont shit ourself when we destructure it,
in the first case, the `this` become window object after destructuring
long story short, don't use `this` keyword in object,
if we have to, use class

## It is better to import assests in JS then adding it in HTML

1. Scripts and stylesheets get minified and bundled together to avoid extra network requests.
2. Missing files cause compilation errors instead of 404 errors for your users.
3. Result filenames include content hashes so you don’t need to worry about browsers caching their old versions.

in short, going through webpack > without wbpack

https://facebook.github.io/create-react-app/docs/using-the-public-folder
