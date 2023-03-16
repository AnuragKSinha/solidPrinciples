# Single Responsibility
* _Every __software component__ should have one and only one responsibility_
    * Software Component can be a class, a method, or a module
    
```java
public class Square{
	int side=5;
	public int calculateArea(){
		return side*side;
    }
    public int calculatePerimeter(){
		return side*4;
    }
    public void draw(){
		if(highResolutionMonitor){
			//Render a high resolution image of a square
		}else{
			//Render a normal image of a square
		}
    }
    public void rotate(int degree){
		// Rotate the image of the square clockwise to
        // the required degree and re-render
    }
}
```
* Cohesion is the degree to which the various parts of a software component are related
   * It has wide variety of items 
   * ![image](https://user-images.githubusercontent.com/26598629/225497717-e1642f8c-129b-4800-bbbe-e74e54b21945.jpg)

