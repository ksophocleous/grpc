
list(APPEND grpc++_public_headers
    "${grpc_SOURCE_DIR}/include/grpc++/async_generic_service.h"
    "${grpc_SOURCE_DIR}/include/grpc++/async_unary_call.h"
    "${grpc_SOURCE_DIR}/include/grpc++/auth_context.h"
    "${grpc_SOURCE_DIR}/include/grpc++/auth_property_iterator.h"
    "${grpc_SOURCE_DIR}/include/grpc++/byte_buffer.h"
    "${grpc_SOURCE_DIR}/include/grpc++/channel_arguments.h"
    "${grpc_SOURCE_DIR}/include/grpc++/channel_interface.h"
    "${grpc_SOURCE_DIR}/include/grpc++/client_context.h"
    "${grpc_SOURCE_DIR}/include/grpc++/completion_queue.h"
    "${grpc_SOURCE_DIR}/include/grpc++/config.h"
    "${grpc_SOURCE_DIR}/include/grpc++/config_protobuf.h"
    "${grpc_SOURCE_DIR}/include/grpc++/create_channel.h"
    "${grpc_SOURCE_DIR}/include/grpc++/credentials.h"
    "${grpc_SOURCE_DIR}/include/grpc++/fixed_size_thread_pool.h"
    "${grpc_SOURCE_DIR}/include/grpc++/generic_stub.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/call.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/client_unary_call.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/grpc_library.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/internal_stub.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/proto_utils.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/rpc_method.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/rpc_service_method.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/serialization_traits.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/service_type.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/sync.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/sync_cxx11.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/sync_no_cxx11.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/thd.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/thd_cxx11.h"
    "${grpc_SOURCE_DIR}/include/grpc++/impl/thd_no_cxx11.h"
    "${grpc_SOURCE_DIR}/include/grpc++/server.h"
    "${grpc_SOURCE_DIR}/include/grpc++/server_builder.h"
    "${grpc_SOURCE_DIR}/include/grpc++/server_context.h"
    "${grpc_SOURCE_DIR}/include/grpc++/server_credentials.h"
    "${grpc_SOURCE_DIR}/include/grpc++/slice.h"
    "${grpc_SOURCE_DIR}/include/grpc++/status.h"
    "${grpc_SOURCE_DIR}/include/grpc++/status_code_enum.h"
    "${grpc_SOURCE_DIR}/include/grpc++/stream.h"
    "${grpc_SOURCE_DIR}/include/grpc++/thread_pool_interface.h"
    "${grpc_SOURCE_DIR}/include/grpc++/time.h")

list(APPEND grpc++_private_headers
    "${grpc_SOURCE_DIR}/src/cpp/client/secure_credentials.h"
    "${grpc_SOURCE_DIR}/src/cpp/common/secure_auth_context.h"
    "${grpc_SOURCE_DIR}/src/cpp/server/secure_server_credentials.h"
    "${grpc_SOURCE_DIR}/src/cpp/client/channel.h"
    "${grpc_SOURCE_DIR}/src/cpp/common/create_auth_context.h")

list(APPEND grpc++_source_files
    "${grpc_SOURCE_DIR}/src/cpp/client/secure_channel_arguments.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/secure_credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/auth_property_iterator.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/secure_auth_context.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/secure_create_auth_context.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/secure_server_credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/channel.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/channel_arguments.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/client_context.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/create_channel.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/generic_stub.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/insecure_credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/client/internal_stub.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/call.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/completion_queue.cc"
    "${grpc_SOURCE_DIR}/src/cpp/common/rpc_method.cc"
    "${grpc_SOURCE_DIR}/src/cpp/proto/proto_utils.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/async_generic_service.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/create_default_thread_pool.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/fixed_size_thread_pool.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/insecure_server_credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/server.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/server_builder.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/server_context.cc"
    "${grpc_SOURCE_DIR}/src/cpp/server/server_credentials.cc"
    "${grpc_SOURCE_DIR}/src/cpp/util/byte_buffer.cc"
    "${grpc_SOURCE_DIR}/src/cpp/util/slice.cc"
    "${grpc_SOURCE_DIR}/src/cpp/util/status.cc"
    "${grpc_SOURCE_DIR}/src/cpp/util/time.cc")

if(MSVC)
	list(APPEND additional_compile_flags
		"/wd4800")
endif()

list(APPEND inc_dirs
	"${grpc_SOURCE_DIR}"
	"${grpc_SOURCE_DIR}/include")

add_library(grpc++ ${grpc++_public_headers} ${grpc++_private_headers} ${grpc++_source_files})
target_link_libraries(grpc++ grpc libprotobuf ${additional_libs})
set_target_properties(grpc++ PROPERTIES
	COMPILE_DEFINITIONS "${compile_definitions}"
	COMPILE_FLAGS "${additional_compile_flags}"
	INCLUDE_DIRECTORIES "${inc_dirs}")

install(TARGETS grpc++
    EXPORT ${CMAKE_PROJECT_NAME}Config
    LIBRARY DESTINATION lib COMPONENT dev
    ARCHIVE DESTINATION lib COMPONENT dev
    RUNTIME DESTINATION bin COMPONENT bin)
