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
#include <algorithm>

template<typename T>
concept scalar = std::integral<T> || std::floating_point<T>;

namespace std
{
enum align
{
  adaptive = 0,
  element  = 1,
  vector   = 2,
};

template<scalar OT, size_t ON>
using element_aligned_type = std::array<OT, ON>;
template<scalar OT, size_t ON, size_t POW2 = std::bit_ceil<size_t>(ON)>
using vector_aligned_type __attribute__((vector_size(sizeof(OT) * POW2))) = OT;
template<scalar OT, size_t ON, enum align OA = align::adaptive>
using array_type = std::conditional_t<(OA == align::element || ((OA == align::adaptive) && (std::popcount(ON) != 1))), element_aligned_type<OT,ON>, vector_aligned_type<OT,ON>>;

template<scalar T, size_t N, enum align A = align::adaptive>
struct fixed_valarray
{

fixed_valarray(std::initializer_list<T> src)
{
        std::copy_n(src.begin(), std::min<size_t>(src.size(), N), &data[0]);
}
T& operator[](size_t i) { return data[i]; }
private:
array_type<T,N,A> data;
};

};


namespace vec
{
template<size_t N>
using f32 = std::fixed_valarray<float, N>;
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
