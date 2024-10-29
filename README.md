# language-ideas
Collection of ideas for a possible future language

- C++, Chuck, Nabla, rakudo and OCaml influenced types
- old school linear algebra vector/matrix type operators including subscript range/indices

```cpp
#include <cstdlib>
#include <cstdio>
#include <bit>
#include <array>
#include <numeric>

template<typename T>
concept scalar = std::integral<T> || std::floating_point<T>;

enum align
{
  adaptive = 0,
  element  = 1,
  vector   = 2,
};

/* fixed value array with interface of valarray but with element/vector aligned fixed static underlying array type */
template<enum align A, scalar T, size_t N>
class fixed_valarray
{
public:
constexpr static size_t pow2 = sizeof(T) * std::bit_ceil<size_t>(N);
    using element_aligned_array_type = std::array<T,N>;
    using vector_aligned_array_type __attribute__((vector_size(pow2))) = T;
    using array_type = std::conditional_t<(A == align::element || ((A == align::adaptive) && (std::popcount(N) != 0))),
                       element_aligned_array_type, vector_aligned_array_type>;
/* implement all std::valarray interface functions here */
private:
    array_type data;
};
```
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
