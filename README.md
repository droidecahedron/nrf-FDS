# nrf-FDS
example of using FDS in nRF for test read/write in an application

## [FDS documentation](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_fds.html&cp=9_1_3_16)

### [FDS Usage Example](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_fds_usage.html)

in sdk_config.h, for lining up `NRF_DFU_APP_DATA_AREA_SIZE`, it must be a multiple of flash page size in bytes. Since  `FDS_VIRTUAL_PAGES=3`

(1024 words / page) x (4 bytes/word) x (3 pages) = 12288
If you were to change it to 2 for example, it would be 8192.
