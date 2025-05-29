# Ex06 BMI Calculator
## Date: 

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
### BMICalculator.js
```
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import '../App.css';

function BMICalculator() {
  const [height, setHeight] = useState('');
  const [weight, setWeight] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!height || !weight || isNaN(height) || isNaN(weight) || height <= 0 || weight <= 0) {
      setError('Please enter valid positive numbers for height and weight.');
      return;
    }
    setError('');

    // Navigate to /result with query params
    navigate(`/result?height=${height}&weight=${weight}`);
  };

  return (
    <div className="bmi-box">
      <h2>BMI Calculator</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label>Height (cm): </label>
          <input
            type="text"
            value={height}
            onChange={(e) => setHeight(e.target.value)}
            placeholder="e.g., 170"
          />
        </div>
        <div>
          <label>Weight (kg): </label>
          <input
            type="text"
            value={weight}
            onChange={(e) => setWeight(e.target.value)}
            placeholder="e.g., 65"
          />
        </div>
        {error && <p className="error">{error}</p>}
        <button type="submit">Calculate BMI</button>
      </form>

      {/* Add your text here inside the box */}
      <p className="developer-text">
        Developed by <strong>VASUNDRA SRI R</strong> | Reg no <strong>212222230168</strong>
      </p>
    </div>
  );
}

export default BMICalculator;
```
### Home.js
```
import React from 'react';
import { Link } from 'react-router-dom';
import '../App.css';

function Home() {
  return (
    <div className="bmi-box">
      <h1>Welcome to the BMI Calculator</h1>
      <p>Calculate your Body Mass Index easily.</p>
      <Link to="/bmi">
        <button>Go to BMI Calculator</button>
      </Link>
    </div>
  );
}

export default Home;
```
### Result.js
```
import React from 'react';
import { useNavigate, useLocation } from 'react-router-dom';
import '../App.css';

function Result() {
  const navigate = useNavigate();
  const location = useLocation();

  const params = new URLSearchParams(location.search);
  const heightCm = parseFloat(params.get('height'));
  const weightKg = parseFloat(params.get('weight'));

  if (!heightCm || !weightKg || heightCm <= 0 || weightKg <= 0) {
    return (
      <div className="bmi-box">
        <h2>Error</h2>
        <p>Invalid input data. Please calculate again.</p>
        <button onClick={() => navigate('/bmi')}>Go Back</button>
      </div>
    );
  }

  const heightM = heightCm / 100;
  const bmi = weightKg / (heightM * heightM);
  const bmiRounded = bmi.toFixed(2);

  let category = '';
  if (bmi < 18.5) category = 'Underweight';
  else if (bmi < 24.9) category = 'Normal weight';
  else if (bmi < 29.9) category = 'Overweight';
  else category = 'Obese';

  return (
    <div className="bmi-box">
      <h2>Your BMI Result</h2>
      <p>Height: {heightCm} cm</p>
      <p>Weight: {weightKg} kg</p>
      <p><strong>BMI:</strong> {bmiRounded}</p>
      <p><strong>Category:</strong> {category}</p>
      <button onClick={() => navigate('/bmi')}>Calculate Again</button>
    </div>
  );
}

export default Result;
```
### App.js
```
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Home from './components/Home';
import BMICalculator from './components/BMICalculator';
import Result from './components/Result';
import './App.css';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/bmi" element={<BMICalculator />} />
        <Route path="/result" element={<Result />} />
      </Routes>
    </Router>
  );
}

export default App;
```
### App.css
```
.bmi-box {
  max-width: 500px;      
  margin: 50px auto;     
  padding: 40px 35px;    
  border: 2px solid #4a90e2;
  border-radius: 12px;
  background-color: #f0f8ff;
  box-shadow: 0 0 15px rgba(74, 144, 226, 0.35);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
}

.bmi-box h2 {
  margin-bottom: 25px;
  color: #4a90e2;
  font-size: 2rem;      
}

.bmi-box form div {
  margin-bottom: 18px;
  text-align: left;
}

.bmi-box label {
  display: block;
  margin-bottom: 6px;
  font-weight: 600;
  font-size: 1.1rem;    
}

.bmi-box input {
  width: 100%;
  padding: 10px 12px;
  border: 1.5px solid #4a90e2;
  border-radius: 8px;
  font-size: 1.1rem;    
  box-sizing: border-box;
}

.bmi-box button {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 12px 18px;
  font-size: 1.2rem;    
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.bmi-box button:hover {
  background-color: #357abd;
}

.error {
  color: red;
  font-weight: 600;
  margin-bottom: 18px;
  font-size: 1rem;
}

.developer-text {
  margin-top: 30px;
  font-size: 1rem;        
  color: #555;
  font-style: italic;
}
```

## OUTPUT
![Screenshot 2025-05-29 192221](https://github.com/user-attachments/assets/f7b002d7-bd82-4ae5-9b0c-31a0a6b98842)

![Screenshot 2025-05-29 192238](https://github.com/user-attachments/assets/bf534335-35a3-43dd-b26f-ff7d234a5be7)

![Screenshot 2025-05-29 192252](https://github.com/user-attachments/assets/3b23aa8b-c3bf-4ba4-986b-3e50bfbcfb5d)

## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
