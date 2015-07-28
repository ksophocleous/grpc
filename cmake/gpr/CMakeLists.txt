
list(APPEND gpr_public_headers
    "${grpc_SOURCE_DIR}/include/grpc/support/alloc.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/atm.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/atm_gcc_atomic.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/atm_gcc_sync.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/atm_win32.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/cancellable_platform.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/cmdline.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/cpu.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/histogram.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/host_port.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/log.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/log_win32.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/port_platform.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/slice.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/slice_buffer.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/string_util.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/subprocess.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/sync.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/sync_generic.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/sync_posix.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/sync_win32.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/thd.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/time.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/tls.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/tls_gcc.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/tls_msvc.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/tls_pthread.h"
    "${grpc_SOURCE_DIR}/include/grpc/support/useful.h")

list(APPEND gpr_private_headers
    "${grpc_SOURCE_DIR}/src/core/support/env.h"
    "${grpc_SOURCE_DIR}/src/core/support/file.h"
    "${grpc_SOURCE_DIR}/src/core/support/murmur_hash.h"
    "${grpc_SOURCE_DIR}/src/core/support/stack_lockfree.h"
    "${grpc_SOURCE_DIR}/src/core/support/string.h"
    "${grpc_SOURCE_DIR}/src/core/support/string_win32.h"
    "${grpc_SOURCE_DIR}/src/core/support/thd_internal.h")

list(APPEND gpr_source_files
    "${grpc_SOURCE_DIR}/src/core/support/alloc.c"
    "${grpc_SOURCE_DIR}/src/core/support/cancellable.c"
    "${grpc_SOURCE_DIR}/src/core/support/cmdline.c"
    "${grpc_SOURCE_DIR}/src/core/support/cpu_iphone.c"
    "${grpc_SOURCE_DIR}/src/core/support/cpu_linux.c"
    "${grpc_SOURCE_DIR}/src/core/support/cpu_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/cpu_windows.c"
    "${grpc_SOURCE_DIR}/src/core/support/env_linux.c"
    "${grpc_SOURCE_DIR}/src/core/support/env_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/env_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/file.c"
    "${grpc_SOURCE_DIR}/src/core/support/file_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/file_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/histogram.c"
    "${grpc_SOURCE_DIR}/src/core/support/host_port.c"
    "${grpc_SOURCE_DIR}/src/core/support/log.c"
    "${grpc_SOURCE_DIR}/src/core/support/log_android.c"
    "${grpc_SOURCE_DIR}/src/core/support/log_linux.c"
    "${grpc_SOURCE_DIR}/src/core/support/log_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/log_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/murmur_hash.c"
    "${grpc_SOURCE_DIR}/src/core/support/slice.c"
    "${grpc_SOURCE_DIR}/src/core/support/slice_buffer.c"
    "${grpc_SOURCE_DIR}/src/core/support/stack_lockfree.c"
    "${grpc_SOURCE_DIR}/src/core/support/string.c"
    "${grpc_SOURCE_DIR}/src/core/support/string_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/string_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/subprocess_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/sync.c"
    "${grpc_SOURCE_DIR}/src/core/support/sync_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/sync_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/thd.c"
    "${grpc_SOURCE_DIR}/src/core/support/thd_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/thd_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/time.c"
    "${grpc_SOURCE_DIR}/src/core/support/time_posix.c"
    "${grpc_SOURCE_DIR}/src/core/support/time_win32.c"
    "${grpc_SOURCE_DIR}/src/core/support/tls_pthread.c")

if(UNIX)
    find_package(Threads)
    set(additional_libraries ${CMAKE_THREAD_LIBS_INIT})
endif()

add_library(gpr ${gpr_public_headers} ${gpr_private_headers} ${gpr_source_files})
target_link_libraries(gpr ${additional_libraries})
set_target_properties(gpr PROPERTIES
	COMPILE_DEFINITIONS "${compile_definitions}"
    IMPORT_PREFIX ""
    IMPORT_SUFFIX _imp${CMAKE_IMPORT_LIBRARY_SUFFIX}
    INTERFACE_INCLUDE_DIRECTORIES "$<BUILD_INTERFACE:${grpc_SOURCE_DIR}>;$<BUILD_INTERFACE:${grpc_SOURCE_DIR}/include>;$<INSTALL_INTERFACE:include>"
    INCLUDE_DIRECTORIES "${grpc_SOURCE_DIR};${grpc_SOURCE_DIR}/include"
    PREFIX "")

install(TARGETS gpr
    EXPORT ${CMAKE_PROJECT_NAME}Config
    LIBRARY DESTINATION lib COMPONENT dev
    ARCHIVE DESTINATION lib COMPONENT dev
    RUNTIME DESTINATION bin COMPONENT bin)
