# Project definition
project('rawrrew', 'c',
    version: '0.5.0',
    default_options: ['c_std=c99'],
    meson_version: '>=0.46.0')

# Set compiler warning flags
compiler = meson.get_compiler('c')
compiler_args = compiler.get_supported_arguments([
    '-Wall',
    '-Wmissing-declarations',
    '-Wmissing-prototypes',
    '-Wstrict-prototypes',
    '-Wbad-function-cast',
    '-Wsign-compare',
    '-Wnested-externs',
    '-Wshadow',
    '-Waggregate-return',
    '-Wcast-align',
    '-Wextra',
    '-Wold-style-definition',
    '-Wdeclaration-after-statement',
    '-Wuninitialized',
    '-Wshorten-64-to-32',
    '-pedantic',
])
add_project_arguments(compiler_args, language: 'c')

# Dependency: math
math_dep = compiler.find_library('m', required: false)

# Dependency: re
# Note: We need to force using our own fork until re has accepted all our patches
re_dep = dependency('librawrre',
    version: '>=0.5.9',
    fallback: ['re', 're_dep'],
    required: true)

# Dependencies list
dependencies = [
    math_dep,
    re_dep,
]

# Features: IPv6
add_project_arguments('-DHAVE_INET6', language: 'c')

# Includes
include_dir = include_directories('include')
subdir('include')

# Sources & Modules
# TODO: Make which to build configurable
subdir('src')

# Build library
rew = library(meson.project_name(), sources,
    dependencies: dependencies,
    include_directories: include_dir,
    install: true,
    version: meson.project_version())

# Install headers
install_headers(includes, subdir: 'rew')

# Declare dependency
rew_dep = declare_dependency(
    include_directories: include_dir,
    link_with: rew)

# Generate pkg-config file
pkg = import('pkgconfig')
pkg.generate(rew,
    name: 'librew',
    description: 'A sister library to libre with experimental code',
    url: 'https://github.com/alfredh/rew',
    subdirs: 'rew')
