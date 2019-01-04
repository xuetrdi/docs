========
Overview
========

- Dense matrix and array manipulations(稠密矩阵和数组操作)
- Sparse linear algebra(稀疏线性代数)
- Extending/Customizing Eigen(扩展与自定义)
- General topic(通用)

Matrix class
------------

  在Eigen中，所有矩阵和向量都是Matrix模版类的对象。向量只是矩阵的一种特殊情况。

  Matrix模版的前三个参数:

  - Scalar: 标量类型
  - RowAtCompileTime: 编译时已知的矩阵的行数
  - ColAtCompileTime: 编译时已知的矩阵的列数
  
  Eigen提供了typedef来涵盖通用的情况。比如: typedef Matrix<float, 4, 4> Matrix4f;

Vectors
-------

  Vectors只是Matrices的一种特殊情况。具有一行或者一列，他们有一列的情况最常见的，这种矢量称为列矢量，通常缩写为矢量。
  具有一行的情况下称为行向量。

  typedef Vector3f是3个浮点数的: typedef Matrix<float, 3, 1> Vector3f;
  行向量: typedef Matrix<int, 1, 2> RowVector2i;

MatrixXd
--------

  特殊值动态。

  typedef Matrix<double, Dynamic, Dynamic> MatrixXd;

  typedef Matrix<int, Dynamic, 1> VectorXi;

  动态列数的固定行数: Matrix<float, 3, Dynamic>

Constructors
------------

  默认构造函数始终可用，从不执行任何动态内存分配，也从不初始化矩阵系数。比如：Matrix3f a;

  对于矩阵，始终首先传递行数。对于矢量，只需传递矢量大小。

Coefficient accessors(系数访问器)
---------------------------------

  Eigen中的主要系数访问器和增便器是重载的小括号运算符。
  对于矩阵，行索引始终首先传递。对于向量，只需要传递一个索引。编号从0开始。