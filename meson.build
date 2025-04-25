project(
    'chuffed'
  , ['cpp']
  , version : '0.13.3'
  , meson_version: '>= 1.3.0'
  , default_options: {
        'cpp_std': 'gnu++23'
      }
  )

cpp = meson.get_compiler('cpp')

project_inc = include_directories('.')

bison = find_program('bison', version: '>= 3.4')
flex  = find_program('flex')

pkg = import('pkgconfig')

if get_option('enable_cpprofiler')
    add_project_arguments('-DHAS_PROFILER', language: ['cpp'])
endif


chuffed_lib_files = files()

subdir('chuffed')
subdir('thirdparty')

chuffed_lib = static_library(
    'chuffed'
  , chuffed_lib_files
  , link_whole: thirdparty_murmur_lib
  , install: true
  )

chuffed_dep = declare_dependency(
    link_with: [chuffed_lib, chuffed_fzn_lib]
  , include_directories: project_inc
  )

pkg.generate(
    name: 'chuffed'
  , filebase: 'chuffed'
  , description: 'Chuffed CP solver'
  , version: meson.project_version()
  , libraries: chuffed_lib
  , requires: [
        'chuffed_fzn'
      ]
  )

executable(
    'fzn-chuffed'
  , 'chuffed' / 'flatzinc' / 'fzn-chuffed.cpp'
  , dependencies: [chuffed_dep]
  , install: true
  )

fs = import('fs')
fzn_chuffed_path = fs.relative_to(
    get_option('bindir') / 'fzn-chuffed'
  , get_option('datadir') / 'minizinc' / 'solvers'
  )

chuffed_conf_data = configuration_data()
chuffed_conf_data.set_quoted('REL_INSTALL_BINARY', fzn_chuffed_path)
chuffed_conf_data.set_quoted('CHUFFED_VERSION', meson.project_version())
chuffed_conf_file = configure_file(
    input: 'chuffed.msc.meson.in'
  , output: 'chuffed.msc'
  , configuration: chuffed_conf_data
  )
install_data(
    chuffed_conf_file
  , install_dir: get_option('datadir') / 'minizinc' / 'solvers'
  )

examples = [
    'bibd'
  , 'blackhole'
  , 'concert_hall'
  , 'concert_hall_sym'
  , 'golomb'
  , 'graceful_graph'
  , 'graph_colouring'
  , 'graph_colouring_sym'
  , 'graph_colouring_sym2'
  , 'grid_colouring'
  , 'latin_square'
  , 'magic_square'
  , 'mosp'
  , 'nn_queens'
  , 'non'
  , 'photo'
  , 'queens'
  , 'rcpsp'
  , 'shift'
  , 'steel_mill'
  , 'wreg_shift'
  ]
zlib_examples = [
    'cross'
  , 'fdpent'
  , 'nurse'
  ]

if get_option('build_examples')
    foreach ex : examples
        executable(
            ex
          , 'chuffed' / 'examples' / f'@ex@.cpp'
          , dependencies: [chuffed_dep]
          , install: true
          )
    endforeach
    zlib_dep = dependency('zlib', required: false)
    if zlib_dep.found()
        foreach ex : zlib_examples
            executable(
                ex
              , 'chuffed' / 'examples' / f'@ex@.cpp'
              , dependencies: [chuffed_dep, zlib_dep]
              , install: true
              )
        endforeach
    endif
endif
