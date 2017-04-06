# nrf51-sdk
Wrapper for Nordic NRF51 SDK, for using meson as a build system.

# Prerequisites
This SDK needs the Nordic SDK 12.2.0 (latest stable to date) to be installed in `/opt/nrf51-sdk`. If you want to change
the path, simply edit the `nrf51-sdk` link to point to the right direction.

# How to install
Then from your project, include this directory as a meson subproject, and use the variables
`c_flags`, `i` and `l` to get the compilation flags (including some args that are ifdef'd in
the SDK headers), the include dirs, and the static library containing the SDK. For example:


    project/
        subprojects/
            sdk -> This repo
    meson.build
    main.c
    
The meson.build file you would use from the project would look like:

    project('app', 'c')
    sdk = subproject('sdk')

    [... yada yada ...]
    
    e = executable('app', files('main.c'),
            link_with: [ sdk.get_variable('l')],
            link_args: sdk.get_variable('link_args'),
            c_args: sdk.get_variable('c_args'))

Lots of modules are made optional through mesonconf.

To build your project, a cross-file for PCA10028 boards is added, called `pca10028.cross`, it
can be used after setting the exe_wrapper to something else (like the `true` program, for example)
To use it simply start the build with the command

    # meson build --cross-file=<path to sdk>/pca10028.cross
