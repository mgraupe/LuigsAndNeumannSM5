Luigs and Neumann SM-5
======================

A python class for communicating with the Luigs &amp; Neumann SM-5 control box.


`LandNSM5` implements a class for communicating with a Luigs & Neumann SM-5 control box to control the manipulators. The SM-5 must be connected with a USB cable. 

This class uses the python `serial` package which allows for communication with serial devices through `write` and `read`. The communication properties (BaudRate, Terminator, etc.) are set when invoking the serial object with `serial.Serial(...)`. For the SM-5: The Baud rate is 38400 Baud, 8 Data bit, NoParity and 1 Stopbit afterswitch power on (see 'Interface description V1.8.pdf' manual). 

The SM-7 or SM-8-4 control boxes might be accessed using this class, but this has not been tested. 

##Requires

The following python packages are required by the class. 

* import serial
* import struct
* numpy 
* binascii
* ctypes
* time 

##Methods
  Create the object. The object is opened with `serial.Serial(...)`.
  
    * sm5 = LandNSM5()

  Various functions from the manual are implemented such as
  
    * goSteps(device,axis,steps)
		Moves specified number of steps. 
		devive ... Number (int) of connected device (e.g., 1,2)
		axis ... Identifier (string) of axis to move (e.g., 'x', 'y', 'z')
		steps ... Number (int) of steps to move.
    * stepSlowDistance(device,axis,distance)
		Sets the step width. 
		devive ... Number (int) of connected device (e.g., 1,2)
		axis ... Identifier (string) of axis to move (e.g., 'x', 'y', 'z')
		distance ... Step width in \mu m (float).
    * goVariableFastToAbsolutePosition(device,axis,absolutePosition)
		Moves fast to absolute position.
		absolutePosition ... Absolute position in \mu m (float). 
    * goVariableSlowToAbsolutePosition(device,axis,absolutePosition)
		Moves slow to absolute position.
		absolutePosition ... Absolute position in \mu m (float). 
    * goVariableFastToRelativePosition(device,axis,relativePosition)
		Moves fast to relative position.
		relativePosition ... Relative move in \mu m (float). 
    * goVariableSlowToRelativePosition(device,axis,relativePosition)
		Moves slow to relative position.
		relativePosition ... Relative move in \mu m (float). 
    * stepIncrement(device,axis)
		Moves one positive step of specified axis. 
    * stepDecrement(device,axis)
		Moves one negative step of specified axis. 
    * switchOffAxis(device,axis)
		Removes current from specified motor.
    * switchOnAxis(device,axis)
		Re-establishes current from specified motor.
    * setAxisToZero(device,axis)
		Zeros counter on specified axis.
    * getPosition(device,axis)
		Queries position of specified axis.

Futher functions can be easily implemented from the manual following the structure of the class fuctions. 

##Properties

* verbose - The level of program messages displayed (0 or 1). 
* timeOut - Timout (in sec) of the serial connection. 
* sleepTime - Delay (in sec) between write and read from the serial connection. 
* maxLoops - Maximal number of read - write loop tries before abort. 


##Example session

```python
In [1]: import LandNSM5_1
  
In [2]: sm5 = LandNSM5_1.LandNSM5()
    Serial<id=0x98b0860, open=True>(port='COM5', baudrate=38400, bytesize=8, parity='N', stopbits=1, timeout=1, xonxoff=False, rtscts=False, dsrdtr=False)
    SM5 ready
  
In [3]: pos = sm5.getPosition(1,'x')
    Establish connection to SM5 .  established
    send 0101 16010101011021 16 33
    done
  
In [4]: print pos
    -3481.796875
  
In [5]: sm5.goVariableFastToRelativePosition(1,'x',-15.)
    Establish connection to SM5 .  established
    send 004a 16004a0501000070C16B65 107 101
    done
   
In [6]: del sm5
    Connection to Luigs and Neumann SM5 closed
```
  
  
