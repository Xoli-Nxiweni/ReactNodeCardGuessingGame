# Card Guessing Game

## Description

This project is a card guessing game built with a **React frontend** and a **Node.js backend**. Users match pairs of cards by clicking on them. If the cards match, they remain flipped. If they don't match, they flip back. The game is won when all pairs are matched. The Node.js backend serves the React app.

## Features

- Match pairs of cards by flipping them.
- Cards flip back if they don't match.
- Game is won when all pairs are matched.
- Reset button to start a new game.

---

## Project Structure

```bash
/card-guessing-game
├── client           # React frontend
├── server.js        # Node.js backend server
└── package.json     # Backend dependencies
```

## Installation

### Step 1: Clone the repository

```bash
git clone https://github.com/Xoli-Nxiweni/ReactNodeCardGuessingGame.git
cd ReactNodeCardGuessingGame
```

### Step 2: Install backend dependencies

```bash
npm install
```

### Step 3: Setup the React frontend

Navigate to the `client` directory and install frontend dependencies:

```bash
cd client
npm install
```

---

## Running the Project

### Step 1: Start the React app in development mode

```bash
cd client
npm run dev
```

### Step 2: Start the Node.js server

Open a new terminal and run the backend server:

```bash
cd ..
node server.js
```

### Step 3: Play the game

Once both the frontend and backend are running, open `http://localhost:5000` in your browser to play the game.

---

## Backend Details (Node.js)

### Server Setup

- **Node.js** with **Express** serves the React app and handles routing.
- Static files from the React app are served using the following route:

```javascript
app.use(express.static(path.join(__dirname, 'client/build')));
```

- For any non-API requests, the server returns the `index.html` file to support React Router:

```javascript
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'client/build', 'index.html'));
});
```

### Running the Build

To build the React app for production and serve it using Node.js:

```bash
cd client
npm run build
```

This will generate static files in the `client/build` folder, which are served by the backend when deployed.

---

## Frontend Details (React)

### Game Logic

The card guessing game is managed through the **CardGame** component:

- **CardGame.js**:
   - Manages game state with `useState` and `useEffect`.
   - Cards are shuffled and displayed in a grid.
   - When two selected cards match, they remain flipped. Otherwise, they flip back after a brief delay.
   - The game is won when all pairs are matched.
   - The "Reset Game" button reshuffles the cards and resets the game state.

### UI Styling

- **CardGame.css** styles the game grid and card elements, making the game visually appealing.
   - `.card` classes handle the display of the cards.
   - `.win-message` shows the congratulatory message upon winning.

---

## Author

**Xoli Nxiweni**  
Email: xolinxiweni@gmail.com  
GitHub: [Xoli-Nxiweni](https://github.com/Xoli-Nxiweni)

---
