find_package(Crashpad REQUIRED)
# message(STATUS "CRASHPAD_INCLUDE_DIR = ${CRASHPAD_INCLUDE_DIR}")
# message(STATUS "CRASHPAD_INCLUDE_DIRS = ${CRASHPAD_INCLUDE_DIRS}")
# message(STATUS "CRASHPAD_FOUND = ${CRASHPAD_FOUND}")
# message(STATUS "CRASHPAD_OBJECT = ${CRASHPAD_OBJECT}")
# message(STATUS "CRASHPAD_OBJECTS = ${CRASHPAD_OBJECTS}")
# message(STATUS "CRASHPAD_LIBRARIES = ${CRASHPAD_LIBRARIES}")
# message(STATUS "CRASHPAD_LIBRARY_DIRECTORIES = ${CRASHPAD_LIBRARY_DIRECTORIES}")
link_directories(example_crashpad PRIVATE ${CRASHPAD_LIBRARY_DIRECTORIES})

add_executable(example_crashpad example_crashpad.cpp)

target_link_libraries(example_crashpad PUBLIC bun)
target_include_directories(example_crashpad PUBLIC ${CRASHPAD_INCLUDE_DIRS})
target_link_libraries(example_crashpad PRIVATE ${CRASHPAD_LIBRARIES})
target_sources(example_crashpad PRIVATE ${CRASHPAD_OBJECTS})
