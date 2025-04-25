if get_option('enable_var_impact')
    add_project_arguments('-DHAS_VAR_IMPACT', language: ['cpp'])
    chuffed_lib_files += files('impact.cpp')
    install_headers(
        'impact.h'
      , subdir: 'chuffed/branching'
      )
endif

chuffed_lib_files += files('branching.cpp')

install_headers(
    'branching.h'
  , subdir: 'chuffed/branching'
  )
