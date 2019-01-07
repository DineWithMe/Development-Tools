# Habbit

Habbit is actually a rule which not able to translate into linter yet.

## Do not destructuring the parameter

consider these two piece of codes

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

the later would have much more flexibility plus the cost of conversion to later is small and only one time.
```

## Modular Function

Create modular function for any code that you use more than once to improve code cleanness and maintainability.

## String and Number Constant

Create string or number constant for any string or number that you use more than once.

Sure you can use code editor search and replace utility but that is a dangerous thing to do, it is safer and more convenience to just create a constant.

## Object Property Accessor Constant

Object property is place where many thing can go wrong because of mistype, apply a constant to the accessor can prevent this type of problem.

With constant you can also easily share it within back end and front end because object is heavily involve in api.

## default case must not be "default" if possible

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

Of course you can modify the code to something like

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

If possible we should take care as much case as possible, and the default case should alert the developer there is a not take-care.
