# Modern C++ Meta-programming

A practical reference to templates, type manipulation, compile-time computation, and generic programming. Includes fundamental concepts and design principles that make generic C++ easier to reason about.

- [Modern C++ Meta-programming](#modern-c-meta-programming)
  - [1. Fundamentals](#1-fundamentals)
    - [Templates: code parameterized by types and values](#templates-code-parameterized-by-types-and-values)
    - [Template argument deduction](#template-argument-deduction)
    - [Type versus value categories](#type-versus-value-categories)
    - [Forwarding references and reference collapsing](#forwarding-references-and-reference-collapsing)
    - [Type deduction: `auto`, `decltype`, and `decltype(auto)`](#type-deduction-auto-decltype-and-decltypeauto)
  - [2. Type traits and type transformations](#2-type-traits-and-type-transformations)
    - [Type traits](#type-traits)
    - [`_t` and `_v` helper aliases](#_t-and-_v-helper-aliases)
    - [`std::decay` versus `std::remove_cvref` (**C++20**)](#stddecay-versus-stdremove_cvref-c20)
    - [`std::conditional`](#stdconditional)
    - [`std::common_type` and `std::common_reference`](#stdcommon_type-and-stdcommon_reference)
    - [`std::declval`](#stddeclval)
    - [`std::void_t`](#stdvoid_t)
  - [3. SFINAE, overload resolution, and specialization](#3-sfinae-overload-resolution-and-specialization)
    - [SFINAE](#sfinae)
    - [`std::enable_if`](#stdenable_if)
    - [Partial specialization](#partial-specialization)
    - [Full specialization](#full-specialization)
    - [Tag dispatch](#tag-dispatch)
    - [Overload resolution and implicit conversions](#overload-resolution-and-implicit-conversions)
  - [4. Variadic templates and parameter packs](#4-variadic-templates-and-parameter-packs)
    - [Variadic templates](#variadic-templates)
    - [Parameter-pack expansion](#parameter-pack-expansion)
    - [Fold expressions](#fold-expressions)
    - [`std::integer_sequence` and `std::index_sequence`](#stdinteger_sequence-and-stdindex_sequence)
  - [5. Compile-time computation](#5-compile-time-computation)
    - [`constexpr`](#constexpr)
    - [`static_assert`](#static_assert)
    - [`if constexpr`](#if-constexpr)
    - [`consteval` and `constinit` (**C++20**)](#consteval-and-constinit-c20)
    - [`std::is_constant_evaluated` (**C++20**)](#stdis_constant_evaluated-c20)
    - [Constant template parameters](#constant-template-parameters)
  - [6. Concepts and constraints (**C++20**)](#6-concepts-and-constraints-c20)
    - [Named concepts](#named-concepts)
    - [`requires` expressions](#requires-expressions)
    - [`requires` clauses](#requires-clauses)
    - [Standard concepts](#standard-concepts)
    - [Constraint composition and subsumption](#constraint-composition-and-subsumption)
    - [Concepts versus SFINAE](#concepts-versus-sfinae)
  - [7. Generic programming utilities](#7-generic-programming-utilities)
    - [`std::invoke` and `std::invoke_result`](#stdinvoke-and-stdinvoke_result)
    - [`std::apply`](#stdapply)
    - [`std::forward_like` (**C++23**)](#stdforward_like-c23)
    - [Class template argument deduction (CTAD)](#class-template-argument-deduction-ctad)
  - [8. Quick reference: common techniques](#8-quick-reference-common-techniques)

## 1. Fundamentals

### Templates: code parameterized by types and values

Templates let you write code that works with multiple types or compile-time values.

```cpp
template<typename T>
T max_value(T a, T b) {
    return a < b ? b : a;
}

template<typename T, std::size_t N>
struct Buffer {
    T data[N];
};
```

Template arguments can be types, values, or other templates. The compiler instantiates specializations as needed.

**Remember:** Templates are a language mechanism, not necessarily a metaprogramming technique. They become metaprogramming when you use them to inspect types, select implementations, or compute results at compile time.

### Template argument deduction

The compiler deduces template arguments from function arguments when possible.

```cpp
template<typename T>
void process(T value);

int n = 42;
process(n);  // T = int
```

Deduction rules are sensitive to references, `const`, arrays, and value categories. They are not identical to `auto` deduction in every context.

### Type versus value categories

Every expression has a type and a value category: *lvalue*, *xvalue*, or *prvalue*.

```cpp
int x = 42;

x;            // lvalue
std::move(x); // xvalue
42;            // prvalue
```

These distinctions influence overload resolution, reference binding, and whether move operations can be selected.

### Forwarding references and reference collapsing

A deduced `T&&` parameter is a forwarding reference when `T` is a deduced, cv-unqualified template parameter.

```cpp
template<typename T>
void wrapper(T&& value) {
    target(std::forward<T>(value));
}
```

- An lvalue argument causes `T` to deduce as an lvalue reference.
- An rvalue argument causes `T` to deduce as a non-reference type.
- Reference collapsing reduces combinations of references: `&` dominates, so `T& &&` becomes `T&`.

**Key distinction:** `std::move` expresses an intention to treat an object as movable; `std::forward<T>` preserves the value category of a deduced argument.

### Type deduction: `auto`, `decltype`, and `decltype(auto)`

```cpp
int x = 42;

auto a = x;            // int
decltype(x) b = x;     // int
decltype((x)) c = x;   // int&
decltype(auto) d = (x); // int&
```

- `auto` generally drops references and top-level cv-qualifiers.
- `decltype` follows special rules for unparenthesized identifiers and otherwise considers value category.
- `decltype(auto)` applies those `decltype` rules to deduction.

These are essential when writing generic wrappers, forwarding functions, and APIs that must preserve references.

## 2. Type traits and type transformations

### Type traits

Utilities in `<type_traits>` inspect properties of types or produce transformed types.

```cpp
std::is_integral_v<int>
std::is_same_v<int, unsigned int>
std::is_base_of_v<Base, Derived>
std::is_trivially_copyable_v<T>
```

Common categories:

- **Classification:** `is_pointer`, `is_array`, `is_enum`, `is_class`.
- **Relationships:** `is_same`, `is_base_of`, `is_convertible`.
- **Properties:** `is_copy_constructible`, `is_nothrow_move_constructible`, `is_trivially_copyable`.
- **Transformations:** `remove_reference`, `remove_cv`, `add_pointer`, `decay`.

Use traits to express requirements and select behavior based on properties of a type.

### `_t` and `_v` helper aliases

Prefer the shorthand forms where available.

```cpp
std::remove_reference_t<T>
std::decay_t<T>
std::is_integral_v<T>
```

The `_t` forms expose a transformed type; the `_v` forms expose a Boolean trait value.

### `std::decay` versus `std::remove_cvref` (**C++20**)

These transformations are similar but not interchangeable.

```cpp
using A = std::decay_t<const int&>;       // int
using B = std::remove_cvref_t<const int&>; // int

using C = std::decay_t<int[4]>;           // int*
using D = std::remove_cvref_t<int[4]>;    // int[4]
```

`decay` models transformations resembling by-value function argument deduction, including array-to-pointer and function-to-pointer conversion. `remove_cvref` removes references and top-level cv-qualifiers without decaying arrays or functions. <Cite refs={["turn521657search0"]}/>

### `std::conditional`

Select one of two types based on a compile-time Boolean.

```cpp
using T = std::conditional_t<condition, TypeA, TypeB>;
```

Useful for type selection. Both type arguments must be valid types, even though only one is selected.

### `std::common_type` and `std::common_reference`

Find a common type or reference type for a set of types.

```cpp
using T = std::common_type_t<int, double>; // double
```

`std::common_reference` (**C++20**) is particularly relevant to generic code involving references, ranges, and proxy types. A common reference is not necessarily the same as a common value type.

### `std::declval`

Obtain a hypothetical expression of a type in an unevaluated context.

```cpp
using Result = decltype(
    std::declval<T&>().some_method()
);
```

Useful for inspecting expression types without constructing a `T`. It must not be evaluated.

### `std::void_t`

A compact building block for detecting whether types or expressions are valid during substitution.

```cpp
template<typename T, typename = void>
struct has_value_type : std::false_type {};

template<typename T>
struct has_value_type<T, std::void_t<typename T::value_type>>
    : std::true_type {};
```

This is the classic detection idiom: substitution succeeds when `T::value_type` exists and fails otherwise.

For new C++20 code, a concept or `requires` expression is often clearer.

## 3. SFINAE, overload resolution, and specialization

### SFINAE

*Substitution Failure Is Not An Error.*

When substituting template arguments into certain immediate-context constructs fails, that candidate can be removed from overload resolution instead of making the entire program ill-formed.

```cpp
template<typename T,
         std::enable_if_t<std::is_integral_v<T>, int> = 0>
void process(T);
```

Historically, SFINAE was widely used to constrain templates, detect capabilities, and control overload selection.

**Modern guidance:** Recognize SFINAE because you will encounter it in existing libraries and older code. Prefer concepts and `requires` clauses for new C++20 interfaces when they express the requirement more clearly.

### `std::enable_if`

Conditionally provide a type only when a compile-time condition is true.

```cpp
template<typename T,
         typename = std::enable_if_t<std::is_integral_v<T>>>
void process(T);
```

It remains a standard facility, but many common uses for constraining functions are easier to express with concepts.

### Partial specialization

Provide a specialized implementation when some template arguments match a particular pattern.

```cpp
template<typename T>
struct TypeInfo {
    static constexpr bool is_pointer = false;
};

template<typename T>
struct TypeInfo<T*> {
    static constexpr bool is_pointer = true;
};
```

Partial specialization is available for class and variable templates, but not function templates. Function-template customization typically uses overloads instead.

### Full specialization

Provide a specific implementation for particular template arguments.

```cpp
template<typename T>
struct Traits;

template<>
struct Traits<int> {
    static constexpr int id = 1;
};
```

Useful for customization and type-specific behavior. Keep specialization rules and the point at which specializations must be declared in mind.

### Tag dispatch

Select an implementation by dispatching on a type that encodes a property.

```cpp
template<typename T>
void process_impl(T value, std::true_type);

template<typename T>
void process_impl(T value, std::false_type);

template<typename T>
void process(T value) {
    process_impl(value, std::is_integral<T>{});
}
```

An older but still useful pattern to recognize. For new interfaces, `if constexpr` or concepts may make the intent simpler.

### Overload resolution and implicit conversions

Generic code must account for overload ranking, template deduction, reference binding, and implicit conversions.

```cpp
void f(int);
void f(double);

template<typename T>
void f(T);
```

Adding a template overload can change which function is selected. Be careful when mixing constrained and unconstrained templates or adding implicit conversion paths.

## 4. Variadic templates and parameter packs

### Variadic templates

Accept an arbitrary number of template arguments.

```cpp
template<typename... Args>
void log(Args&&... args);
```

Parameter packs underpin tuples, generic wrappers, forwarding utilities, and many standard-library abstractions.

### Parameter-pack expansion

Expand a pack in a context that accepts a sequence of elements.

```cpp
template<typename... Args>
auto make_tuple_like(Args&&... args) {
    return std::tuple<Args...>(
        std::forward<Args>(args)...
    );
}
```

The `...` expansion is a core technique for forwarding arbitrary argument lists and composing generic interfaces.

### Fold expressions

Apply an operator over a parameter pack.

```cpp
template<typename... Args>
auto sum(Args... args) {
    return (... + args);
}

template<typename... Args>
bool all_true(Args... args) {
    return (... && args);
}
```

A fold can replace many recursive variadic-template implementations. Empty packs are only supported for certain unary folds, including `&&`, `||`, and `,`; a sum over an empty pack needs explicit handling.

### `std::integer_sequence` and `std::index_sequence`

Represent a sequence of compile-time integer values.

```cpp
template<std::size_t... I>
void process_indices(std::index_sequence<I...>);
```

Useful for expanding tuple elements, indexing parameter packs, and writing generic utilities that operate on a fixed number of compile-time elements.

## 5. Compile-time computation

### `constexpr`

Allow eligible variables and functions to participate in constant evaluation.

```cpp
constexpr int square(int n) {
    return n * n;
}

static_assert(square(8) == 64);
```

A `constexpr` function may also be called at runtime. The important distinction is whether a particular evaluation is required to be a constant expression.

### `static_assert`

Validate a condition at compile time.

```cpp
static_assert(sizeof(void*) == 8,
              "Expected a 64-bit pointer");
```

Useful for enforcing type properties, representation assumptions, and API requirements. Avoid treating platform assumptions as universal guarantees.

### `if constexpr`

Discard a branch during template instantiation when its condition is false.

```cpp
template<typename T>
void process(const T& value) {
    if constexpr (std::is_integral_v<T>) {
        // Integral-specific implementation
    } else {
        // Other implementation
    }
}
```

Useful when different types require different implementations without writing separate specializations. This belongs to metaprogramming because it enables compile-time selection within an ordinary function body.

### `consteval` and `constinit` (**C++20**)

- `consteval`: Declare an immediate function; its invocations must be evaluated at compile time.
- `constinit`: Require a variable with static or thread storage duration to be statically initialized. It does not make the variable `const`.

```cpp
consteval int square_now(int n) {
    return n * n;
}

constinit int global_counter = 0;
```

Use `consteval` when compile-time evaluation is part of the contract. Use `constinit` to avoid unintended dynamic initialization of eligible globals or thread-local objects.

### `std::is_constant_evaluated` (**C++20**)

Detect whether the current evaluation is a constant evaluation.

```cpp
if (std::is_constant_evaluated()) {
    // Constant-evaluation path
} else {
    // Runtime path
}
```

Useful when an operation must behave differently at compile time and runtime. Be careful: its result depends on the evaluation context, not merely on whether a function was declared `constexpr`.

### Constant template parameters

Templates can accept compile-time values, not just types.

```cpp
template<typename T, std::size_t N>
struct Buffer {
    T data[N];
};
```

C++20 expanded the kinds of values that can be used as non-type template parameters, including certain structural class types.

```cpp
template<auto Value>
struct Constant {};
```

Useful for compile-time configuration and type-level encoding of fixed values.

## 6. Concepts and constraints (**C++20**)

Concepts are named sets of requirements on template arguments. They make generic interfaces more readable and diagnostics more useful.

### Named concepts

```cpp
template<typename T>
concept Integral = std::is_integral_v<T>;

template<Integral T>
T add(T a, T b) {
    return a + b;
}
```

A concept describes a requirement; it does not create a new type.

### `requires` expressions

Check whether types support particular expressions or nested types.

```cpp
template<typename T>
concept HasValueType = requires {
    typename T::value_type;
};

template<typename T>
concept Addable = requires(T a, T b) {
    a + b;
};
```

Requirements can check expression validity, required types, return-type constraints, and `noexcept` properties. <Cite refs={["turn521657search1","turn521657search5"]}/>

### `requires` clauses

Constrain a template or function declaration.

```cpp
template<typename T>
    requires std::is_integral_v<T>
T add(T a, T b) {
    return a + b;
}
```

This makes the requirement part of the declaration and overload-resolution process rather than hiding it inside the function body.

### Standard concepts

Useful concepts from `<concepts>` include:

- `std::same_as`
- `std::derived_from`
- `std::convertible_to`
- `std::integral` and `std::floating_point`
- `std::movable` and `std::copyable`
- `std::invocable`
- `std::predicate`
- `std::regular`

Prefer a standard concept when it precisely captures the requirement instead of defining a redundant custom one.

### Constraint composition and subsumption

Combine constraints using `&&` and `||`. More constrained overloads can be preferred when their constraints subsume those of other candidates.

```cpp
template<typename T>
concept Numeric = std::integral<T> || std::floating_point<T>;

template<Numeric T>
void process(T);
```

Constraint ordering has specific language rules. Two constraints that appear logically equivalent are not necessarily interchangeable for overload ordering if they are formed differently.

### Concepts versus SFINAE

| Technique | Typical use |
| --- | --- |
| Type traits | Inspect or transform types |
| SFINAE / `enable_if` | Conditionally participate in overload resolution |
| `void_t` | Detect types or expressions through substitution |
| Concepts / `requires` | Express template requirements directly |
| `if constexpr` | Select implementation branches inside a function or class template |

Concepts do not eliminate type traits or every use of SFINAE. They provide a clearer language-level interface for many constraints.

## 7. Generic programming utilities

### `std::invoke` and `std::invoke_result`

`std::invoke` provides uniform invocation for functions, function objects, member-function pointers, and member-data pointers.

```cpp
auto result = std::invoke(callable, args...);
```

`std::invoke_result_t<F, Args...>` obtains the result type of invoking `F` with `Args...`, when that invocation is valid.

Use these instead of manually reproducing the language's callable invocation rules. `std::invoke_result` is the modern replacement for the removed `std::result_of`.

### `std::apply`

Invoke a callable using the elements of a tuple-like object as arguments.

```cpp
auto args = std::make_tuple(1, 2);
auto result = std::apply(add, args);
```

Useful for bridging tuple-based storage and variadic function interfaces.

### `std::forward_like` (**C++23**)

Forward an expression using the constness and value category of another type.

```cpp
auto&& value = std::forward_like<decltype(*this)>(member);
```

Useful in generic wrappers and proxy-like abstractions where an accessor should preserve the relevant cv-qualification and value category of its enclosing object. It complements `std::forward`, which is based on a deduced template argument.

### Class template argument deduction (CTAD)

Deduce class-template arguments from constructor arguments.

```cpp
std::pair p{1, 2.5}; // std::pair<int, double>
std::vector v{1, 2, 3}; // std::vector<int>
```

Deduction guides customize deduction when constructor arguments alone would not produce the intended type. Useful to recognize when reading generic-library code, even if you rarely write custom guides yourself.

## 8. Quick reference: common techniques

| If you need to… | Investigate |
| --- | --- |
| Inspect a type | Type traits |
| Transform a type | `remove_cvref`, `decay`, `conditional` |
| Check an expression | `decltype`, `declval`, `void_t`, `requires` |
| Constrain a template | Concepts and `requires` clauses |
| Select an implementation | Overloads, specialization, `if constexpr` |
| Process arbitrary arguments | Variadic templates, pack expansion, fold expressions |
| Preserve argument categories | Forwarding references, `std::forward` |
| Determine a callable's result type | `std::invoke_result_t` |
| Invoke a callable uniformly | `std::invoke`, `std::apply` |
| Compute a value at compile time | `constexpr`, `consteval`, `static_assert` |
| Express a resource lifetime | RAII, smart pointers, rule of zero |
| Improve generic API diagnostics | Named concepts and constrained overloads |

**Compatibility note:** C++20 and C++23 library support depends on the compiler and standard-library implementation. Check support for individual facilities rather than assuming that enabling a language standard guarantees every library feature is available.
