#MCU-FreeRTOS-Frame

使用CLion、Keil和STM32CubeMX构建项目。

CLion配置依据稚晖君的教程 [优雅の嵌入式开发](https://zhuanlan.zhihu.com/p/145801160)

使用FreeRTOS注意事项：

1. 项目中`your_peoject/Middlewares/Third_Party/FreeRTOS/Source/portable/RVDS/ARM_CM4F`中的`prot.c`，`portmacro.h`会编译出错，例如各种类型名未被定义、找不到`uint32_t`等等。

   解决方案：

   1. 从网上下载你所使用的FreeRTOS版本，在类似路径下找到`your_freertos_download_path/FreeRTOS-Kernel-10.2.1/FreeRTOS/Source/portable/GCC/ARM_CM4F`，将这个目录下的`port.c`和`portmacro.h`复制覆盖`your_peoject/Middlewares/Third_Party/FreeRTOS/Source/RVDS/ARM_CM4F`下的文件。
   2. 在`CubeMX`中找到项目使用的`Default Diretory`，从那里面的类似目录下找到`GCC`下的`ARM_CM4F`，复制粘贴到`RVDS`的目录中

2. 将CMakeLists中的

   ```cmake
   SET(FPU_FLAGS "-mfloat-abi=hard -mfpu=fpv4-sp-d16")
   add_definitions(-DARM_MATH_CM4 -DARM_MATH_MATRIX_CHECK -DARM_MATH_ROUNDING)
   add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
   add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
   add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
   ```

   取消注释，若没有可以加上去。

通过步骤1和2就能在`CLion`编译成功了！

3. 若使用keil编译，将设置中的`ARM Compiler`设置为`Use default compiler version 6`。

   通过步骤3可以在`Keil`中编译成功。

