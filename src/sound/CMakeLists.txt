add_library(sound STATIC
    bgmusic.c
    snd_codec.c
    snd_codec.h
    snd_dma.c
    snd_flac.c
    snd_flac.h
    snd_mem.c
    snd_mix.c
    snd_mp3.c
    snd_mp3.h
    snd_mp3tag.c
    snd_sdl.c
    snd_vorbis.c
    snd_vorbis.h
    snd_wave.c
    snd_wave.h
)

set(LIBS ${SDL2_LIBRARIES})
if(UNIX AND NOT APPLE)
    list(APPEND LIBS
        ${VORBIS_LIBRARY}
        ${VORBISFILE_LIBRARY}
        ${FLAC_LIBRARY}
        ${MAD_LIBRARY}
    )
elseif(PS2)
    list(APPEND LIBS
        ${VORBIS_LIBRARY}
        ${VORBISFILE_LIBRARY}
        ${FLAC_LIBRARY}
        ${MAD_LIBRARY}
        ${OGG_LIBRARY}
    )
else()
    list(APPEND LIBS
        Vorbis::vorbisfile
        mad::mad
        FLAC::FLAC
    )
endif()

target_include_directories(sound PRIVATE ${CMAKE_BINARY_DIR} "../")
target_include_directories(sound PUBLIC "./include")
if(PS2)
    target_compile_options(sound PRIVATE
        -Wno-incompatible-pointer-types
    )
endif()
target_link_libraries(sound PUBLIC common console mathlib memory)
target_link_libraries(sound PRIVATE ${LIBS} client cmd host model sys)
