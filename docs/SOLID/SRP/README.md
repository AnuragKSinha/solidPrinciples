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
* If we decide to go with no sql DB like mongoDB most of this code need to change,hence we can see the 
  Student class is tightly coupled with the DB layer. 
  * The Student class should ideally deal with Student related functionalities like getting studentId,DOB,Address etc.Student class should be
    aware of the low level details related to dealing with DB layer. 
![image](https://user-images.githubusercontent.com/26598629/225510091-b359a7a1-8a28-44b2-9480-55b0081a1169.png)
    
* To fix this we will separate out DB layer from Student class and refer DB layer from Student class.By doing so we have removed the tight coupling.
    * _Loose Coupling helps attain better adherence to __Single Responsibility Principle___
    * Now if we change the underlying DB then Student class does not need to get changed and recompiled you only need to change DB layer class
    
```java
import java.sql.DriverManager;
import java.sql.Statement;

public class Student {
	private String studentId;

	private String studentDOB;

	private String address;

	public void save() {
		new StudentRepository().save(this);
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
// Responsibility: Handle core student profile data
```
```java
public class StudentRepository{
	public void save(Student student){
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
}
// Responsibility: Handle Database operations
```
## Reason To Change
* __Every __software component__ should have one and only one ~~responsibility~~ reason to change__[As per Uncle Bob]
    * If a software component has more reasons to change, then the frequency of changes to it will increase.Every change to a software component opens up the posibility of introducing bugs into the software. So if there are frequent changes to a software component, the possibility of introducing a bug goes up.And this will require more time and effort to be spent on re-testing the software after changes are done. 

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
* Reasons To change
    * A change in the student id format
    * A change in the student name format
    * A change in the database backend,as advised by the technical team
* We will repeat the same steps that we took previously, we will separate the Student class from DB layer. And create a diff Repository class.
    * Reason to change in Student class
       * A change in the student id format
       * A change in the student name format
    * Reason to change in StudentRepository class
       * A change in Database backend, as per technical team requirement
    > If the reasons to change are closely related to one another then we can combine those reasons together. Hence the 2 reasons to change in Student class is closely related to Student and hence can be treated as one. Therefore both Student class and StudentRepository class now follows the Single Reason to Change and hence adhere to Single Responsibility Principle
       
 ## Summary 
 * The __Reason to change__ and __Responsibility__ are pretty much the same and same set of actions needs to be taken. 
 	* The Software component should be highly cohesive and should have low coupling in order to follow **SRP**.	
