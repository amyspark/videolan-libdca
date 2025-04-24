project('libdca', 'c',
    version: '0.0.7',
    default_options: [
        'c_std=gnu99,c11', # inttypes.h for MSVC
        'optimization=3',
        'debug=false',
        'warning_level=1',
    ],
    meson_version: '>= 1.3.0',
)

cc = meson.get_compiler('c')

project_args = ['-DHAVE_CONFIG_H']

if not get_option('debug')
    project_args += cc.get_supported_arguments(
        '-fomit-frame-pointer',
    )
endif

# nocona seems like a better choice than i686 -- amyspark
project_args += cc.get_supported_arguments('-mtune=ultrasparc', '-mtune=nocona')

m_dep = cc.find_library('m', required: false)

cfg_data = configuration_data()

cfg_data.set10('HAVE_STRING_H', cc.has_header('string.h'))
cfg_data.set('HAVE_SYS_TIME_H', cc.has_header('sys/time.h'))
cfg_data.set('HAVE_SYS_TIMEB_H', cc.has_header('sys/timeb.h'))
cfg_data.set('HAVE_IO_H', cc.has_header('io.h'))
cfg_data.set('HAVE_STRUCT_TIMEVAL', cc.has_type('struct timeval', prefix: '#include <sys/time.h>\n#include <time.h>'))

intl_dep = dependency('intl', required: false)
cfg_data.set('HAVE_LIBINTL_H', cc.has_header('libintl.h', dependencies: intl_dep))

cfg_data.set('WORDS_BIGENDIAN', host_machine.endian() == 'big')

cfg_data.set('HAVE_MEMALIGN', cc.has_function('memalign'))
cfg_data.set('HAVE_GETTIMEOFDAY', cc.has_function('gettimeofday'))
cfg_data.set('HAVE_FTIME', cc.has_function('ftime'))
# https://www.gnu.org/software/autoconf/manual/autoconf-2.60/html_node/Obsolete-Macros.html
cfg_data.set('RETSIGTYPE', 'void')

cfg_data.set_quoted('PACKAGE', meson.project_name())
cfg_data.set_quoted('VERSION', meson.project_version())

if get_option('fixed').allowed()
    cfg_data.set('LIBDCA_FIXED', true)
elif get_option('double').allowed()
    cfg_data.set('LIBDCA_DOUBLE', true)
endif

winmm_lib = cc.find_library('winmm', required: get_option('win'))
cfg_data.set('LIBAO_WIN', winmm_lib.found() and get_option('win').allowed())

config_h = configure_file(
    output: 'config.h',
    configuration: cfg_data,
)

incdir = [include_directories('.')]

subdir('include')
subdir('libdca')
subdir('libao')
subdir('src')
