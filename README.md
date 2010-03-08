# AS3-Global-Object `v2.1`

## Description

AS3 Global Object is a Singleton that lets you store dynamic variables in a globally accessible location within your Actionscript 3 application (also known as the missing _global property). This enables developers to accomplish things like self registering visual components, global events and event listeners.

Global Object was created and is maintained by `Paulius Uza` (http://www.uza.lt) and `InRuntime Ltd.` (http://www.inruntime.com). As of November 6, 2009 project is hosted on GitHub.

## Examples

### Recommended Syntax

	// We highly recommend using dollar sign as a shorthand for accessing Global Object!
	public var $:Global = Global.getInstance();
	
### Weak References

AS3 Global Object uses weak references for storing data. This means that if the only pointer to your data is from within Global Object it is eligible for garbage collection. As of v2.1 you can turn this feature off by passing 'false' in the Global Object constructor:

	public var $:Global = Global.getInstance(false);
	
### Instantiating Global Object

`Assuming: Test.as`

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
	
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
	
`Assuming: Test.as`

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
	
		public class Test extends Sprite 
		{
 			// This is an example how you should NOT instantiate the Global Object
			// Global Object is a singleton and has to be initialized using .getInstance() function 
			// Therefore the following with throw and exception:
			private var $:Global = new Global();
		}
	}
	
### Setting and getting dynamic variables

`Assuming: Test.as`

	package  {
		import flash.display.*;
		import com.inruntime.utils.*;
		
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

`Assuming: Test.as`

	package {
		import flash.display.Sprite;
		import com.inruntime.utils.Global;
		import com.inruntime.utils.GlobalEvent;
		
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

### Using Global Object to access Stage from anywhere

`Assuming: StageExample.as`

	package {
		import flash.display.Sprite;
		import com.inruntime.utils.*;
		
		public class StageExample extends Sprite 
		{
			// Let's instantiate Global Object for the first time
			private var $:Global = Global.getInstance();
			
			public function StageExample()
			{
				// Let's put the stage reference into our newly instantiated Global Object:
				$.stage = this.stage;
				// Now let's initialize our child class
				var test:TestClass = new TestClass();
			}
		}
	}
	
`Assuming: TestClass.as`	

	package {
		import com.inruntime.utils.*;
		
		public class TestClass {
			// Let's get the reference to our Global Object inside the test class
			private var $:Global = Global.getInstance();
			public function TestClass() {
				// The following line will trace '[object Stage]'
				trace($.stage);
			}
		}
	}