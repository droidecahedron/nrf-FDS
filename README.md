# nrf-FDS
example of using FDS in nRF5 SDK for test read/write in an application
(from SDK17.1 for nRF52840)

|FDS documentation|
|---|
|[FDS Infocenter Main Page](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_fds.html&cp=9_1_3_16)|
|[FDS Functionality](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_fds_functionality.html)|
|[FDS Usage Example](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.1.0%2Flib_fds_usage.html)|



### Notes
> in sdk_config.h for bootloader, for lining up `NRF_DFU_APP_DATA_AREA_SIZE`, it must be a multiple of flash page size in bytes. Since  `FDS_VIRTUAL_PAGES=3`
> 
> (1024 words / page) x (4 bytes/word) x (3 pages) = `12288`
> If you were to change it to 2 for example, it would be `8192`.


![image](https://github.com/droidecahedron/nrf-FDS/assets/63935881/12805487-bf19-49bb-bf3b-55a45caa022d)

(from functionality section in infocenter... be mindful of which values are used for file IDs and record key values.)

(Also, from the default, bump up number of pages reserved for FDS)
> Should be three when the PM is used. That's two actual pages and one for garbage collection. If your app is going to use the FDS as well then you should bump it by at least a page (though you'll get away with not bumping if your usage is light).

### Extra Wisdom:
- The bootloader doesn't use the FDS itself because that would balloon the size too much.
  - So you build the bootloader with a constant that tells it how many pages of flash to avoid, working backwards from its own start addr.
- That space gets subtracted from the space that it can use for staging firmware updates. This keeps your settings safe during DFU, but only if you want it to.
- The end of the MBR (Master Boot Record)[^1] is fixed. The end of the SD can be read at runtime (its hex file has an info struct). The start addr of the bootloader is set in one of two locations. From that info the fstorage module and bootloader can work things out.
- The end of the SD is also given to the application so it's known to the linker.

[^1]: The main functionality of the MBR is to provide an interface to allow in-system updates of the application, the SoftDevice, and bootloader firmware.
