---
layout: project
type: project
image: img/Snake_OG-logo.jpg
title: "Temperature Converter"
date: 2024-7-14
published: true
labels:
  - Java
summary: "A temperature converter program that I created using Java"
---

In this project I gained a deeper understanding of fundamental Java concepts and practical programming skills. This was my first personal programming project outside of class after I took ICS 111. Many concepts implemented aren't too complex though I have learned about the importance of code structure. This project will be remembered as the project that ignites my programming career!


```
import java.util.Scanner;

public class TemperatureConverter {

    public static void main(String[] args) {
        
      
        Scanner input = new Scanner(System.in);
        
      
        System.out.print("Enter temperature value: ");
        double temp = input.nextDouble();       
      
        System.out.print("Enter temperature scale (C, F, or K): ");
        String scale = input.next();
      
        double celsius;
        switch(scale.toUpperCase()) {
            case "C":
                celsius = temp;
                break;
            case "F":
                celsius = (temp - 32) * 5 / 9;
                break;
            case "K":
                celsius = temp - 273.15;
                break;
            default:
                System.out.println("Invalid temperature scale.");
                return;
        }
      
        double fahrenheit = celsius * 9 / 5 + 32;
      
        double kelvin = celsius + 273.15;
      
        System.out.println("Celsius: " + celsius + " C");
        System.out.println("Fahrenheit: " + fahrenheit + " F");
        System.out.println("Kelvin: " + kelvin + " K");
    }

}
```
