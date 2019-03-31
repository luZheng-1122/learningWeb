# Practice List

## JS
1. 判断multiple level object中的某个key是否存在
   compare two objects: 
      * use `_.isEqual()` in [lodash](https://lodash.com/docs#isEqual).
      * reinvent the wheel: https://gomakethings.com/check-if-two-arrays-or-objects-are-equal-with-javascript/
      * existence check with 'in' (./Fundamentals.md Object/Existence check)
      * obj.hasOwnProperty(key)
2. error handler tool
3. module import/export
4. setTimeout refactor
5. arrow function refactor/function declaration/function expression
6. promise refactor 
   * when to use callback
   * when to use promise
   * when to use .then
   * when to use async/await
   * use finally to clean up loading indicators
7. this 的用法
8. prototype inheritance
9. coding style: es-lint (airbnb), function usage [JSDoc](https://github.com/jsdoc3/jsdoc)
10. refactor loop:
    1.  when need to iterate an array: use forEach, for, for...of
    2.  when need to iterate and return a new array: use map
    3.  when need to calculate a singe value: use reduce
11. refactor time difference measurement, with Date.now()
12. refactor JSON.stringfy(value, replacer): replacer can help you select the keys you want or passing a mapping function as replacer to convert any value.
13. refactor with rest and spread operator
14. refactor with destructuring
15. refactor with default parameter values
16. logical operator refactor

## React
1. authentication
2. international
3. localstorage practice
4. upload image
5. access control
6. locally testing (end to end, unit snapshot, code usage) and CICD testing
7. input validation 
8. loading indicators
9. pagination (different pags or feed flow)

## html/css
1. modal window

## Node.js + RESTful
1. MVC
2. epxress server
3. connect db
4. ts + tslint
5. locally testing (unit test) and CICD testing

## devops
1. build react project (webpack, babel)
2. npm