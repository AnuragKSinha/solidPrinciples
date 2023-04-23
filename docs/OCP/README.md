# Open For Extension And Closed For Modifications
* __Software Components should be closed for modification,but open for extension__
  * Example of One State Insurance Company: For All your health insurance needs!
```java
public class InsurancePremiumDiscountCalculator {
  public int calculatePremiumDiscountPercentage(HealthInsuranceCustomerProfile customer){
      if(customer.isLoyalCustomer()){
        return 20;
      }
      return 0;
  }
}

public class HealthInsuranceCustomerProfile {
  public boolean isLoyalCustomer(){
    return true;// or false
  }
}
```
  * Now One State Insurance Company has acquired a Vechile Insurance company and its tag line has change to: All your Health and Vechile insurance needs!
    * And now we have to modify the calculator class because the calculator method currently takes in HealthInsuranceCustomerProfile object.
      And we want the calculator class to take in VechileInsuranceCustomerProfile Object as well.
    * Only way here is to create a overloaded method in InsurancePremiumDiscountCalculator class. 
    * __This does not follow OCP as each and every time when a new type of Insurance needs to be added then the existing code needs to change.__
 ```java
 public class InsurancePremiumDiscountCalculator {
  public int calculatePremiumDiscountPercentage(HealthInsuranceCustomerProfile customer){
      if(customer.isLoyalCustomer()){
        return 20;
      }
      return 0;
  }
   public int calculatePremiumDiscountPercentage(VechileInsuranceCustomerProfile customer){
      if(customer.isLoyalCustomer()){
        return 20;
      }
      return 0;
  }
}

public class HealthInsuranceCustomerProfile {
  public boolean isLoyalCustomer(){
    return true;// or false
  }
}

public class VechileInsuranceCustomerProfile {
  public boolean isLoyalCustomer(){
    return true;// or false
  }
}
 
 ```
  * Lets refactor to solve this problem 

 ```java
 public class InsurancePremiumDiscountCalculator {
  public int calculatePremiumDiscountPercentage(CustomerProfile customer){
      if(customer.isLoyalCustomer()){
        return 20;
      }
      return 0;
  }
  
public interface CustomerProfile {
    public boolean isLoyalCustomer();
}

public class HealthInsuranceCustomerProfile implements CustomerProfile {
  @Override
  public boolean isLoyalCustomer(){
    return true;// or false
  }
}

public class VechileInsuranceCustomerProfile implements CustomerProfile {
  @Override
  public boolean isLoyalCustomer(){
    return true;// or false
  }
}
 
 ```
