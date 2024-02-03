# INTRODUCTION

This program conducts the revised simplex algorithm on a LP problem defined by a matrix of constraints and an objective function. It outputs the optimal value reached by revised simplex algorithm, and the solution array in the format of reporting the value of each variable at the optimal point.  

# DESCRIPTION OF THE ALGORITHM

Algorithm takes in an initial ordered list of basic variable indexes (starting from 0 to match with the array indexing) and non-basic variable indexes. Initially non-basic variables list consist of each variable specified by coefficients in the given constraints matrix in the test cases. This is because under the spefication of the homework, the problem is a maximization problem with lesser than or equal to constraints of non-negative RHS, thus the initial set of basic variables are the added slack variables. As defined in revised simplex algorithm, a matrix `B` which consists of the columns of `A` (the initial coefficient matrix extended by the columns of the newly added slack variables) that has the basic variables in them. Then this matrix `B` is checked to see if its singular, as the next step to take its inverse, and if it is singular, it shows that the problem is unbounded. Then the inverse of `B` is calculated. `c_b` matrix is also calculated, which is the coefficients of the objective function that correspond to basic variables. Then for each non-basic variable index, coefficient of `c` in that index is priced out, meaning that its value is subtracted from the resulf of multplying `B^_1`, `A` and `c_b`. Then the minimum of these new coefficient values are determined and checked if its positive, as if its positive it means that no further optimization is possible and the optimal value is reached, in that case the optimal value and the solution is outputted. If that's not the case, the index of the coefficient with the minimum value is chosen as the pivot column and the ratio test is applied on the positive column elements to determine the pivot. If all of these column elements are non-positive, it means the LP is unbounded thus the algorithm exits and outputs a message stating that the problem is unbounded. If its not unbounded, the row with the minimum of the ratios of right hand side to the column element is chosen and that row index is returned. The basic variable of the row with that row index is swapped with the non-basic variable that had the most negative objective function coefficient that was calculated in the pricing out step. Thus one element exits the list of non-basic variable list and one enters, and the same for the basic variable list, so they are swapped. After this, the algorithm is repeated undefinitely until either the all coefficients of calculated `c` , which is titled as `c*` is positive or it is found out that the LP is unbounded. 

# OUTPUT OF THE PROGRAM
When `simplex.py` is run in the same folder with the data files given, this output is printed as specified in the homework's documentation. The second case is unbounded thus the revised simplex algorithm cannot function in its usual operation, thus the unboundedness is catched by two checks, the first checks if the B matrix is singular and the second checks if there is a column of a non-basic variable that is full of negative numbers, if at least one of these is true then the case is unbounded and thus Data 2 is reported as unbounded as B matrix in the last iteration for it turns out to be singular. 

```
Data 1:

[0.6 1.2 0.  0.  0. ]
z = 4.2

Data 2:

Solution is unbounded.

Data 3:

[ 0.304  0.     0.     0.543  0.37   0.     0.     0.     0.     2.043
  4.196 13.522  0.     2.957  0.   ]
z = 5.391
```

# CONCLUSION


