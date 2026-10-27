# GrandMaster Chess — Full-Stack Real-Time Platform

A modern, high-performance full-stack chess web application built from scratch with **React 18**, **Vite**, **Tailwind CSS**, **Node.js**, **Express**, **Socket.IO**, and **Chess.js**.

Designed with a premium dark tournament aesthetic inspired by Chess.com and Lichess, featuring full FIDE chess rules, asynchronous multi-tier AI engine, real-time multiplayer rooms, clocks with increments, move history with past board position exploration, custom board themes, and rich sound synthesizer.

---

## 🌟 Key Features

### 1. Game Modes
- **Play Against Computer (AI)**:
  - 4 Difficulty Levels:
    - **Level 1 — Beginner (~800 ELO)**: Casual play, captures simple undefended pieces.
    - **Level 2 — Casual (~1200 ELO)**: Positional heuristics and center control.
    - **Level 3 — Intermediate (~1600 ELO)**: Minimax search with Alpha-Beta pruning & Piece-Square tables.
    - **Level 4 — Master (~2000 ELO)**: Deep Minimax search + Quiescence capture search with root alpha-beta pruning.
  - Asynchronous calculation with realistic "Thinking..." indicator; never freezes or blocks the UI.
  - Play as White, Black, or Random.
  - Multi-move Undo & Redo.
- **Pass & Play (Local Multiplayer)**:
  - Play on a single device with shared screen.
  - Auto-flip board on turn option or manual one-click flip.
  - Full Undo & Redo history support.
- **Online Multiplayer (Real-Time Socket.IO)**:
  - Instant room creation with a unique 6-character room code (e.g. `K9W4X2`).
  - Shareable room invite codes.
  - Time controls: **1+0 Bullet**, **3+2 Blitz**, **5+0 Rapid**, **10+0 Rapid**, **15+10 Classical**, and **Unlimited**.
  - Authoritative server-side move validation using `Chess.js`.
  - Live clock synchronization and flag detection (timeout loss).
  - In-game real-time chat.
  - Draw offer, accept/decline dialogs, and instant resignation.
  - Rematch system with automatic color alternation.
  - Graceful disconnection and reconnection handling.

### 2. Chess Engine & Rules Compliance
- Standard starting position (White bottom, Black top or flippable).
- Strict legal move generation and check detection.
- Disallows moves that expose or leave the king in check.
- **Castling**: Kingside (`O-O`) and Queenside (`O-O-O`).
- **En Passant**: Dynamic pawn capture tracking.
- **Pawn Promotion**: Interactive modal for promoting pawns to **Queen**, **Rook**, **Bishop**, or **Knight**.
- **Draw Conditions**:
  - Stalemate
  - Threefold Repetition
  - 50-Move Rule
  - Insufficient Material (King vs King, King + Bishop vs King, etc.)
  - Mutual Agreement

### 3. User Interface & Design
- **Dark Theme Palette**: Deep charcoal (`#161512`), slate card backgrounds, crisp white text, and subtle vibrant green accents (`#81b64c`).
- **Piece Sets**: Crisp, tournament-standard vector SVG pieces.
- **Visual Feedback**:
  - Selected square highlight.
  - Legal move dots (empty squares) and capture rings (opponent pieces).
  - Subtle golden tint highlighting the last move (`from` and `to`).
  - Pulsing radial red indicator on the king's square when in check.
  - Low-time warning (<30 seconds) on clocks.
- **Board Customization**:
  - **Classic Green** (Chess.com style)
  - **Charcoal Dark** (Lichess style)
  - **Tournament Wood**
  - **Oceanic Blue**
  - Toggleable legal move dots.
  - Toggleable board coordinates (ranks 1-8, files a-h).
- **Move History Sidebar**:
  - Standard algebraic notation (SAN) grouped in move pairs (e.g. `1. e4 e5 2. Nf3 Nc6`).
  - **Interactive Position Review**: Click any historical move or use navigation controls (`|<`, `<`, `>`, `>|`) to review past positions directly on the board.
  - One-click copy for **PGN** and **FEN** strings.
- **Tactile Audio Synthesizer**:
  - Built-in Web Audio API synthesizer for moves, captures, castling, checks, promotion, and victory/defeat fanfares.
  - Works offline with zero network audio latency and no missing file dependencies.

### 4. Player Statistics & Persistence
- Stored locally via `LocalStorage` (no database required for full local/AI/online play).
- Tracks games played, wins, losses, draws, and win rate percentage.
- Breakdown by mode (Computer, Pass & Play, Online).
- Recent games log with outcome badges, dates, move counts, and end reasons.

---

## 🛠 Technology Stack

- **Frontend**:
  - [React 18](https://react.dev/)
  - [Vite 6](https://vitejs.dev/)
  - [Tailwind CSS 3](https://tailwindcss.com/)
  - [Chess.js](https://github.com/jhlywa/chess.js) (Rule engine)
  - [Lucide React](https://lucide.dev/) (Icons)
  - [Canvas Confetti](https://github.com/catdad/canvas-confetti) (Celebrations)
  - [Socket.IO Client](https://socket.io/) (Real-time synchronization)
- **Backend**:
  - [Node.js](https://nodejs.org/) (ES Modules)
  - [Express.js](https://expressjs.com/) (HTTP & REST API)
  - [Socket.IO](https://socket.io/) (WebSockets server)
  - [Chess.js](https://github.com/jhlywa/chess.js) (Server-side move verification)
  - [CORS](https://github.com/expressjs/cors) & [Dotenv](https://github.com/motdotla/dotenv)

---

## 📁 Project Structure

```text
asd/
├── client/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── ChatPanel.jsx        # Online multiplayer chat
│   │   │   ├── ChessBoard.jsx       # 8x8 interactive board with themes
│   │   │   ├── GameControls.jsx     # Flip, undo/redo, resign, draw, new game
│   │   │   ├── GameOverModal.jsx    # Victory/defeat popup with confetti
│   │   │   ├── MoveHistory.jsx      # SAN notation & board position playback
│   │   │   ├── Navbar.jsx           # Header, mode switcher, quick toggles
│   │   │   ├── OnlineLobbyModal.jsx # Create and join room dialog
│   │   │   ├── PlayerCard.jsx       # Clocks, captured pieces, material score
│   │   │   ├── PromotionModal.jsx   # Pawn promotion picker (Q, R, B, N)
│   │   │   ├── SettingsModal.jsx    # Themes, sounds, coordinates
│   │   │   └── StatsModal.jsx       # Win rate, match history
│   │   ├── utils/
│   │   │   ├── aiEngine.js          # Minimax + Alpha-Beta AI engine
│   │   │   ├── chessPieces.jsx      # High-fidelity SVG vector pieces
│   │   │   ├── sound.js             # Web Audio API sound generator
│   │   │   └── storage.js           # LocalStorage helpers
│   │   ├── App.jsx                  # Main application orchestrator
│   │   ├── index.css                # Tailwind directives & sleek styling
│   │   └── main.jsx                 # React root entry
│   ├── index.html                   # HTML5 page template
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   └── vite.config.js
├── server/
│   ├── src/
│   │   ├── gameManager.js           # Rooms, player states, timers, chess rules
│   │   └── index.js                 # Express app & Socket.IO events
│   ├── .env                         # Server environment variables
│   ├── .env.example
│   ├── package.json
│   └── test-chess.js                # Core rules & server test suite
├── package.json                     # Monorepo root scripts
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** (v18.0 or higher recommended, tested on Node v22)
- **npm** (v9.0 or higher)

### 1. Installation

From the project root directory, install all client and server dependencies:

```bash
# Install root, server, and client dependencies
npm run install:all
```

Or install them individually:
```bash
cd server && npm install
cd ../client && npm install
```

### 2. Running the Application

You can start both the server and client concurrently or in separate terminal windows.

#### Terminal 1 — Start Backend Server (Port 3001)
```bash
cd server
npm run dev
```
The server will start at `http://localhost:3001`.

#### Terminal 2 — Start Frontend Client (Port 5173)
```bash
cd client
npm run dev
```
The Vite development server will start at `http://localhost:5173`. Open this URL in your web browser.

---

## 🧪 Testing

The codebase includes test suites for verifying all chess rules, move validation, AI calculation, and multiplayer logic.

### Test Chess Rules & Server Logic
```bash
cd server
npm test
```
Verifies:
- Standard initial moves
- En Passant pawn capture
- Castling kingside (`O-O`)
- Check detection and illegal move rejection
- Pawn promotion to Queen
- Checkmate (`Fool's Mate`)
- Stalemate detection
- GameManager room creation, turn assignment, move validation, clock synchronization, and resignation.

### Test AI Engine Across Difficulty Levels
```bash
cd client
npm test
```
Verifies:
- Level 1 (Beginner) legal move generation
- Level 2 (Casual) legal move generation
- Level 3 (Intermediate) minimax legal move generation
- Level 4 (Master) alpha-beta + quiescence search and tactical defense against Scholar's mate (`g7->g6`).

---

## 🌐 Configuring Optional Services

### Environment Variables (`server/.env`)
The server uses the following environment variables (defined in `server/.env.example`):

```env
PORT=3001
CLIENT_ORIGIN=http://localhost:5173
NODE_ENV=development
```

### Optional MongoDB Integration
The platform stores game history and user preferences persistently in the browser via `LocalStorage`, so a database is not required for out-of-the-box local or online play. For persistent user accounts across multiple browsers/devices, a MongoDB URI can be added to `server/.env`:
```env
MONGODB_URI=mongodb://localhost:27017/grandmaster_chess
```

---

## 📋 Summary of Implemented Features

| Feature | Status | Details |
|---|---|---|
| **Chess Rules** | ✅ Complete | FIDE standard legal moves, check, checkmate, stalemate, castling, en passant, promotion, 50-move rule, threefold repetition, insufficient material |
| **Move Notation** | ✅ Complete | Standard algebraic notation (SAN), auto-scrolling, review past positions on board, copy PGN & FEN |
| **Play vs Computer** | ✅ Complete | 4 difficulty levels (800-2000 ELO), Minimax + Alpha-Beta + Quiescence, non-blocking asynchronous calculation, AI thinking status |
| **Pass & Play** | ✅ Complete | Two players on same device, manual/auto board flip, undo/redo |
| **Online Multiplayer** | ✅ Complete | Socket.IO rooms, 6-character room codes, server-side move validation, in-game chat, draw offers, resignation, rematch |
| **Clocks & Timers** | ✅ Complete | 1+0, 3+2, 5+0, 10+0, 15+10, Unlimited with increments and timeout flag fall |
| **Visual Customization**| ✅ Complete | 4 Board themes (Classic Green, Charcoal Dark, Tournament Wood, Oceanic Blue), coordinates toggle, legal move indicators |
| **Sound Effects** | ✅ Complete | Zero-latency Web Audio API synthesizer for moves, captures, checks, castling, promotions, game over |
| **Responsiveness** | ✅ Complete | Touch-friendly drag-and-drop & click-to-move for mobile, tablet, and desktop |

