load('//:buckaroo_macros.bzl', 'buckaroo_deps_from_package')
load('//:subdir_glob.bzl', 'subdir_glob')

null_srcs = glob([
  'src/**/null_*.c',
])

linux_srcs = glob([
  'src/**/posix_*.c',
  'src/**/linux_*.c',
])

macos_srcs = glob([
  'src/**/cocoa_*.c',
  'src/**/*.m',
])

windows_srcs = glob([
  'src/**/win32*.c',
])

wayland_srcs = glob([
  'src/**/wl_*.c',
  'src/**/wgl_*.c',
])

x11_srcs = glob([
  'src/**/x11_*.c',
])

platform_srcs = \
  null_srcs + linux_srcs + macos_srcs + \
  windows_srcs + wayland_srcs + x11_srcs

linux_pp_flags = [
  '-D_GLFW_X11=1',
]

linux_deps = \
  buckaroo_deps_from_package('github.com/buckaroo-pm/pkg-config-gl') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/pkg-config-x11') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-dl') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-pthread')

macos_deps = \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-cocoa') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-io-kit') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-core-foundation') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-core-video')

cxx_library(
  name = 'glfw',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', '**/*.h'),
  ]),
  headers = subdir_glob([
    ('src', '**/*.h'),
  ]),
  platform_preprocessor_flags = [
    ('linux.*', linux_pp_flags),
  ],
  srcs = glob([
    'src/**/*.c',
  ], exclude = platform_srcs),
  platform_srcs = [
    ('linux.*', linux_srcs + x11_srcs),
    ('macos.*', macos_srcs),
    ('windows.*', windows_srcs),
  ],
  licenses = [
    'LICENSE.md',
  ],
  platform_deps = [
    ('linux.*', linux_deps),
    ('macos.*', macos_deps),
  ],
  visibility = [
    'PUBLIC',
  ],
)
