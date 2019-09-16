# Immix
Create next immutable patch by mutating the current.

### Use
```js
import immix from 'immix'

immix(data, (draft, patches) => {
  console.log(patches) // []
  draft++
  console.log(draft) // { count: 1 }
  console.log(patches) // [ op:'replace', path='/count', value:1 ]
  console.log(data) // { count: 0 }
})
```
