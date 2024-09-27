To create the **Card Guessing Game** with a **Node.js backend** and **React frontend**, we will restructure the previous plan using React for the frontend, while Node.js will serve the React app and manage basic routing.

---

### **1. Planning the Game:**

#### **Objective:**
Create a React-based card guessing game where users can match pairs of cards. Cards flip when clicked, and if they don’t match, they flip back. The game is won when all pairs are matched.

---

### **2. Project Setup:**

#### **Step 1: Backend Setup (Node.js with Express)**

1. **Initialize the Node.js project:**
   ```bash
   mkdir card-guessing-game
   cd card-guessing-game
   npm init -y
   npm install express
   ```

2. **Create the folder structure:**
   ```
   /card-guessing-game
   ├── client
   ├── server.js
   └── package.json
   ```

3. **Set up the server:**
   - **server.js**:
     This serves the React build files and runs the Express server.
   ```javascript
   const express = require('express');
   const path = require('path');
   const app = express();

   // Serve static files from the React app
   app.use(express.static(path.join(__dirname, 'client/build')));

   // Serve index.html for all other routes
   app.get('*', (req, res) => {
     res.sendFile(path.join(__dirname, 'client/build', 'index.html'));
   });

   const PORT = process.env.PORT || 5000;
   app.listen(PORT, () => {
     console.log(`Server running on port ${PORT}`);
   });
   ```

#### **Step 2: React Frontend Setup (Vite)**

1. **Create the React app using Vite:**
   ```bash
   cd card-guessing-game
   npm create vite@latest client --template react
   cd client
   npm install
   ```

2. **React project structure:**
   ```
   /client
   ├── public
   ├── src
   │   ├── components
   │   │   └── CardGame.js
   │   ├── App.jsx
   │   └── index.css
   ├── vite.config.js
   └── package.json
   ```

---

### **3. Design the Card Guessing Game in React:**

#### **Step 1: Create the `CardGame` Component**

- **CardGame.js**:
   ```javascript
   import React, { useState, useEffect } from 'react';
   import './CardGame.css';

   const generateShuffledCards = () => {
     const values = Array.from({ length: 18 }, (_, i) => i + 1);
     const cards = [...values, ...values].sort(() => Math.random() - 0.5);
     return cards.map((value, index) => ({
       id: index,
       value,
       isFlipped: false,
       isMatched: false
     }));
   };

   const CardGame = () => {
     const [cards, setCards] = useState([]);
     const [selectedCards, setSelectedCards] = useState([]);
     const [matchedPairs, setMatchedPairs] = useState(0);
     const [win, setWin] = useState(false);

     useEffect(() => {
       setCards(generateShuffledCards());
     }, []);

     useEffect(() => {
       if (matchedPairs === 18) {
         setWin(true);
       }
     }, [matchedPairs]);

     const handleCardClick = (card) => {
       if (selectedCards.length === 2 || card.isFlipped || card.isMatched) {
         return;
       }

       const newCards = cards.map((c) =>
         c.id === card.id ? { ...c, isFlipped: true } : c
       );
       setCards(newCards);
       setSelectedCards([...selectedCards, card]);

       if (selectedCards.length === 1) {
         checkForMatch(selectedCards[0], card);
       }
     };

     const checkForMatch = (card1, card2) => {
       if (card1.value === card2.value) {
         setMatchedPairs(matchedPairs + 1);
         const updatedCards = cards.map((c) =>
           c.value === card1.value
             ? { ...c, isMatched: true }
             : c
         );
         setCards(updatedCards);
         setSelectedCards([]);
       } else {
         setTimeout(() => {
           const resetCards = cards.map((c) =>
             c.id === card1.id || c.id === card2.id
               ? { ...c, isFlipped: false }
               : c
           );
           setCards(resetCards);
           setSelectedCards([]);
         }, 1000);
       }
     };

     const resetGame = () => {
       setCards(generateShuffledCards());
       setSelectedCards([]);
       setMatchedPairs(0);
       setWin(false);
     };

     return (
       <div className="game-container">
         <h1>Card Guessing Game</h1>
         {win && <div className="win-message">Congratulations, You Win!</div>}
         <div className="grid">
           {cards.map((card) => (
             <div
               key={card.id}
               className={`card ${card.isFlipped || card.isMatched ? 'flipped' : ''}`}
               onClick={() => handleCardClick(card)}
             >
               {card.isFlipped || card.isMatched ? card.value : ''}
             </div>
           ))}
         </div>
         <button className="reset-button" onClick={resetGame}>
           Reset Game
         </button>
       </div>
     );
   };

   export default CardGame;
   ```

#### **Step 2: Update App Component**

- **App.jsx**:
   ```jsx
   import React from 'react';
   import CardGame from './components/CardGame';
   import './App.css';

   function App() {
     return (
       <div className="App">
         <CardGame />
       </div>
     );
   }

   export default App;
   ```

#### **Step 3: Styling the Game**

- **CardGame.css**:
   ```css
   .game-container {
     text-align: center;
     margin: 20px;
   }

   .grid {
     display: grid;
     grid-template-columns: repeat(6, 100px);
     grid-gap: 10px;
     margin: 20px auto;
     max-width: 700px;
   }

   .card {
     width: 100px;
     height: 150px;
     background-color: #ccc;
     border: 1px solid #000;
     display: flex;
     justify-content: center;
     align-items: center;
     font-size: 24px;
     cursor: pointer;
     transition: background-color 0.3s ease;
   }

   .card.flipped {
     background-color: #f0f0f0;
   }

   .card.matched {
     background-color: #8f8;
     cursor: default;
   }

   .win-message {
     font-size: 24px;
     margin-top: 20px;
     color: green;
   }

   .reset-button {
     margin-top: 20px;
   }
   ```

---

### **4. Testing:**

You can run the React app in development mode to test the card logic:
```bash
cd client
npm run dev
```

#### **Manual Testing:**
- Verify that cards are flipped correctly when clicked.
- Ensure the cards flip back if they don’t match.
- Check that the "win" message appears when all pairs are matched.
- Test that the reset button works and reshuffles the cards.

---

### **5. Running the Game:**

To build the React app and serve it with Node.js:
1. Build the React app:
   ```bash
   cd client
   npm run build
   ```

2. Start the Node.js server:
   ```bash
   cd ..
   node server.js
   ```

Visit `http://localhost:5000` in the browser to play the game!

---

### **Conclusion:**

This version of the card guessing game is built with **React** for the frontend and **Node.js** as the backend to serve the app. You can further enhance the game with features like a timer or adding levels for more difficulty.