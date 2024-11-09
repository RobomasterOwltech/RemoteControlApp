# syntax=docker/dockerfile:1.3-labs
# ===== ===== ===== ===== ===== =====
# This image is for compiling STM32 HAL Projects
# My reference from: https://www.youtube.com/watch?v=imUiQkO9YHM
# Since this images uses heredocs, you need buildkit
# ===== ===== ===== ===== ===== =====

# ===== =====  Base image  ===== =====
FROM ubuntu:22.04

# ===== =====  Arguments  ===== =====
# Download GNU ARM
ARG compiler_dwn_link=https://developer.arm.com/-/media/Files/downloads/gnu/13.2.rel1/binrel/arm-gnu-toolchain-13.2.rel1-x86_64-arm-none-eabi.tar.xz?rev=e434b9ea4afc4ed7998329566b764309&hash=CA590209F5774EE1C96E6450E14A3E26
# Host project location 
#ARG wDirHost
    # You actually can't have a variable for the mounting point
# Tag versioning
    # See https://docs.docker.com/reference/dockerfile/#arg
ARG FreeRTOS_tag
ARG STM32F1_tag
ARG STM32F3_tag
ARG STM32F4_tag
ARG STM32H7_tag
ARG USB_tag
ARG eigen_ver

ENV FreeRTOS_tag=${FreeRTOS_tag:-v11.1.0}
ENV STM32F1_tag=${STM32F1_tag:-v1.8.5}
ENV STM32F3_tag=${STM32F3_tag:-v1.11.5}
ENV STM32F4_tag=${STM32F4_tag:-v1.28.0}
ENV STM32H7_tag=${STM32H7_tag:-v1.11.0}
ENV USB_tag=${USB_tag:-v2.11.0}
ENV eigen_ver=${eigen_ver:-3.4.0}

# ===== ===== ===== ===== ===== =====
# TODO: 
#   * Define version for cmake, make, compiler, repos
#   * Extract libraries to somewhere
# ===== ===== Actual image ===== =====
# ===== Update repos
    # As a multi-line script
RUN apt update && \
    apt upgrade -y && \
    apt install -y cmake make xz-utils git clang-format && \
    mkdir -p /opt/gcc-arm-none-eabi/ /tmp/gcc-arm-none-eabi/


# ===== Install compiler

ADD "$compiler_dwn_link" /tmp/compiler.tar.xz

RUN tar xf /tmp/compiler.tar.xz -C /tmp/gcc-arm-none-eabi/ && \
    mv /tmp/gcc-arm-none-eabi/arm-gnu-toolchain-*/* /opt/gcc-arm-none-eabi/ && \
    rm -rf /tmp/*

# Symlink to /usr/local/bin
    # TODO: Why?
RUN ln -s /opt/gcc-arm-none-eabi/bin/* /usr/local/bin

# Add compiler to the path, note how we don't use export to set it globally
ENV PATH="$PATH:/opt/gcc-arm-none-eabi/bin" 

# ===== STM32 dependencies
WORKDIR /opt
# Download CMSIS V2 FreeRTOS
    # I think this not entirely necessary
# Currently supported chips: 
    # Since these are the Firmware package, it includes HAL drivers and CMSIS MCU config
    # STM32F4 processor series  
    # Release notes: https://htmlpreview.github.io/?https://github.com/STMicroelectronics/STM32CubeF4/blob/master/Release_Notes.html
    # STM32H7 processor series
    # Release notes: https://htmlpreview.github.io/?https://github.com/STMicroelectronics/STM32CubeH7/blob/master/Release_Notes.html
    # STM32F1 processor series
    # STM32F3 processor series
RUN git clone --depth 1 --branch $FreeRTOS_tag https://github.com/ARM-software/CMSIS-FreeRTOS.git && \
    git clone --recursive --depth 1 --branch $STM32F4_tag https://github.com/STMicroelectronics/STM32CubeF4.git && \
    git clone --recursive --depth 1 --branch $STM32H7_tag https://github.com/STMicroelectronics/STM32CubeH7.git && \
    git clone --recursive --depth 1 --branch $STM32F1_tag https://github.com/STMicroelectronics/STM32CubeF1.git && \
    git clone --recursive --depth 1 --branch $STM32F3_tag https://github.com/STMicroelectronics/STM32CubeF3.git && \
    git clone --depth 1 --branch $USB_tag https://github.com/STMicroelectronics/stm32_mw_usb_device.git && \
    git clone --branch  $eigen_ver https://gitlab.com/libeigen/eigen.git


# ===== ===== Build Project ===== =====

# Change working dir to project location in container
WORKDIR /home/myProjects

# Keep the container running
ENTRYPOINT [ "tail", "-f", "/dev/null" ]
