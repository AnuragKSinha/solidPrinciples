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
## Cohesion
* Cohesion is the degree to which the various parts of a software component are related
   * It has wide variety of items 
   > Contents here has low cohesion
    ![image](https://user-images.githubusercontent.com/26598629/225497717-e1642f8c-129b-4800-bbbe-e74e54b21945.jpg)
   * Properly Segregated Garbage and all respective bins content has common relation  
   > Contents here has high cohesion
    ![image](https://user-images.githubusercontent.com/26598629/225498385-27085419-b773-4edd-b8e3-3ab1f59be14e.jpg)
   * Cohesion of functions in this class 
   ![image](https://user-images.githubusercontent.com/26598629/225502929-753c9bde-d07b-4e0e-89be-9b183d4e7c67.png)
     * If we take all the methods as a whole then the level of Cohesion is low so it needs a reshuffle to increase the level of cohesion
    
* By this we have split the methods in two classes, but we have increased the level of cohesion in each of the classes
```java
public class Square{
	int side=5;
	public int calculateArea(){
		return side*side;
    }
    public int calculatePerimeter(){
		return side*4;
    }
}
// Each of the methods in Square class are closely related as both of them deal with measurements of the square
// Responsibility: Measurements of Square
```
```java
public class SquareUI{
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
// Each of the methods in SquareUI class are closely related as both of them deal with rendering of the square
// Responsibility: Rendering images of squares
```
* We should always AIM for high cohesion within a component
    * _Higher Cohesion helps attain better adherence to __Single Responsibility Principle___
    * If there is high cohesion between all methods of a class then we can assign Single Responsibility to all the methods as a whole
    
## Coupling 
* Coupling is defined as the level of inter-dependency between various software components
    * Example of highly coupled code

```java
import java.sql.DriverManager;
import java.sql.Statement;

public class Student {
	private String studentId;

	private String studentDOB;

	private String address;

	public void save() {
		//Serialize object into a string representation
		String objectStr = MyUtils.serializeIntoAString(this);
		Connection connection = null;
		Statement stmt = null;
		try {
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MyDB",
                    "root", "password");
			stmt = connection.createStatement();
			stmt.execute("INSERT INTO STUDENT VALUES ("+ objectStr + ")");
		}catch(Exception ex){
			ex.getStackTrace();
		}
	}
	public String getStudentId(){
		return studentId;
    }
    public String setStudentId(String studentId){
		this.studentId=studentId;
    }
    .....
    .....
    .......
}
```




