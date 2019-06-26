# ES6 Error Handling
## Error handling done right in with ES6 syntax

```javascript
// values to test
const currVals = {
  firstName: "testName",
  lastName: "",
  zipCode: "12345",
  state: "AX"
};

// these are the functions that test our inputs. Each function only handles one type of check.
const checkIfEmpty = (fieldValue, fieldName) =>
  fieldValue.length == 0 ? `${fieldName} cannot be empty` : "";

const checkCharMinLength = (fieldValue, fieldName, compareLength) =>
  fieldValue.length <= compareLength
    ? `${fieldName} must be at least ${compareLength} characters`
    : "";

const checkCharEqualLength = (fieldValue, fieldName, compareLength) =>
  fieldValue.length !== compareLength
    ? `${fieldName} must be ${compareLength} characters`
    : "";

const checkCharMaxLength = (fieldValue, fieldName, compareLength) =>
  fieldValue.length >= compareLength
    ? `${fieldName} must be less than ${compareLength} characters`
    : "";

// for each field, we can specify what type of checks we can to do.
// For 'checkCharMinLength', we use currying. We only get two parameters passed to these function.
// The third parameter is the length to be compared, which we can hardcode here itself.
const inputCriteria = {
  firstName: [
    checkIfEmpty,
    (fieldValue, fieldName) => checkCharMinLength(fieldValue, fieldName, 2),
    (fieldValue, fieldName) => checkCharMaxLength(fieldValue, fieldName, 20)
  ],
  lastName: [
    checkIfEmpty,
    (fieldValue, fieldName) => checkCharMinLength(fieldValue, fieldName, 2),
    (fieldValue, fieldName) => checkCharMaxLength(fieldValue, fieldName, 20)
  ],
  zipCode: [
    checkIfEmpty,
    (fieldValue, fieldName) => checkCharEqualLength(fieldValue, fieldName, 5)
  ],
  state: [
    checkIfEmpty,
    (fieldValue, fieldName) => checkCharEqualLength(fieldValue, fieldName, 2)
  ]
};

// spread operator / destructuring / functional JS
const getErrorMessage = (inputs, criteria) => {

  // For each keys in input we run the test.
  return Object.keys(inputs)
    .reduce((acc, fieldName) => {
    
      // we want to return the accumulator with additional field if there is error for that field,
      // or else we return empty string
      return [
        ...acc,
        ...criteria[fieldName].reduce((acc, test) => {
        
          // we only keep the first error. All other are ignored for that field.
          // if we want to show all the errors for the field, we can use .map()
          if (!acc.length) {
            let testRes = test(inputs[fieldName], fieldName);
            if (testRes) {
              acc.push(testRes);
            }
            return acc;
          }
          
          return acc;
        }, [])
      ];
    }, [])
    
     // we filter the result and only return the field that has some value in it.
    .filter(error => error.length);
};

console.log(getErrorMessage(currVals, inputCriteria));

```
