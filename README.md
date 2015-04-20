//# Adafruit_RA8875 *NO PIXEL SUPPORT
//Adafruit's Arduino driver for the RA8875 TFT driver
//BeagleBone Black (BBB) WORKING EXAMPLE
//UPDATE the following functions and add
//********************

//USING SPI0; must show  "P-O-L Override Board Name,00A0,Override Manuf,BB-SPIDEV0"
//cat /sys/devices/bone_capemgr.*/slots
// 0: 54:PF---
// 1: 55:PF---
// 2: 56:PF---
// 3: 57:PF---
// 4: ff:P-O-L Bone-LT-eMMC-2G,00A0,Texas Instrument,BB-BONE-EMMC-2G
// 5: ff:P-O-- Bone-Black-HDMI,00A0,Texas Instrument,BB-BONELT-HDMI
// 6: ff:P-O-- Bone-Black-HDMIN,00A0,Texas Instrument,BB-BONELT-HDMIN
// 7: ff:P-O-L Override Board Name,00A0,Override Manuf,BB-SPIDEV0

static const char *spi_name = "/dev/spidev1.0";
struct spi_ioc_transfer xfer;
char rxBuffer[1];
char txBuffer[1];
uint32_t data= 0;
int res=0;
int spiDev = open(spi_name, O_RDWR);

#define SPI_SPEED_HZ 2000
#define SPI_DELAY 100
uint32_t mode = SPI_MODE_0;

void  writeData(uint8_t d)
{
 // SPI.transfer(RA8875_DATAWRITE);
	  txBuffer[1] = RA8875_DATAWRITE;
	  txBuffer[0] = d ;
	  xfer.tx_buf = ( unsigned long )txBuffer;
	  xfer.rx_buf = ( unsigned long )rxBuffer;
	  xfer.len = 2;
	  xfer.speed_hz = SPI_SPEED_HZ;
	  xfer.cs_change = 1;
	  xfer.bits_per_word = 16;
	  xfer.delay_usecs= SPI_DELAY;
	  ioctl(spiDev,SPI_IOC_WR_MODE, &mode);
	  res = ioctl(spiDev, SPI_IOC_MESSAGE(1), &xfer);

}

uint8_t  readData(void)
{
//  SPI.transfer(RA8875_DATAREAD);
//  uint8_t x = SPI.transfer(0x0);
//  return x;
	  txBuffer[1] = RA8875_DATAREAD;
	  txBuffer[0] = 0x00 ; //0x4E = 0x7, 0x63 = 0x1F //verify operation
	  xfer.tx_buf = ( unsigned long )txBuffer;
	  xfer.rx_buf = ( unsigned long )rxBuffer;
	  xfer.len = 2;
	  xfer.speed_hz = SPI_SPEED_HZ;
	  xfer.cs_change = 1;
	  xfer.bits_per_word = 16;
	  xfer.delay_usecs= SPI_DELAY;
	  ioctl(spiDev,SPI_IOC_WR_MODE, &mode);
	  res = ioctl(spiDev, SPI_IOC_MESSAGE(1), &xfer);

	  return rxBuffer[0];
}

void  writeCommand(uint8_t d)
{
//  SPI.transfer(RA8875_CMDWRITE);
//  SPI.transfer(d);
	  txBuffer[1] = RA8875_CMDWRITE;
	  txBuffer[0] = d ; //0x4E = 0x7, 0x63 = 0x1F //verify operation
	  xfer.tx_buf = ( unsigned long )txBuffer;
	  xfer.rx_buf = ( unsigned long )rxBuffer;
	  xfer.len = 2;
	  xfer.speed_hz = SPI_SPEED_HZ;
	  xfer.cs_change = 1;
	  xfer.bits_per_word = 16;
	  xfer.delay_usecs= SPI_DELAY;
	  ioctl(spiDev,SPI_IOC_WR_MODE, &mode);
	  res = ioctl(spiDev, SPI_IOC_MESSAGE(1), &xfer);
}

uint8_t  readStatus(void)
{
//  SPI.transfer(RA8875_CMDREAD);
//  uint8_t x = SPI.transfer(0x0);
//  return x;
	  txBuffer[1] = RA8875_CMDREAD;
	  txBuffer[0] = 0x00 ; //0x4E = 0x7, 0x63 = 0x1F //verify operation
	  xfer.tx_buf = ( unsigned long )txBuffer;
	  xfer.rx_buf = ( unsigned long )rxBuffer;
	  xfer.len = 2;
	  xfer.speed_hz = SPI_SPEED_HZ;
	  xfer.cs_change = 1;
	  xfer.bits_per_word = 16;
	  xfer.delay_usecs= SPI_DELAY;
	  ioctl(spiDev,SPI_IOC_WR_MODE, &mode);
	  res = ioctl(spiDev, SPI_IOC_MESSAGE(1), &xfer);
	  return rxBuffer[0];
}



