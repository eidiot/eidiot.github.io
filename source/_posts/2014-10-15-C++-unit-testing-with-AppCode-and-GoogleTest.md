title: C++ unit testing with AppCode and GoogleTest
date: 2014-10-15 21:35:14
tags:
---
AppCode started to support [Google Test](https://code.google.com/p/googletest/) [since 2.5 EAP](http://blog.jetbrains.com/objc/2013/09/appcode-2-5-eap-opens-with-cocoapods-and-google-test/). Steps to get it work:

0. [Get Google Test](https://code.google.com/p/googletest/downloads/list)
0. Open `{gtest_path}/xcode/gtest.xcodeproj` and build `gtest.framework`
0. Create a "Command Line Tool" project or target
0. Add `gtest.framework` to project and the new target
0. Set Up the Executable Run Environment. 3 options:
  * [Setting the "DYLD_FRAMEWORK_PATH" environment variable](https://code.google.com/p/googletest/wiki/XcodeGuide#Set_Up_the_Executable_Run_Environment)
  * [Use static libs instead of framework](http://dennycd.me/google-test-xcode-mac-osx/)
  * I just copy `gtest.framework` to `/Library/Frameworks/`
0. Make sure your project and google test use the same C++ standard library
<!-- more -->
  * If they are different you can [make google test to use libc++](http://dennycd.me/google-test-xcode-mac-osx/)
  * I change my test target to use the same one as google test instead. (libstdc++) ![C++ Standard Library](lib.png)
0. In AppCode, `Run/Edit Configurations`, Add new Google Test Configurations ![Add Run Configuration](run.png)
0. Set target to the created "Command Line Tool"
0. Run google test from main.cpp
```c++
#include <gtest/gtest.h>
int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```  
Add your tests and happy testing!
![Google Test Support in AppCode](googleTest.png)
