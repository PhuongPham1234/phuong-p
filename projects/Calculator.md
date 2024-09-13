---
layout: project
type: project
image: img/Calculator/calculator-gradient-square-icon-vector-8304808.jpg
title: "Calculator"
date: 2024-03-12
published: true
labels:
  - Java
summary: "A simple calculator implemented using stack as data structure for ICS 211 ."
---


In this project I gained experience with handling stack and the errors that could potentially come up such as stack overflow. Utilizing my knowlegde of operands and operators, I successfully implement the program to work on post-fix user input.

Here is the source code for the program:
```
package edu.ics211.h10;
import java.util.Stack;

public class StackCalculator {

	public static String calculate(String[] args) {

		Stack operand = new Stack<>();

		StringBuilder str = new StringBuilder();

		for( int i = 0; i < args.length; i++) {

			String input = args[i];
			char firstChar = input.charAt(0);

			// Check if the first index of the string is a digit, will throw an exception if the there 
			// are invalid character following the first index and have trouble pushing into stack
			if (Character.isDigit(firstChar)) {
				try {
					long number = Long.parseLong(input);
					operand.push(number);
				} catch (NumberFormatException e) {
					str.append("syntax error \n");
				}
			}
			
			// Check if the current index in the argument is an operator
			else if (input.equals("+") || input.equals("-") || input.equals("*") || input.equals("/") || input.equals("%") || input.equals("^") ) {

				// Check if there is at least 2 values in the stack
				if ( operand.size() < 2) {
					str.append("syntax error \n");
				}else {

					// push 2 values from the stack
					long operand1 = (long) operand.pop();
					long operand2 = (long) operand.pop();

					// calculations for each operators
					switch (input) {

					case "+": 
						long sum = operand1 + operand2;
						str.append(sum + "\n");
						operand.push(sum);
						break;
					case "-":
						long result = operand2 - operand1;
						str.append(result + "\n");
						operand.push(result);
						break;
					case "*":
						long product = operand1 * operand2;
						str.append(product + "\n");
						operand.push(product);
						break;
					case "/":
						long quotient = operand2 / operand1;
						str.append(quotient + "\n");
						operand.push(quotient);
						break;
					case "%":
						long modulo = operand2 % operand1;
						str.append(modulo + "\n");
						operand.push(modulo);
						break;
					case "^":
						long power = (long) Math.pow(operand2, operand1);
						str.append(power + "\n");
						operand.push(power);
						break;
					}
				} 
			}
			else {
				str.append("syntax error \n");
			}
			
			
		} // end of for loop
		
		
		return str.toString();
	} // end of calculate method


	public static void main(String[] args) {
		System.out.println(calculate(args));
	}
}

```
