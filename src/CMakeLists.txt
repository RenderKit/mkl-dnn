#===============================================================================
# Copyright 2016-2018 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#===============================================================================

set(TARGET_NAME ${LIB_NAME})

file(GLOB_RECURSE HEADERS
    ${CMAKE_CURRENT_SOURCE_DIR}/../include/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/../include/*.hpp
    )
#file(GLOB_RECURSE SOURCES
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.c
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.h
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp
#    )
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/common
    ${CMAKE_CURRENT_SOURCE_DIR}/cpu/xbyak
    )

# Add only files that are actually used
file(GLOB_RECURSE SOURCES
    common/*.cpp
    common/*.hpp
    cpu/cpu_barrier.cpp
    cpu/cpu_barrier.hpp
    cpu/cpu_batch_normalization_pd.hpp
    #cpu/cpu_batch_normalization_utils.cpp
    #cpu/cpu_batch_normalization_utils.hpp
    cpu/cpu_concat.cpp
    cpu/cpu_concat.hpp
    cpu/cpu_convolution_pd.hpp
    cpu/cpu_deconvolution_pd.hpp
    cpu/cpu_eltwise_pd.hpp
    cpu/cpu_engine.cpp
    cpu/cpu_engine.hpp
    cpu/cpu_inner_product_pd.hpp
    cpu/cpu_isa_traits.hpp
    cpu/cpu_lrn_pd.hpp
    cpu/cpu_memory.cpp
    cpu/cpu_memory.hpp
    cpu/cpu_pooling_pd.hpp
    cpu/cpu_primitive.hpp
    cpu/cpu_reducer.cpp
    cpu/cpu_reducer.hpp
    cpu/cpu_reorder.cpp
    cpu/cpu_reorder_pd.hpp
    cpu/cpu_rnn_pd.hpp
    cpu/cpu_shuffle_pd.hpp
    cpu/cpu_softmax_pd.hpp
    cpu/cpu_sum.cpp
    cpu/cpu_sum.hpp
    #cpu/gemm_convolution.cpp
    #cpu/gemm_convolution.hpp
    #cpu/gemm_convolution_utils.cpp
    #cpu/gemm_convolution_utils.hpp
    #cpu/gemm/gemm.cpp
    #cpu/gemm/gemm.hpp
    #cpu/gemm/gemm_utils.cpp
    #cpu/gemm/gemm_utils.hpp
    #cpu/gemm_inner_product.cpp
    #cpu/gemm_inner_product.hpp
    #cpu/gemm/jit_avx512_common_gemm_f32.cpp
    #cpu/gemm/jit_avx512_common_gemm_f32.hpp
    #cpu/gemm/jit_avx_gemm_f32.cpp
    #cpu/gemm/jit_avx_gemm_f32.hpp
    #cpu/gemm/os_blas.hpp
    #cpu/gemm/ref_gemm.cpp
    #cpu/gemm_u8s8s32x_inner_product.cpp
    #cpu/gemm_u8s8s32x_inner_product.hpp
    #cpu/gemm_x8s8s32x_convolution.cpp
    #cpu/gemm_x8s8s32x_convolution.hpp
    #cpu/jit_avx2_1x1_conv_kernel_f32.cpp
    #cpu/jit_avx2_1x1_conv_kernel_f32.hpp
    #cpu/jit_avx2_1x1_convolution.cpp
    #cpu/jit_avx2_1x1_convolution.hpp
    cpu/jit_avx2_conv_kernel_f32.cpp
    cpu/jit_avx2_conv_kernel_f32.hpp
    cpu/jit_avx2_convolution.cpp
    cpu/jit_avx2_convolution.hpp
    #cpu/jit_avx512_common_1x1_conv_kernel.cpp
    #cpu/jit_avx512_common_1x1_conv_kernel.hpp
    #cpu/jit_avx512_common_1x1_convolution.cpp
    #cpu/jit_avx512_common_1x1_convolution.hpp
    cpu/jit_avx512_common_conv_kernel.cpp
    cpu/jit_avx512_common_conv_kernel.hpp
    cpu/jit_avx512_common_convolution.cpp
    cpu/jit_avx512_common_convolution.hpp
    cpu/jit_avx512_common_convolution_winograd.cpp
    cpu/jit_avx512_common_convolution_winograd.hpp
    cpu/jit_avx512_common_conv_winograd_kernel_f32.cpp
    cpu/jit_avx512_common_conv_winograd_kernel_f32.hpp
    #cpu/jit_avx512_common_lrn.cpp
    #cpu/jit_avx512_common_lrn.hpp
    cpu/jit_avx512_core_fp32_wino_conv_2x3.cpp
    cpu/jit_avx512_core_fp32_wino_conv_2x3.hpp
    cpu/jit_avx512_core_fp32_wino_conv_4x3.cpp
    cpu/jit_avx512_core_fp32_wino_conv_4x3.hpp
    cpu/jit_avx512_core_fp32_wino_conv_4x3_kernel.cpp
    cpu/jit_avx512_core_fp32_wino_conv_4x3_kernel.hpp
    #cpu/jit_avx512_core_i8i8_pooling.cpp
    #cpu/jit_avx512_core_i8i8_pooling.hpp
    #cpu/jit_avx512_core_u8s8s32x_deconvolution.cpp
    #cpu/jit_avx512_core_u8s8s32x_deconvolution.hpp
    #cpu/jit_avx512_core_u8s8s32x_wino_convolution.cpp
    #cpu/jit_avx512_core_u8s8s32x_wino_convolution.hpp
    #cpu/jit_avx512_core_x8s8s32x_1x1_conv_kernel.cpp
    #cpu/jit_avx512_core_x8s8s32x_1x1_conv_kernel.hpp
    #cpu/jit_avx512_core_x8s8s32x_1x1_convolution.cpp
    #cpu/jit_avx512_core_x8s8s32x_1x1_convolution.hpp
    #cpu/jit_avx512_core_x8s8s32x_conv_kernel.cpp
    #cpu/jit_avx512_core_x8s8s32x_conv_kernel.hpp
    #cpu/jit_avx512_core_x8s8s32x_convolution.cpp
    #cpu/jit_avx512_core_x8s8s32x_convolution.hpp
    cpu/jit_generator.hpp
    cpu/jit_primitive_conf.hpp
    #cpu/jit_sse42_1x1_conv_kernel_f32.cpp
    #cpu/jit_sse42_1x1_conv_kernel_f32.hpp
    #cpu/jit_sse42_1x1_convolution.cpp
    #cpu/jit_sse42_1x1_convolution.hpp
    cpu/jit_sse42_conv_kernel_f32.cpp
    cpu/jit_sse42_conv_kernel_f32.hpp
    cpu/jit_sse42_convolution.cpp
    cpu/jit_sse42_convolution.hpp
    cpu/jit_transpose_src_utils.cpp
    cpu/jit_transpose_src_utils.hpp
    #cpu/jit_uni_1x1_conv_utils.hpp
    #cpu/jit_uni_batch_normalization.cpp
    #cpu/jit_uni_batch_normalization.hpp
    #cpu/jit_uni_dw_conv_kernel_f32.cpp
    #cpu/jit_uni_dw_conv_kernel_f32.hpp
    #cpu/jit_uni_dw_convolution.cpp
    #cpu/jit_uni_dw_convolution.hpp
    #cpu/jit_uni_eltwise.cpp
    #cpu/jit_uni_eltwise.hpp
    #cpu/jit_uni_lrn.cpp
    #cpu/jit_uni_lrn.hpp
    #cpu/jit_uni_lrn_kernel_f32.cpp
    #cpu/jit_uni_lrn_kernel_f32.hpp
    cpu/jit_uni_pooling.cpp
    cpu/jit_uni_pooling.hpp
    cpu/jit_uni_pool_kernel_f32.cpp
    cpu/jit_uni_pool_kernel_f32.hpp
    cpu/jit_uni_reorder.cpp
    cpu/jit_uni_reorder.hpp
    cpu/jit_uni_reorder_utils.cpp
    #cpu/nchw_pooling.cpp
    #cpu/nchw_pooling.hpp
    #cpu/ncsp_batch_normalization.cpp
    #cpu/ncsp_batch_normalization.hpp
    #cpu/nhwc_pooling.cpp
    #cpu/nhwc_pooling.hpp
    #cpu/nspc_batch_normalization.cpp
    #cpu/nspc_batch_normalization.hpp
    #cpu/ref_batch_normalization.cpp
    #cpu/ref_batch_normalization.hpp
    #cpu/ref_concat.hpp
    #cpu/ref_convolution.cpp
    #cpu/ref_convolution.hpp
    #cpu/ref_deconvolution.cpp
    #cpu/ref_deconvolution.hpp
    #cpu/ref_eltwise.cpp
    #cpu/ref_eltwise.hpp
    #cpu/ref_inner_product.cpp
    #cpu/ref_inner_product.hpp
    #cpu/ref_lrn.cpp
    #cpu/ref_lrn.hpp
    #cpu/ref_pooling.cpp
    #cpu/ref_pooling.hpp
    #cpu/ref_rnn.cpp
    #cpu/ref_rnn.hpp
    #cpu/ref_shuffle.cpp
    #cpu/ref_shuffle.hpp
    #cpu/ref_softmax.cpp
    #cpu/ref_softmax.hpp
    #cpu/ref_sum.hpp
    #cpu/simple_concat.cpp
    #cpu/simple_concat.hpp
    cpu/simple_q10n.hpp
    cpu/simple_reorder.hpp
    #cpu/simple_sum.cpp
    #cpu/simple_sum.hpp
    cpu/wino_reorder.hpp
    cpu/xbyak/*.h
    )

# propagate SRC specific flags
append(CMAKE_C_FLAGS "${CMAKE_SRC_CCXX_FLAGS}")
append(CMAKE_CXX_FLAGS "${CMAKE_SRC_CCXX_FLAGS}")

# propagate no warning flags
append(CMAKE_C_FLAGS "${CMAKE_CCXX_NOWARN_FLAGS}")
append(CMAKE_CXX_FLAGS "${CMAKE_CCXX_NOWARN_FLAGS}")

# propagate sanitizer flags
append(CMAKE_C_FLAGS "${CMAKE_CCXX_SANITIZER_FLAGS}")
append(CMAKE_CXX_FLAGS "${CMAKE_CCXX_SANITIZER_FLAGS}")

if(NOT MKLDNN_VERBOSE)
    add_definitions(-DDISABLE_VERBOSE)
endif()

if(MKLDNN_ENABLE_CONCURRENT_EXEC)
    add_definitions(-DMKLDNN_ENABLE_CONCURRENT_EXEC)
endif()

if(MKLDNN_THREADING MATCHES "TBB")
    include_directories(${TBB_INCLUDE_DIR})
    list(APPEND EXTRA_LIBS ${TBB_LIBRARIES})
    if(UNIX)
        list(APPEND EXTRA_LIBS pthread)
        if(NOT(APPLE))
            list(APPEND EXTRA_LIBS rt)
        endif()
    endif()
endif()

if(VTUNE_ROOT)
    include_directories(${VTUNE_ROOT}/include)
    add_definitions(-DJIT_PROFILING_VTUNE)
endif()

if(WIN32)
    add_definitions(-D_WIN)
    add_definitions(-DNOMINMAX)
    # Correct 'jnl' macro/jit issue
    if(${CMAKE_CXX_COMPILER_ID} STREQUAL "Intel")
        append(CMAKE_CXX_FLAGS "/Qlong-double")
    endif()
endif()

if(CMAKE_CXX_COMPILER_ID STREQUAL "Intel")
    # to make computations more stable and to align the jitted code
    # with the reference one use precise division and square root
    # by default
    file(GLOB FILES_REQUIRED_PREC_SQRT
        ${CMAKE_CURRENT_SOURCE_DIR}/cpu/*normalization*.cpp)
    file(GLOB FILES_REQUIRED_PREC_DIV
        ${CMAKE_CURRENT_SOURCE_DIR}/cpu/*normalization*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/cpu/ref_rnn.cpp)
    if(WIN32)
        set_source_files_properties(${FILES_REQUIRED_PREC_SQRT}
            PROPERTIES COMPILE_FLAGS "/Qprec-sqrt")
        set_source_files_properties(${FILES_REQUIRED_PREC_DIV}
            PROPERTIES COMPILE_FLAGS "/Qprec-div")
    else()
        set_source_files_properties(${FILES_REQUIRED_PREC_SQRT}
            PROPERTIES COMPILE_FLAGS "-prec-sqrt")
        set_source_files_properties(${FILES_REQUIRED_PREC_DIV}
            PROPERTIES COMPILE_FLAGS "-prec-div")
    endif()
endif()

add_library(${TARGET_NAME} ${MKLDNN_LIBRARY_TYPE} ${HEADERS} ${SOURCES})

# Compile the library for SSE4.2
set_target_properties(${TARGET_NAME} PROPERTIES COMPILE_FLAGS "${ISA_FLAGS_SSE42}")

#Add mkldnn.dll to execution PATH
if(NOT(MINGW))
    set(CTESTCONFIG_PATH "${CTESTCONFIG_PATH}\;${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_BUILD_TYPE}" PARENT_SCOPE)
else()
    # CMake with "MSYS Makefiles" generator seems to build libmkldnn.dll in a directory without build type.
    set(CTESTCONFIG_PATH "${CTESTCONFIG_PATH}\;${CMAKE_CURRENT_BINARY_DIR}" PARENT_SCOPE)
endif()
target_link_libraries(${TARGET_NAME} ${${TARGET_NAME}_LINKER_LIBS} ${EXTRA_LIBS})
set_property(TARGET ${TARGET_NAME} PROPERTY CXX_STANDARD 11)
set_property(TARGET ${TARGET_NAME} PROPERTY CXX_STANDARD_REQUIRED ON)
set_property(TARGET ${TARGET_NAME} PROPERTY VERSION "${PROJECT_VERSION}.0")
set_property(TARGET ${TARGET_NAME} PROPERTY SOVERSION "0")

if(MINGW)
    # We need to install *.dll into bin/ and *.a into lib/. 
    install(TARGETS ${TARGET_NAME}
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib${LIB_SUFFIX}
    )
else()
    install(TARGETS ${TARGET_NAME} DESTINATION lib${LIB_SUFFIX})
endif()
install(FILES ${HEADERS} DESTINATION include)
