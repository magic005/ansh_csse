---
toc: true
comments: false
layout: post
title: stats calc
description: statistics calculator
type: tangibles
courses: { compsci: {week: 2} }
---

<html>
<head>
  <title>Statistics Calculator</title>
  <style>
    button {
        margin: 5px;
    }
    button:hover {
        background-color: white;
    }

  </style>
</head>
<body>
  <br>
  <label for="dataInput">  data (comma-separated):</label>
  <textarea id="dataInput" rows="4" cols="50"></textarea>
  <br>
  <button onclick="calculateMean()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">mean</button>
  <button onclick="calculateMedian()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">median</button>
  <button onclick="calculateMode()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">mode</button>
  <button onclick="calculateStandardQuarts()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">quarts.</button>
  <button onclick="calculateStandardDeviation()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">SD</button>
  <button onclick="calculateMAD()" style="color:#67dbff; background: black; border-radius: 4px; border: solid;">MAD</button>

  <br>
  <br>
  <div id="result"></div>

  <script>
    function calculateMean() {
        const inputData = document.getElementById("dataInput").value;
        const dataValues = inputData.split(',').map(val => parseFloat(val.trim()));

        const sum = dataValues.reduce((acc, val) => acc + val, 0);
        const mean = sum / dataValues.length;

        displayResult(`thing: ${mean}`);
    }

    function calculateMedian() {
        const inputData = document.getElementById("dataInput").value;
        const dataValues = inputData.split(',').map(val => parseFloat(val.trim()));

        const sortedValues = dataValues.sort((a, b) => a - b);
        const middle = Math.floor(sortedValues.length / 2);

        let median;
        if (sortedValues.length % 2 === 0) {
        median = (sortedValues[middle - 1] + sortedValues[middle]) / 2;
        } else {
        median = sortedValues[middle];
        }

        displayResult(`thing: ${median}`);
    }

    function calculateMode() {
        const inputData = document.getElementById("dataInput").value;
        const dataValues = inputData.split(',').map(val => parseFloat(val.trim()));

        const freqMap = {};
        dataValues.forEach(val => {
        freqMap[val] = (freqMap[val] || 0) + 1;
        });

        let modes = [];
        let maxFreq = 0;
        for (const val in freqMap) {
        if (freqMap[val] > maxFreq) {
            modes = [val];
            maxFreq = freqMap[val];
        } else if (freqMap[val] === maxFreq) {
            modes.push(val);
        }
        }

        displayResult(`thing: ${modes.join(', ')}`);
    }

    function calculateStandardDeviation() {
        const inputData = document.getElementById("dataInput").value;
        const dataValues = inputData.split(',').map(val => parseFloat(val.trim()));

        const mean = dataValues.reduce((acc, val) => acc + val, 0) / dataValues.length;
        const squaredDiffs = dataValues.map(val => (val - mean) ** 2);
        const variance = squaredDiffs.reduce((acc, val) => acc + val, 0) / dataValues.length;
        const standardDeviation = Math.sqrt(variance);

        displayResult(`thing: ${standardDeviation}`);
    }

    function calculateMAD() {
        const inputData = document.getElementById("dataInput").value;
        const dataValues = inputData.split(',').map(val => parseFloat(val.trim()));
        const sum = dataValues.reduce((acc, val) => acc + val, 0);
        const mean = sum / dataValues.length;
        
        


    }

    function displayResult(result) {
        document.getElementById("result").innerHTML = result;
    }
  </script>
</body>
</html>





