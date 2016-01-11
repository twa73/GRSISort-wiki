Coding standards promote code that is presented in a standardized format and easier to understand. For GRSISort, we base our coding standards on ROOT's [coding conventions](https://root.cern.ch/coding-conventions). Please adhere to these standards. Additional standards are listed below.
* When casting variables, please use a `static_cast`. For example, use:
```
static_cast<Int_t> x
```
rather than 
```
(Int_t) x
```