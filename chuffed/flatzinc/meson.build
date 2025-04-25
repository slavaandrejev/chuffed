fzn_lexer = custom_target(
    input: 'lexer.lxx'
  , output: 'lexer.yy.cpp'
  , command: [
        flex
      , '-L'
      , '-o', '@OUTPUT@'
      , '@INPUT@'
      ]
  )

fzn_parser = custom_target(
    input: 'parser.yxx'
  , output: ['parser.tab.cpp', 'parser.tab.h']
  , command: [
        bison
      , '-o', '@OUTPUT0@'
      , '--defines=@OUTPUT1@'
      , '-l'
      , '@INPUT@'
      ]
  )

install_headers(
    'ast.h'
  , 'flatzinc.h'
  , subdir: 'chuffed/flatzinc'
  )

chuffed_fzn_lib = static_library(
    'chuffed_fzn'
  , 'flatzinc.cpp'
  , 'fzn-chuffed.cpp'
  , 'registry.cpp'
  , fzn_lexer
  , fzn_parser
  , include_directories: [project_inc]
  , cpp_args: [
        '-Wno-unused-function'
      , '-Wno-unneeded-internal-declaration'
      , '-Wno-unused-but-set-variable'
      ]
  , install: true
  )

chuffed_fzn_dep = declare_dependency(
    link_with: chuffed_fzn_lib
  , include_directories: [project_inc]
  )

pkg.generate(
    name: 'chuffed_fzn'
  , filebase: 'chuffed_fzn'
  , description: 'FlatZinc interface for Chuffed constraint solver'
  , version: meson.project_version()
  , libraries: chuffed_fzn_lib
  )

install_subdir(
    'mznlib'
  , install_dir: get_option('datadir') / 'minizinc' / 'chuffed'
  , strip_directory: true
  )
