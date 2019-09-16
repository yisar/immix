# Immix
Create next immutable patch by mutating the current.

### Use
```js
import immix from 'immix'

const data = {
  count: 0
}

immix(data, (draft, patches) => {
  draft++
  console.log(draft) // { count: 1 }
  console.log(patches) // [ { op: 'replace', path: '/count', value: 1, oldValue: 0 } ]
  console.log(data) // { count: 0 }
})
```
