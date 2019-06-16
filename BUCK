load('//:buckaroo_macros.bzl', 'buckaroo_deps_from_package')
load('//:subdir_glob.bzl', 'subdir_glob')

null_srcs = glob([
  'src/**/null_*.c',
])

macos_srcs = glob([
  'src/cocoa_*.c',
  'src/cocoa_*.m',
  'src/nsgl_*.m',
  'src/posix_tls.c',
  'src/vulkan.c',
])

linux_srcs = glob([
  'src/linux_*.c',
])

posix_srcs = glob([
  'src/posix_*.c',
])

mir_srcs = glob([
  'src/mir_*.c',
])

egl_srcs = glob([
  'src/egl_*.c',
])

x11_srcs = glob([
  'src/x11_*.c',
])

glx_srcs = glob([
  'src/glx_*.c',
])

wgl_srcs = glob([
  'src/wgl_*.c',
])

wl_srcs = glob([
  'src/wl_*.c',
])

windows_srcs = glob([
  'src/win32_*.c',
])

vulkan_srcs = glob([
  'src/vulkan.c',
])

platform_srcs = \
  null_srcs + macos_srcs + windows_srcs + linux_srcs + \
  posix_srcs + mir_srcs + x11_srcs + glx_srcs + \
  egl_srcs + wgl_srcs + wl_srcs + vulkan_srcs

macos_flags = [
  '-D_GLFW_COCOA',
  '-D_GLFW_USE_MENUBAR',
  '-D_GLFW_USE_RETINA',
]

linux_flags = [
  '-D_GLFW_X11',
]

windows_flags = [
  '-D_GLFW_WIN32',
]

macos_exported_linker_flags = [
  '-framework', 'Cocoa',
  '-framework', 'IOKit',
  '-framework', 'CoreVideo',
]

windows_exported_linker_flags = [
  'User32.lib',
  'Gdi32.lib',
  'Shell32.lib',
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
    ('include', 'GLFW/*.h'),
  ]),
  headers = subdir_glob([
    ('src', '*/*.h'),
  ]),
  srcs = glob([
    'src/**/*.c',
  ], exclude = platform_srcs),
  platform_srcs = [
    ('linux.*', linux_srcs + posix_srcs + x11_srcs + egl_srcs + glx_srcs + vulkan_srcs),
    ('macos.*', macos_srcs),
    ('windows.*', windows_srcs + egl_srcs + wgl_srcs + vulkan_srcs),
  ],
  compiler_flags = [
    '-D_GLFW_BUILD_DLL',
  ],
  platform_compiler_flags = [
    ('^macos.*', macos_flags),
    ('^linux.*', linux_flags),
    ('^windows.*', windows_flags),
  ],
  exported_platform_linker_flags = [
    ('^macos.*', macos_exported_linker_flags),
    ('^windows.*', windows_exported_linker_flags),
  ],
  platform_deps = [
    ('linux.*', linux_deps),
    ('macos.*', macos_deps),
  ],
  licenses = [
    'LICENSE.md',
  ],
  visibility = [
    'PUBLIC',
  ],
)
