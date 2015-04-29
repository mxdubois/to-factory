# to-factory

Convert classes into factory functions.

Allows ES6 classes to be instantiated with or without `new`.

Allows your API consumers to not need to divine (there's no built-in way to know) which functions you intend to be called with `new`, and which functions are to be called without.

## Example

### without `to-factory`

Must call a class with `new`, cannot call class as a function:

```js
class Person {
  constructor(name) {
    this.name = name
  }
}

new Person('alice') // ok
Person('bob') // TypeError: Cannot call a class as a function
```

### with `to-factory`

Call a class with `new` or as a function:

```js
// exact same Person class as above example

Person = toFactory(Person)

const personA = new Person('created with new')
console.log(personA.name) // => 'created with new'

const personB = Person('created without new')
console.log(personB.name) // => 'created without new'
```

Inheritance, etc all works as expected

```js
// Inheritance works as expected
class BigPerson extends Person {
  constructor(name) {
    super(name.toUpperCase())
  }
})

BigPerson = toFactory(BigPerson)

const bigPersonA = new BigPerson('created with new')
console.log(bigPersonA.name) // => 'CREATED WITH NEW'

const bigPersonB = BigPerson('created without new')
console.log(bigPersonB.name) // => 'CREATED WITHOUT NEW'
```

## Why

The "can't call without new" restriction on ES6 classes create a needless split between tools which accept classes and tools which accept regular functions – as there is no built-in means for a tool to determine whether the function they've been passed would like to be called with `new` or without `new`, it just has to pick one and hope for the best, I guess.

While it's true that some real-world functions will insist on being constructors in much the same way, it's not command and at least it is possible to patch such code and/or submit a pull request explaining that insisting on `new` is uneccessarily rigid and provides little benefit when it's entirely possible to transparently handle both cases. With ES6 classes we're left with fewer options as this rigidity is baked right into the implementation.

## See Also

* [is-class](http://npmjs.com/package/is-class)

## License

MIT
