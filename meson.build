project(
  'shapely',
  'c', 'cython',
  version: run_command(['generate_version.py', '--print'], check: true).stdout().strip(),
  license: 'BSD-3'
)

py = import('python').find_installation(pure: false)
py_dep = py.dependency()
cy = meson.get_compiler('cython')
versioneer = files('generate_version.py')

# generator() doesn't accept compilers, only found programs - cast it.
cython = find_program(cy.cmd_array()[0])
cython_args = ['-3', '--fast-fail', '--output-file', '@OUTPUT@', '--include-dir', '@BUILD_ROOT@', '@INPUT@']
cython_gen = generator(cython,
  arguments : cython_args,
  output : '@BASENAME@.c',
)

incdir_numpy = run_command(py,
  ['-c', 'import os; os.chdir(".."); import numpy; print(numpy.get_include())'],
  check : true
).stdout().strip()
inc_np = include_directories(incdir_numpy)

geos_dep = dependency('geos')

add_project_arguments('-DNPY_NO_DEPRECATED_API=0', language : 'c')
add_project_arguments('-DNPY_TARGET_VERSION=NPY_1_20_API_VERSION', language : 'c')

subdir('src')
subdir('shapely')
