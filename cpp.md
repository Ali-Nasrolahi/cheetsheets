# Modern C++

A practical reference to useful C++ language features and standard-library facilities. Focused on remembering what exists, when to use it, and what to watch out for.

- [Modern C++](#modern-c)
  - [1. Types, declarations, and initialization](#1-types-declarations-and-initialization)
    - [Type deduction: `auto`](#type-deduction-auto)
    - [`decltype` and `decltype(auto)`](#decltype-and-decltypeauto)
    - [Type aliases: `using`](#type-aliases-using)
    - [`nullptr`](#nullptr)
    - [Scoped enums: `enum class`](#scoped-enums-enum-class)
    - [Attributes](#attributes)
    - [Braced initialization](#braced-initialization)
    - [Defaulted and deleted functions](#defaulted-and-deleted-functions)
    - [Delegating constructors](#delegating-constructors)
    - [`explicit`](#explicit)
    - [`constexpr`](#constexpr)
    - [Inline variables (C++17)](#inline-variables-c17)
    - [Thread-local storage](#thread-local-storage)
    - [Binary literals and digit separators](#binary-literals-and-digit-separators)
  - [2. Memory, ownership, and object lifetime](#2-memory-ownership-and-object-lifetime)
    - [Move semantics and `std::move`](#move-semantics-and-stdmove)
    - [Smart pointers](#smart-pointers)
    - [`std::string_view`](#stdstring_view)
    - [`std::span` (C++20)](#stdspan-c20)
    - [`std::byte` (C++17)](#stdbyte-c17)
    - [`std::exchange`](#stdexchange)
    - [`std::to_address` (C++20)](#stdto_address-c20)
  - [3. Containers and data structures](#3-containers-and-data-structures)
    - [`std::array`](#stdarray)
    - [Unordered containers](#unordered-containers)
    - [Tuples and structured bindings](#tuples-and-structured-bindings)
    - [`std::optional`](#stdoptional)
    - [`std::variant`](#stdvariant)
    - [`std::any`](#stdany)
    - [Container erasure (C++20)](#container-erasure-c20)
    - [`std::flat_map` and `std::flat_set` (C++23)](#stdflat_map-and-stdflat_set-c23)
    - [`std::mdspan` (C++23)](#stdmdspan-c23)
  - [4. Algorithms and ranges](#4-algorithms-and-ranges)
    - [Standard algorithms](#standard-algorithms)
    - [Ranges (C++20)](#ranges-c20)
    - [`std::ssize` (C++20)](#stdssize-c20)
  - [5. Functions, lambdas, and control flow](#5-functions-lambdas-and-control-flow)
    - [Lambda expressions](#lambda-expressions)
    - [`override` and `final`](#override-and-final)
    - [`noexcept`](#noexcept)
    - [`std::ref` and `std::reference_wrapper`](#stdref-and-stdreference_wrapper)
    - [`if` and `switch` with initialization (C++17)](#if-and-switch-with-initialization-c17)
    - [`std::function`](#stdfunction)
    - [`std::async`](#stdasync)
  - [6. Concurrency and synchronization](#6-concurrency-and-synchronization)
    - [`std::thread`](#stdthread)
    - [`std::jthread` (C++20)](#stdjthread-c20)
    - [Mutexes and locks](#mutexes-and-locks)
    - [Atomics](#atomics)
    - [Coordination primitives (C++20)](#coordination-primitives-c20)
    - [Synchronized output (C++20)](#synchronized-output-c20)
  - [7. Strings, time, files, and diagnostics](#7-strings-time-files-and-diagnostics)
    - [`std::chrono`](#stdchrono)
    - [User-defined literals](#user-defined-literals)
    - [Raw string literals](#raw-string-literals)
    - [`std::filesystem` (C++17)](#stdfilesystem-c17)
    - [`std::format` (C++20)](#stdformat-c20)
    - [`std::print` and `std::println` (C++23)](#stdprint-and-stdprintln-c23)
    - [`std::source_location` (C++20)](#stdsource_location-c20)
    - [`std::expected` (C++23)](#stdexpected-c23)
    - [Monadic operations on `optional` and `expected` (C++23)](#monadic-operations-on-optional-and-expected-c23)
  - [8. Low-level and binary-data utilities](#8-low-level-and-binary-data-utilities)
    - [`std::endian` (C++20)](#stdendian-c20)
    - [`std::bit_cast` (C++20)](#stdbit_cast-c20)
    - [`std::byteswap` (C++23)](#stdbyteswap-c23)
    - [`std::to_underlying` (C++23)](#stdto_underlying-c23)
  - [9. Coroutines and modules](#9-coroutines-and-modules)
    - [Coroutines (C++20)](#coroutines-c20)
    - [Modules (C++20)](#modules-c20)
  - [10. Quick reference: C++20 and C++23 additions](#10-quick-reference-c20-and-c23-additions)

## 1. Types, declarations, and initialization

### Type deduction: `auto`

Deduce a variable's type from its initializer.

```cpp
auto n = 42;                  // int
auto p = &n;                  // int*
const auto& ref = n;          // const int&
auto values = std::vector<int>{1, 2, 3};
```

`auto` generally drops top-level `const` and references. Use `const auto&` to avoid copies when reading objects through references.

### `decltype` and `decltype(auto)`

- `decltype(expr)` obtains the type of an expression without evaluating it.
- `decltype(auto)` deduces a type using `decltype` rules, including preserving references where appropriate.

```cpp
int x = 42;

auto a = (x);           // int
decltype(auto) b = (x); // int&
```

**Gotcha:** `decltype(x)` and `decltype((x))` differ. An unparenthesized variable name yields its declared type; other expressions follow value-category rules.

### Type aliases: `using`

A cleaner alternative to `typedef`, including for template aliases.

```cpp
using Buffer = std::vector<std::byte>;
using Callback = std::function<void(int)>;
```

### `nullptr`

Use the dedicated null pointer literal instead of `NULL` or `0`.

```cpp
void f(int);
void f(int*);

f(nullptr); // calls f(int*)
```

Unlike `NULL`, `nullptr` does not behave as an integer argument.

### Scoped enums: `enum class`

Avoid implicit conversions to integers and enumerator names leaking into the surrounding scope.

```cpp
enum class Color : unsigned int {
    Red   = 0xff0000,
    Green = 0x00ff00,
    Blue  = 0x0000ff
};
```

Specify the underlying type when representation matters, but remember that this alone does not define a portable binary format.

### Attributes

Standardized double-bracket attributes provide information to the compiler.

```cpp
[[nodiscard]] int compute();
[[maybe_unused]] int debug_flag = 0;

[[deprecated("use new_api instead")]]
void old_api();
```

Useful attributes include `[[nodiscard]]` (C++17), `[[maybe_unused]]` (C++17), `[[fallthrough]]` (C++17), and `[[deprecated]]` (C++14).

### Braced initialization

```cpp
int x{42};
std::vector<int> values{1, 2, 3};
```

Braces reject narrowing conversions, but `std::initializer_list` constructors can change overload selection.

```cpp
std::vector<int> a(3, 7); // three elements, all 7
std::vector<int> b{3, 7}; // two elements: 3 and 7
```

### Defaulted and deleted functions

```cpp
struct Resource {
    Resource() = default;
    Resource(const Resource&) = delete;
};
```

- `= default`: Request the compiler-generated implementation.
- `= delete`: Prohibit a function or overload.

Useful for expressing ownership and copyability rules explicitly.

### Delegating constructors

One constructor can delegate initialization to another constructor of the same class.

```cpp
struct Connection {
    Connection(int fd, bool blocking) {
        // initialization
    }

    Connection(int fd) : Connection(fd, true) {}
};
```

### `explicit`

Prevent unwanted implicit conversions through constructors and conversion functions.

```cpp
struct Port {
    explicit Port(int value) : value(value) {}
    int value;
};

Port p{80};
// Port p2 = 80; // error
```

C++20 additionally supports conditional explicitness with `explicit(condition)`.

### `constexpr`

Allow eligible variables and functions to participate in constant evaluation.

```cpp
constexpr int square(int n) {
    return n * n;
}

constexpr int n = square(8);
```

`constexpr` does not require every invocation to execute at compile time. C++20 introduced `consteval` for immediate functions, whose invocations must be evaluated at compile time.

### Inline variables (C++17)

Define variables in headers without violating the one-definition rule when the applicable inline requirements are met.

```cpp
inline constexpr int default_port = 8080;
```

Useful for header-defined constants and static data members.

### Thread-local storage

```cpp
thread_local unsigned request_count = 0;
```

Each thread has its own instance. Useful for per-thread state and counters, but initialization and destruction can have runtime costs.

### Binary literals and digit separators

```cpp
constexpr unsigned flags = 0b1010'0101;
constexpr auto size = 1'000'000;
```

Binary literals and digit separators were introduced in C++14. They improve the readability of masks, constants, and large numbers.

## 2. Memory, ownership, and object lifetime

### Move semantics and `std::move`

Transfer resources instead of necessarily copying them.

```cpp
std::vector<int> a(1000);
std::vector<int> b = std::move(a);
```

`std::move` is a cast that enables move overloads to be selected; it does not itself move anything.

A moved-from object remains valid, but its state is generally unspecified unless the relevant operation documents otherwise.

### Smart pointers

Prefer explicit ownership over manual `new` and `delete`.

```cpp
auto unique = std::make_unique<Widget>();
auto shared = std::make_shared<Widget>();
```

- `std::unique_ptr`: Exclusive ownership; movable, not copyable.
- `std::shared_ptr`: Shared ownership through reference counting.
- `std::weak_ptr`: Non-owning reference to a `shared_ptr`-managed object; useful for breaking ownership cycles.

**Rule of thumb:** Prefer `unique_ptr` unless shared lifetime is genuinely required. Reference counting introduces overhead and can obscure ownership relationships.

### `std::string_view`

A non-owning view of a contiguous character sequence.

```cpp
void parse(std::string_view input);
```

Useful for read-only string parameters without requiring a `std::string` allocation or copy.

**Lifetime warning:** The referenced characters must outlive every access. A view does not extend the lifetime of its backing storage, and `data()` is not guaranteed to point to a null-terminated string at the end of the view.

### `std::span` (C++20)

A non-owning view over a contiguous sequence of objects.

```cpp
void process(std::span<const std::byte> buffer);
```

Useful for APIs that accept arrays, vectors, and other contiguous storage without taking ownership or requiring a specific container type.

Like `string_view`, `span` does not extend the backing storage's lifetime. It is not an owning container.

### `std::byte` (C++17)

Represent raw storage as bytes rather than characters or numeric values.

```cpp
std::byte buffer[256]{};
buffer[0] = std::byte{0xff};
```

Useful for generic buffers and object-representation operations. It is not a universal replacement for `uint8_t` or `unsigned char`, particularly when numeric arithmetic or C API interoperability is required.

### `std::exchange`

Replace a value and return its previous value.

```cpp
auto old = std::exchange(value, replacement);
```

Useful for implementing move operations, resetting state, and transferring ownership of handles.

### `std::to_address` (C++20)

Obtain a raw address from a pointer-like object, including supported fancy pointer types.

Useful when writing allocator-aware or low-level generic code. Prefer ordinary `get()` or `data()` when those are the appropriate interfaces.

## 3. Containers and data structures

### `std::array`

A fixed-size, contiguous container whose size is part of its type.

```cpp
std::array<int, 3> values{1, 2, 3};
```

Unlike built-in arrays, it supports standard container operations and integrates directly with iterators and algorithms.

### Unordered containers

`std::unordered_map`, `std::unordered_set`, and their multi-key variants use hash tables.

Average-case lookup, insertion, and removal are constant time; worst-case complexity can be linear.

Use ordered containers such as `std::map` when sorted iteration or logarithmic worst-case operations matter. Unordered containers can incur rehashing costs and have less predictable memory-access patterns.

### Tuples and structured bindings

`std::tuple` stores a fixed number of potentially heterogeneous values.

```cpp
auto result = std::make_tuple(42, std::string("hello"));
auto value = std::get<0>(result);
```

`std::tie` assigns tuple elements to existing variables. Structured bindings provide a more convenient alternative:

```cpp
auto [id, name] = result;
```

Use `auto& [key, value]` or `const auto& [key, value]` when references to existing elements are desired.

### `std::optional`

Represent a value that may or may not exist.

```cpp
std::optional<int> find_id();

if (auto id = find_id()) {
    use(*id);
}
```

Use `std::nullopt` for the empty state. `optional` represents absence, not a general error-handling mechanism.

### `std::variant`

Represent one value selected from a fixed set of alternative types.

```cpp
std::variant<int, std::string> value = 42;
value = std::string("hello");

if (auto p = std::get_if<std::string>(&value)) {
    use(*p);
}
```

Prefer it to `void*` and a manually maintained type tag when the alternatives are known. `std::get<T>` throws `std::bad_variant_access` if the active alternative does not match.

### `std::any`

Store a value of an arbitrary copy-constructible type with runtime type information.

```cpp
std::any value = 42;
value = std::string("hello");

auto p = std::any_cast<std::string>(&value);
```

Useful when the type is not known at compile time. It is safer than untyped `void*` storage but does not provide static type safety at the point of retrieval.

### Container erasure (C++20)

Use `std::erase` and `std::erase_if` to remove matching elements without manually implementing the erase-remove idiom.

```cpp
std::vector<int> values{1, 2, 3, 2, 4};

std::erase(values, 2);

std::erase_if(values, [](int n) {
    return n % 2 == 0;
});
```

Works with the supported standard containers, with overloads and behavior depending on the container type.

### `std::flat_map` and `std::flat_set` (C++23)

Ordered associative containers backed by sequence-like storage rather than the node-based structure traditionally used by `std::map` and `std::set`.

Worth considering when compact storage and cache locality matter more than insertion and erasure costs. They are not drop-in performance improvements for every workload.

### `std::mdspan` (C++23)

A non-owning multidimensional view over data.

```cpp
std::mdspan<double, std::dextents<std::size_t, 2>> matrix(
    data, rows, columns);

matrix(2, 3) = 1.0;
```

Useful for matrices, numerical computing, and multidimensional data where the layout and strides may vary. It does not own the underlying storage.

## 4. Algorithms and ranges

### Standard algorithms

Prefer standard algorithms over hand-written loops when they express the operation clearly.

```cpp
std::sort(values.begin(), values.end());

auto it = std::find(values.begin(), values.end(), target);

bool found = std::any_of(values.begin(), values.end(),
                         [](int x) { return x > 100; });
```

Useful facilities include `std::sort`, `std::find`, `std::find_if`, `std::transform`, `std::copy_if`, `std::accumulate`, `std::any_of`, `std::all_of`, and `std::none_of`.

Remember that iterator invalidation, algorithmic complexity, and allocation behavior depend on the container and operation.

### Ranges (C++20)

Ranges provide range-based algorithms and composable lazy views.

```cpp
namespace rv = std::ranges::views;

auto result = values
    | rv::filter([](int n) { return n > 0; })
    | rv::transform([](int n) { return n * 2; });

for (int n : result) {
    use(n);
}
```

Useful views include `filter`, `transform`, `take`, `drop`, `join`, and `reverse`.

**Caveats:** Views often do not own their underlying data. Lazy evaluation can repeat work, and some views require their source to remain alive. Ranges are not automatically faster than ordinary algorithms.

### `std::ssize` (C++20)

Obtain a signed size value, useful when comparing container sizes with signed indices.

```cpp
auto n = std::ssize(values);
```

Prefer avoiding signed/unsigned comparisons where possible; `ssize` does not eliminate every conversion hazard.

## 5. Functions, lambdas, and control flow

### Lambda expressions

Create callable objects directly where they are used.

```cpp
auto twice = [](int x) { return x * 2; };
```

Capture variables by value or reference:

```cpp
int threshold = 10;

auto predicate = [threshold](int x) {
    return x > threshold;
};
```

C++14 added generic lambda parameters and init-capture:

```cpp
auto identity = [](auto x) { return x; };

auto counter = [n = 0]() mutable {
    return ++n;
};
```

Init-capture evaluates the initializer when the closure is created. Reference captures must not outlive the referenced objects.

C++17 added `[*this]` for capturing a copy of the current object.

### `override` and `final`

```cpp
struct Base {
    virtual void run();
};

struct Derived final : Base {
    void run() override;
};
```

`override` verifies that a function actually overrides a virtual base-class function. `final` prevents further overriding or derivation, depending on where it is applied.

### `noexcept`

Declare that a function does not throw exceptions.

```cpp
void release() noexcept;
```

A `noexcept` function that lets an exception escape causes `std::terminate`. Exception specifications also affect function types and generic-library behavior.

### `std::ref` and `std::reference_wrapper`

Wrap a reference so it can be copied as an object.

```cpp
int value = 42;

std::thread t(worker, std::ref(value));
t.join();
```

Useful with APIs that normally copy or decay their arguments. The referenced object must outlive every use of the wrapper.

### `if` and `switch` with initialization (C++17)

Limit a variable's scope to the control statement.

```cpp
if (auto it = map.find(key); it != map.end()) {
    use(it->second);
}
```

Particularly useful for lookup results and status variables.

### `std::function`

A general-purpose, copyable type-erased callable wrapper.

```cpp
std::function<void(int)> callback = [](int n) {
    process(n);
};
```

Useful for runtime-selected callbacks and APIs that need a uniform callable type.

It can incur indirect-call and possibly allocation overhead. Prefer templates or direct callable types when runtime type erasure is unnecessary. Use `std::move_only_function` (C++23) when a type-erased callable must support move-only targets.

### `std::async`

Launch a task and retrieve its result through `std::future`.

```cpp
auto result = std::async(std::launch::async, [] {
    return compute();
});

auto value = result.get();
```

Without an explicit launch policy, the implementation may defer execution. `std::async` is useful for coarse-grained tasks but is not a general-purpose thread pool or asynchronous I/O framework.

## 6. Concurrency and synchronization

### `std::thread`

Create and manage threads using the standard library.

```cpp
std::thread worker([] {
    do_work();
});

worker.join();
```

A joinable `std::thread` must be joined or detached before destruction; otherwise, its destructor calls `std::terminate`.

Prefer explicit joining over detaching unless detached lifetime and shutdown behavior are carefully designed.

### `std::jthread` (C++20)

A joining thread whose destructor requests stop and joins automatically.

```cpp
std::jthread worker([](std::stop_token stop) {
    while (!stop.stop_requested()) {
        do_work();
    }
});
```

`std::jthread` simplifies cleanup and cooperative cancellation, but stop requests do not forcibly interrupt blocking system calls or arbitrary operations. The worker must respond to cancellation.

### Mutexes and locks

- `std::mutex`: Exclusive locking.
- `std::recursive_mutex`: Recursive locking; use only when required by the design.
- `std::shared_mutex` (C++17): Multiple readers or one writer.
- `std::lock_guard`: Simple scoped locking.
- `std::unique_lock`: Flexible locking, unlocking, and deferred acquisition.
- `std::scoped_lock` (C++17): Convenient locking of multiple mutexes.

```cpp
std::scoped_lock lock(mutex_a, mutex_b);
```

Locking multiple mutexes together helps avoid deadlocks caused by inconsistent acquisition order. It does not prevent deadlocks involving other locks or unrelated synchronization protocols.

### Atomics

`std::atomic` and `std::atomic_flag` provide atomic operations and memory-ordering controls.

```cpp
std::atomic<int> counter{0};

counter.fetch_add(1, std::memory_order_relaxed);
```

`memory_order_relaxed` guarantees atomicity but does not establish synchronization for other memory accesses. Choose ordering according to the actual inter-thread protocol, not as a blanket performance optimization.

Atomic operations are not necessarily lock-free; use `is_lock_free()` when that property matters.

### Coordination primitives (C++20)

- `std::latch`: One-shot countdown synchronization.
- `std::barrier`: Repeated phase-based synchronization.
- `std::counting_semaphore`: Limit concurrent access to a resource or represent available permits.
- Atomic wait/notify operations: Block until an atomic value changes, rather than repeatedly polling.

These facilities complement mutexes and condition variables; they do not replace the need to define a correct synchronization protocol.

### Synchronized output (C++20)

`std::osyncstream` buffers output and emits it as a synchronized chunk.

```cpp
std::osyncstream(std::cout) << "worker completed\n";
```

Useful for preventing interleaving between concurrent writers using the appropriate synchronized-output mechanism. It does not make arbitrary shared state thread-safe.

## 7. Strings, time, files, and diagnostics

### `std::chrono`

Provides duration, clock, and time-point types.

```cpp
using namespace std::chrono_literals;

auto timeout = 250ms;
auto start = std::chrono::steady_clock::now();
```

Use `steady_clock` for elapsed-time measurements and deadlines that must not jump with wall-clock adjustments. Use `system_clock` for wall-clock time.

C++20 added calendar and time-zone facilities to `chrono`.

### User-defined literals

Create domain-specific literal syntax.

```cpp
auto timeout = 250ms;
```

The standard library provides duration literals through `std::chrono_literals`.

### Raw string literals

Avoid escaping backslashes and quotes.

```cpp
auto pattern = R"(\d+\.\d+)";
```

Custom delimiters can be used if the content contains the default terminator sequence.

### `std::filesystem` (C++17)

Provides paths, directory iteration, file status, and filesystem operations.

```cpp
namespace fs = std::filesystem;

for (const auto& entry : fs::directory_iterator{"."}) {
    std::cout << entry.path() << '\n';
}
```

Filesystem operations can fail due to permissions, races, missing paths, and other OS-level conditions. Choose between error-code overloads and exceptions according to the error-handling model.

### `std::format` (C++20)

Type-aware formatted output using format strings.

```cpp
auto message = std::format("id={}, name={}", id, name);
```

Useful for readable formatting without stream chaining or C-style format-string type mismatches. Availability and formatting support can vary by standard-library implementation.

### `std::print` and `std::println` (C++23)

Write formatted output directly to a stream or standard output.

```cpp
std::println("id={}, name={}", id, name);
```

Useful for straightforward formatted output. Prefer these over building a temporary formatted string when direct output is all that is needed.

### `std::source_location` (C++20)

Capture source-location information without explicitly passing file names and line numbers.

```cpp
void log(std::string_view message,
         std::source_location loc =
             std::source_location::current()) {
    std::println("{}:{}: {}",
                 loc.file_name(), loc.line(), message);
}
```

Useful for diagnostics, logging, and assertion helpers.

### `std::expected` (C++23)

Represent either a successful value or an error.

```cpp
std::expected<int, Error> parse(std::string_view input);

auto result = parse(input);

if (!result) {
    handle_error(result.error());
} else {
    use(*result);
}
```

Use it when errors are part of the normal return contract and callers should handle them explicitly. Unlike `optional`, it can carry an error value. It does not automatically propagate errors through arbitrary function calls.

### Monadic operations on `optional` and `expected` (C++23)

Use `and_then`, `transform`, and `or_else` to compose operations that may fail or produce absent values.

```cpp
auto result = parse(input)
    .and_then(validate)
    .transform(convert);
```

These operations can reduce nested conditionals in pipelines. Exact applicability depends on the value and error types and the operation's return type.

## 8. Low-level and binary-data utilities

### `std::endian` (C++20)

Inspect the native endianness of the target system.

```cpp
if constexpr (std::endian::native == std::endian::little) {
    // Little-endian native representation
}
```

Useful when implementing binary protocols or interpreting native object representations. Native endianness alone does not define a portable serialization format.

### `std::bit_cast` (C++20)

Copy the object representation of one trivially copyable type into another compatible trivially copyable type of equal size.

```cpp
float f = 1.0f;
auto bits = std::bit_cast<std::uint32_t>(f);
```

Useful for inspecting representations without type-punning through an invalid pointer cast. It does not make the representation portable across architectures or eliminate padding and representation concerns.

### `std::byteswap` (C++23)

Reverse the byte order of an integral value.

```cpp
auto swapped = std::byteswap(value);
```

Useful for byte-order conversions and binary formats. It reverses the bytes; it does not itself determine whether a conversion is needed on the current architecture.

### `std::to_underlying` (C++23)

Obtain an enum's underlying integer value.

```cpp
enum class Status : unsigned char { ok = 0, error = 1 };

auto raw = std::to_underlying(Status::error);
```

Prefer this over a handwritten cast when converting an enum to its underlying type.

## 9. Coroutines and modules

### Coroutines (C++20)

C++20 introduced language support for coroutines using `co_await`, `co_yield`, and `co_return`.

Coroutines can suspend and resume execution without requiring a dedicated operating-system thread for each suspended operation.

Typical uses include asynchronous task frameworks, generators, and event-driven systems. The language provides the coroutine machinery, but it does not prescribe a universal task scheduler, executor, or asynchronous I/O runtime.

### Modules (C++20)

Modules provide a language-level mechanism for exporting declarations and importing them elsewhere.

```cpp
import std; // Available only with suitable C++23 library support
```

Modules can reduce repeated parsing and improve encapsulation compared with textual inclusion. Toolchain support, build-system integration, and interoperability with existing header-heavy libraries remain practical considerations.

## 10. Quick reference: C++20 and C++23 additions

| Facility | Standard | Main use |
| --- | --- | --- |
| `std::span` | C++20 | Non-owning contiguous view |
| Ranges and views | C++20 | Composable algorithms and lazy pipelines |
| `std::jthread` and stop tokens | C++20 | Joining threads and cooperative cancellation |
| `std::latch`, `std::barrier`, semaphores | C++20 | Thread coordination |
| `std::format` | C++20 | Type-aware formatting |
| `std::source_location` | C++20 | Diagnostics and logging |
| `std::endian`, `std::bit_cast` | C++20 | Low-level representation handling |
| Calendar and time-zone support | C++20 | Civil time and time zones |
| Coroutines and modules | C++20 | Suspension-based execution and modular compilation |
| `std::expected` | C++23 | Value-or-error return types |
| `std::print`, `std::println` | C++23 | Formatted output |
| `std::mdspan` | C++23 | Multidimensional non-owning views |
| `std::flat_map`, `std::flat_set` | C++23 | Ordered associative containers with sequence-like storage |
| `std::move_only_function` | C++23 | Type-erased move-only callables |
| `std::byteswap`, `std::to_underlying` | C++23 | Byte-order and enum utilities |

**Compatibility note:** Standard-version availability does not guarantee complete support in every compiler and standard library. Check the target toolchain, especially for C++20 formatting, ranges, coroutines, modules, and newer C++23 facilities.
