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
</html># 
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin-top: 50px;
}

.poll-container {
    width: 300px;
    margin: 0 auto;
}

.buttons button {
    font-size: 20px;
    padding: 10px;
    margin: 20px;
    width: 120px;
    cursor: pointer;
}

#results {
    margin-top: 30px;
}
// Initialize Firebase
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};

const app = firebase.initializeApp(firebaseConfig);
const db = firebase.firestore(app);

// Get the buttons and results div
const pizzaButton = document.getElementById('pizzaButton');
const tacoButton = document.getElementById('tacoButton');
const resultsDiv = document.getElementById('results');
const pizzaPercentage = document.getElementById('pizzaPercentage');
const tacoPercentage = document.getElementById('tacoPercentage');

// Event listeners for button clicks
pizzaButton.addEventListener('click', () => submitVote('pizza'));
tacoButton.addEventListener('click', () => submitVote('tacos'));

// Submit the vote to Firebase Firestore
async function submitVote(choice) {
    try {
        // Save the vote in Firestore
        await db.collection('votes').add({
            choice: choice,
            timestamp: firebase.firestore.FieldValue.serverTimestamp()
        });

        // Get updated vote counts and percentages
        getVotePercentages();
    } catch (error) {
        console.error('Error submitting vote:', error);
    }
}

// Function to get and display the vote percentages
async function getVotePercentages() {
    try {
        const snapshot = await db.collection('votes').get();
        const totalVotes = snapshot.size;

        if (totalVotes === 0) {
            pizzaPercentage.textContent = '0%';
            tacoPercentage.textContent = '0%';
            return;
        }

        let pizzaVotes = 0;
        let tacoVotes = 0;

        snapshot.forEach(doc => {
            const data = doc.data();
            if (data.choice === 'pizza') pizzaVotes++;
            if (data.choice === 'tacos') tacoVotes++;
        });

        const pizzaPercentageValue = ((pizzaVotes / totalVotes) * 100).toFixed(2);
        const tacoPercentageValue = ((tacoVotes / totalVotes) * 100).toFixed(2);

        pizzaPercentage.textContent = `${pizzaPercentageValue}%`;
        tacoPercentage.textContent = `${tacoPercentageValue}%`;

        resultsDiv.style.display = 'block';
    } catch (error) {
        console.error('Error getting vote percentages:', error);
    }
}

// Initialize vote percentages
getVotePercentages();
