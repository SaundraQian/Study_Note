---
description: Algonquin College CST8288 note
---

# Strategy pattern

The Strategy pattern consists of these three main roles: Context, Strategy, and ConcreteStrategy.

* Context: holds a reference to a strategy class and is provided to the client for use.&#x20;
* Strategy: This is an abstract role, usually implemented by an interface or abstract class. This role gives all the interfaces required for the concrete strategy class.&#x20;
* ConcreteStrategy: packages the associated algorithm or behavior.

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption><p>Week 2 PPT - P9</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption><p>Week 2 PPT - P10</p></figcaption></figure>

````java
//Strategy - interface
public interface UnitConversion {
     double convert(double value);
}

```

//CClient
public class UnitConverter {
  private UnitConversion unitConversion;
   
  public UnitConverter( UnitConversion temperature){
       this.temperature = temperature;
   }
  
   public double executeStrategy(double value) {
        return temperature.convert(value);
   }


//general class- concrete strategy A
public class CFconverter implements UnitConversion  {
	private final double convFactor=1.8;
	private final double convOrigin=32.0;
        @Override
	
	public double convert(double celsius) {
		return celsius*convFactor + convOrigin;
	}
}

```

//general class- concrete strategy B
public class FCconverter implements UnitConversion {
	private final double convFactor=1.8;
	private final double convOrigin=32.0;
	
	@Override
	public double convert(double fahrenheit) {
		return (fahrenheit - convOrigin)/convFactor;
	}
}

//context
```java
public class UnitConverterTest {
    public static void main(String[] args) {
        UnitConverter converter = new UnitConverter(new FCconverter());
	System.out.printf("%5.2f in Fahrenheit is %5.2f Celsius\n", 70.0, converter.executeStrategy(70.0));
		
	UnitConverter converter2 = new UnitConverter(new CFconverter());
	System.out.printf("%5.2f in Celsius is %5.2f Fahrenheit\n", 20.0, converter2.executeStrategy(20.0));
		
        UnitConverter converter3 = new UnitConverter(new KMconverter());
	System.out.printf("%5.2f in Kilometers is %5.2f miles\n", 70.0, converter3.executeStrategy(70.0));
		
	UnitConverter converter4 = new UnitConverter(new MKconverter());
        System.out.printf("%5.2f in miles is %5.2f Kiloemters\n", 20.0, converter4.executeStrategy(20.0));
	}

}
```
````
