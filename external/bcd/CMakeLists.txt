include(CheckFunctionExists)

check_function_exists(getpwnam_r HAS_GETPWNAM_R)
check_function_exists(getgrnam_r HAS_GETGRNAM_R)
check_function_exists(sigtimedwait HAS_SIGTIMEDWAIT)

add_library(bcd STATIC bcd-amalgamated.c include/bcd.h)
target_include_directories(bcd PUBLIC include)

if(ANDROID)
    target_compile_definitions(bcd PRIVATE BCD_NDK_BUILD)
endif()

if (HAS_GETPWNAM_R)
    target_compile_definitions(bcd PRIVATE BCD_HAS_GETPWNAM_R)
endif()

if (HAS_GETGRNAM_R)
    target_compile_definitions(bcd PRIVATE BCD_HAS_GETGRNAM_R)
endif()

if (HAS_SIGTIMEDWAIT)
    target_compile_definitions(bcd PRIVATE BCD_HAS_SIGTIMEDWAIT)
endif()