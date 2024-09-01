py.extension_module(
  '_geometry_helpers',
  cython_gen.process('_geometry_helpers.pyx'),
  include_directories: '../src',
  dependencies : [py_dep, geos_dep],
  install: true,
  subdir: 'shapely',
)

py.extension_module(
  '_geos',
  cython_gen.process('_geos.pyx'),
  include_directories: '../src',
  dependencies : [py_dep, geos_dep],
  install: true,
  subdir: 'shapely',
)

py.install_sources([
    '__init__.py',
    '_enum.py',
    '_geometry.py',
    '_ragged_array.py',
    '_version.py',
    'affinity.py',
    'constructive.py',
    'coordinates.py',
    'coords.py',
    'creation.py',
    'decorators.py',
    'errors.py',
    'geos.py',
    'io.py',
    'linear.py',
    'measurement.py',
    'ops.py',
    'plotting.py',
    'predicates.py',
    'prepared.py',
    'set_operations.py',
    'speedups.py',
    'strtree.py',
    'testing.py',
    'validation.py',
    'wkb.py',
    'wkt.py',
  ],
  subdir: 'shapely',
)

subdir('algorithms')
subdir('geometry')
subdir('tests')
subdir('vectorized')
