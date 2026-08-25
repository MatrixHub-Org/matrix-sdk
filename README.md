# MatrixHub

Zero-dependency matrix library for JavaScript. Create, transform, and decompose dense matrices in Node.js and the browser.

[![npm](https://img.shields.io/npm/v/matrixhub.svg)](https://www.npmjs.com/package/matrixhub)
[![license](https://img.shields.io/npm/l/matrixhub.svg)](./LICENSE)

## Install

```bash
npm install matrixhub
```

## Quick start

ESM:

```js
import { Matrix } from 'matrixhub';

const matrix = Matrix.ones(5, 5);
```

CommonJS:

```js
const { Matrix } = require('matrixhub');

const matrix = Matrix.ones(5, 5);
```

## Features

- Dense matrices, symmetric matrices, and distance matrices
- Element-wise math and in-place updates
- Matrix multiplication, including Strassen
- Statistics: mean, variance, covariance, correlation
- Decompositions: SVD, EVD, LU, QR, Cholesky, NIPALS
- Linear solve, inverse, pseudo-inverse, determinant
- Zero production dependencies

## Examples

### Arithmetic

```js
const { Matrix } = require('matrixhub');

const A = new Matrix([
  [1, 1],
  [2, 2],
]);

const B = new Matrix([
  [3, 3],
  [1, 1],
]);

Matrix.add(A, B); // [[4, 4], [3, 3]]
Matrix.sub(A, B); // [[-2, -2], [1, 1]]
A.mmul(B);        // [[4, 4], [8, 8]]
Matrix.mul(A, 10); // [[10, 10], [20, 20]]
Matrix.div(A, 10); // [[0.1, 0.1], [0.2, 0.2]]
```

In place:

```js
const C = new Matrix([
  [3, 3],
  [1, 1],
]);

C.add(A);
C.sub(A);
C.mul(10);
C.div(10);
```

### Math helpers

```js
const A = new Matrix([
  [1, 1],
  [-1, -1],
]);

Matrix.exp(A);
Matrix.cos(A);
Matrix.abs(A); // or A.abs()
```

`abs`, `acos`, `acosh`, `asin`, `asinh`, `atan`, `atanh`, `cbrt`, `ceil`, `clz32`, `cos`, `cosh`, `exp`, `expm1`, `floor`, `fround`, `log`, `log1p`, `log10`, `log2`, `round`, `sign`, `sin`, `sinh`, `sqrt`, `tan`, `tanh`, `trunc`

### Shape and values

```js
A.rows;
A.columns;
A.size;
A.get(0, 0);
A.set(1, 0, 10);
A.isSquare();
A.diag();
A.mean();
A.norm();
A.transpose();
```

### Reduce along an axis

```js
const M = new Matrix([
  [1, 2, 3],
  [4, 5, 6],
]);

const sumOf = (vector) => vector.reduce((total, value) => total + value, 0);

M.applyAlongAxis(sumOf, 'row');    // [6, 15]
M.applyAlongAxis(sumOf, 'column'); // [5, 7, 9]
```

### Constructors

```js
Matrix.zeros(3, 2);
Matrix.ones(2, 3);
Matrix.eye(3, 4);
```

### Concat

```js
const M = new Matrix([
  [1, 2],
  [3, 4],
]);

M.concat([[5, 6]]);
M.concat(Matrix.columnVector([5, 6]), 'column');
```

### Inverse and solve

```js
const {
  Matrix,
  inverse,
  solve,
  linearDependencies,
  QrDecomposition,
  LuDecomposition,
  CholeskyDecomposition,
  EigenvalueDecomposition,
} = require('matrixhub');

const A = new Matrix([
  [2, 3, 5],
  [4, 1, 6],
  [1, 3, 0],
]);

const inverseA = inverse(A);
A.mmul(inverseA); // identity (within rounding)

const x = solve(A, Matrix.columnVector([1, 2, 3]));
```

Singular or least-squares cases can use SVD:

```js
inverse(A, true);
solve(A, B, true);
A.pseudoInverse();
```

### Decompositions

```js
const QR = new QrDecomposition(A);
QR.orthogonalMatrix;
QR.upperTriangularMatrix;

const LU = new LuDecomposition(A);
LU.lowerTriangularMatrix;
LU.upperTriangularMatrix;
LU.pivotPermutationVector;

const cholesky = new CholeskyDecomposition(A);
cholesky.lowerTriangularMatrix;

const e = new EigenvalueDecomposition(A);
e.realEigenvalues;
e.imaginaryEigenvalues;
e.eigenvectorMatrix;
```

## License

[MIT](./LICENSE)
