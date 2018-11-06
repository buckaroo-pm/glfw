macos_sources = glob([
  'src/cocoa_*.c',
  'src/cocoa_*.m',
  'src/nsgl_*.m',
  'src/posix_tls.c',
  'src/vulkan.c',
])

linux_sources = glob([
  'src/linux_*.c',
])

posix_sources = glob([
  'src/posix_*.c',
])

mir_sources = glob([
  'src/mir_*.c',
])

egl_sources = glob([
  'src/egl_*.c',
])

x11_sources = glob([
  'src/x11_*.c',
])

glx_sources = glob([
  'src/glx_*.c',
])

wgl_sources = glob([
  'src/wgl_*.c',
])

wl_sources = glob([
  'src/wl_*.c',
])

windows_sources = glob([
  'src/win32_*.c',
])

vulkan_sources = glob([
  'src/vulkan.c',
])

platform_sources = macos_sources + windows_sources + linux_sources + posix_sources + \
                   mir_sources + x11_sources + glx_sources + egl_sources + \
                   wgl_sources + wl_sources + vulkan_sources

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

linux_linker_flags = [
  '-lX11', 
]

macos_exported_linker_flags = [
  '-framework', 'Cocoa',
  '-framework', 'IOKit',
  '-framework', 'CoreVideo',
]

prebuilt_cxx_library(
  name = 'pthread', 
  header_only = True, 
  exported_linker_flags = [
    '-lpthread', 
  ],
)

prebuilt_cxx_library(
  name = 'x11', 
  header_only = True, 
  exported_linker_flags = [
    '-lX11', 
    '-lXrandr', 
    '-lXi', 
    '-lXxf86vm', 
    '-lXcursor', 
    '-lXinerama', 
  ],
)

prebuilt_cxx_library(
  name = 'dl', 
  header_only = True, 
  exported_linker_flags = [
    '-ldl', 
  ],
)

cxx_library(
  name = 'glfw',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'GLFW/*.h'),
  ]),
  headers = subdir_glob([
    ('src', '*.h'),
  ]),
  srcs = glob([
    'src/*.c',
  ], excludes = platform_sources),
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
    # TODO: Add user switch for x11 etc. 
    ('^linux.*', linux_sources + posix_sources + vulkan_sources + egl_sources + glx_sources + x11_sources), 
    ('^windows.*', windows_sources),
  ],
  compiler_flags = [
    '-D_GLFW_BUILD_DLL',
  ],
  platform_compiler_flags = [
    ('default', macos_flags),
    ('^macos.*', macos_flags),
    ('^linux.*', linux_flags),
    ('^windows.*', windows_flags),
  ], 
  exported_platform_linker_flags = [
    ('default', macos_exported_linker_flags),
    ('^macos.*', macos_exported_linker_flags),
  ],
  deps = [
    '//deps:tinycthread', 
    '//deps:glad', 
    '//deps:linmath', 
    '//deps:vulkan', 
  ], 
  platform_deps = [
    ('^linux.*', [ ':pthread', ':dl', ':x11' ]), 
  ], 
  visibility = [
    'PUBLIC',
  ],
)
