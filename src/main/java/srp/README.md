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
   * ![garbage](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.123rf.com%2Fphoto_142951586_crowded-street-litter-bin-with-household-garbage-and-waste-vector-cartoon-illustration-on-a-white.html&psig=AOvVaw3hLhWA4trSVu_pF5oeC0Ow&ust=1679020934149000&source=images&cd=vfe&ved=0CBAQjRxqFwoTCID-3MS23_0CFQAAAAAdAAAAABAE)