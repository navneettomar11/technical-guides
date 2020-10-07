# What Is Refactoring?


# The Starting Point
The sample program is very simple. It is a program to calculate and print a statement of a customer's charges at a video store. The program is told which movies a customer rented and for how long. It then calculates the charges, which depend on how long the movie is rented, and identifies the type movie. There are three kinds of movies: Regular, Children and new releases. In addition to calculating charges, the statement also computes frequent renter points, which vary depending on whether the film is a new release.
[!Class diagram of the Starting-point class](assets/refactoring_fig01.png)

**Movie**
Movie is just a simple data class
```java
public class Movie {

  public static final int  CHILDRENS = 2;   
  public static final int  REGULAR = 0;    
  public static final int  NEW_RELEASE = 1;

  private String _title;    
  private int _priceCode;

  public Movie(String title, int priceCode) {        
    _title = title;        
    _priceCode = priceCode;    
  }          
  public int getPriceCode() {       
     return _priceCode;    
   }

  public void setPriceCode(int arg) {     
     _priceCode = arg;    
   }

  public String getTitle (){        
    return _title;    
  };  
}
```
**Rental**
The rental class represents a customer renting a movie.

```java
class Rental {      
  private Movie _movie;      
  private int _daysRented;

  public Rental(Movie movie, int daysRented) {        
    _movie = movie;        
    _daysRented = daysRented;      
  }      
  public int getDaysRented() {       
     return _daysRented;      
   }      
   public Movie getMovie() {       
      return _movie;      
    }   
  }
```
**Customer**
The customer class represents the customer of the store. Like the other classes it has dat and accessors:

```java
class Customer {    
  private String _name;    
  private Vector _rentals = new Vector();

  public Customer (String name){       
     _name = name;    
   };

  public void addRental(Rental arg) {      
    _rentals.addElement(arg);    
  }    
  public String getName (){        
    return _name;    
  }
}
```
Customers also has the method that produces a statement. The body for this method is on the facing page.
```java
public String statement() {         
  double totalAmount = 0;         
  int frequentRenterPoints = 0;         
  Enumeration rentals = _rentals.elements();         
  String result = "Rental Record for " + getName() + "\n";         
  while (rentals.hasMoreElements()) {             
    double thisAmount = 0;             
    Rental each = (Rental) rentals.nextElement();
    //determine amounts for each line             
    switch (each.getMovie().getPriceCode()) {                 
      case Movie.REGULAR:                     
      thisAmount += 2;                     
      if (each.getDaysRented() > 2) {
        thisAmount += (each.getDaysRented() - 2) * 1.5;
      }
      break;                 
      case Movie.NEW_RELEASE:
      thisAmount += each.getDaysRented() * 3;
      break;
      case Movie.CHILDRENS:
      thisAmount += 1.5;
      if (each.getDaysRented() > 3) thisAmount += (each.getDaysRented() - 3) * 1.5;
      break;  
    }

    // add frequent renter points
    frequentRenterPoints ++;
    // add bonus for a two day new release rental
    if ((each.getMovie().getPriceCode() == Movie.NEW_RELEASE) && each.getDaysRented() > 1) frequentRenterPoints ++;

    //show figures for this rental
    result += "\t" + each.getMovie().getTitle()+ "\t" + String.valueOf(thisAmount) + "\n";
    totalAmount += thisAmount;

    }         
    //add footer lines         
    result +=  "Amount owed is " + String.valueOf(totalAmount) + "\n";        
    result += "You earned " + String.valueOf(frequentRenterPoints) + " frequent renter points";         
    return result;
  }
```
