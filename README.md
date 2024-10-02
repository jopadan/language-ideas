# language-ideas
Collection of ideas for a possible future language

- C++, Chuck, Nabla, rakudo and OCaml influenced types
- old school linear algebra vector/matrix type operators including subscript range/indices

```perl6
template<typename T>
concept scalar = std::integral<T> || std::floating_point<T>;

template<scalar T, size_t n>
auto op(T^n args...);

template<scalar T, size_t n>
auto op{indices...,dst}(T^n args...);

template<scalar T, size_t n>
auto op[first,last,dst](T^n args...);

(** scalar product with optional variable arg count > 2 *)
|(T^n args...) = (arg[0] * ... * arg[n-1]).sum();

(** vector product using hodge star operator with specializations for ((n or subvec.len()) <= 4) *)
*(T^n args...) = hodge(arg[0], ..., arg[n-2]); 

```
