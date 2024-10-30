# language-ideas
Collection of ideas for a possible future language

- C++, Chuck, Nabla, rakudo and OCaml influenced types
- old school linear algebra vector/matrix type operators including subscript range/indices

```perl6
template<scalar T, size_t n>
auto op(T^n args...);

template<scalar T, size_t n>
auto op{indices...,dst}(T^n args...);

template<scalar T, size_t n>
auto op[first,last,dst](T^n args...);

(** scalar product with optional variable arg count > 2 *)
|(T^n args...) = std::accumulate<T>(arg[0] * ... * arg[sizeof...(args)-1]);

(** vector product using hodge star operator with specializations for ((n or subvec.len()) <= 4) *)
*(T^n args...) = hodge(arg[0], ..., arg[n-2]); 

(** laplace operator *)
%(T^n args...) = laplace(arg[0], ..., arg[n-2]);

(** determinant *)
?(T^n args...) = det(arg[0], ..., arg[n-1]);

(** derivative *)
fun operator'(const fun& arg);
```
- implement linear algebra n-dimensional vector type with adaptive element/vector alignment according to n
```
realize std::fixed_valarray with std::mdspan using extents::static_extents based on kokkos examples
template<scalar T, size_t N>
using fixed_valarray = std::mdspan<T, static_extents(N)>

template<size_t N>
struct vec : fixed_valarray<float, N>
{
}
template<size_t N>
struct qut : fixed_valarray<float, N>
{
}

template<size_t ROWS, size_T COLS>
struct mat : fixed_valarray<float, COLS, ROWS>
{
}
```
