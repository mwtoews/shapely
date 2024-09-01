py.extension_module(
  'lib', # shapely.lib
  'c_api.c',
  'coords.c',
  'geos.c',
  'lib.c',
  'pygeom.c',
  'pygeos.c',
  'strtree.c',
  'ufuncs.c',
  'vector.c',
  install: true,
  subdir: 'shapely',
  dependencies: [py_dep, geos_dep],
)
