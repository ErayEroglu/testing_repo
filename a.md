# INTRODUCTION

This program provides a solution to a given linear algebra problem. It is divided into several functions, each has a crucial role in the process of solving linear equations. Input is taken as a text file. After reading the file, the program creates an augmented matrix and starts doing row operations. After the execution is done, a solution is printed to the console. Unless there exists a solution, that means we have an inconsistent problem and this is also printed to the console as an output.  We will examine these row operation functions in detail to understand how they contribute to solving linear equations.

# DESCRIPTION OF THE ALGORITHM

For a given matrix `A`, its diagonal values (`i, i for ith row`) are checked, if the diagonal value for a given row is non-zero, this row is divided by this value so that the row is normalized to having `1` as its pivot value. Then this scaled row is subtracted from the other rows by multiplying this pivot row by the elements that are in the same column with the pivot, so that in a column that has a pivot element only the the pivot has the value of `1` while the other elements of the column has `0`. If the diagonal value for a given row is zero, the other rows that doesn't already have a pivot element are iterated through to check if they have a non-zero element in tha column, and if there is such a row, they are swapped with the row of the current iteration, and the operations explained in the above case are conducted. After these operations the matrix `A` is transformed into the upper triangular form.

The rows of this upper triangular matrix `U` is searched through to see if there is a row full of zeroes except the right hand side vector `b`. If there is such a row with a non-zero `b` value, it means that zero should equal a non-zero value, hence the system is inconsistent and the program terminates for this case. If there are at least one row with a zero `b` vector element, it means that there are infinitely many solutions as there are at least one arbitrary variable. The indices of these free variables are recorded to be used in the output generation.

This upper triangular matrix `U` is iterated in a bottom-up manner, by checking the rightmost diagonal element of the `U` matrix. This element is divided by the last element of the right hand side vector `b`, so that the last of the solution variables is calculated. If this calculation necessiates a zero division, it means that there is an arbitrary variable, and its value is fixed as 0 as this gives a trivial arbitrary solution out of the infinite amount of solutions. Then, this solution is variable is used in the above row to calculate the the other solution variable, and this is repeated until all the solution are found via backwards substitution. Whether there is an unique solution, arbitrary solution, or no solutions is reported and the output is generated according to the solution type. If there is an unique solution, the input file is read once again to generate the original matrix, and its inverse is calculated by the adjugate matrix method and its printed. 


# IMPLEMENTATION

## `exchangeRows(matrix, firstRowIndex, secondRowIndex):` 

This function is responsible for swapping two rows within a given matrix. This operation is useful during the Gaussian elimination process when rearranging rows to simplify the matrix.

## `interpretMatrix(data, interpretAsSquare):`

This function plays a crucial role in reading and interpreting matrix data. After interpreting the data, this function stores the data in a two dimensional array.
	
If the `interpretAsSquare` is passed as True, the last column of the passed input is ignored and only the `A` matrix is returned for a `A|b` augmented matrix input. This is useful while passing the `A` matrix for the inverse finding operation. 

## `scaleRow(matrix, rowIndex, scalar):`
This function divides a row that is specified by the `rowIndex` by the amount of `scalar` to the scale the given row.

## `combineRows(matrix, sourceRowIndex, destinationRowIndex, scalar):`

The `combineRows` function is responsible for combining rows by multiplying one row by a constant and adding that value to another row. It has an important role in the process of creating zeros below the pivot element.

If the resulting value of the element is less than the arbitrarily very small value `10 ** -7`, it is treated as 0 to counter the floating point arithmetic errors that are inherent to computers. 

## `eliminationStep(matrix, pivot, pivotRowIndex):`

This function is the repeated step of `eliminateMatrix`, and it is responsible for conducting the necessary steps of Gauss-Jordan elimination for that given row specified in the parameter `pivotRowIndex`. It scales the row that has the pivot so that the pivot becomes 1, and then subtracts this pivot row from the rows below, so that the values below the pivot becomes 0 and the matrix gains the upper triangular form in the end. 

## `eliminateMatrix(matrix):`
This is the primary function that conducts Gauss-Jordan elimination on the `matrix`. It iterates through the all rows, to give an example for its flow, if its on the `i`th row, it checks the element in `matrix[i][i]` and see if its non-zero. If its non-zero its selected as the pivot element and `eliminationStep` is called on it. If its zero, then a new pivot is selected by searching through the other rows which were not subject to `eliminationStep` beforehand so that one row is not selected for multiple pivots, if there is a non-zero value on the column that conforms to these checks, then the row of this value is selected as the new pivot row and its swapped with the current `i`th row by calling `exchangeRows` and the operation is resumed by calling `elimationStep` on the newly swapped row. This swapping also ensures that rows with pivots are always in correct position.   

## `substituteBackwards(matrix, solution):`

Substituting backwards is the final step in solving a system of linear equations. It performs backward substitution on an upper triangular matrix, which we have after the elimination part is done, and then it finds the solutions for the variables. It classifies the type of solution as unique, arbitrary, or inconsistent. A unique solution can be found if and only if the rank of the original matrix is equal to the  number of rows. If the rank is less than the number of rows, there are two possible outcomes: infinitely many solutions or inconsistent system. Inconsistent system means there isn’t any possible solution, so in this case program is unable to propose any answer. The systems which have infinitely many solutions are handled by taking the result of division by zero as zero, which leads to fixing the free variables of such a system as `0`. After this step, backwards substitution can resume in the usual manner as if it had a unique solution, albeit now an arbitrary solution with the free variables fixed as `0`.  

## `findInverse(matrix):`

The `findInverse` function is dedicated to finding the inverse of a matrix using adjugate matrix method. This function is only used in the case of the unique solution. It calculates the cofactor matrix of the matrix, transposes it to get the adjugate matrix, and multiplies this matrix by `1 / det(matrix)` to reach the inverse of the `matrix` as `A^-1 = adj(A= * (1 / det(A)`. The original matrix from the input file is passed into this function as its not related to the elimination operations.

## `transpose(matrix):`
Transposes `matrix` and returns the transposed matrix.

## `getInnerMatrix(matrix, rowIndex, columnIndex):`
Returns the matrix that is constructed by cutting out the column and the row that the element specified by the `matrix[rowElement][columnIndex]` is in. So if the matrix is `MxN` it returns a new `M-1xN-1` matrix which doesn't have the row specified by the `rowIndex` and the column specified by the `columnIndex`.  

## `findDeterminant(matrix):`
Calculates the determinanf of a `matrix` and returns its determinant.

## `printSolution(solution, inverse, solutionType):`
This function presents the result of the given system to the user. If there are infinitely many solutions, it prints an arbitrary one. It also prints which variables are arbitrarily chosen. The main functionality of this method is to make the program more user-friendly.

## `formatElement(element):`
This is a utility function to format integers and floats for printing. Floats are rounded to three decimals, which is important for clarity and precision.

# OUTPUT OF THE PROGRAM
When the `matrix.py` is run in the same folder with the data files given, this output is printed as specified in the homework's documentation. Only the second and fourth test cases are invertible and have an unique solution, meanwhile the first has infinitely many solutions as it has a row of zeroes when eliminated, and the third case is inconsistent as it has a row of zeroes followed by a non-zero value in the augmented matrix's `b` column. Floats are rounded to precision of 3 digits after decimal point so that it doesn't look convoluted, but they are calculated with much higher precision in the intermediary steps.  
```
1. Arbitrary variables: x3
Arbitrary solution: 6.6 1.8 0
 
2. Unique solution: 1 -0.5 1.5
Inverted A: 0.5 0.167 0.333
            1 0.4 0.2
            0 0.133 0.067
 
3. Inconsistent problem
 
4. Unique solution: 1.597 0.277 0.767 -0.692 0.974 -1.038
Inverted A: 0.213 -0.093 0.038 0.05 -0.046 -0.156
            -0.001 -0.047 -0.041 0.105 -0.071 0.168
            -0.053 -0.026 0.069 -0.029 0.087 -0.063
            -0.066 0.092 -0.022 -0.028 -0.043 0.079
            0.082 -0.059 -0.028 0.03 -0.008 0.052
            -0.141 0.122 -0.017 -0.071 0.07 -0.024
```

# CONCLUSION

In this program, we coded a Python program which is designed to manipulate matrices and solve a system of linear equations. The program’s functions are explicitly explained in the implementation part of this report. In conclusion, the program tries to solve the problem of `Ax = b`, and if a solution exists it provides a proper solution. In the case of no solution, it lets users know that the given system is inconsistent. If there are infinitely many solutions, it provides an arbitrary solution.

