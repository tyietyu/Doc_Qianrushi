 

[澄清 BSC slave - Raspberry Pi 论坛](https://forums.raspberrypi.com/viewtopic.php?p=810290#p810290.)

 

https://raspberrypi.stackexchange.com/questions/30074/raspberry-pi-as-slave

 

https://www.raspberrypi.org/forums/viewtopic.php?&t=5125

 

http://offis.github.io/raspi-directhw/group__spisl.html

 

使用PIO编程spi_slave

 

.program spi_slave

  .define public CS 5

  .define public SCK 6;

Pins;

MOSI IN pin 0;

MISO OUT pin 0 pull

​    loop1 : wait 0 gpio CS

​    loop2 : out pins 1 wait 0 gpio SCK

in pins 1 wait 1 gpio SCK

jmp loop1 

% c-sdk

{

  static inline void spi_slave_program_init(

​    PIO pio, uint sm, uint offset, uint cs, uint sck, uint mosi, uint miso)

  {

​    pio_sm_config c = spi_slave_program_get_default_config(offset);

​    sm_config_set_out_pins(&c, miso, 1);

​    sm_config_set_in_pins(&c, mosi);

​    sm_config_set_in_shift(&c, false, true, 8);

​    sm_config_set_out_shift(&c, false, true, 8);

​    pio_gpio_init(pio, cs);

​    pio_gpio_init(pio, sck);

​    pio_gpio_init(pio, miso);

​    pio_gpio_init(pio, mosi);

​    pio_sm_set_consecutive_pindirs(pio, sm, miso, 1, true);

​    pio_sm_init(pio, sm, offset, &c);

​    pio_sm_set_enabled(pio, sm, true);

  }

%}

 

C 代码

 

\#include <stdio.h>

\#include "pico/stdlib.h"

\#include "hardware/pio.h"

// the assembled program

\#include "pgs_spi_slave.pio.h"

int my_pio(uint8_t **inBuf*, uint8_t **outBuf*, int *count*)

{

  int in, out;

  PIO pio = pio0;

  uint offset = pio_add_program(pio, &spi_slave_program);

  uint sm = pio_claim_unused_sm(pio, true);

  spi_slave_program_init(pio, sm, offset, spi_slave_CS, spi_slave_SCK, 4, 7);

  out = 0;

  for (in = 0; in < count; in++)

  {

​    pio_sm_put_blocking(pio, sm, inBuf[in] << 24);

​    while (!pio_sm_is_rx_fifo_empty(pio, sm))

​    {

​      outBuf[out++] = pio_sm_get(pio, sm);

​    }

  }

  sleep_us(50);

  while (!pio_sm_is_rx_fifo_empty(pio, sm))

  {

​    outBuf[out++] = pio_sm_get(pio, sm);

  }

}

来自 <https://forums.raspberrypi.com/viewtopic.php?t=300589&sid=1ec51eaee9ed1bebfb2a90a143848da2&start=25> 

 

 

 

Spi_slave Mode正常的例程

 

 

// #include "types.h"

\#include <stdio.h>

\#include <inttypes.h>

\#include "pico/stdlib.h"

\#include "hardware/gpio.h"

\#include "hardware/spi.h"

\#include "pico/bootrom.h"

\#define LED_PIN (25u)

\#define SPI_PIN_MISO (0u)

\#define SPI_PIN_CS (1u)

\#define SPI_PIN_CLK (2u)

\#define SPI_PIN_MOSI (3u)

// Shamelessly stolen from the button example.

\#include "hardware/sync.h" // save_and_disable_interrupts()...

\#include "hardware/structs/ioqspi.h"

\#include "hardware/structs/sio.h"

bool __no_inline_not_in_flash_func(get_bootsel_button)(void)

{

  const uint CS_PIN_INDEX = 1;

  // Must disable interrupts, as interrupt handlers may be in flash, and we

  // are about to temporarily disable flash access!

  uint32_t flags = save_and_disable_interrupts();

  // Set chip select to Hi-Z

  hw_write_masked(&ioqspi_hw->io[CS_PIN_INDEX].ctrl,

​          GPIO_OVERRIDE_LOW << IO_QSPI_GPIO_QSPI_SS_CTRL_OEOVER_LSB,

​          IO_QSPI_GPIO_QSPI_SS_CTRL_OEOVER_BITS);

  // Note we can't call into any sleep functions in flash right now

  for (volatile int i = 0; i < 1000; ++i)

​    ;

  // The HI GPIO registers in SIO can observe and control the 6 QSPI pins.

  // Note the button pulls the pin *low* when pressed.

  bool button_state = !(sio_hw->gpio_hi_in & (1u << CS_PIN_INDEX));

  // Need to restore the state of chip select, else we are going to have a

  // bad time when we return to code in flash!

  hw_write_masked(&ioqspi_hw->io[CS_PIN_INDEX].ctrl,

​          GPIO_OVERRIDE_NORMAL << IO_QSPI_GPIO_QSPI_SS_CTRL_OEOVER_LSB,

​          IO_QSPI_GPIO_QSPI_SS_CTRL_OEOVER_BITS);

  restore_interrupts(flags);

  return button_state;

}

int main(void)

{

  stdio_init_all();

  gpio_init(LED_PIN);

  gpio_put(LED_PIN, 0);

  gpio_set_dir(LED_PIN, GPIO_OUT);

  // The 3DS uses 16.756991 MHz.

  // The SDK for some reason picks the next lowest (15625000) clock

  // instead of the next highest (20833333).

  // 15625000 seems to work though.

  spi_init(spi0, 20833333); // At 125 Mhz (default).

  spi_set_format(spi0, 8, SPI_CPOL_1, SPI_CPHA_1, SPI_MSB_FIRST);

  spi_set_slave(spi0, true);

  gpio_set_function(SPI_PIN_MISO, GPIO_FUNC_SPI);

  gpio_set_function(SPI_PIN_CS, GPIO_FUNC_SPI);

  gpio_set_function(SPI_PIN_CLK, GPIO_FUNC_SPI);

  gpio_set_function(SPI_PIN_MOSI, GPIO_FUNC_SPI);

  sleep_ms(1);

  do

  {

​    gpio_put(LED_PIN, 1);

​    uint8_t rxbuf[2];

​    const uint8_t resp[2] = {0xBE, 0xEF};

​    spi_read_blocking(spi0, 0xE1, rxbuf, 2);

​    spi_write_blocking(spi0, resp, 2);

​    printf("Hello from 3DS: %02" PRIX8 "%02" PRIX8 "\n", rxbuf[0], rxbuf[1]);

​    gpio_put(LED_PIN, 0);

​    sleep_ms(1000);

  } while (!get_bootsel_button());

  reset_usb_boot(0, 0);

}

 

 

说明：

1.PIO定义的0号管脚是通过sm_config_set_in_pins(&c, mosi);这个函数的指定的GPIO管脚为0号管脚，之后的其余管脚都是以这一个管脚为基础管脚进行向右偏移得到，例如MOSI ：1 CS：3 CLK：5；对应PIO程序的管脚设置:

 

.program spi_cpha0_rx

 

; 引脚定义说明:

; 这里的引脚编号是相对于基础引脚(base pin)的偏移量，而非实际的GPIO编号

; 引脚可以不连续分配，只需确保实际GPIO编号大于基础引脚编号，从而保证

偏移量为正值

 

; 例如：如果在C代码中设置基础引脚为GPIO 16，则:

; PIN 0 -> MOSI (GPIO 16) - 数据输入引脚

; PIN 1 -> CS  (GPIO 17) - 片选信号

; PIN 2 -> SCK (GPIO 18) - 时钟信号

; 实际使用时需要通过sm_config_set_in_pins()等函数设置基础引脚（即PIN 0）

 

; 以下仅针对 pico 作为从机接收 spi mode 0 格式(CPOL=0, CPHA=0)

;.wrap_target

;wait 0 pin 1   ; 等待CS信号激活(低电平)

 

;wait_rising:

;wait 1 pin 2   ; Wait for rising clock edge on SCK (pin 2)

;in pins, 1    ; Read RX (pin 0) into the ISR, data is captured on clock's rising edge

 

;wait_falling:

;wait 0 pin 2   ; Wait for the falling edge of SCK to complete the cycle

 

;.wrap

 

 

; 以下仅针对 pico 作为从机接收 spi mode 1 格式(CPOL=0, CPHA=1)

.wrap_target

wait 0 pin 1   ; 等待CS信号激活(低电平)

 

; Mode 1 (CPHA=1)中，第一个边沿是上升沿，但不采样数据

wait 1 pin 2   ; 等待第一个时钟上升沿

 

wait 0 pin 2   ; 等待时钟下降沿

in pins, 1    ; 在下降沿采样MOSI数据，读取1位到ISR

 

.wrap

 