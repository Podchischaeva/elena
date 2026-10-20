<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Haunted Apartments — Have/Has got & To be</title>
  <style>
    :root {
      --bg: #130d24;
      --panel: #20163a;
      --panel2: #2b1d4c;
      --purple: #7a5cff;
      --pink: #ff6eb6;
      --gold: #f7d467;
      --green: #7ae7a3;
      --red: #ff6a6a;
      --text: #f5f2ff;
      --muted: #d0c6ef;
      --door: #4b2f75;
      --door-hover: #5d3d8d;
      --shadow: rgba(0,0,0,0.28);
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: "Trebuchet MS", "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #2b1953 0%, #130d24 35%, #0d0915 100%);
      color: var(--text);
      min-height: 100vh;
    }
    .game {
      max-width: 1100px;
      margin: 0 auto;
      padding: 24px 16px 50px;
    }
    .header {
      text-align: center;
      margin-bottom: 18px;
    }
    .title {
      margin: 0;
      font-size: clamp(2rem, 4vw, 3.2rem);
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: #fff4d6;
      text-shadow: 3px 3px 0 #3d2a5c;
    }
    .subtitle {
      margin-top: 8px;
      font-size: 1.15rem;
      color: var(--muted);
    }
    .stars-row {
      display: flex;
      justify-content: center;
      gap: 8px;
      margin-top: 10px;
      flex-wrap: wrap;
    }
    .star {
      font-size: 1.5rem;
      opacity: 0.25;
      filter: grayscale(0.5);
      transition: 0.2s;
    }
    .star.active {
      opacity: 1;
      filter: none;
      transform: scale(1.12);
    }
    .door-grid {
      display: grid;
      grid-template-columns: repeat(4, minmax(120px, 1fr));
      gap: 18px;
      margin-top: 18px;
    }
    .door {
      background: linear-gradient(180deg, #2c1d45 0%, #1d1531 100%);
      border: 3px solid #7a5cff;
      border-radius: 18px;
      padding: 14px 10px;
      min-height: 110px;
      text-align: center;
      cursor: pointer;
      box-shadow: 0 8px 18px var(--shadow);
      transition: transform 0.15s ease, border-color 0.15s ease;
      position: relative;
      overflow: hidden;
    }
    .door:hover:not(.locked) {
      transform: translateY(-3px);
      border-color: #ffb1d5;
    }
    .door.locked {
      opacity: 0.55;
      cursor: not-allowed;
      filter: grayscale(0.15);
    }
    .door .icon {
      font-size: 2.3rem;
      margin-bottom: 6px;
      display: block;
    }
    .door .num {
      font-size: 1.2rem;
      font-weight: bold;
      color: #f7d467;
    }
    .door .label {
      font-size: 0.82rem;
      color: var(--muted);
      margin-top: 6px;
      letter-spacing: 0.05em;
      text-transform: uppercase;
    }
    .door.solved {
      border-color: var(--green);
      background: linear-gradient(180deg, #22352b 0%, #1c2d25 100%);
    }
    .room-panel {
      background: rgba(28, 20, 46, 0.92);
      border: 3px solid #6d57b6;
      border-radius: 24px;
      box-shadow: 0 10px 22px rgba(0,0,0,0.3);
      padding: 20px 18px 18px;
      margin-top: 22px;
    }
    .room-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
      margin-bottom: 12px;
    }
    .room-title {
      font-size: clamp(1.4rem, 2vw, 2rem);
      margin: 0;
      color: #fff3ca;
    }
    .status-badge {
      padding: 8px 12px;
      border-radius: 999px;
      font-size: 0.8rem;
      font-weight: bold;
      text-transform: uppercase;
      letter-spacing: 0.06em;
      background: #3d2d65;
      color: #f7d467;
      border: 1px solid #8d73d5;
    }
    .question-box {
      background: rgba(15, 12, 26, 0.7);
      border: 2px solid #403160;
      border-radius: 16px;
      padding: 18px 16px;
      margin-bottom: 18px;
      text-align: center;
      font-size: clamp(1.1rem, 2vw, 1.5rem);
      line-height: 1.5;
    }
    .answer-wrap {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: center;
      margin: 16px 0 20px;
      min-height: 62px;
      padding: 12px;
      background: rgba(36, 25, 51, 0.7);
      border-radius: 16px;
      border: 2px dashed #7d6cc8;
    }
    .answer-slot {
      min-width: 94px;
      padding: 10px 12px;
      border-radius: 10px;
      background: #ecf4ff;
      color: #1e1b2f;
      font-weight: bold;
      text-align: center;
      cursor: pointer;
      box-shadow: inset 0 -3px 0 rgba(0,0,0,0.1);
      transition: transform 0.1s ease, background 0.15s ease;
    }
    .answer-slot:hover {
      background: #ffe3f0;
      transform: translateY(-2px);
    }
    .word-bank {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin-top: 10px;
    }
    .word {
      border: 2px solid #7a5cff;
      border-radius: 12px;
      background: linear-gradient(180deg, #e8d1ff, #d7c0ff);
      color: #1e1b2f;
      font-weight: 700;
      cursor: pointer;
      padding: 10px 16px;
      transition: 0.15s ease;
      box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }
    .word:hover:not(:disabled) {
      transform: translateY(-2px);
      border-color: #ff9bc6;
    }
    .word:disabled {
      opacity: 0.35;
      cursor: not-allowed;
      filter: grayscale(0.5);
    }
    .controls {
      display: flex;
      justify-content: center;
      gap: 12px;
      flex-wrap: wrap;
      margin-top: 14px;
    }
    button { font: inherit; }
    .btn {
      background: linear-gradient(180deg, #ffcc66, #f4b934);
      color: #2a1d17;
      border: none;
      border-radius: 12px;
      padding: 12px 18px;
      font-weight: 800;
      cursor: pointer;
      box-shadow: 0 5px 0 rgba(0,0,0,0.18);
      transition: transform 0.15s ease;
    }
    .btn:hover { transform: translateY(-1px); }
    .btn.secondary {
      background: linear-gradient(180deg, #9ed9ff, #67afff);
      color: #0f1c36;
    }
    .btn.danger {
      background: linear-gradient(180deg, #ff8a8a, #ff5f5f);
      color: #fff;
    }
    .message {
      margin-top: 10px;
      min-height: 28px;
      text-align: center;
      font-weight: bold;
      font-size: 1.05rem;
    }
    .message.good { color: var(--green); }
    .message.bad { color: var(--red); }
    .secret-room {
      display: none;
      margin-top: 26px;
      background: linear-gradient(180deg, #32215a 0%, #20152f 100%);
      border: 3px solid #f7d467;
      border-radius: 20px;
      padding: 20px;
      text-align: center;
    }
    .secret-room.visible {
      display: block;
      animation: pop 0.4s ease;
    }
    @keyframes pop {
      0% { transform: scale(0.96); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }
    .secret-room h3 {
      margin: 0 0 10px;
      font-size: clamp(1.6rem, 2vw, 2.3rem);
      color: #fff0ae;
    }
    .secret-room p {
      margin: 0;
      font-size: 1.1rem;
      color: var(--muted);
    }
    @media (max-width: 720px) {
      .door-grid {
        grid-template-columns: repeat(2, minmax(130px, 1fr));
      }
    }
  </style>
</head>
<body>
  <div class="game">
    <header class="header">
      <h1 class="title">Haunted Apartments</h1>
      <div class="subtitle">Have / Has got &amp; To be</div>
      <div class="stars-row" id="starsRow"></div>
    </header>

    <div id="doorGrid" class="door-grid"></div>

    <section id="roomPanel" class="room-panel" aria-live="polite">
      <div class="room-header">
        <h2 id="roomTitle" class="room-title">Choose a door</h2>
        <span id="roomBadge" class="status-badge">Room</span>
      </div>
      <div id="questionBox" class="question-box">
        Pick a door to explore. Build the sentence correctly.
      </div>
      <div id="answerWrap" class="answer-wrap"></div>
      <div id="wordBank" class="word-bank"></div>
      <div class="controls">
        <button id="clearBtn" class="btn secondary" type="button">Clear</button>
        <button id="checkBtn" class="btn" type="button">Check</button>
        <button id="resetBtn" class="btn danger" type="button" style="display:none;">Reset Game</button>
      </div>
      <div id="message" class="message"></div>
    </section>

    <div id="secretRoom" class="secret-room">
      <h3>Secret Room Unlocked! 🎉</h3>
      <p>Congratulations! You completed all 8 apartments.</p>
    </div>
  </div>

  <script>
    const rooms = [
      { id: 1, name: "Apartment 1", icon: "🕷️", prompt: "Build the sentence: The ghost is in the hallway.", words: ["The", "ghost", "is", "in", "the", "hallway", "a", "very"], answer: ["The", "ghost", "is", "in", "the", "hallway"] },
      { id: 2, name: "Apartment 2", icon: "🕸️", prompt: "Build the sentence: I am scared.", words: ["I", "am", "scared", "very", "is", "are", "we"], answer: ["I", "am", "scared"] },
      { id: 3, name: "Apartment 3", icon: "🦇", prompt: "Build the sentence: We are in the kitchen.", words: ["We", "are", "in", "the", "kitchen", "am", "they", "happy"], answer: ["We", "are", "in", "the", "kitchen"] },
      { id: 4, name: "Apartment 4", icon: "👻", prompt: "Build the sentence: She has got a lantern.", words: ["She", "has", "got", "a", "lantern", "have", "is", "blue"], answer: ["She", "has", "got", "a", "lantern"] },
      { id: 5, name: "Apartment 5", icon: "🧟", prompt: "Build the sentence: The monster has got claws.", words: ["The", "monster", "has", "got", "claws", "have", "are", "sharp"], answer: ["The", "monster", "has", "got", "claws"] },
      { id: 6, name: "Apartment 6", icon: "🕯️", prompt: "Build the sentence: My friends are in the attic.", words: ["My", "friends", "are", "in", "the", "attic", "am", "house"], answer: ["My", "friends", "are", "in", "the", "attic"] },
      { id: 7, name: "Apartment 7", icon: "🩸", prompt: "Build the sentence: He has got a key.", words: ["He", "has", "got", "a", "key", "are", "we", "small"], answer: ["He", "has", "got", "a", "key"] },
# elena
