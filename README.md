# AppCodeTemp
Template to create FreeRTOS tasks using the team's middleware

-------------------------------------------------------------------

## How to use this repo:

1. Clone it `git clone git@github.com:RobomasterOwltech/AppCodeRepo.git`
1. Download the submodules `git submodule init && git submodule update`
1. In case any of the dependencies needs to be updated, simply do: `git submodule update --remote`

## Having problems with git?:

* Make sure that `postStartCommand` value is set at the `.devcontainer/devcontainer.json`.
* Make sure to have created and register ssh keys for Github, as done in [DeployTools](https://github.com/RobomasterOwltech/deployTools)

## How to change DevBoard

1. At the moment you clone the repo, modify the branch for the `robotConfig` repo.
1. Change the OwlDefines variables at `.vscode/c_cpp_properties.json`

## Compile:

1. From the project folder call make 

        make
    1. You might need to specify the compiler path, in which case use:
        
            make GCC_PATH=/path/2/compiler
    
    1. Alternatively, you can add it to your path, or set it as an environment variable. 
1. If your build failed, then you can clean the `build` folder
        
        make clean

### Add a file to be compiled 

1. Save your changes to the file.
1. Copy it's relative path.
1. If it is a C, add it in the `C_SOURCES` variable from the `Makefile` 
1. In case your file is referencing any library, make sure it is present in the `C_INCLUDES`

arm-none-eabi-gcc -c -mcpu=cortex-m4 -mthumb -mfpu=fpv4-sp-d16 -mfloat-abi=hard -DUSE_HAL_DRIVER -DSTM32F407xx -IrobotConfig/inc -Ieigen/Eigen -IDrivers/STM32F4xx_HAL_Driver/Inc -IDrivers/STM32F4xx_HAL_Driver/Inc/Legacy -IDrivers/CMSIS/Device/ST/STM32F4xx/Include -IDrivers/CMSIS/Include -IMiddlewares/Third_Party/FreeRTOS/Source/include -IMiddlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS -IMiddlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F -IMiddlewares/ST/stm32_mw_usb_device/Class/CDC/Inc -IMiddlewares/ST/stm32_mw_usb_device/Core/Inc -IowltechMiddleware/inc -Og -Wall -fdata-sections -ffunction-sections -g -gdwarf-2 -MMD -MP -MF"build/main.d" -Wa,-a,-ad,-alms=build/main.lst robotConfig/src/main.cpp -o build/main.o

## NOTES:

### C/C++ standard
If you were to change the C/C++ standard used, remember to change it in the `IntelliSense`, `makefile`, and `c_cpp_properties.json`

For more [formatting options](https://clang.llvm.org/docs/ClangFormatStyleOptions.html) see the link.
### Make for other STM32 boards:

TODO: 
* Change the configuration on `c_cpp_properties.json`
* Change the defines on the `Makefile`
* Edit owlware to automatically include the correct library on each module

### Running for compile

Currently, we're using `make` as our compilation tool. 

By default, make will build whats specified at first, this is the defacto target.

    make

If you wish to compile any other target, then do:

    make targetName

The syntax is like:

```
targetName: dependency1
    compiler -flags [args...]
```

*Note:* Targets may also be defined as dependencies, thus calling a compilation chain

**FYI:** Make can also execute bash commands, so that's why you'll sometimes see commands being used in the `Makefile`. 

In fact the syntax, for declaring, using, and even storing [variables](https://www.gnu.org/savannah-checkouts/gnu/make/manual/make.html#Reading) is the same!


## Resources

If you would like to learn more about the used tools and information, then check:

* [Quick intro to Makefiles](https://www.youtube.com/watch?v=aw9wHbFTnAQ) (Video)
* [GNU Make manual](https://www.gnu.org/savannah-checkouts/gnu/make/manual/make.html#Reading)
* The GNU Make [Book](https://learning.oreilly.com/library/view/the-gnu-make/9781457189883/). John Graham-Cumming. No Starch Press
* [Example on using Eigen](https://youtu.be/RgKreY1HK_8?si=aPSwUOyWYGdj3Nyl&t=284)
