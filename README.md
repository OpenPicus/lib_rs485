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
  char msg[100] = "GET data\r\n";
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
		
		while(RS485BufferSize(2)==1)
		{
		  RS485Read(2,resp,1);
		  strcat(data,resp);
		}
		
		for(i=0;i<100;i++)
		{
			sprintf(msg,"Value: %d\r\n",i);
		  UARTWrite(1, msg);
		
		  RS485Flush(2);
		  RS485Write(2,msg);
		}
		if(i==100)
		  RS485Off(2);
	}
}
</pre>
