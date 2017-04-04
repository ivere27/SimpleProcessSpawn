# SimpleProcessSpawn

* c++11 process spawn based on libuv(uv_spawn wrapper)
* a single header file


## Usage
```cpp
#include <iostream>
#include "SimpleProcessSpawn.hpp"

using namespace std;
using namespace spawn;

int main() {
  uv_loop_t* uv_loop = uv_default_loop();
  char* args[2];
  args[0] = (char*)"./child";
  args[1] = NULL;

  SimpleProcessSpawn process(uv_loop, args);
  process.on("error", [](Error &&error){
    cout << error.name << endl;
    cout << error.message << endl;
  })
  .on("response", [](Response &&response){
    cout << "exit code : " << response.exitStatus << endl;
    cout << "signal : " << response.termSignal << endl;
    cout << response.stdout.str();
    cout << response.stderr.str();
  })
  .spawn();

  return uv_run(uv_loop, UV_RUN_DEFAULT);
}
```

## build or $ make
```bash
g++ --std=c++11 child.cpp -o child && ./child 1000
g++ example.cpp --std=c++11 -I./libuv/include/ ./libuv/.libs/libuv.a -lpthread -o example && ./example
```

## about
* `This is experimental yet. use at your own purpose!`
* a project of 'Second Compiler'.

# License

MIT