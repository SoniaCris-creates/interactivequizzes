<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Kfoort! - Fun Learning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script&display=swap" rel="stylesheet">
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f0f8ff;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 30px;
    margin: 0;
    min-height: 100vh;
    position: relative;
    box-sizing: border-box;
  }
  header {
    text-align: center;
    margin-bottom: 30px;
    color: #1a73e8;
  }
  header h1 { font-size: 3rem; margin: 0; }
  header p {
    font-family: 'Dancing Script', cursive;
    font-size: 1.6rem;
    margin: 20px 0 40px 0;
    font-weight: 600;
    color: #555;
  }
  form {
    display: flex;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 20px;
  }
  label {
    white-space: nowrap;
    margin-right: 8px;
    width: 100px;
    text-align: right;
    font-weight: bold;
    color: #333;
  }
  input[type="text"] {
    min-width: 3cm;
    padding: 6px 8px;
    font-size: 1rem;
    border: 2px solid #1a73e8;
    border-radius: 5px;
  }
  button { /* General button style for Access button */
    background-color: #1a73e8;
    color: white;
    border: none;
    padding: 10px 18px;
    font-size: 1rem;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button:hover { background-color: #1669c1; }
  #message {
    margin-top: 15px;
    font-weight: bold;
    color: green;
    font-size: 1.2rem;
    text-align: center;
  }
  footer#mainFooter { /* Specific selector for main page footer */
    width: 100%;
    max-width: 600px;
    margin-top: 20px;
    border-top: 1px solid #1a73e8;
    padding-top: 10px;
    text-align: center;
    color: #1a73e8;
    font-family: 'Dancing Script', cursive;
    font-size: 1rem;
  }
  /* Removed #musicPlayer and related Vocaroo styles */
</style>
</head>
<body>

<header id="mainHeader">
  <h1>🎉 Kfoort! 🎉</h1>
  <p>US QUIZZ-Let's have fun and learn together!</p>
</header>

<form id="accessForm">
  <label for="studentName">Student name:</label>
  <input type="text" id="studentName" name="studentName" autocomplete="off" />
  <label for="date">Date:</label>
  <input type="text" id="date" name="date" placeholder="dd/mm/yyyy" autocomplete="off" maxlength="10" />
  <label for="code">Code:</label>
  <input type="text" id="code" name="code" autocomplete="off" />
  <button type="submit">Access</button>
</form>

<div id="message"></div>
<footer id="mainFooter">Digital Resource Designed by Sonia Monfort.</footer>
<div id="kfoortRootContainer" style="display: none; width: 100%;"></div>

<script>
  const form = document.getElementById('accessForm');
  const message = document.getElementById('message');
  // let audioElement = null; // Removed as Vocaroo is removed

  let kfoortQuestions = [];
  let currentQuestionIndex = 0;
  let score = 0;
  let incorrectAnswers = 0;
  let studentNameForKfoort = "";

  const kfoortRootContainer = document.getElementById('kfoortRootContainer');
  const mainHeader = document.getElementById('mainHeader');
  const accessForm = document.getElementById('accessForm');
  const mainFooter = document.getElementById('mainFooter');
  const messageDiv = document.getElementById('message');

  function generateKfoortQuestions() {
    return [
      // U.S. States & Culture Quiz - 40 Questions
      {
        type: 'us_states_culture',
        question: "What's the capital of New York?",
        options: ["Albany", "Richmond", "Colombia", "New York city"],
        correctAnswer: "Albany"
      },
      {
        type: 'us_states_culture',
        question: "What's the typical food in New York?",
        options: ["Donough", "Beagel", "Hot dog", "Hamburgers"],
        correctAnswer: "Hot dog"
      },
      {
        type: 'us_states_culture',
        question: "When was New York founded?",
        options: ["July 5th", "September 14th", "July 26th", "December 10th"],
        correctAnswer: "July 26th"
      },
      {
        type: 'us_states_culture',
        question: "What's the capital of Hawaii?",
        options: ["Honolulu", "Oahu", "Indiana", "Boston"],
        correctAnswer: "Honolulu"
      },
      {
        type: 'us_states_culture',
        question: "How many people live in the capital of Hawaii?",
        options: ["875,000 inhabitants", "905,000 inhabitants", "1,322,000 inhabitants", "787,000 inhabitants"],
        correctAnswer: "875,000 inhabitants"
      },
      {
        type: 'us_states_culture',
        question: "What is the typical food of Hawaii?",
        options: ["Fish & pineapple", "Watermelon", "Coconuts and hamburger", "Coconuts & pineapple"],
        correctAnswer: "Coconuts & pineapple"
      },
      {
        type: 'us_states_culture',
        question: "Which are the most important monuments in Florida?",
        options: ["Disney Park & biggest castle", "El David & biggest castle", "Disney Park", "All of them"],
        correctAnswer: "Disney Park & biggest castle"
      },
      {
        type: 'us_states_culture',
        question: "What's the capital of Florida?",
        options: ["Tallahassee", "Miami", "Orlando", "Albany"],
        correctAnswer: "Tallahassee"
      },
      {
        type: 'us_states_culture',
        question: "What's the name of the basketball stadium in Florida?",
        options: ["United Center", "Kaseya Center", "Port Arena", "Arena Center"],
        correctAnswer: "Kaseya Center" // PDF did not mark a correct answer. Using common knowledge. User may want to verify/change. [cite: 1]
      },
      {
        type: 'us_states_culture',
        question: "What's the typical food in Chicago?",
        options: ["Sandwich & pig", "Ice cream and sushi", "Hot dogs & hamburgers", "Hot dogs & pizza"],
        correctAnswer: "Hot dogs & pizza"
      },
      {
        type: 'us_states_culture',
        question: "Which state is between New York and Boston and famous for its colorful fall trees?",
        options: ["Vermont", "Connecticut", "New Hampshire", "Maine"],
        correctAnswer: "Connecticut"
      },
      {
        type: 'us_states_culture',
        question: "What's the capital of Illinois?",
        options: ["Prince Charles", "Saint John", "Chicoutimi", "Springfield"],
        correctAnswer: "Springfield"
      },
      {
        type: 'us_states_culture',
        question: "What's the most important food in Texas?",
        options: ["Chicken", "Tex-Mex", "Hot dog", "Fish and Chips"],
        correctAnswer: "Tex-Mex" // PDF did not mark a correct answer. Using common knowledge. User may want to verify/change. [cite: 1]
      },
      {
        type: 'us_states_culture',
        question: "What's the capital of Texas?",
        options: ["San Antonio", "Hollywood", "Austin", "San Francisco"],
        correctAnswer: "Austin"
      },
      {
        type: 'us_states_culture',
        question: "What's the most popular dance in Texas?",
        options: ["Hip hop", "Reggae", "Classic", "Country"],
        correctAnswer: "Country"
      },
      {
        type: 'us_states_culture',
        question: "In which year and city was the first McDonald's created?",
        options: ["1940 in San Bernardino", "1981 in Sacramento", "1901 in San Francisco", "1970 in Los Angeles"],
        correctAnswer: "1940 in San Bernardino"
      },
      {
        type: 'us_states_culture',
        question: "Which is the capital of California?",
        options: ["Sacramento", "San Diego", "Santa Monica", "Los Angeles"],
        correctAnswer: "Sacramento"
      },
      {
        type: 'us_states_culture',
        question: "Which is the typical food in California?",
        options: ["Hot dogs & hamburgers", "Tacos & burritos", "Pizza & pasta", "Fish & chips"],
        correctAnswer: "Tacos & burritos"
      },
      {
        type: 'us_states_culture',
        question: "What's the capital of Alabama?",
        options: ["Montgomery", "Huntsville", "Birmingham", "Mobile"],
        correctAnswer: "Montgomery"
      },
      {
        type: 'us_states_culture',
        question: "What is the most famous canyon in the U.S.?",
        options: ["The Great Canyon", "The Big Canyon", "Grand Canyon", "Colorado Hole"],
        correctAnswer: "Grand Canyon"
      },
      {
        type: 'us_states_culture',
        question: "What sport is most popular in the U.S.?",
        options: ["Tennis", "Basketball", "Baseball", "American football"],
        correctAnswer: "American football"
      },
      {
        type: 'us_states_culture',
        question: "What is Route 66 known for?",
        options: ["Being a famous mountain", "A shopping center", "An old road across the U.S.", "A theme park"],
        correctAnswer: "An old road across the U.S." // PDF did not mark a correct answer. Using common knowledge. User may want to verify/change. [cite: 1]
      },
      {
        type: 'us_states_culture',
        question: "What's the national bird of the United States?",
        options: ["Owl", "Pigeon", "Eagle", "Hawk"],
        correctAnswer: "Eagle"
      },
      {
        type: 'us_states_culture',
        question: "Which holiday is celebrated on the 4th of July?",
        options: ["Thanksgiving", "Labor Day", "Independence Day", "Veterans Day"],
        correctAnswer: "Independence Day"
      },
      {
        type: 'us_states_culture',
        question: "Where is the Statue of Liberty?",
        options: ["Boston", "Chicago", "New York", "Los Angeles"],
        correctAnswer: "New York"
      },
      {
        type: 'us_states_culture',
        question: "Which famous bridge is in San Francisco?",
        options: ["Brooklyn Bridge", "Golden Gate Bridge", "Washington Bridge", "Liberty Bridge"],
        correctAnswer: "Golden Gate Bridge"
      },
      {
        type: 'us_states_culture',
        question: "What is Hollywood famous for?",
        options: ["Beaches", "Farms", "Film industry", "Mines"],
        correctAnswer: "Film industry"
      },
      {
        type: 'us_states_culture',
        question: "What are the colors of the U.S. flag?",
        options: ["Red, yellow, green", "Blue, white, red", "White, black, red", "Red, green, blue"],
        correctAnswer: "Blue, white, red"
      },
      {
        type: 'us_states_culture',
        question: "What is Thanksgiving mainly about?",
        options: ["Eating pizza", "Visiting museums", "Giving thanks and sharing food", "Playing football"],
        correctAnswer: "Giving thanks and sharing food"
      },
      {
        type: 'us_states_culture',
        question: "What is the most famous U.S. theme park?",
        options: ["Six Flags", "Universal Studios", "Disneyland", "Playland"],
        correctAnswer: "Disneyland"
      },
      {
        type: 'us_states_culture',
        question: "Which U.S. state is famous for jazz music?",
        options: ["Chicago", "New Orleans", "New York", "Detroit"],
        correctAnswer: "New Orleans"
      },
      {
        type: 'us_states_culture',
        question: "What city is known as the \"Big Apple\"?",
        options: ["Boston", "Chicago", "New York City", "Miami"],
        correctAnswer: "New York City"
      },
      {
        type: 'us_states_culture',
        question: "What's the longest river in the United States?",
        options: ["Mississippi River", "Colorado River", "Hudson River", "Ohio River"],
        correctAnswer: "Mississippi River"
      },
      {
        type: 'us_states_culture',
        question: "Who was the first President of the United States?",
        options: ["Abraham Lincoln (born in Kentucky)", "George Washington (born in Virginia)", "Thomas Jefferson (born in Virginia)", "John Adams (born in Massachusetts)"],
        correctAnswer: "George Washington (born in Virginia)" 
      },
      {
        type: 'us_states_culture',
        question: "What is the U.S. currency called?",
        options: ["Pound", "Euro", "Dollar", "Peso"],
        correctAnswer: "Dollar"
      },
      {
        type: 'us_states_culture',
        question: "Which island state is part of the U.S.?",
        options: ["Greenland", "Jamaica", "Hawaii", "Fiji"],
        correctAnswer: "Hawaii"
      },
      {
        type: 'us_states_culture',
        question: "Where is the White House located?",
        options: ["New York", "Washington D.C.", "Philadelphia", "Boston"],
        correctAnswer: "Washington D.C."
      },
      {
        type: 'us_states_culture',
        question: "Which U.S. state is known for its casinos in Las Vegas?",
        options: ["California", "Nevada", "Utah", "Colorado"],
        correctAnswer: "Nevada"
      },
      {
        type: 'us_states_culture',
        question: "Which ocean is on the west coast of the USA?",
        options: ["Atlantic Ocean", "Pacific Ocean", "Indic Ocean", "Arctic Ocean"],
        correctAnswer: "Pacific Ocean"
      },
      {
        type: 'us_states_culture',
        question: "What do Americans traditionally eat on Thanksgiving?",
        options: ["Chicken", "Pizza", "Turkey", "Hot dog"],
        correctAnswer: "Turkey"
      }
    ];
  }

  function addKfoortStyles() {
    const styleSheet = document.createElement("style");
    styleSheet.type = "text/css";
    styleSheet.innerText = `
      body.kfoort-active {
        justify-content: center; padding-top: 20px; padding-bottom: 20px;
      }
      .kfoort-fixed-header { /* New style for the fixed header */
        font-family: 'Dancing Script', cursive;
        color: #1a73e8; /* Same color as mainFooter */
        font-size: 1rem; /* Same font-size as mainFooter */
        text-align: center;
        padding: 10px 0;
        margin-bottom: 10px; /* Space below this header */
        width: 100%;
        max-width: 850px; /* Match kfoort-container width */
      }
      .kfoort-container {
        width: 100%; max-width: 850px; min-height: 550px; /* Increased min-height for header */
        background-color: #3b0f70; padding: 30px; border-radius: 10px;
        box-shadow: 0 5px 20px rgba(0,0,0,0.2); text-align: center; margin: 0 auto;
        color: white; display: flex; flex-direction: column; justify-content: space-between;
      }
      .kfoort-question-area {
        background-color: #FFFFFF; color: #333; padding: 20px;
        border-radius: 8px; margin-bottom: 25px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        /* This is the 'cuadro blanco más grande' */
      }
      .kfoort-context-text-box { /* Recuadro de texto (violeta) */
        background-color: #e6e0f8; /* Light violet */ color: #333; font-weight: bold;
        padding: 15px; border-radius: 6px; margin-bottom: 15px;
        font-size: clamp(1rem, 2.5vw, 1.3rem); text-align: left;
        /* This is 'ligeramente más pequeño' as it's inside kfoort-question-area */
      }
      .kfoort-question-text { /* The actual question text */
        font-size: clamp(1.2rem, 3vw, 1.6rem); word-wrap: break-word; min-height: 50px;
        display: flex; align-items: center; justify-content: center; text-align: center;
      }
      .kfoort-instructional-hint {
        font-size: clamp(0.8rem, 2vw, 1rem); color: #555; margin-top: 8px; font-style: italic;
      }
      .kfoort-options-grid {
        display: grid; grid-template-columns: repeat(2, 1fr); grid-template-rows: repeat(2, auto);
        gap: 1rem; /* Approx 16px for '1cm' separation */
        margin-top: auto; width:100%;
      }
      .kfoort-option-btn {
        padding: 20px 10px; font-size: clamp(1rem, 2.5vw, 1.2rem); font-weight: bold;
        color: white; border: none; border-radius: 8px; cursor: pointer;
        transition: transform 0.1s ease, box-shadow 0.1s ease, filter 0.2s;
        min-height: 70px; display: flex; align-items: center; justify-content: center;
        line-height: 1.2; word-break: break-word;
      }
      .kfoort-option-btn:hover { filter: brightness(1.1); }
      .kfoort-option-btn:active {
        transform: translateY(4px); box-shadow: 0 2px 0 var(--shadow-color) !important;
      }
      .kfoort-option-btn.color-red { background-color: #e74c3c; --shadow-color: #c0392b; box-shadow: 0 6px 0 #c0392b; }
      .kfoort-option-btn.color-blue { background-color: #3498db; --shadow-color: #2980b9; box-shadow: 0 6px 0 #2980b9; }
      .kfoort-option-btn.color-green { background-color: #2ecc71; --shadow-color: #27ae60; box-shadow: 0 6px 0 #27ae60; }
      .kfoort-option-btn.color-yellow { background-color: #f1c40f; --shadow-color: #f39c12; box-shadow: 0 6px 0 #f39c12; color: #333; }

      .kfoort-feedback {
        font-size: 1.3rem; font-weight: bold; margin: 20px 0; padding: 12px;
        border-radius: 5px; color: white; min-height: 70px; /* Increased height for button */
        display: flex; flex-direction: column; align-items: center; justify-content: center;
      }
      .kfoort-feedback.incorrect { background-color: #e74c3c; }
      .kfoort-status-bar {
        display: flex; justify-content: space-between; align-items: center;
        padding: 10px 0; font-size: 1rem; color: #ddd;
        border-bottom: 1px solid #5a2a8a; margin-bottom: 20px;
      }
       .kfoort-main-button { /* Used for Start Kfoort, Next Question, View Results, Download, Play Again */
        background-color: #8e44ad; color: white; border: none; padding: 15px 30px;
        font-size: 1.2rem; border-radius: 8px; cursor: pointer;
        transition: background-color 0.3s ease, transform 0.1s ease, box-shadow 0.1s ease;
        margin-top: 20px; /* Adjusted margin */ box-shadow: 0 4px 0 #71368a;
      }
      .kfoort-main-button:hover { background-color: #9b59b6; }
      .kfoort-main-button:active { transform: translateY(2px); box-shadow: 0 2px 0 #71368a; }
      
      .kfoort-view-results-button { /* Specific styling for View Results if needed, inherits from main-button */
        background-color: #27ae60; /* A motivating green */
        box-shadow: 0 4px 0 #229954;
      }
      .kfoort-view-results-button:hover { background-color: #2ecc71; }
      .kfoort-view-results-button:active { transform: translateY(2px); box-shadow: 0 2px 0 #229954; }

      .kfoort-results-text {
        font-size: 1.4rem; line-height: 1.8; margin-bottom: 25px; color: white;
      }
      .kfoort-results-gif { /* small, cheerful, no text */
        max-width: 200px; /* Smaller GIF */ max-height: 200px;
        margin: 20px auto; border-radius: 10px; border: 3px solid white;
      }
    `;
    document.head.appendChild(styleSheet);
  }

  function cmToPx(cm) { return cm * 37.7952755906; }

  function showKfoortStartScreen() {
    document.body.classList.add('kfoort-active');
    mainHeader.style.display = 'none'; accessForm.style.display = 'none';
    mainFooter.style.display = 'none'; messageDiv.style.display = 'none';
    // const musicPlayer = document.getElementById('musicPlayer'); // Vocaroo removed
    // if (musicPlayer) musicPlayer.style.display = 'none'; // Vocaroo removed
    const startKfoortBtn = document.getElementById('startKfoortButton');
    if(startKfoortBtn) startKfoortBtn.style.display = 'none';

    kfoortRootContainer.style.display = 'flex';
    kfoortRootContainer.innerHTML = `<div class="kfoort-container" id="kfoortGameContainer"></div>`;
    kfoortQuestions = generateKfoortQuestions();
    currentQuestionIndex = 0; score = 0; incorrectAnswers = 0;
    displayKfoortQuestion();
  }

  function displayKfoortQuestion() {
    if (currentQuestionIndex >= kfoortQuestions.length) {
      showKfoortResults(); 
      return;
    }

    const questionData = kfoortQuestions[currentQuestionIndex];
    const gameContainer = document.getElementById('kfoortGameContainer');
    const colors = ['red', 'blue', 'green', 'yellow'];
    let questionAreaHTML = '';

    // The US States & Culture questions do not use contextText or instructionalHint by default.
    // This logic is kept for flexibility if question types change.
    if (questionData.type === 'comprehension' && questionData.contextText) {
      questionAreaHTML += `<div class="kfoort-context-text-box">${questionData.contextText}</div>`;
    }
    questionAreaHTML += `<div class="kfoort-question-text">${questionData.question}</div>`;
    if (questionData.type === 'grammar' && questionData.instructionalHint) {
      questionAreaHTML += `<div class="kfoort-instructional-hint">${questionData.instructionalHint}</div>`;
    }

    gameContainer.innerHTML = `
      <div class="kfoort-fixed-header">Digital Resource Designed by Sonia Monfort.</div>
      <div class="kfoort-status-bar">
        <span>Question: ${currentQuestionIndex + 1} / ${kfoortQuestions.length}</span>
        <span>Score: ${score}</span>
      </div>
      <div class="kfoort-question-area">
        ${questionAreaHTML}
      </div>
      <div class="kfoort-options-grid">
        ${questionData.options.map((option, index) => `
          <button class="kfoort-option-btn color-${colors[index % colors.length]}" data-answer="${option}">
            ${option}
          </button>
        `).join('')}
      </div>
      <div class="kfoort-feedback" id="kfoortFeedbackArea"></div>
    `;
    document.querySelectorAll('.kfoort-option-btn').forEach(button => {
      button.addEventListener('click', handleKfoortAnswer);
    });
  }

  function handleKfoortAnswer(event) {
    const selectedAnswer = event.target.dataset.answer;
    const correctAnswer = kfoortQuestions[currentQuestionIndex].correctAnswer;
    const feedbackArea = document.getElementById('kfoortFeedbackArea');
    document.querySelectorAll('.kfoort-option-btn').forEach(btn => btn.disabled = true);

    if (selectedAnswer === correctAnswer) {
      score++;
      feedbackArea.textContent = 'Correct!';
      feedbackArea.className = 'kfoort-feedback correct';
      if (currentQuestionIndex === kfoortQuestions.length - 1) {
        setTimeout(() => { 
          feedbackArea.innerHTML = `You've completed all questions!
          <button id="kfoortViewResultsBtn" class="kfoort-main-button kfoort-view-results-button">View Results</button>`;
          document.getElementById('kfoortViewResultsBtn').addEventListener('click', showKfoortResults);
        }, 1500);
      } else {
        setTimeout(() => { currentQuestionIndex++; displayKfoortQuestion(); }, 1500);
      }
    } else {
      incorrectAnswers++;
      let nextStepHTML = `<button id="kfoortNextQuestionBtn" class="kfoort-main-button" style="margin-top:10px; padding: 10px 20px; font-size: 1rem;">Next Question</button>`;
      if (currentQuestionIndex === kfoortQuestions.length - 1) {
          nextStepHTML = `<button id="kfoortViewResultsBtn" class="kfoort-main-button kfoort-view-results-button" style="margin-top:10px;">View Results</button>`;
      }
      feedbackArea.innerHTML = `Incorrect, but keep going! ${nextStepHTML}`;
      feedbackArea.className = 'kfoort-feedback incorrect';
      
      if (currentQuestionIndex === kfoortQuestions.length - 1) {
          document.getElementById('kfoortViewResultsBtn').addEventListener('click', showKfoortResults);
      } else {
          document.getElementById('kfoortNextQuestionBtn').addEventListener('click', () => {
            currentQuestionIndex++; displayKfoortQuestion();
          });
      }
    }
  }

  function showKfoortResults() {
    const gameContainer = document.getElementById('kfoortGameContainer');
    const percentage = kfoortQuestions.length > 0 ? (score / kfoortQuestions.length) * 100 : 0;
    const note = (percentage / 10).toFixed(1);
    const happyGifUrl = "https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExYjQ5ZnduN2ZreHhvbDIxdmVnanRmbWFyZDU3ajRyZzhobGlpYmRkayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ Mayor岩MayorMayorMayorMayorMayorMayorMayor/giphy.gif";

    gameContainer.innerHTML = `
      <div class="kfoort-fixed-header">Digital Resource Designed by Sonia Monfort.</div>
      <div class="kfoort-question-area" style="background-color: transparent; box-shadow: none; margin-bottom: 15px;">
         <div class="kfoort-question-text" style="color:white; font-size: 1.8rem; margin-bottom:0;">Kfoort Results!</div>
      </div>
      <div class="kfoort-results-text">
        Well done, ${studentNameForKfoort}!<br>
        Correct Answers: ${score}<br>
        Incorrect Answers: ${incorrectAnswers}<br>
        Percentage: ${percentage.toFixed(2)}%<br>
        Score: ${note} / 10
      </div>
      <img src="${happyGifUrl}" alt="Celebration" class="kfoort-results-gif"/>
      <button id="kfoortDownloadResultsBtn" class="kfoort-main-button">Download Results</button>
      <button id="kfoortPlayAgainBtn" class="kfoort-main-button">Play Again</button>
    `;
    document.getElementById('kfoortDownloadResultsBtn').addEventListener('click', downloadKfoortResults);
    document.getElementById('kfoortPlayAgainBtn').addEventListener('click', () => {
        document.body.classList.remove('kfoort-active');
        mainHeader.style.display = ''; accessForm.style.display = ''; mainFooter.style.display = '';
        messageDiv.style.display = ''; messageDiv.textContent = 'Enter your details to play again or start a new session.';
        const startKfoortBtn = document.getElementById('startKfoortButton');
        if(startKfoortBtn) startKfoortBtn.style.display = '';
        kfoortRootContainer.style.display = 'none';
        // const musicPlayer = document.getElementById('musicPlayer'); // Vocaroo removed
        // if (musicPlayer) musicPlayer.style.display = 'block'; // Vocaroo removed
    });
  }

  function downloadKfoortResults() {
    const percentage = kfoortQuestions.length > 0 ? (score / kfoortQuestions.length) * 100 : 0;
    const note = (percentage / 10).toFixed(1);
    const dateObj = new Date();
    const formattedDate = `${String(dateObj.getDate()).padStart(2, '0')}/${String(dateObj.getMonth() + 1).padStart(2, '0')}/${dateObj.getFullYear()}`;
    
    const resultsData = [
        `Kfoort Results`,
        `Student: ${studentNameForKfoort}`,
        `Date: ${formattedDate}`,
        ``, 
        `--------------------`,
        `Total Questions: ${kfoortQuestions.length}`,
        `Correct Answers: ${score}`,
        `Incorrect Answers: ${incorrectAnswers}`,
        `Percentage: ${percentage.toFixed(2)}%`,
        `Score: ${note} / 10`,
        `--------------------`,
        ``, 
        `Well done, keep learning!`
    ];

    const resultsText = resultsData.join('\r\n\r\n'); 

    const blob = new Blob([resultsText], { type: 'text/plain;charset=utf-8' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = `Kfoort_Results_${studentNameForKfoort.replace(/\s+/g, '_')}.txt`;
    document.body.appendChild(link); link.click(); document.body.removeChild(link);
  }

  form.addEventListener('submit', function(event) {
    event.preventDefault();
    const studentNameInput = form.studentName.value.trim();
    const dateInputStr = form.date.value.trim();
    const code = form.code.value.trim();
    const datePattern = /^(\d{2})\/(\d{2})\/(\d{4})$/;

    if (!datePattern.test(dateInputStr)) {
      message.style.color = 'red'; message.textContent = 'Invalid date format. Use dd/mm/yyyy'; return;
    }
    const [, dd, mm, yyyy] = dateInputStr.match(datePattern);
    const now = new Date();
    if (parseInt(mm) !== (now.getMonth() + 1) || parseInt(yyyy) !== now.getFullYear()) {
      message.style.color = 'red'; message.textContent = 'Date must be within the current month and year.'; return;
    }
    if (code !== '13006225') {
      message.style.color = 'red'; message.textContent = 'Invalid access code.'; return;
    }
    if (!studentNameInput) {
      message.style.color = 'red'; message.textContent = 'Please enter your name.'; return;
    }

    studentNameForKfoort = studentNameInput.toUpperCase();
    message.style.color = 'green';
    message.textContent = `Access granted! Welcome, ${studentNameForKfoort} 😊`;
    addKfoortStyles();

    let startButton = document.getElementById('startKfoortButton');
    if (!startButton) {
      startButton = document.createElement('button');
      startButton.id = 'startKfoortButton';
      startButton.textContent = 'Start Kfoort!';
      startButton.classList.add('kfoort-main-button'); 
      message.parentNode.insertBefore(startButton, message.nextSibling);
      startButton.addEventListener('click', showKfoortStartScreen);
    } else {
      startButton.style.display = ''; 
    }

    // All Vocaroo / music player related code has been removed from here.
  });
</script>

</body>
</html>
