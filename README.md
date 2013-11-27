lib_rs485
=========

Flyport library for RS485 communication, released under GPL v.3.<br>
The library allows to communicate through RS485.<br>
More info on wiki.openpicus.com.<br>
1) import files inside Flyport IDE using the external libs button.<br>
2) add following code example in FlyportTask.c:<br>

<pre>
#include "taskFlyport.h"
#include "rs485Helper.h"

void FlyportTask()
{
  char msg[100] = "GET data\n";
  char resp[100] = "\0";
  char data[100] = "\0";
  
  int i=0;
  
  vTaskDelay(100);
  
  RS485Remap(2,p5,p7,p2,p17); //RS485 on the 2nd UART, p5 -> TX, p7 -> RX, p2 -> WE, p17 -> RE
  RS485Init(2, 9600); //Baud rate: 9600 bps
  RS485On(2);

  vTaskDelay(100);
  UARTWrite(1,"Flyport RS485 library test!\r\n");

  while(1)
  {
    UARTWrite(1, msg);
	
    RS485Flush(2);
    RS485Write(2,msg);
	
    vTaskDelay(100);
	
    while(RS485BufferSize(2)>0)
    {
      RS485Read(2,resp,1);  
      resp[1]='\0';		//RS485Read doesn't add the null terminator 
      strcat(data,resp);
    }
	
      UARTWrite(1,data);
      
      // INSERT HERE YOUR CODE (in this case a parsing sequence)
  }
}
</pre>
