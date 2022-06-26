project(TileWorld)
cmake_minimum_required(VERSION 3.16)

add_custom_command(
    PRE_BUILD
    OUTPUT comptime.h
    COMMAND echo #define COMPILE_TIME \"`date '+%Y %b %e %T %Z'`\"
    )



## Objects in base directory + liboshw requirement
add_library(tworld_base OBJECT  cmdline.c encoding.c err.c fileio.c 
                                help.c lxlogic.c messages.c
                                mslogic.c play.c random.c res.c score.c
                                series.c solution.c tworld.c unslist.c)

add_subdirectory(oshw-sdl)

# include_directories(/usr/include/SDL)
add_executable(TileWorld)
# target_include_directories(TileWorld /usr/include/SDL)
if(PSP)
target_link_libraries(TileWorld tworld_base oshw SDL pspirkeyb pspvram psphprm GL pspvfpu psppower pspgu pspctrl pspdebug pspsdk pspdisplay pspge pspaudiolib pspaudio intrafont pspkubridge pspfpu pspge)
		create_pbp_file(
		TARGET TileWorld
		# ICON_PATH ${PROJECT_SOURCE_DIR}/Source/SysPSP/Resources/eboot_icons/icon0.png
		# BACKGROUND_PATH ${PROJECT_SOURCE_DIR}/Source/SysPSP/Resources/eboot_icons/pic1.png
		PREVIEW_PATH NULL
		TITLE TileWorld
		# BUILD_PRX
					)

else()

target_link_libraries(TileWorld tworld_base oshw -lSDL )
endif()