# Liskov Substitution Principle
## Break the hierarchy 
* __Objects should be replaceable with their subtypes without affecting the correctness of the code.__
  * Ostrich is a bird but an ostrich cannot fly. Hence Ostrich extends Bird class and leaves fly method unimplemented.
  * __Unimplemented methods are always indicative of Design flaw.__
  * Ostrich is a bird is correct but if we apply Liskov substitution, Objects should be replaceable with there subtypes witjout affecting the correctness of the code. This test fails as you cannot use all the methods of bird and if you do so then it will throw RuntimeException on Ostrich object which extends Bird Object.
```java
public class Bird {
  public void fly(){
    //Fly high
  }
}
public class Ostrich extends Bird {
  @Override
  public void fly{
    //Unimplemented
    throw new RuntimeException();
  }
}
```

* Code Example which does not follow Liskov Substitution 
```java
public class Car{
  public double getCabinWidth(){
    // return width of the car
  }
}
public class RacingCar extends Car{
  @Override
  public double getCabinWidth(){
    // UnImplemented
  }
  public double getCockpitWidth(){
    //return cockpit width
  }
}
public class CarUtils{
  public static void main(String[] args){
    Car first=new Car();
    Car second=new Car();
    Car third=new RacingCar();
    
    List<Car> myCars = new ArrayList<>();
    myCars.add(first);
    myCars.add(second);
    myCars.add(third);
    
    for(Car car:myCars){
      System.out.println(car.getCabinWidth()); // The cabin width for third car will not print anything as RacingCar leaves CabinWidth unimplemented
    }
  }
}
```
* As a solution to this problem we need to strick the root cause of the problem, which is the inheritance. Racing car should no longer extend Car.Instead we will comeup with common parent for both.
```java
public class Vehicle {
  public double getInteriorWidth(){
    //return interior width
  }
}

public class Car extends Vehicle {
  @Override 
  public double getInteriorWidth(){
    return this.getCabinWidth();
  }
  public double getCabinWidth(){
    // return width of the car
  }
}

public class RacingCar extends Vehicle {
  @Override 
  public double getInteriorWidth(){
    return this.getCockpitWidth();
  }
  public double getCockpitWidth(){
    // return width of the car
  }
}
public class CarUtils{
  public static void main(String[] args){
    Vehcile first=new Car();
    Vehcile second=new Car();
    Vehcile third=new RacingCar();
    
    List<Vehcile> myVehciles = new ArrayList<>();
    myVehciles.add(first);
    myVehciles.add(second);
    myVehciles.add(third);
    
    for(Vehcile vehcile: myVehciles){
      System.out.println(vehcile.getCabinWidth()); // The cabin width for third car will not print anything as RacingCar leaves CabinWidth unimplemented
    }
  }
}
```
* The InteriorWidth is nither cabin width nor cockpit width but a much more generic abstraction called interior width.

## Tell, don't ask

```java
public class Product{
 protected double discount;
 
 public double getDiscount(){
   return discount;
 }
}
public class InHouseProduct extends Product {
 public void applyExtraDiscount(){
   discount=discount*1.5;
 }
}
public class ProductUtils {
 Product p1=new Product();
 Product p2=new Product();
 Product p3=new InHouseProduct();
 List<Product> productList=new ArrayList<>();
 
 productList.add(p1);
 productList.add(p2);
 productList.add(p3);
 for(Product product: productList){
  if(product instanceOf InHouseProduct){
    ((InHouseProduct) product).applyExtraDiscount();
  }
  System.out.println(product.getDiscount());
 }
}
```
 * The above example is not a good design.This is against LSP. We should have dealt will all the objects as Product Objects instead of typecasting to InHouseProduct for some of the Objects.Now we will refactor the example so that it follows LSP.

```java
public class Product{
 protected double discount;
 
 public double getDiscount(){
   return discount;
 }
}
public class InHouseProduct extends Product {
 @Override
 public double getDiscount(){
   this.applyExtraDiscount();
   return discount;
 }
 private void applyExtraDiscount(){
   discount=discount*1.5;
 }
}
public class ProductUtils {
 Product p1=new Product();
 Product p2=new Product();
 Product p3=new InHouseProduct();
 List<Product> productList=new ArrayList<>();
 
 productList.add(p1);
 productList.add(p2);
 productList.add(p3);
 for(Product product: productList){
  System.out.println(product.getDiscount());// InhouseProduct overwrites the getDiscount() method so no special handling required
 }
}
```

## Summary 
* __This Principle will check if the Inheritance is done properly. If its not done properly then LSP will be violated.__
<img width="501" alt="Screenshot 2023-04-23 at 10 46 10 PM" src="https://user-images.githubusercontent.com/26598629/233854554-640655cb-a66f-4fea-82c0-0114f6abfeb7.png">
* Change the 'IS-A' way of thinking
  * __"If it looks like a duck and quacks like a duck but it needs a batteries,you probably have a wrong abstraction."__
