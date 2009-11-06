# AS3-Global-Object `v2.0`

## Description

AS3 Global Object is a Singleton that lets you store dynamic variables in a globally accessible location within your Actionscript 3 application (also known as the missing _global property). This enables developers to accomplish things like self registering visual components, global events and event listeners.

Global Object was created and is maintained by `Paulius Uza` (http://www.uza.lt) and `InRuntime Ltd.` (http://www.inruntime.com). As of November 6, 2009 project is hosted on GitHub.

## Examples

### Recommended Syntax

	// We highly recommend using dollar sign as a shorthand for accessing Global Object!
	public var $:Global = Global.getInstance();
	
### Instantiating Global Object

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
	
		// Assuming Test.as
		public class Test extends Sprite 
		{
 			// initialize the global object
			// you have to repeat this step in every class that will use the global
			private var $:Global = Global.getInstance();
			
			public function Test(){
			 	// your application code here
			}
		}
	}
	
### Wrong way to instantiate Global Object

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
	
		// Assuming Test.as
		public class Test extends Sprite 
		{
 			// This is an example how you should NOT instantiate the Global Object
			// Global Object is a singleton and has to be initialized using .getInstance() function 
			// Therefore the following with throw and exception:
			private var $:Global = new Global();
		}
	}
	
### Setting and getting dynamic variables

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
		
		// Assuming Test.as
		public class Test extends Sprite
		{
			private var $:Global = Global.getInstance();
			private var testSprite:Sprite = new Sprite();
			
			public function Test(){
				//setting variables is easy, global object accepts any name / value pair, even functions
				$.stage = this.stage;
				$.testA = "a"
				$.testB = testSprite;
				$.testF = test;
				
				//getting variables is easy too
				trace($.testA);
				this.addChild($.testB);
				
				//as easy as calling a globally stored function
				$.testF();
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
			private var $:Global = Global.getInstance();
			public function Test()
			{
				// Let's listen for property changes on the global object
				$.addEventListener(GlobalEvent.PROPERTY_CHANGED,onPropChanged);
				
				// Event fires when you set or update a direct property of Global storage, let's try that
				$.variableA = 1;
				$.variableA = 23;
				$.variableA = 41;
				$.variableB = 20;
				$.variableB = $.variableA;
				
				var sp:Sprite = new Sprite();
				$.sp = sp;
				
				// However, event does not fire when you set a property of an object inside Global storage.
				$.sp.x = 10;
				$.sp.y = 23;
				sp.y = 34;
			}
			
			private function onPropChanged(e:GlobalEvent):void {
				// Property name can be accessed through GlobalEvent object;
				trace ("property "+ e.property + " has changed to " + $[e.property]);
			}
		}
	}