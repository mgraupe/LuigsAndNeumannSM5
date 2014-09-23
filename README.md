Luigs and Neumann SM-5
======================

A python class for communicating with the Luigs &amp; Neumann SM-5 control box.


LandNSM5 implements a class for communicating with a Luigs & Neumann SM-5 control box to control the manipulators. The SM-5 must be connected with a USB cable. 

This class uses the python "serial" package which allows for communication with serial devices through 'write' and 'read'. The communication properties (BaudRate, Terminator, etc.) are set when invoking the serial object with serial.Serial(..). For the SM-5: The Baud rate is 38400 Baud, 8 Data bit, NoParity and 1 Stopbit afterswitch power on (see 'Interface description V1.8.pdf' manual). 

The SM-7 or SM-8-4 control boxes might be accessed using this class, but this has not been tested. 


##Methods:
  Create the object. The object is opened with serial.Serial.
  
    * sm5 = LandNSM5()

  Various functions from the manual are implemented such as
  
    * goSteps()
    * stepSlowDistance(...)
    * goVariableFastToAbsolutePosition(...)
    * goVariableSlowToAbsolutePosition(...)
    * goVariableFastToRelativePosition(...)
    * stepIncrement(...)
    * stepDecrement(...)
    * switchOffAxis(...)
    * switchOnAxis(...)
    * setAxisToZero(...)
    * getPosition(...)

See refer to code for required input arguments. Futher functions can be easily implemented from the manual following the structure of the class fuctions. 

##Properties:

verbose - The level of messages displayed (0 or 1). 


##Example session:

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
  
  
