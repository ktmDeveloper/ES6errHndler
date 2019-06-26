# ES6errHndler
Error handling done right in with ES6 syntax

```javascript
const currVals = {
  firstName: 'testName',
  lastName: '',
  zipCode: '12345',
  state: 'AX'
}

const checkIfEmpty = (fieldValue, fieldName) => fieldValue.length == 0 ? `${fieldName} cannot be empty` : '';
const checkCharMinLength = ( fieldValue, fieldName, compareLength ) => fieldValue.length <= compareLength ? `${fieldName} must be at least ${compareLength} characters` : '';
const checkCharEqualLength = ( fieldValue, fieldName, compareLength) => fieldValue.length !== compareLength ? `${fieldName} must be ${compareLength} characters` : '';
const checkCharMaxLength = (fieldValue, fieldName, compareLength) => fieldValue.length >= compareLength ? `${fieldName} must be less than ${compareLength} characters` : '';


const inputCriteria =  {
  firstName: [checkIfEmpty, (fieldValue, fieldName) => checkCharMinLength(fieldValue, fieldName, 2), (fieldValue, fieldName) => checkCharMaxLength(fieldValue, fieldName, 20) ],
  lastName: [checkIfEmpty, (fieldValue, fieldName) => checkCharMinLength(fieldValue, fieldName, 2), (fieldValue, fieldName) => checkCharMaxLength(fieldValue, fieldName, 20) ],
  zipCode: [checkIfEmpty, (fieldValue, fieldName) => checkCharEqualLength(fieldValue, fieldName, 5) ],
  state: [checkIfEmpty, (fieldValue, fieldName) => checkCharEqualLength(fieldValue, fieldName, 2) ],
}

const getErrorMessage = (inputs, criteria) => {
    return Object.keys(inputs).reduce((acc, fieldName) => {
        return [
            ...acc,
            ...criteria[fieldName].reduce((acc, test) => {
                if (!acc.length) {
                    let testRes = test(inputs[fieldName], fieldName);
                    if (testRes) {
                        acc.push(testRes)
                    }
                    return acc
                }
                return acc
            }, [])
        ]
    }, []).filter((error) => error.length)
}

console.log(getErrorMessage(currVals, inputCriteria))
```
