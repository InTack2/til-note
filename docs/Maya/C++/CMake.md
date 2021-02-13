# CMakeによるプラグインビルド環境構築
## 概要
C++はWindowsやLinux、MacなどOSごとにコンパイルする必要がある。  
MayaだとWindowsはVS、Linuxはgccなどになります。  
MayaのpluginもそれぞれのOSに合わせてビルドが必要になる為、その都度環境を作成して行くのは大変な為、CMakeを活用します。  


## 方針(2021/02/13 追記)
基本的に自前でcmakeファイルを用意して行うよりも、  
対応する「devkit」を利用した方が一貫性がありおすすめなので、内容を変更しました。


## 手順

### Mayaの対応するdevkitをダウンロードする
[こちらからダウンロード](https://www.autodesk.com/developer-network/platform-technologies/maya?_ga=2.22332605.720544780.1613183323-906704681.1609486821)

下記の様にわかりやすい所に配置します。  
[![Image from Gyazo](https://i.gyazo.com/0493a0dd6e526a31ce92bd6155454da2.png)](https://gyazo.com/0493a0dd6e526a31ce92bd6155454da2)

### 配置した場所への環境変数を設定する

下記の様に配置した場所へのパスを「DEVKIT_LOCATION」として設定します。  
[![Image from Gyazo](https://i.gyazo.com/f953bbfeddc4f75752e12cd34b85ac94.png)](https://gyazo.com/f953bbfeddc4f75752e12cd34b85ac94)


## 開発時
あとは実際に開発する際に下記の様な「CMakeLists.txt」を用意して完了です。
``` CMakeLists.txt
cmake_minimum_required(VERSION 2.8)

# 先ほど追加したDEVKIT_LOCATION内のplugin用cmakeファイルを利用する。
include($ENV{DEVKIT_LOCATION}/cmake/pluginEntry.cmake)

set(PROJECT_NAME "プロジェクト名")

# CMakeに含めるファイル指定
file(GLOB SOURCE_FILES
   ${CMAKE_SOURCE_DIR}/src/*.h
   ${CMAKE_SOURCE_DIR}/src/*.hpp
   ${CMAKE_SOURCE_DIR}/src/*.cpp
   )

# ライブラリを指定
set(LIBRARIES
     OpenMaya
     Foundation

)

set_property( DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})

# Build plugin
build_plugin()
set_target_properties(${PROJECT_NAME} PROPERTIES VS_DEBUGGER_COMMAND "maya.exe")
set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "/export:initializePlugin /export:uninitializePlugin")
set(CMAKE_CXX_FLAGS_DEBUG "/ZI")

```

詳しいファイル構成などは下記をご参照ください。  
<a href="https://github.com/InTack2/maya-aim-constraint-node"><img src="https://github-link-card.s3.ap-northeast-1.amazonaws.com/InTack2/maya-aim-constraint-node.png" width="460px"></a>




## 以下以前自作していたCMakeファイル
既に利用していませんが、一応残しておきます  

## ファイル構成

ルート  
└build  
└modules  
　└FindMaya.cmake  
└CMakeLists.txt(1個目)  
└src  
　└helloWorldCmd.cpp  
　└CMakeLists.txt(2個目)  

### FindMaya.cmake
Maya用にCMakeモジュールを作成する。  
Mayaバージョンの指定、OSごとに拡張子の変更、読み込む内容の変更の分岐を記載する。  
のちほどコメント追加予定。  

!!! example "FindMaya.cmake"
    ```C
    # Set a default Maya version if not specified
    if(NOT DEFINED MAYA_VERSION)
        set(MAYA_VERSION 2020 CACHE STRING "Maya version")
    endif()

    # OS Specific environment setup
    set(MAYA_COMPILE_DEFINITIONS "REQUIRE_IOSTREAM;_BOOL")
    set(MAYA_INSTALL_BASE_SUFFIX "")
    set(MAYA_TARGET_TYPE LIBRARY)
    if(WIN32)
        # Windows
        set(MAYA_INSTALL_BASE_DEFAULT "C:/Program Files/Autodesk")
        set(MAYA_COMPILE_DEFINITIONS "${MAYA_COMPILE_DEFINITIONS};NT_PLUGIN")
        set(MAYA_PLUGIN_EXTENSION ".mll")
        set(MAYA_TARGET_TYPE RUNTIME)
    elseif(APPLE)
        # Apple
        set(MAYA_INSTALL_BASE_DEFAULT /Applications/Autodesk)
        set(MAYA_COMPILE_DEFINITIONS "${MAYA_COMPILE_DEFINITIONS};OSMac_")
        set(MAYA_PLUGIN_EXTENSION ".bundle")
    else()
        # Linux
        set(MAYA_COMPILE_DEFINITIONS "${MAYA_COMPILE_DEFINITIONS};LINUX")
        set(MAYA_INSTALL_BASE_DEFAULT /usr/autodesk)
        if(MAYA_VERSION LESS 2016)
            # Pre Maya 2016 on Linux
            set(MAYA_INSTALL_BASE_SUFFIX -x64)
        endif()
        set(MAYA_PLUGIN_EXTENSION ".so")
    endif()

    set(MAYA_INSTALL_BASE_PATH ${MAYA_INSTALL_BASE_DEFAULT} CACHE STRING
        "Root path containing your maya installations, e.g. /usr/autodesk or /Applications/Autodesk/")

    set(MAYA_LOCATION ${MAYA_INSTALL_BASE_PATH}/maya${MAYA_VERSION}${MAYA_INSTALL_BASE_SUFFIX})

    # Maya include directory
    find_path(MAYA_INCLUDE_DIR maya/MFn.h
        PATHS
            ${MAYA_LOCATION}
            $ENV{MAYA_LOCATION}
        PATH_SUFFIXES
            "include/"
            "devkit/include/"
    )

    find_library(MAYA_LIBRARY
        NAMES 
            OpenMaya
        PATHS
            ${MAYA_LOCATION}
            $ENV{MAYA_LOCATION}
        PATH_SUFFIXES
            "lib/"
            "Maya.app/Contents/MacOS/"
        NO_DEFAULT_PATH
    )
    set(MAYA_LIBRARIES "${MAYA_LIBRARY}")

    include(FindPackageHandleStandardArgs)
    find_package_handle_standard_args(Maya
        REQUIRED_VARS MAYA_INCLUDE_DIR MAYA_LIBRARY)
    mark_as_advanced(MAYA_INCLUDE_DIR MAYA_LIBRARY)

    if (NOT TARGET Maya::Maya)
        add_library(Maya::Maya UNKNOWN IMPORTED)
        set_target_properties(Maya::Maya PROPERTIES
            INTERFACE_COMPILE_DEFINITIONS "${MAYA_COMPILE_DEFINITIONS}"
            INTERFACE_INCLUDE_DIRECTORIES "${MAYA_INCLUDE_DIR}"
            IMPORTED_LOCATION "${MAYA_LIBRARY}")
        
        if (APPLE AND ${CMAKE_CXX_COMPILER_ID} MATCHES "Clang" AND MAYA_VERSION LESS 2017)
            # Clang and Maya 2016 and older needs to use libstdc++
            set_target_properties(Maya::Maya PROPERTIES
                INTERFACE_COMPILE_OPTIONS "-std=c++0x;-stdlib=libstdc++")
        endif ()
    endif()

    # Add the other Maya libraries into the main Maya::Maya library
    set(_MAYA_LIBRARIES OpenMayaAnim OpenMayaFX OpenMayaRender OpenMayaUI Foundation clew)
    foreach(MAYA_LIB ${_MAYA_LIBRARIES})
        find_library(MAYA_${MAYA_LIB}_LIBRARY
            NAMES 
                ${MAYA_LIB}
            PATHS
                ${MAYA_LOCATION}
                $ENV{MAYA_LOCATION}
            PATH_SUFFIXES
                "lib/"
                "Maya.app/Contents/MacOS/"
            NO_DEFAULT_PATH)
        mark_as_advanced(MAYA_${MAYA_LIB}_LIBRARY)
        if (MAYA_${MAYA_LIB}_LIBRARY)
            add_library(Maya::${MAYA_LIB} UNKNOWN IMPORTED)
            set_target_properties(Maya::${MAYA_LIB} PROPERTIES
                IMPORTED_LOCATION "${MAYA_${MAYA_LIB}_LIBRARY}")
            set_property(TARGET Maya::Maya APPEND PROPERTY
                INTERFACE_LINK_LIBRARIES Maya::${MAYA_LIB})
            set(MAYA_LIBRARIES ${MAYA_LIBRARIES} "${MAYA_${MAYA_LIB}_LIBRARY}")
        endif()
    endforeach()

    function(MAYA_PLUGIN _target)
        if (WIN32)
            set_target_properties(${_target} PROPERTIES
                LINK_FLAGS "/export:initializePlugin /export:uninitializePlugin")
        endif()
        set_target_properties(${_target} PROPERTIES
            PREFIX ""
            SUFFIX ${MAYA_PLUGIN_EXTENSION})
    endfunction()
    ```

### CMakeLists.txt(1個目)

!!! example "CMakeLists.txt"
    ```c
    cmake_minimum_required(VERSION 2.8)
    # specify project name
    set(PROJECT_NAME helloWorldCmd)

    set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/modules)
    set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
    add_subdirectory(src)
    ```

### CMakeLists.txt(2個目)

!!! example "CMakeLists.txt"
    ```c
    # set SOURCE_FILES
    set(SOURCE_FILES
    helloWorldCmd.cpp
    )
    
    find_package(Maya REQUIRED)

    include_directories(${MAYA_INCLUDE_DIR})
    link_directories(${MAYA_LIBRARIES})

    add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES})
    target_link_libraries(${PROJECT_NAME} ${MAYA_LIBRARIES})

    set_target_properties(${PROJECT_NAME} PROPERTIES
        VS_DEBUGGER_COMMAND "maya.exe")

    MAYA_PLUGIN(${PROJECT_NAME})
    ```

## CMakeコマンド
### Visual Studio2019の構成を作成
```bash
cd build
cmake -G "Visual Studio 16 2019" -DMAYA_VERSION=2020 ../
```

### プラグインをDebugビルドする
```bash
cd build
cmake --build . --config Debug
```

### プラグインをReleaseビルドする
```bash
cd build
cmake --build . --config Release
```

## 参考
[CMake: モジュール](https://qiita.com/mrk_21/items/ab32a83a12f5d37acc64)  
[cmake チートシート](https://www.qoosky.io/techs/814fda555d)  