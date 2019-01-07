# Habbit

Habbit is actually a rule which not able to translate into linter yet.

## Do not destructuring the parameter

consider these two piece of codes

```
const func = ({name, age, occupation}) = {
  userStatus = {
    maturity: age > 18 ? 'adult' : 'kid'
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
    maturity: age > 18 ? 'adult' : 'kid'
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
