# Experiment--09-Configuring-UART-in-LPC2148-for-serial-data-transmission-

Name :	S.Sham Rathan


Roll no: 212221230093


Date of experiment : 


### Configuring UART in LPC2148 for serial data transmission 

### Aim: 
To configure internal UART for transferring serial data and display it on the Virtual terminal  
### Components required: 
Proteus ISIS professional suite, Kiel μ vision 5 Development environment 
### Theory: 
	
	The UART Protocol uses only two wires (or pins in a device like microcontroller) to transmit the data. In that, one is for transmitting the data and the pin is called TX pin in the device. The other pin is used to receive the data and is called RX pin.As UART is a serial communication, the data is transmitted in a series of packets. Usually, a packet consists of 4 parts: a start bit, the actual data, a parity bit and stop bits. The following image shows a typical structure of the data packet in UART.

### FIGURE -01 UART PACKET 


### UART in LPC2148
Coming to UART in LPC2148, the LPC214x series of MCUs have two UART blocks called UART0 and UART1. Each UART block is associated with two pins, one for transmission and the other for receiving.
In UART0 block, the TXD0 (Transmit) and RXD0 (Receive) pins in the device are P0.0 and P0.1 respectively. In case of UART1, the TXD1 and RXD1 pins are P0.8 and P0.9 respectively.
UART0	UART1
TXD0	P0.0	TXD1	P0.8
RXD0	P0.1	RXD1	P0.9


Both the UART modules are identical, except the UART1 block has an additional full modem interface. This includes all the pins for RS232 compatibility like flow control pins (CTS, RTS) etc.
Both the UART blocks have 16 byte Receive and Transmit FIFO structures to hold the transmit and receive data. In order to control the data access and assembly, the UART blocks have two registers each.
For the transmitter operation, the TX has two special registers called Transmit Holding Register (THR) and Transmit Shift Register (TSR). In order to transmit the data, it is first sent to THR and then moved to TSR.
UART0 Transmit Holding Register (U0THR): The Transmit Holding Register consists of the top byte of the TX FIFO i.e. the oldest data. Bit 0 (LSB) is the first data to be transmitted.
UART0 Fractional Divider Register (U0FDR): The Fractional Divider Register contains the bits that control the prescale for the baud rate generator. U0FDR contains the divisor and multiplier values for prescaling. Both the divisor (DIVADDVAL) and multiplier (MULVAL) values are stored as 4 – bit values. Bit 0 to Bit 3 in U0FDR contains the pre – scaler divisor value for baud rate generation. For the baud rate generator to have an impact on the baud rate of the UART, the divisor value must not be 0. Bit 4 to bit 7 in U0FDR contains the pre – scaler multiplier value. For proper working of the UART0, the value in multiplier must be greater than or equal to 1. This is also applicable even if the baud rate generator is not used.
UART0 Interrupt Enable Register (U0IER): The Interrupt Enable Register is used to enable or disable the interrupts corresponding to UART0. Bit 0 is for RBR Interrupt, Bit 1 is for THRE interrupt, Bit 2 is for RX Line Status Interrupt, Bit 8 is for End of Auto – baud interrupt and Bit 9 is for Auto – baud Time out interrupt. When a bit is 0, the interrupt is disabled and when the bit is 1, the interrupt is enabled.
UART0 Interrupt Identification Register (U0IIR): The Interrupt Identification Register is used to provide the code for priority and status of the pending interrupt.

UART0 FIFO Control Register (U0FCR): The FIFO Control Register controls the operation of the RX and TX FIFOs in UART0. Bit 0 is used to enable or disable the FIFO. Bit 1 is used to reset the RX FIFO. Bit 2 is used to reset the TX FIFO. Bits 6 and 7 are used to control when the interrupt must occur i.e. after how many receiver characters.
UART0 Line Control Register (U0LCR): The Line Control Register is used to set the format of the data which is transmitted or received.


## Figure -02 UART interface virtual terminal

### Kiel - Program 
```
#include <LPC213x.H>              // LPC21xx definitions                      */
char a;
void uart0_init(){
  PINSEL0 = 0x00000005;           // Enable RxD0 and TxD0                     */
  U0LCR = 0x83;                   // 8 bits, no Parity, 1 Stop bit            */
  U0DLL = 97;                     // 9600 Baud Rate @ 15MHz VPB Clock         */
  U0LCR = 0x03;                   // DLAB = 0                                 */
}
void uart0_putc(char c){
 while(!(U0LSR & 0x20)); // Wait until UART0 ready to send character  
 U0THR = c; // Send character
}
int uart0_getc (void)  {                     
  while (!(U0LSR & 0x01));
  return (U0RBR);
}
int main (void)  {                
  uart0_init();      
  while (1) {                          
  a=uart0_getc();
   uart0_putc(a);
  }                               
}

```

### Output screen shots :
![10](https://user-images.githubusercontent.com/93587823/203741948-d35ae960-140d-454d-b9a9-9505379f7595.png)
![20](https://user-images.githubusercontent.com/93587823/203741991-d6aaaba4-74ec-4913-8699-004940fce1f3.png)

### Result :
UART is programmed for transmitting serial data on virtual terminal  



