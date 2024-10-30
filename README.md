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

Collection of important additions to the existing standard C++ language
- linear algebra n-dimensional fixed extents arithmetic static allocated stack/register vector/matrix type with adaptive element/vector alignment according to n, similiar to [kokkos/mdarray](https://github.com/kokkos/mdarray) and [kokkos/mdspan](https://github.com/kokkos/mdspan)
