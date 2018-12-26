### Class, Object, Method and Property

Class is like a blueprint like Car design from which Object can be created.
Object is instance of the Class like BMW is an instance of Car.
Functions inside a class are called Methods.
Variables inside a class are called Properties.

```php
<?php
// Declare the class
class Car
{
    // Properties
    public $comp;
    public $color = 'beige';
    public $hasSunRoof = true;
 
    // Method that says hello
    public function hello()
    {
        return "beep";
    }
}
 
// Create an instance with new keyword
$bmw = new Car();
$mercedes = new Car();
 
// Get the values
echo $bmw->color; // beige
echo "<br />";
echo $mercedes->color; // beige
echo "<hr />";
 
// Set the values
$bmw->color = 'blue';
$bmw->comp = "BMW";
$mercedes->comp = "Mercedes Benz";
 
// Get the values again
echo $bmw->color; // blue
echo "<br />";
echo $mercedes->color; // beige
echo "<br />";
echo $bmw->comp; // BMW
echo "<br />";
echo $mercedes->comp; // Mercedes Benz
echo "<hr />";
 
// Use the methods to get a beep
echo $bmw->hello(); // beep
echo "<br />";
echo $mercedes->hello(); // beep
```

### $this keyword

$this keyword indicates own class. Own methods and properties can be accessed through $this keyword.

```php
<?php

class Car
{
    // The properties
    public $comp;
    public $color = 'red';
    public $hasSunRoof = true;

    // The method that says hello
    public function hello()
    {
        // accessing own properties and methods using $this
        return "Beep, I am a " . $this->comp .
            ", and my color is" . $this->color.
            ". I am created at " . $this->model() . ".";
    }

    public function model()
    {
        return date('Y');
    }
}

// We can now create an object from the class.
$bmw = new Car();
$mercedes = new Car();

// Set the values of the class properties.
$bmw->color = 'blue';
$bmw->comp = "BMW";
$mercedes->comp = "Mercedes Benz";

// Call the hello method.
echo $bmw->hello();
echo $mercedes->hello();
```

### Method Chaining

When a Class's Methods returns $this keyword, then can be chained together.

```php
<?php

class Car
{
    public $tank;
    
    // Add gallons of fuel to the tank when we fill it.
    public function fill($float)
    {
        $this->tank += $float;

        return $this;
    }
    
    // Substract gallons of fuel from the tank as we ride the car.
    public function ride($float)
    {
        $miles = $float;
        $gallons = $miles / 50;
        $this->tank -= $gallons;

        return $this;
    }
}

// Create a new object from the Car class.
$bmw = new Car();
 
// Add 10 gallons of fuel, then ride 40 miles, 
// and get the number of gallons in the tank. 
$tank = $bmw->fill(10)->ride(40)->tank;
 
// Print the results to the screen.
echo "The number of gallons left in the tank: " . $tank . " gal.";
```

### Access Modifier

Access modifiers add Encapsulation to class properties and methods,
that means it declares the visibility of the properties and methods.

Access modifiers are Public, Private and Protected.
Public modifier allows a code from outside or inside the class to access the class's methods and properties.
Private modifier prevents access to a class's methods or properties from any code that is outside the class.
Protected modifier, which allows code usage from both inside the class and from its child classes.

```php
<?php

class Car
{
    // Private access modifier denies access to the property and method from outside the class’s scope
    private $model;
 
    // Public access modifier allows the access from outside the class
    public function setModel($model)
    {
        $this->model = $model;
    }

    public function getModel()
    {
        return "The car model is  " . $this->model;
    }
}

$mercedes = new Car();
//Sets the car’s model
$mercedes->setModel("Mercedes benz");
//Gets the car’s model
echo $mercedes->getModel();
```

### Magic Method and Constant

The "magic" methods are ones with special names, starting with two underscores, 
which denote methods which will be triggered in response to particular PHP events.

PHP provide a set of special predefined constants that change depending on where they are used.
These constants are called magic constants, begin and end with two underscores.

```php
<?php

class Car
{
    private $model = '';
  
    // __construct magic method
    public function __construct($model = null)
    {
        if ($model) {
            $this->model = $model;
        }
    }

    public function getCarModel()
    {
        // We use the __CLASS__ magic constant in order to get the class name
        return " The <b>" . __class__ . "</b> model is: " . $this->model;
    }
}

$car1 = new Car('Mercedes');

echo $car1->getCarModel();
```

## Inheritence

In inheritance, we have a parent class with its own methods and properties,
and a child class (or classes) that can use the code from the parent. 

```php
<?php
// The parent class
class Car
{
    // The $model property is protected, so it can be accessed 
    // from within the class and its child classes
    protected $model;
   
    // Public setter method
    public function setModel($model)
    {
        $this->model = $model;
    }
}
  
// The child class
class SportsCar extends Car
{
    public function hello()
    {
        // Has no problem to get a protected property that belongs to the parent
        return "beep! I am a <i>" . $this->model . "</i><br />";
    }
}
  
// Create an instance from the child class
$sportsCar1 = new SportsCar();
  
// Set the class model name
$sportsCar1->setModel('Mercedes Benz');
  
// Get the class model name
echo $sportsCar1->hello();
```

### Override

Child class can have its own properties and methods same as parent,
it can override the properties and methods of the parent class.

In order to prevent the method in the child class from overriding the parent’s methods,
we can prefix the method in the parent with the **final** keyword.

```php
<?php
// The parent class has hello method that returns "beep".
class Car
{
    // Child class can override this method
    public function hello()
    {
        return "beep";
    }

    // Prevents the child class from overriding
    final public function tryToOverride()
    {
        return "try hard";
    }
}
   
// The child class has hello method that returns "Hallo"
class SportsCar extends Car
{
    public function hello()
    {
        return "Hallo";
    }

    // This code will throw PHP Fatal error: "Cannot override"
    // public function tryToOverride()
    // {
    //     return "trying for lifetime";
    // }
}
      
// Create a new object
$sportsCar1 = new SportsCar();

// Get the result of the hello method
echo $sportsCar1->hello();
```

### Abstruct Class and Method

```php
<?php
// Abstract classes are declared with the abstract keyword, and contain abstract methods.
abstract class Car
{
    // Abstract classes can have properties
    protected $tankVolume;
   
    // Abstract classes can have non abstract methods
    public function setTankVolume($volume)
    {
        $this->tankVolume = $volume;
    }
   
    // Abstract method
    abstract public function calcNumMilesOnFullTank();
}

class Toyota extends Car
{
    // Since we inherited abstract method, we need to define it in the child class, 
    // by adding code to the method's body.
    public function calcNumMilesOnFullTank()
    {
        return $miles = $this->tankVolume * 33;
    }

    public function getColor()
    {
        return "beige";
    }
}

$toyota1 = new Toyota();
$toyota1->setTankVolume(10);
echo $toyota1->calcNumMilesOnFullTank();//330
echo $toyota1->getColor();//beige
```

### Interface

```php
<?php

interface Car
{
    // Can include public abstract methods and constants
    public function setModel($name);

    public function getModel();
}

interface Vehicle
{
    public function setHasWheels($bool);

    public function getHasWheels();
}

class miniCar implements Car, Vehicle
{
    // Implements interface methods and may have its own code
    private $model;
    private $hasWheels;

    public function setModel($name)
    {
        $this->model = $name;
    }

    public function getModel()
    {
        return $this->model;
    }

    public function setHasWheels($bool)
    {
        $this->hasWheels = $bool;
    }

    public function getHasWheels()
    {
        return ($this->hasWheels) ? "has wheels" : "no wheels";
    }

    public function talk()
    {
        return "wow";
    }
}
```

### Polymorphism

```php
<?php
// According to the Polymorphism principle, methods in 
// different classes that do similar things should have the same name
interface Shape
{
    public function calcArea();
}

class Circle implements Shape
{
    private $radius;

    public function __construct($radius)
    {
        $this->radius = $radius;
    }
    
    // calcArea calculates the area of circles 
    public function calcArea()
    {
        return $this->radius * $this->radius * pi();
    }
}

class Rectangle implements Shape
{
    private $width;
    private $height;

    public function __construct($width, $height)
    {
        $this->width = $width;
        $this->height = $height;
    }
    
    // calcArea calculates the area of rectangles   
    public function calcArea()
    {
        return $this->width * $this->height;
    }
}

$circ = new Circle(3);
$rect = new Rectangle(3, 4);

echo $circ->calcArea();
echo $rect->calcArea();
```

### Type Hinting

```php
<?php
// With Type hinting we can specify the expected data type 
// (arrays, objects, interface, etc.) for an argument in a function declaration
class Car
{
    protected $driver;
    protected $model;
    protected $hasSunRoof;
    protected $numberOfDoors;
    protected $price;
       
    // The constructor can only get Driver objects as arguments.
    public function __construct(Driver $driver)
    {
        $this->driver = $driver;
    }

    // string type hinting
    public function setModel(string $model)
    {
        $this->model = $model;
    }

    // boolean type hinting
    public function setHasSunRoof(bool $value)
    {
        $this->hasSunRoof = $value;
    }

    // integer type hinting
    public function setNumberOfDoors(int $value)
    {
        $this->numberOfDoors = $value;
    }

    // float type hinting
    public function setPrice(float $value)
    {
        $this->price = $value;
    }
}

class Driver
{

}

// Car accepts Driver type
$driver1 = new Driver();
$car1 = new Car($driver1);

$car1->setModel('BMW');
$car1->setHasSunRoof(true);
$car1->setNumberOfDoors(4);
$car1->setPrice(1234.56);
```

### Static Method and Property

```php
<?php

class Utilis
{
    // Declaring class properties or methods as static makes them 
    // accessible without needing an instantiation of the class.
    public static $numCars = 0;

    public static function addToNumCars($int)
    {
        $int = (int)$int;
        self::$numCars += $int;
    }
}

echo Utilis::$numCars;

Utilis::addToNumCars(3);
echo Utilis::$numCars;

Utilis::addToNumCars(-1);
echo Utilis::$numCars;
```

### Trait

```php
<?php
// Traits are a mechanism for code reuse in single inheritance languages such as PHP
// Reuse sets of methods from trait freely in several independent classes
trait Hello
{
    public function sayHello()
    {
        echo 'Hello ';
    }
}

trait World
{
    public function sayWorld()
    {
        echo 'World';
    }
}

class MyHelloWorld
{
    // Multiple Traits Usage 
    use Hello, World;
    public function sayExclamationMark()
    {
        echo '!';
    }
}

$o = new MyHelloWorld();
$o->sayHello();
$o->sayWorld();
$o->sayExclamationMark();
```