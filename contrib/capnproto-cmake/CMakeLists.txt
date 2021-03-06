set (CAPNPROTO_SOURCE_DIR ${ClickHouse_SOURCE_DIR}/contrib/capnproto/c++/src)

set (KJ_SRCS
    ${CAPNPROTO_SOURCE_DIR}/kj/array.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/common.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/debug.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/exception.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/io.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/memory.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/mutex.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/string.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/hash.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/table.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/thread.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/main.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/arena.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/test-helpers.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/units.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/encoding.c++

    ${CAPNPROTO_SOURCE_DIR}/kj/refcount.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/string-tree.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/time.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/filesystem.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/filesystem-disk-unix.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/filesystem-disk-win32.c++
    ${CAPNPROTO_SOURCE_DIR}/kj/parse/char.c++
)

add_library(kj ${KJ_SRCS})
target_include_directories(kj SYSTEM PUBLIC ${CAPNPROTO_SOURCE_DIR})

set (CAPNP_SRCS
    ${CAPNPROTO_SOURCE_DIR}/capnp/c++.capnp.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/blob.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/arena.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/layout.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/list.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/any.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/message.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/schema.capnp.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/serialize.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/serialize-packed.c++

    ${CAPNPROTO_SOURCE_DIR}/capnp/schema.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/schema-loader.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/dynamic.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/stringify.c++
)

add_library(capnp ${CAPNP_SRCS})
target_link_libraries(capnp PUBLIC kj)

# The library has substandard code
if (COMPILER_GCC)
    set (SUPPRESS_WARNINGS -Wno-non-virtual-dtor -Wno-sign-compare -Wno-strict-aliasing -Wno-maybe-uninitialized
        -Wno-deprecated-declarations -Wno-class-memaccess)
elseif (COMPILER_CLANG)
    set (SUPPRESS_WARNINGS -Wno-non-virtual-dtor -Wno-sign-compare -Wno-strict-aliasing -Wno-deprecated-declarations)
endif ()

target_compile_options(kj PRIVATE ${SUPPRESS_WARNINGS})
target_compile_options(capnp PRIVATE ${SUPPRESS_WARNINGS})

set (CAPNPC_SRCS
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/type-id.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/error-reporter.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/lexer.capnp.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/lexer.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/grammar.capnp.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/parser.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/node-translator.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/compiler/compiler.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/schema-parser.c++
    ${CAPNPROTO_SOURCE_DIR}/capnp/serialize-text.c++
)

add_library(capnpc ${CAPNPC_SRCS})
target_link_libraries(capnpc PUBLIC capnp)
