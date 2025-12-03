project(
  'shapely',
  'c', 'cython',
  version: '2.2.0.dev0',
  license: 'BSD-3',
  meson_version: '>=1.2.1',
  default_options: ['buildtype=release', 'c_std=c11', 'warning_level=2'],
)

py = import('python').find_installation(pure: false)
py_dep = py.dependency()

# Previous shapely versions permitted multiple ways to specify GEOS dependency
# which are not supported (in principle)
ignored_env_vars = run_command(
    py,
    [
        '-c',
        '''
from os import environ
for name in ['GEOS_INCLUDE_PATH', 'GEOS_LIBRARY_PATH', 'GEOS_CONFIG']:
    if name in environ:
        print(name + "=" + environ[name])
        ''',
    ],
    check: true,
).stdout().strip().split()
if ignored_env_vars != []
    warning(
        'These previously-supported environment variable(s) are not supported:',
        ignored_env_vars,
        '; use `cmake_prefix_path` approach instead')
endif

# A custom GEOS installation can be provided with:
# 1) -Dpkg_config_path=/path/to/geos-install/lib/pkgconfig
# 2) -Dcmake_prefix_path=/path/to/geos-install

if get_option('pkg_config_path') != []
  geos_dep_method = 'pkg-config'
elif get_option('cmake_prefix_path') != []
  geos_dep_method = 'cmake'
else  # default
  geos_dep_method = 'auto'
endif

geos_dep = dependency('geos', version : '>=3.10',
  required : false,  # e.g. sdist build does not require geos
  method : geos_dep_method)

cy = meson.get_compiler('cython')

# generator() doesn't accept compilers, only found programs - cast it.
cython = find_program(cy.cmd_array()[0])
cython_args = ['-3', '--fast-fail', '--output-file', '@OUTPUT@', '--include-dir', '@BUILD_ROOT@', '@INPUT@']
cython_gen = generator(cython,
  arguments : cython_args,
  output : '@BASENAME@.c',
)

# does not work for numpy <2?
numpy_dep = dependency('numpy', required : false, version : '>=1.25')

incdir_numpy = run_command(
    py,
    [
        '-c',
        '''
import os
import numpy as np
try:
    # Check if include directory is inside the shapely dir
    # e.g. a venv created inside the shapely dir
    # If so, convert it to a relative path
    incdir = os.path.relpath(np.get_include())
except Exception:
    incdir = np.get_include()
print(incdir)
        ''',
    ],
    check: true,
).stdout().strip()
inc_np = include_directories(incdir_numpy)

if not numpy_dep.found()
  numpy_dep = declare_dependency(include_directories : inc_np)
endif

add_project_arguments('-DNPY_NO_DEPRECATED_API=0', language : 'c')
add_project_arguments('-DNPY_TARGET_VERSION=NPY_1_23_API_VERSION', language : 'c')

subdir('src')
subdir('shapely')
