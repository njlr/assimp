include_defs('//BUCKAROO_DEPS')

def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

genrule(
  name = 'revision.h',
  out = 'revision.h',
  cmd = 'echo "#ifndef ASSIMP_REVISION_H_INC \n' +
  '#define ASSIMP_REVISION_H_INC \n' +
  ' \n' +
  '#define GitVersion 0x0 \n' +
  '#define GitBranch "buckaroo-managed-dep" \n' +
  ' \n' +
  '#endif // ASSIMP_REVISION_H_INC\n" > $OUT',
)

windows_sources = glob([
  'code/C4DImporter.cpp',
])

platform_sources = windows_sources

macos_flags = [
  '-DASSIMP_BUILD_NO_C4D_IMPORTER',
]

linux_flags = [
  '-DASSIMP_BUILD_NO_C4D_IMPORTER',
]

prebuilt_cxx_library(
  name = 'assimp-headers',
  header_only = True,
  header_namespace = '',
  exported_headers = subdir_glob([
    ('code', '**/*.h'),
  ]),
  visibility = [
    '//...',
  ],
)

cxx_library(
  name = 'assimp',
  header_namespace = '',
  exported_headers = merge_dicts(subdir_glob([
    ('include', 'assimp/**/*.hpp'),
    ('include', 'assimp/**/*.h'),
    ('include', 'assimp/**/*.inl'),
  ]), {
    'revision.h': ':revision.h',
  }),
  srcs = glob([
    'code/**/*.cpp',
    'contrib/irrXML/**/*.cpp',
  ], excludes = platform_sources),
  platform_srcs = [
    ('^windows.*', windows_sources),
  ],
  compiler_flags = [
    '-std=c++11',
    '-DASSIMP_BUILD_NO_OWN_ZLIB', # zlib is managed via Buckaroo
  ],
  platform_compiler_flags = [
    ('default', linux_flags),
    ('^macos.*', macos_flags),
    ('^linux.*', linux_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
  deps = BUCKAROO_DEPS + [
    ':assimp-headers',
    '//contrib/clipper:clipper',
    '//contrib/ConvertUTF:ConvertUTF',
    # '//contrib/irrXML:irrXML',
    '//contrib/openddlparser:openddlparser',
    '//contrib/poly2tri:poly2tri',
    '//contrib/rapidjson:rapidjson',
    '//contrib/unzip:unzip',
  ],
)
