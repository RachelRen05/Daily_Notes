project address:[https://github.com/ros2/rmw_dps/tree/qos](https://github.com/ros2/rmw_dps/tree/qos)

* issue1
```bash
In file included from inc/cpp/dps/QoS.hpp:22:0,
                 from inc/cpp/dps/Node.hpp:26,
                 from src/Node.cpp:20:
inc/cpp/dps/CborStream.hpp:102:3: error: identifier 'noexcept' is a keyword in C++11 [-Werror=c++0x-compat]
   const uint8_t * data() const noexcept { return buffer_.base; }
   ^
In file included from /usr/include/c++/5/mutex:35:0,
                 from inc/cpp/dps/Publisher.hpp:20,
                 from src/Node.cpp:21:
/usr/include/c++/5/bits/c++0x_warning.h:32:2: error: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
 #error This file requires compiler and library support \
  ^
In file included from src/Node.cpp:21:0:
inc/cpp/dps/Publisher.hpp:48:3: error: identifier 'nullptr' is a keyword in C++11 [-Werror=c++0x-compat]
   virtual DPS_Status publish(TxStream && payload, PublicationInfo * info = nullptr);
   ^
```

* issue2
```bash
--- stderr: dps_for_iot_cmake_module                                                                                                                                                  
In file included from inc/dps/dps.h:39:0,
                 from inc/dps/private/dps.h:31,
                 from src/bitvec.h:33,
                 from src/bitvec.c:28:
inc/dps/uuid.h:60:1: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 DPS_Status DPS_InitUUID();
 ^
In file included from inc/dps/private/dps.h:31:0,
                 from src/bitvec.h:33,
                 from src/bitvec.c:28:
inc/dps/dps.h:73:1: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 DPS_NodeAddress* DPS_CreateAddress();
 ^
inc/dps/dps.h:369:1: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 DPS_MemoryKeyStore* DPS_CreateMemoryKeyStore();
 ^
In file included from src/bitvec.h:33:0,
                 from src/bitvec.c:28:
inc/dps/private/dps.h:240:1: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 uint32_t DPS_Rand();
 ^
```
* issue3
```bash
--- stderr: dps_for_iot_cmake_module                                                                                                                                              
src/cbor.c: In function 'CBOR_DecodeFloat':
src/cbor.c:647:34: error: comparison between signed and unsigned integer expressions [-Werror=sign-compare]
                 if ((uint64_t)*f != i64) {
                                  ^
src/cbor.c: In function 'CBOR_DecodeDouble':
src/cbor.c:707:34: error: comparison between signed and unsigned integer expressions [-Werror=sign-compare]
                 if ((uint64_t)*d != i64) {
                                  ^
```
* issue4
```bash
--- stderr: dps_for_iot_cmake_module
src/uuid.c:70:1: error: missing initializer for field 'mutex' of 'struct <anonymous>' [-Werror=missing-field-initializers]
 } context = { UV_ONCE_INIT, DPS_OK };
 ^
src/uuid.c:69:16: note: 'mutex' declared here
     uv_mutex_t mutex;
                ^
src/uuid.c:94:13: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 static void InitUUID()
             ^
src/uuid.c:171:10: error: function declaration isn't a prototype [-Werror=strict-prototypes]
 uint32_t DPS_Rand()
          ^
cc1: all warnings being treated as errors
```

* issue5
```bash
--- stderr: dps_for_iot_cmake_module
In file included from inc/cpp/dps/QoS.hpp:22:0,
                 from inc/cpp/dps/Node.hpp:26,
                 from src/Node.cpp:20:
inc/cpp/dps/CborStream.hpp:102:3: error: identifier 'noexcept' is a keyword in C++11 [-Werror=c++0x-compat]
   const uint8_t * data() const noexcept { return buffer_.base; }
   ^
In file included from /usr/include/c++/5/mutex:35:0,
                 from inc/cpp/dps/Publisher.hpp:20,
                 from src/Node.cpp:21:
/usr/include/c++/5/bits/c++0x_warning.h:32:2: error: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
 #error This file requires compiler and library support \
  ^
```
