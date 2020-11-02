# Channel

**Not tested in production**

Thread-safe container for sharing data between threads.

Use stream operators to push (>>) and fetch (<<) items. Range-based for loop supported.

Tested with GCC and Clang. 

## Requirements
* C++11/14/17
* CMake 3.12+

## Installation
See [CMakeLists.txt](./examples/cmake-project/CMakeLists.txt) from the [CMake project example](./examples/cmake-project).

## Usage

```c++
#include <cassert>

#include "channel.hpp"

int main() {
    Channel<int> channel; // unbuffered

    int in = 1;
    int out = 0;

    // Send to channel
    in >> channel;

    // Read from channel
    out << channel;

    assert(out == 1);
}
```

```c++
#include "channel.hpp"

int main() {
    Channel<int> channel{2}; // buffered

    int in = 1;

    // Send to channel
    in >> channel;
    in >> channel;
    in >> channel; // blocking because capacity is 2 (and no one reads from channel)
}
```

```c++
#include "channel.hpp"

int main() {
    Channel<int> channel{2}; // buffered

    int in = 1;
    int out = 0;

    // Send to channel
    in >> channel;
    in >> channel;

    // Read from channel
    out << channel;
    out << channel;
    out << channel; // blocking because channel is empty (and no one writes on it)
}
```

```c++
#include <iostream>

#include "channel.hpp"

int main() {
    Channel<int> channel;

    int in1 = 1;
    in1 >> channel;

    int in2 = 2;
    in2 >> channel;

    for (const auto out : channel) {  // blocking: forever waiting for channel items
        std::cout << out << '\n';
    }
}
```

See [examples](examples).

<br>

Developed with [CLion](https://www.jetbrains.com/?from=serializer)

<a href="https://www.jetbrains.com/?from=serializer">![JetBrains](jetbrains.svg)</a>