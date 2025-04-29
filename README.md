<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pizza vs Tacos Poll</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="poll-container">
        <h1>Pizza or Tacos?</h1>
        <div class="buttons">
            <button id="pizzaButton">Pizza</button>
            <button id="tacoButton">Tacos</button>
        </div>
        <div id="results" style="display: none;">
            <h2>Results</h2>
            <p>Pizza: <span id="pizzaPercentage">0%</span></p>
            <p>Tacos: <span id="tacoPercentage">0%</span></p>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>
    <script src="script.js"></script>
</body>
</html># Cln
