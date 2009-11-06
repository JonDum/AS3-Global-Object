# AS3-Global-Object `v2.0`

## Description

AS3 Global Object is a Singleton that lets you store dynamic variables in a globally accessible location within your Actionscript 3 application (also known as the missing _global property). This enables developers to accomplish things like self registering visual components, global events and event listeners.

Global Object is maintained by Paulius Uza (http://www.uza.lt) and InRuntime Ltd. (http://www.inruntime.com). As of November 6, 2009 project is hosted on GitHub.

## Examples

### Initializing Global Object

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
	
		// Assuming Test.as
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
	
### Setting and getting dynamic variables

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
		
		// Assuming Test.as
		public class Test extends Sprite
		{
			private var global:Global = Global.getInstance();
			private var testSprite:Sprite = new Sprite();
			
			public function Test(){
				//setting variables is easy, global object accepts any name / value pair, even functions
				global.stage = this.stage;
				global.testA = "a"
				global.testB = testSprite;
				global.testF = test;
				
				//getting variables is easy too
				trace(global.testA);
				this.addChild(testB);
				
				//as easy as calling a globally stored function
				global.testF();
			}
			
			private function test():void {
					trace("this is a global test");
			}
		}
	}
	
### Watching a global variable

	package {
		import flash.display.Sprite;
		import com.inruntime.utils.Global;
		import com.inruntime.utils.GlobalEvent;
		
		// Assuming Test.as
		public class Test extends Sprite 
		{
			private var global:Global = Global.getInstance();
			public function Test()
			{
				// Let's listen for property changes on the global object
				global.addEventListener(GlobalEvent.PROPERTY_CHANGED,onPropChanged);
				
				// Event fires when you set or update a direct property of Global storage, let's try that
				global.variableA = 1;
				global.variableA = 23;
				global.variableA = 41;
				global.variableB = 20;
				global.variableB = global.variableA;
				var sp:Sprite = new Sprite();
				global.sp = sp;
				
				// However, event does not fire when you set a property of an object inside Global storage.
				global.sp.x = 10;
				global.sp.x = 23;
				sp.x = 34;
			}
			
			private function onPropChanged(e:GlobalEvent):void {
				// Property name can be accessed through GlobalEvent object;
				trace ("property "+ e.property + " has changed to " + global[e.property]);
			}
		}
	}