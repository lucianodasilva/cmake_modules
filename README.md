# cmake_modules
Simple CMake scripts to automatically load folders as static lib "modules".

## Usage

Include the modules.cmake file from the CMakeList.txt where you wish to use the "modules" from:

```cmake
include (<path to files>/modules.cmake)
```

Declare each of the available "modules". Be mindful of dependencies between "modules" and sort their definitions acordingly.

```cmake
add_<module type>_module ( < module name > < module path > [ module dependencies ... ] )
```

In most "add module" methods, the script will search for two folders within the specified path, one for source files (src) and another for include files (include). Also if any CMakeList.txt file is found in the defined path it will be included.
The added target will share its name with the defined "module" name, therefore, cmake funcions that depend on a target name will also work if you pass them the "module" name as a parameter.

The available "module" type functions are as follows:

 - **add_static_module** - creates a static library.
 - **add_shared_module** - creates a shared library.
 - **add_executable_module** - creates an executable.
 - **add_custom_module** - creates seaches for a CMakeList.txt file in the determined folder and executes it. This method will not add any cmake target by itself.

ex:
 ```cmake
add_static_module (core_module ${base_path}/modules/core)
add_static_module (mod1 ${base_path}/modules/mod1 core)
add_executable_module (exe ${base_path}/modules/exe core mod1)
 ```

To be able to add further build target dependencies to the added "module", you can use the **add_module_dependencies** method to add further dependencies to the "module" target.
ex:
```cmake
if (MSVC)
	list(APPEND core_dependencies libClang libSomeOtherImportantLib)
else ()
	list (APPEND core_dependencies clang SomeOtherImportantLib)  
endif()

add_module_dependencies (core ${core_dependencies})
```