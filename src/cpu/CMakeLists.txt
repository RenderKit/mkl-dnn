#===============================================================================
# Copyright 2019 Intel Corporation
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

#file(GLOB_RECURSE SOURCES
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.h
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.c
#    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
#    )

file(GLOB_RECURSE SOURCES
    bfloat16.cpp
    cpu_barrier.cpp
    cpu_barrier.hpp
    cpu_concat.cpp
    cpu_convolution_pd.hpp
    cpu_eltwise_pd.hpp
    cpu_engine.cpp
    cpu_engine.hpp
    cpu_isa_traits.hpp
    cpu_memory_storage.hpp
    cpu_pooling_pd.hpp
    cpu_reducer.cpp
    cpu_reducer.hpp
    cpu_reorder.cpp
    cpu_reorder_pd.hpp
    cpu_stream.hpp
    cpu_sum.cpp
    jit_avx2_conv_kernel_f32.cpp
    jit_avx2_conv_kernel_f32.hpp
    jit_avx2_convolution.cpp
    jit_avx2_convolution.hpp
    jit_avx512_common_conv_kernel.cpp
    jit_avx512_common_conv_kernel.hpp
    jit_avx512_common_convolution.cpp
    jit_avx512_common_convolution.hpp
    jit_avx512_common_convolution_winograd.cpp
    jit_avx512_common_convolution_winograd.hpp
    jit_avx512_common_conv_winograd_kernel_f32.cpp
    jit_avx512_common_conv_winograd_kernel_f32.hpp
    jit_avx512_core_f32_wino_conv_2x3.cpp
    jit_avx512_core_f32_wino_conv_2x3.hpp
    jit_avx512_core_f32_wino_conv_4x3.cpp
    jit_avx512_core_f32_wino_conv_4x3.hpp
    jit_avx512_core_f32_wino_conv_4x3_kernel.cpp
    jit_avx512_core_f32_wino_conv_4x3_kernel.hpp
    jit_generator.hpp
    jit_primitive_conf.hpp
    jit_sse41_conv_kernel_f32.cpp
    jit_sse41_conv_kernel_f32.hpp
    jit_sse41_convolution.cpp
    jit_sse41_convolution.hpp
    jit_transpose_src_utils.cpp
    jit_transpose_src_utils.hpp
    jit_uni_eltwise.cpp
    jit_uni_eltwise.hpp
    jit_uni_pooling.cpp
    jit_uni_pooling.hpp
    jit_uni_pool_kernel.cpp
    jit_uni_pool_kernel.hpp
    jit_uni_reorder.cpp
    jit_uni_reorder.hpp
    jit_uni_reorder_utils.cpp
    jit_utils/jitprofiling/ittnotify_config.h
    jit_utils/jitprofiling/ittnotify_types.h
    jit_utils/jitprofiling/jitprofiling.c
    jit_utils/jitprofiling/jitprofiling.h
    jit_utils/jit_utils.cpp
    jit_utils/jit_utils.hpp
    simple_q10n.hpp
    simple_reorder.hpp
    wino_reorder.hpp
    xbyak/xbyak_bin2hex.h
    xbyak/xbyak.h
    xbyak/xbyak_mnemonic.h
    xbyak/xbyak_util.h
    )

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/xbyak
    )

if(CMAKE_CXX_COMPILER_ID STREQUAL "Intel")
    # to make computations more stable and to align the jitted code
    # with the reference one use precise division and square root
    # by default
    file(GLOB FILES_REQUIRED_PREC_SQRT
        ${CMAKE_CURRENT_SOURCE_DIR}/*normalization*.cpp)
    file(GLOB FILES_REQUIRED_PREC_DIV
        ${CMAKE_CURRENT_SOURCE_DIR}/*normalization*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/ref_rnn.cpp)
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
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    file(GLOB FILES_REQUIRED_BIGOBJ
        ${CMAKE_CURRENT_SOURCE_DIR}/cpu_engine.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/cpu_reorder.cpp)
	if(WIN32)
		set_source_files_properties(${FILES_REQUIRED_BIGOBJ}
			PROPERTIES COMPILE_FLAGS "/bigobj")
	endif()
endif()

set(OBJ_LIB ${LIB_NAME}_cpu)
add_library(${OBJ_LIB} OBJECT ${SOURCES})
set_target_properties(${OBJ_LIB} PROPERTIES COMPILE_FLAGS "${DNNL_ARCH_OPT_FLAGS}")
target_link_libraries_build(${OBJ_LIB}
    "${EXTRA_SHARED_LIBS};${EXTRA_STATIC_LIBS}")
set(${LIB_NAME}_SUB_OBJS ${${LIB_NAME}_SUB_OBJS}
    $<TARGET_OBJECTS:${OBJ_LIB}> PARENT_SCOPE)
