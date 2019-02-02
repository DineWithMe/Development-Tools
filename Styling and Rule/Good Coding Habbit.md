# Habit

Habit is actually a rule which not able to translate into linter yet.

## Pure function

If allowed, make pure function, this make testing easier.

## Modular Function

Create modular function for any code that you use more than once to improve code cleanness and maintainability.

## Export at Index File

For every folder, create a index file that import all the exported functions in the folder and export with that index file. This is make importing much cleaner because you only import one file(index file) for every folder.

## String and Number Constant

Create string or number constant for any string or number that you use more than once.

Sure you can use code editor search and replace utility but that is a dangerous thing to do, it is safer and more convenience to just create a constant.

## Object Property Accessor Constant

Object property is place where many thing can go wrong because of mistype, apply a constant to the accessor can prevent this type of problem.

With constant you can also easily share it within back end and front end because object is heavily involved in api.

## Minimal cases for "default"

consider this code:

```
const status = 'pass'

if (status==='failed'){
  discard()
} else {
  sendToCustomer()
}
```

now imagine in future there would be another status = 'retest', you would also send this to customer.

Of course you can modify the code to something like this:

```
const status = 'pass'

if (status==='pass'){
  sendToCustomer()
} else {
  discard()
}
```

which is much safer, you still discard the retest unit but better than sending possible defect unit to customer.

However we are talking about such mistake in general and how to close the possibility of such mistake.

We should take care as much cases as possible, and the default case should alert the developer that there is a not taken care case.

The less cases default take care of, the safer it is.

I think the ideal solution is to have a enum case where it forces us to fill all the case for every item and throw compilation error if we do not do so, but currently js does not support enum.

## Create more dotenv file rather than environment cases

consider this code:

```
if (process.env.ENV === 'production'){
  port = 3000
} else if (process.env.ENV === 'development'){
  port = 4000
}
```

now imagine you have another new EVN value "pre-staging", what you need to do is change the code, this is not safe because you may forget to change all the related codes.

a recommended way to do so is, create different dotenv file for each and change your code to:

```
port = process.env.PORT
```

with this, your code is lesser and consistent across all the environment, you dont need to change code for every environment
furthermore you can quickly identify whether the problem is come from code or environment variable when behavior is not the same for different environment.

For some case, you need extra trick like parsing, for example when you need an array for your environment variable.

## Keep the parameter

consider these two piece of codes:

```
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

```
const func = (userData) = {
  const { name, age, occupation } = userData
  userStatus = {
    maturity: age > 18 ? 'adult' : 'not adult'
    ...userData
  }
}

```

the later would have much more flexibility plus the cost of conversion from former to later is small and is only one time.

## Object as Parameter

Consider these two pieces of codes:

```
const floatingPoint = (denominator, numerator)=>{
  return numerator/denominator
}
floatingPoint(4,5)
floatingPoint(5,4)
```

```
const floatingPoint = (fraction)=>{
  const { denominator, numerator } = fraction
  return numerator/denominator
}
floatingPoint({ denominator: 4, numerator: 5 })
floatingPoint({ numerator: 5, denominator: 4 })
```

In this case we want to look for floating point of 4/5, if we use the first code, we have higher chance of confusing our self when assigning values to arguments.

The 2nd code greatly reduce such error although it require more typing.

## Circular Referencing Solution

imagine such case:

```
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

A quit solution for this is by using eval():

```
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

However eval() is evil https://stackoverflow.com/questions/86513/why-is-using-the-javascript-eval-function-a-bad-idea

So a better way is to change the code structure and use property accessor:

```
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
