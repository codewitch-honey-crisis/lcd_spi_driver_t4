# lcd_spi_driver_t4

A DMA enabled SPI driver for LCDs. This driver provides shared code for implementing different SPI connected LCD devices on the Teensy 4.x

To implement a driver:

Publicly derive from lcd_spi_driver and implement `initialize()`, `set_rotation()` and `write_address_window()`

See https://github.com/codewitch-honey-crisis/ssd1351_t4, https://github.com/codewitch-honey-crisis/st7789_t4, and https://github.com/codewitch-honey-crisis/ili9341_t4 for example implementations.

To use a driver:

These drivers are not full fledged graphics libraries. They are meant for use with libraries like [LVGL](https://lvgl.io) and [UIX](https://honeythecodewitch.com/uix) which produce bitmaps.

All drivers deriving from this support `begin()`, `rotation()`, `flush()`, `flush_async()` and `on_flush_complete_callback()`

1. `begin()` initializes the display driver and must be called before any of the other functions

2. `rotation()` takes a single integer 0-3 which indicate the number of clockwise 90 degree rotations from top dead center.

3. `flush()` takes integer coordinates and a pointer to bitmap data. It synchronously writes the bitmap at the specified coordinates. This routine blocks.

4. `flush_async()` takes integer coordinates, a pointer to bitmap data, and a boolean indicating whether to flush the CPU data cache for that bitmap. It asynchronously writes the bitmap to the display. The cache does not need to be cleared if the bitmap is in DTCM memory such as defined as a static array, but otherwise it should be cleared.

5. `on_flush_complete_callback()` takes a callback method and an optional user defined state argument. The callback is invoked whenever a flush completes (whether asynchronous or synchronous) and the state is passed along with the callback.
