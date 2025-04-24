dcadec = executable(
    'dcadec',
    files(
        'dcadec.c',
        'getopt.h',
        'getopt.c',
        'gettimeofday.h',
        'gettimeofday.c',
    ),
    c_args: project_args,
    include_directories: incdir,
    link_with: [libao_lib, libdca_lib],
    dependencies: [m_dep, winmm_lib],
    install: true,
)

extract_dca = executable(
    'extract_dca',
    files(
        'extract_dca.c',
        'getopt.h',
        'getopt.c',
    ),
    c_args: project_args,
    include_directories: incdir,
    link_with: [libao_lib, libdca_lib],
    dependencies: [m_dep, winmm_lib],
    install: true,
)

install_man(
    files(
        'dcadec.1',
        'extract_dca.1',
    )
)
