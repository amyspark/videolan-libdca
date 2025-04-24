makedef = find_program('makedef.py')

makedef_args = [
    makedef,
    '--regex',
    '^dca_.',
]

if host_machine.system() in ['darwin', 'ios']
    makedef_args += ['--os', 'darwin']
elif host_machine.system() in ['windows', 'cygwin']
    makedef_args += ['--os', 'win']
else
    makedef_args += ['--os', 'linux']
endif

if cc.symbols_have_underscore_prefix()
    makedef_args += ['--prefix', '_']
else
    makedef_args += ['--prefix', '']
endif

makedef_tool_args = []

if cc.get_define('_MSC_VER') != ''
    dumpbin = find_program('dumpbin')
    makedef_tool_args += ['--dumpbin', dumpbin]
else
    nm = find_program('nm')
    makedef_tool_args += ['--nm', nm]
endif

symbols = configure_file(
    command: [makedef_args, '--list', '@INPUT@'],
    input: 'libdca.symbols',
    output: 'libdca.def',
    capture: true,
)

version_script = meson.current_build_dir() / 'libdca.def'

if host_machine.system() in ['darwin', 'ios']
    vflag = '-Wl,-exported_symbols_list,@0@'.format(version_script)
else
    vflag = '-Wl,--version-script,@0@'.format(version_script)
endif

libdca_lib = library(
    'dca',
    files(
        'dca_internal.h',
        'tables.h',
        'tables_quantization.h',
        'tables_huffman.h',
        'tables_fir.h',
        'tables_adpcm.h',
        'tables_vq.h',
        'bitstream.h',
        'bitstream.c',
        'parse.c',
        'downmix.c',
    ) + config_h,
    c_args: project_args,
    include_directories: incdir,
    dependencies: m_dep,
    link_args: cc.get_supported_link_arguments(vflag),
    link_depends: symbols,
    vs_module_defs: symbols,
    install: true,
)

pkg = import('pkgconfig')

libdca = pkg.generate(
    libdca_lib,
    name: 'libdca',
    description: 'A decoding library for DTS Coherent Acoustics streams.',
)

libao = pkg.generate(
    libdca_lib,
    name: 'libdts',
    description: 'Backward compatibility place-holder for libdca.',
)
