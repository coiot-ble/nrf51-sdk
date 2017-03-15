project('nrf51-sdk', 'C')

assert(meson.is_cross_build(), 'SDK must be cross-compiled')

dir = 'nrf51-sdk'
dir_components = dir + '/components'
dir_lib = dir_components + '/libraries'
dir_nrf = dir_components + '/drivers_nrf'
dir_ble = dir_components + '/ble'
dir_ble_sv = dir_ble + '/ble_services'
dir_softdevice = dir_components + '/softdevice'
dir_toolchain = dir_components + '/toolchain'


# try to add source file automatically...
SOURCE_AUTO = true
# ... or not (name is different from option name)
NO_SOURCE_AUTO = false
options = [
    [dir_lib, SOURCE_AUTO, ['sensorsim']],
    [dir_ble, SOURCE_AUTO, ['ble_advertising', 'ble_db_discovery', 'ble_dtm', 'ble_racp',
      'nrf_ble_qwr']],
    [dir_ble_sv, NO_SOURCE_AUTO, ['ble_ancs_c']],
    [dir_ble_sv, SOURCE_AUTO, ['ble_ans_c', 'ble_bas_c', 'ble_bas', 'ble_cscs',
      'ble_cts_c', 'ble_dis', 'ble_gls', 'ble_hids', 'ble_hrs_c', 'ble_hrs', 'ble_hts',
      'ble_ias_c', 'ble_ias', 'ble_lbs_c', 'ble_lbs', 'ble_lls', 'ble_nus_c', 'ble_nus',
      'ble_rscs_c', 'ble_rscs', 'ble_tps']],
    ]

cdata = configuration_data()
foreach optgrps : options
  foreach opt: optgrps[2]
    cdata.set10(opt.to_upper() + '_ENABLED', get_option(opt))
  endforeach
endforeach
configure_file(output:'sdk_options.h', configuration: cdata)

options_s = []
options_i = []
foreach optgrp : options
  optdir = optgrp[0]
  foreach opt : optgrp[2]
    options_i += optdir + '/' + opt
    if optgrp[1] == SOURCE_AUTO
      options_s += optdir + '/' + opt + '/' + opt + '.c'
    endif
  endforeach
endforeach

if get_option('ble_ancs_c')
  foreach f: ['ancs_app_attr_get.c', 'ancs_attr_parser.c', 'ancs_tx_buffer.c', 'nrf_ble_ancs_c.c']
    options_s += dir_ble_sv + '/ble_ancs_c/' + f
  endforeach
endif

if get_option('ble_cscs')
  options_s += dir_ble_sv + '/ble_cscs/ble_sc_ctrlpt.c'
endif

if get_option('ble_dtm')
  options_s += dir_ble + '/ble_dtm/ble_dtm_hw_nrf51.c'
endif

if get_option('ble_gls')
  options_s += dir_ble + '/ble_gls/ble_gls_db.c'
endif


bsp = files(
    dir_lib + '/log/src/nrf_log_backend_serial.c',
    dir_lib + '/log/src/nrf_log_frontend.c',
    dir_lib + '/button/app_button.c',
    dir_lib + '/util/app_error.c',
    dir_lib + '/util/app_error_weak.c',
    dir_lib + '/timer/app_timer.c',
    dir_lib + '/crc16/crc16.c',
    dir_lib + '/fds/fds.c',
    dir_lib + '/fstorage/fstorage.c',
    dir_lib + '/hardfault/hardfault_implementation.c',
    dir_lib + '/fifo/app_fifo.c',
    dir_lib + '/uart/app_uart_fifo.c',
    dir_lib + '/util/app_util_platform.c',
    dir_lib + '/util/nrf_assert.c',
    dir_lib + '/uart/retarget.c',
    dir_lib + '/util/sdk_errors.c',
    dir_lib + '/util/sdk_mapped_flags.c',
    dir_components + '/boards/boards.c',
    dir_nrf + '/clock/nrf_drv_clock.c',
    dir_nrf + '/common/nrf_drv_common.c',
    dir_nrf + '/gpiote/nrf_drv_gpiote.c',
    dir_nrf + '/uart/nrf_drv_uart.c',
    dir_lib + '/bsp/bsp.c',
    dir_lib + '/bsp/bsp_btn_ble.c',
    dir_lib + '/bsp/bsp_nfc.c',
    dir_ble + '/common/ble_advdata.c',
    dir_ble + '/common/ble_conn_params.c',
    dir_ble + '/common/ble_conn_state.c',
    dir_ble + '/common/ble_srv_common.c',
    dir_ble + '/peer_manager/gatt_cache_manager.c',
    dir_ble + '/peer_manager/gatts_cache_manager.c',
    dir_ble + '/peer_manager/id_manager.c',
    dir_ble + '/peer_manager/peer_data.c',
    dir_ble + '/peer_manager/peer_data_storage.c',
    dir_ble + '/peer_manager/peer_database.c',
    dir_ble + '/peer_manager/peer_id.c',
    dir_ble + '/peer_manager/peer_manager.c',
    dir_ble + '/peer_manager/pm_buffer.c',
    dir_ble + '/peer_manager/pm_mutex.c',
    dir_ble + '/peer_manager/security_dispatcher.c',
    dir_ble + '/peer_manager/security_manager.c',
    dir_ble_sv + '/ble_ans_c/ble_ans_c.c',
    dir_softdevice + '/common/softdevice_handler/softdevice_handler.c',
    )
bsp_inc = include_directories(
    dir_nrf + '/comp',
    dir_nrf + '/twi_master',
    dir_ble_sv + '/ble_ancs_c',
    dir_ble_sv + '/ble_ias_c',
    dir_softdevice + '/s130/headers',
    dir_lib + '/pwm',
    dir_lib + '/usbd/class/cdc/acm',
    dir_lib + '/usbd/class/hid/generic',
    dir_lib + '/usbd/class/msc',
    dir_lib + '/usbd/class/hid',
    dir_lib + '/log',
    dir_ble_sv + '/ble_gls',
    dir_lib + '/fstorage',
    dir_nrf + '/i2s',
    dir_lib + '/gpiote',
    dir_nrf + '/gpiote',
    dir_components + '/boards',
    dir_nrf + '/common',
    dir_nrf + '/adc',
    dir_softdevice + '/s130/headers/nrf51',
    dir_ble_sv + '/ble_bas_c',
    dir_ble_sv + '/ble_hrs_c',
    dir_lib + '/queue',
    dir_toolchain + '/cmsis/include',
    dir_ble_sv + '/ble_rscs_c',
    dir_nrf + '/uart',
    dir_ble + '/common',
    dir_ble_sv + '/ble_lls',
    dir_nrf + '/wdt',
    dir_lib + '/bsp',
    dir_ble_sv + '/ble_bas',
    dir_lib + '/experimental_section_vars',
    dir_ble_sv + '/ble_ans_c',
    dir_lib + '/slip',
    dir_lib + '/mem_manager',
    dir_lib + '/csense_drv',
    dir_nrf + '/hal',
    dir_ble_sv + '/ble_nus_c',
    dir_nrf + '/rtc',
    dir_ble_sv + '/ble_ias',
    dir_lib + '/usbd/class/hid/mouse',
    dir_nrf + '/ppi',
    dir_ble_sv + '/ble_dfu',
    dir_nrf + '/twis_slave',
    dir_components + '/',
    dir_lib + '/scheduler',
    dir_ble_sv + '/ble_lbs',
    dir_ble_sv + '/ble_hts',
    dir_nrf + '/delay',
    dir_lib + '/crc16',
    dir_nrf + '/timer',
    dir_lib + '/util',
    dir_nrf + '/pwm',
    dir_lib + '/usbd/class/cdc',
    dir_lib + '/csense',
    dir_nrf + '/rng',
    dir_lib + '/low_power_pwm',
    dir_lib + '/hardfault',
    dir_ble_sv + '/ble_cscs',
    dir_lib + '/uart',
    dir_lib + '/hci',
    dir_lib + '/usbd/class/hid/kbd',
    dir_nrf + '/spi_slave',
    dir_nrf + '/lpcomp',
    dir_lib + '/timer',
    dir_nrf + '/power',
    dir_lib + '/usbd/config',
    dir_toolchain + '',
    dir_lib + '/led_softblink',
    dir_nrf + '/qdec',
    dir_ble_sv + '/ble_cts_c',
    dir_nrf + '/spi_master',
    dir_ble_sv + '/ble_nus',
    dir_ble_sv + '/ble_hids',
    dir_nrf + '/pdm',
    dir_lib + '/crc32',
    dir_lib + '/usbd/class/audio',
    dir_ble + '/peer_manager',
    dir_nrf + '/swi',
    dir_ble_sv + '/ble_tps',
    dir_ble_sv + '/ble_dis',
    dir_components + '/device',
    dir_lib + '/button',
    dir_lib + '/usbd',
    dir_nrf + '/saadc',
    dir_ble_sv + '/ble_lbs_c',
    dir_lib + '/fds',
    dir_lib + '/twi',
    dir_nrf + '/clock',
    dir_ble_sv + '/ble_rscs',
    dir_nrf + '/usbd',
    dir_softdevice + '/common/softdevice_handler',
    dir_ble_sv + '/ble_hrs',
    dir_lib + '/log/src'
)


segger_rtt = files(
    dir + '/external/segger_rtt/RTT_Syscalls_GCC.c',
    dir + '/external/segger_rtt/SEGGER_RTT.c',
    dir + '/external/segger_rtt/SEGGER_RTT_printf.c'
)
segger_rtt_inc = include_directories(
    dir + '/external/segger_rtt'
)

nrf51_gcc = files(
    dir_toolchain + '/gcc/gcc_startup_nrf51.S',
    dir_toolchain + '/system_nrf51.c'
)
nrf51_gcc_inc = include_directories(
    dir_toolchain + '/gcc'
)

i = [include_directories('.'), bsp_inc, segger_rtt_inc, nrf51_gcc_inc, include_directories(options_i)]

gcclink = meson.current_source_dir() + '/' + dir_toolchain + '/gcc'
link_args = ['-Wl,-L' + gcclink, '-L ' + gcclink ]

l = static_library('sdk', bsp, segger_rtt, nrf51_gcc,
    sources: options_s,
    include_directories: i,
    c_args: [
      '-Wall', '-Werror',
      '-O3', '-g3',
      '-DNRF_LOG_ENABLED', '-DNRF_LOG_DEFAULT_LEVEL=4',
      '-DSWI_DISABLE0', '-DBLE_STACK_SUPPORT_REQD',
      '-DSOFTDEVICE_PRESENT','-DS130','-DNRF_SD_BLE_API_VERSION=2'],
    link_args: link_args)

cmd_flash_softdevice = ['nrfjprog', '--program', meson.current_source_dir() + '/' + dir_softdevice + '/s130/hex/s130_nrf51_2.0.1_softdevice.hex', '--verify', '-f', 'nrf51', '--sectorerase', '-r']
