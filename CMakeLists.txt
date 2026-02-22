cmake_minimum_required (VERSION 4.2.3)
project (learn-opengl)

# find_package(OpenGL REQUIRED)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Ask the user to give the paths to GLFW, GLEW and GLM.
# On Linux this can be found automatically with pkg-config; 
# see http://stackoverflow.com/questions/6889264/cmake-flags-for-opengl-using-glew-and-glfw, for instance.

# set(OPENGL_INCLUDE_PATH ../libraries-cpp/OpenGL/includes)

set(OPENGL_INCLUDES ../libraries-cpp/OpenGL/includes)

set(GLFW_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/../libraries-cpp/OpenGL/lib/GLFW/glfw3.lib")
# set(GLFW_LIBRARY ../libraries-cpp/OpenGL/lib/GLFW/glfw3)

# set(GLEW_INCLUDE_PATH "" CACHE PATH "The directory that contains GL/glew.h" )
# set(GLEW_LIBRARY "" CACHE FILEPATH "libglew.a or glew.lib" )

# set(GLM_INCLUDE_PATH "" CACHE PATH "The directory that contains glm/glm.hpp" )


set(MY_HEADER_FILES
	# put all your .h here. Right now we have none.
	# Example : 
	# ../common/shader.hpp
	src/shader.h
)

set(MY_SOURCE_FILES
    src/glad.c
	src/main.cpp
	src/shader.cpp
)



# include_directories(
# 	${OPENGL_INCLUDE_PATH}
# )


add_executable(learn-opengl
	${MY_HEADER_FILES}
	${MY_SOURCE_FILES}
)

add_custom_command(TARGET learn-opengl POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory "$<TARGET_FILE_DIR:learn-opengl>/shaders"
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${CMAKE_CURRENT_SOURCE_DIR}/src/shaders"
        "$<TARGET_FILE_DIR:learn-opengl>/shaders"
    COMMENT "Copiando shaders al directorio de salida..."
)

add_custom_command(TARGET learn-opengl POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory "$<TARGET_FILE_DIR:learn-opengl>/textures"
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${CMAKE_CURRENT_SOURCE_DIR}/src/textures"
        "$<TARGET_FILE_DIR:learn-opengl>/textures"
    COMMENT "Copiando texturas al directorio de salida..."
)

target_include_directories(learn-opengl PRIVATE 
    ${OPENGL_INCLUDES}
)

target_link_libraries(learn-opengl
	"${GLFW_LIBRARY}"
    opengl32
	# ${GLFW_LIBRARY}
	# ${GLEW_LIBRARY}
)
