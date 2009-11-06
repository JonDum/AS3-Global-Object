# AS3-Global-Object `VERSION 2.0`

## Description

AS3 Global Object is a Singleton that lets you store dynamic variables in a globally accessible location within your Actionscript 3 application (also known as the missing _global property). This enables developers to accomplish things like self registering visual components, global events and event listeners.

As of November 6, 2009 project is hosted on GitHub.

Documentation can still be found at
http://www.uza.lt/codex/as3-global-object/

## Examples

### Initializing Global Object

package  {
	import flash.display.*;
	import com.inruntime.utils.*;
	
	public class Test extends Sprite 
	{
 		// initialize the global object
		// you have to repeat this step in every class that will use the global
		private var global:Global = Global.getInstance();
		public function Test(){
			 // your application code here
		}
	}
}