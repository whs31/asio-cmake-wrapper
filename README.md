# ASIO CMake Wrapper
#### Утилита для использования библиотеки ASIO в CMake-проектах.
![CMake](https://img.shields.io/badge/CMake-%23008FBA.svg?style=for-the-badge&logo=cmake&logoColor=white)
- Предоставляет `Standalone`, а не `Boost` версию ASIO
- Корректно линкует библиотеку с `Threads`, включая новые версии Windows

#### Использование
###### Добавить сабмодуль:
```bash
git submodule add {ссылка_на_сабмодуль} libs/asio
git submodule update --init --recursive
```
###### Регистрация в CMake:
```cmake
add_subdirectory(libs/asio)
target_link_libraries(${PROJECT_NAME} 
  PRIVATE asio
)
```
###### Использование в C++:
```cpp
#include <asio/ip/udp.hpp>

using namespace asio::ip;

udp::socket socket;
udp::endpoint endpoint;
/* ... */
```