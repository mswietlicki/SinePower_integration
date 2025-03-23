# SinePower_integration
I want to integrate Campervan Home Assistant setup with old Waeco MSI2312T 12V 2300W inverter.

## Description

Inverter can be configured to connect to Remote, PC or ESP32 using RS232 via RJ11 socket (item 12 in fig.5 in MSI2312T Manual)

Inverter is using 12V signaling so 2+ channel level coverter will be needed to talk to ESP32.

RS232 (RJ11 Socket) pins seams to be:

- GND
- RXD
- TXD
- +12V

I do not know what kind of current can this port handle by I will try to power my ESP32 from it via 3.3V buck converter.

## Communication Protocol Overview

### Setup

Type: RS-232
Bitrate: 4800
Data bits: 8
Stop bits: 1
Prity: None
Inverted: Yes
Encoding: ASCII (LSB)

### Protocol

*I could not find documentation of the protocol so description below is a result of my experiments and will be incomplete.*

Protocol mode seams to be Request-Response

#### Messages Format

Request

```
<COMMAND> <PARAMETER>\r\n
```

Response: 

```
<DATA>\r\n=>\r\n
```

| Command | PARAMETER | Description | Example | Response |
| --- | --- | --- | --- | --- |
| QURY | 0 | Query inverter power source | QURY 0\r\n, 00\r\n=>\r\n | 00 - Normal, C2 - Grid power |
| QURY | 1 | Query inverter load | QURY 1\r\n, 57\r\n=>\r\n | 2 digit load percentage |
| QURY | 2 | Unknow query | QURY 2\r\n, 00\r\n=>\r\n | 2 digits |
| QURY | 4 | Unknow query | QURY 4\r\n, 00\r\n=>\r\n | 2 digits |
| PWRS | 2 | Set power level to normal ? | PWRS 2\r\n | 0\r\n=>\r\n |
| POWER | 0 | Turns off inverter | POWER 0\r\n | |



