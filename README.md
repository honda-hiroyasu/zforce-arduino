# Neonode zForce AIR interface library for Arduino

An Arduino library for communicating with the [Neonode zForce Air Module] optical touch sensor. Handles the fundamental BER encoded ASN.1 messages.

[Neonode zForce Air Module]: https://www.digikey.com/short/p3374r

# Method Overview
## Public Methods

| Data Type   | Method          | Parameter                                               | Description                                                                                                                                                                                      | Return                                                                                   |
|-------------|-----------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Constructor | Zforce          | None                                                    | Not used.                                                                                                                                                                                        | N/A                                                                                      |
| void        | Start           | int dataReady                                           | Used to initiate the I2C connection and set the current dataReady pin.                                                                                                                           | N/A                                                                                      |
| int         | Read            | uint8_t* payload                                        | Initiates an I2C read sequence by calling the read method in the I2C library.  This can also be used externally to read the ASN.1 serialized messages without parsing them.                      | Error code according to the atmel data sheet  for the corresponding mcu . 0 for success. |
| int         | Write           | uint8_t* payload                                        | Initiates  an I2C write sequence by calling the write method in the I2C library.  This can also be used externally to write ASN.1 serialized messages that are not yet supported by the library. | Error code according to the atmel data sheet  for the corresponding mcu . 0 for success. |
| bool        | Enable          | bool isEnabled                                          | Writes an enable message to the sensor and depending on the parameter either sends enable or disable.                                                                                            | True if the write succeeded.                                                             |
| bool        | TouchActiveArea | uint16_t minX uint16_t minY uint16_t maxX uint16_t maxY | Writes a touch active area message to the sensor with the passed parameters.                                                                                                                     | True if the write succeeded.                                                             |
| bool        | FlipXY          | bool isFlipped                                          | Writes a flip xy message to the sensor with the passed parameters.                                                                                                                               | True if the write succeeded.                                                             |
| bool        | ReverseX        | bool isReversed                                         | Writes a reverse x message to the sensor with the passed parameters.                                                                                                                             | True if the write succeeded.                                                             |
| bool        | ReverseY        | bool isReversed                                         | Writes a reverse y message to the sensor with the passed parameters.                                                                                                                             | True if the write succeeded.                                                             |
| bool        | ReportedTouches | uint8_t touches                                         | Writes a reported touches message to the sensor with the passed parameters.                                                                                                                      | True if the write succeeded.                                                             |
| int         | GetDataReady    | None                                                    | Performs a digital read on the data ready pin.                                                                                                                                                   | The current status of the data ready pin.                                                |
| Message*    | GetMessage      | None                                                    | Checks if the data ready pin is HIGH and calls the method VirtualParse if it is.                                                                                                                 | A message pointer which will be NULL if the data ready pin is LOW.                       |
| void        | DestroyMessage  | Message* msg                                            | Deletes the passed message pointer and sets it to null.                                                                                                                                          | N/A                                                                                      |

## Private Methods
| Data Type | Method               | Parameter                                    | Description                                                                                                                                              | Return             |
|-----------|----------------------|----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| Message*  | VirtualParse         | uint8_t* payload                             | Checks if the payload contains a response or if it contains a notification and calls the appropriate method to parse the payload and populate a message. | A message pointer. |
| void      | ParseTouchActiveArea | TouchActiveAreaMessage* msg uint8_t* payload | Parsing of a touch active area response.                                                                                                                 | N/A                |
| void      | ParseEnable          | EnableMessage* msg uint8_t* payload          | Parsing of an enable response.                                                                                                                           | N/A                |
| void      | ParseReportedTouches | ReportedTouchesMessage* msg uint8_t* payload | Parsing of a reported touches response.                                                                                                                  | N/A                |
| void      | ParseReverseX        | ReverseXMessage* msg uint8_t* payload        | Parsing of a reverse x response.                                                                                                                         | N/A                |
| void      | ParseReverseY        | ReverseYMessage* msg uint8_t* payload        | Parsing of a reverse y response.                                                                                                                         | N/A                |
| void      | ParseFlipXY          | FlipXYMessage* msg uint8_t* payload          | Parsing of a flip xy response.                                                                                                                           | N/A                |
| void      | ParseTouch           | TouchMessage* msg uint8_t* payload           | Parsing of a touch notification.                                                                                                                         | N/A                |
| void      | ParseResponse        | uint8_t* payload Message** msg               | Calls the appropriate method depending on which type of response the payload contains.                                                                   | N/A                |
| void      | ClearBuffer          | uint8_t* buffer                              | Sets all values in the passed byte array to zero. Is called by "GetMessage" after parsing the data.                                                      | N/A                |
