---
toc: true
comments: false
layout: post
title: Calculator MD
description: ~scientific calculator
type: tangibles
courses: { compsci: {week: 2} }
---

<style>
  .calculator-output {
    /* calulator output 
      top bar shows the results of the calculator;
      result to take up the entirety of the first row;
      span defines 4 columns and 1 row
    */
    grid-column: span 5;
    grid-row: span 1;
  
    padding: 0.75em;
    font-size: 20px;
    border: 5px solid black;
    border-radius: 8px;
  
    display: flex;
    align-items: center;
  }
  .calculator-clear {
    background-color: black;
  }

  .calculator-equals {
    background-color: black;
  }

</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
      <!--result-->
      <div class="calculator-output" id="output">0</div>
      <!--row 1-->
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-operation">+</div>
      <div class="calculator-operation">-</div>
      <!--row 2-->
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      <div class="calculator-operation">!</div>
      <div class="calculator-operation">^</div>
      <!--row 3-->
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      <div class="calculator-number">9</div>
      <div class="calculator-operation">*</div>
      <div class="calculator-operation">/</div>
      <!--row 4-->
      <div class="calculator-clear">A/C</div>
      <div class="calculator-number">0</div>
      <div class="calculator-number">.</div>
      <div class="calculator-equals">=</div>
      <div class="calculator-operation">()</div>
      <!--row 5-->
      
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
  // initialize important variables to manage calculations
  var firstNumber = null;
  var operator = null;
  var nextReady = true;
  // build objects containing key elements
  const output = document.getElementById("output");
  const numbers = document.querySelectorAll(".calculator-number");
  const operations = document.querySelectorAll(".calculator-operation");
  const clear = document.querySelectorAll(".calculator-clear");
  const equals = document.querySelectorAll(".calculator-equals");

  // Number buttons listener
  numbers.forEach(button => {
    button.addEventListener("click", function() {
      number(button.textContent);
    });
  });

  // Event listener for number key presses
  document.addEventListener("keydown", function(event) {
    // Check if the pressed key is a number key (0-9)
    const pressedKey = event.key;
    if (!isNaN(pressedKey) && pressedKey !== " ") {
      const numberKeys = document.querySelectorAll(".calculator-number");
      numberKeys.forEach(numberKey => {
        if (numberKey.textContent === pressedKey) {
          number(pressedKey);
        }
      });
    }
  });

  /*document.addEventListener("keydown", function(event) {
    const pressedKey = event.key;
    if (!isNaN(pressedKey)) {
      const operationKeys = document.querySelectorAll(".calculator-operation");
      operationKeys.forEach(operationKey => {
        if (operationKey.textContent === pressedKey) {
          operation(pressedKey);
        }  
      });
    }
  });*/

  document.addEventListener("keydown", function(event) {
    const pressedKey = event.key;
    const operationKeys = ["+","-","*", "!", "/", "^","()"];
    if (operationKeys.includes(pressedKey)) {
      operation(pressedKey);
    }
  });

  document.addEventListener("keydown", function(event) {
    const pressedKey = event.key;
    if (pressedKey === "Enter") {
      equal();
    }
  });

  document.addEventListener("keydown", function(event) {
    const pressedKey = event.key;
    if (pressedKey === "Backspace") {
      clearCalc();
    }
  });

  document.addEventListener("keydown", function(event) {
    if (event.key === 'w') {
      const link = 'https://www.desmos.com/calculator/szfporzixs';
      window.open(link);
    }
  });

  // Number action
  function number (value) { // function to input numbers into the calculator
      if (value != ".") {
          if (nextReady == true) { // nextReady is used to tell the computer when the user is going to input a completely new number
              output.innerHTML = value;
              if (value != "0") { // if statement to ensure that there are no multiple leading zeroes
                  nextReady = false;
              }
          } else {
              output.innerHTML = output.innerHTML + value; // concatenation is used to add the numbers to the end of the input
          }
      } else { // special case for adding a decimal; can't have two decimals
          if (output.innerHTML.indexOf(".") == -1) {
              output.innerHTML = output.innerHTML + value;
              nextReady = false;
          }
      }
  }

  // Operation buttons listener
  operations.forEach(button => {
    button.addEventListener("click", function() {
      operation(button.textContent);
    });
  });

  // Operator action
  function operation (choice) { // function to input operations into the calculator
      if (firstNumber == null) { // once the operation is chosen, the displayed number is stored into the variable firstNumber
          firstNumber = parseInt(output.innerHTML);
          nextReady = true;
          operator = choice;
          return; // exits function
      }
      // occurs if there is already a number stored in the calculator
      firstNumber = calculate(firstNumber, parseFloat(output.innerHTML)); 
      operator = choice;
      output.innerHTML = firstNumber.toString();
      nextReady = true;
  }

  // Calculator
  function calculate (first, second) { // function to calculate the result of the equation
      let result = 0;
      switch (operator) {
          case "+":
              result = first + second;
              break;
          case "-":
              result = first - second;
              break;
          case "*":
              result = first * second;
              break;
          case "/":
              if (second === 0) {
                result = "bro rly";
              } else {
                  result = first / second;
              }
              break;
          case "!":
              let fact = 1
              for (i = 1; i<= first; i++){
                fact *= i;
              }
              result = fact;
              break;
          case "^":
              result = first ** second;
              break;
          case "()":
              window.open("https://www.desmos.com/calculator/szfporzixs");
              result = "3.141592653589793238462643383279502884197169";
              break;
          default: 
              break;
      }
      return result;
  }

  // Equals button listener
  equals.forEach(button => {
    button.addEventListener("click", function() {
      equal();
    });
  });

  // Equal action
  function equal () { // function used when the equals button is clicked; calculates equation and displays it
      firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
      output.innerHTML = firstNumber.toString();
      nextReady = true;
  }

  // Clear button listener
  clear.forEach(button => {
    button.addEventListener("click", function() {
      clearCalc();
    });
  });

  // A/C action
  function clearCalc () { // clears calculator
      firstNumber = null;
      output.innerHTML = "0";
      nextReady = true;
  }
</script>