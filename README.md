<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>  Quiz App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1> Quran Quiz App</h1>
    
        <div class="question" id="question"></div>
        <div class="options" id="options"></div>
        <div id="score">Score: 0</div>
        <div id="timer"></div>
        <div id="result-container"></div>
        <button id="restart-button" 
        class="blue-button" 
        style="display:none;" 
        onclick="restartQuiz()">
        Restart Quiz</button>
    </div>

    <script src="ex.js"></script>
</body>
</html>
#CSS
/* Add these styles to adjust the font size for results, questions, and options */
#result-container {
    font-size: 22px; /* Adjust the font size for the result container as needed */
}

#result-container p {
    font-size: 22px; /* Adjust the font size for paragraphs in the result container as needed */
}

.options button {
     
    font-size: 22px; /* Adjust the font size for options as needed */
    /* Add any other styling for options buttons */
}

.container {
    text-align: center;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    padding: 20px;
}

h1 {
    color: #3498db;
}

.question {
    font-size: 25px;
    margin-bottom: 20px;
    color: #2c3e50;
}

.options button {
    background-color: #3498db;
    color: #fff;
    padding: 10px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

.options button:hover {
    background-color: #2980b9;
}

#score {
    margin-top: 20px;
    font-size: 18px;
    color: #27ae60;
}.blue-button {
    background-color: #3498db;
    color: #fff;
    font-size: 16px; /* Adjust the font size as needed */
    padding: 10px 20px; /* Adjust the padding as needed */
    margin: 5px; /* Add margin to increase space between buttons */
    border: none;
    cursor: pointer;

}

.blue-button {
    background-color: #3498db;
    color: #fff;
}


.blue-button:hover, .red-button:hover {
    background-color: #2980b9;
}

#timer {
    font-size: 1.5em;
    margin-top: 10px;
}

#JS
const questions = [
    {
        question: "আল কুরআন নাজিলের পদ্দতি কয়টি?",
        options: ["৩টি ", "৭টি", "৫টি", "৬টি"],
        answer: "৭টি",
        userAnswerIndex: -1
    },
    {
        question: "রুহুল আমিন বলতে কাকে বুজানো হয়েছে?",
        options: ["ইসরাফিল ", "আজরাইয়েল", " জিবরাইয়েল", "মিকাইয়ল"],
        answer: " জিবরাইয়েল",
        userAnswerIndex: -1
    },
    {
        question: "প্রধান ওহি লেখক ছিলেন?",
        options: ["উমর (রা)", "মুয়াবিয়া (রা)", "জায়েদ বিন সাবেত(রা)",],
        answer: "জায়েদ বিন সাবেত(রা)",
        userAnswerIndex: -1
    },
    {
        question: "তোমার মতে হাদিস সংকলনের হুকুম কি?",  
        options: ["ওয়াজিব"," মাকরুহ","ফরজ","মুস্থাহাব"],
        answer: "ফরজ",
        userAnswerIndex: -1
    },
    { 
        question: "হজরত উমরের এই সংকলননীতির সাথে কোন খলিফার সংকলননীতির মিল পাওয়া যায় ?",
        options: ["উমর(রা)","উসমান(রা)","আলী(রা)","আবু বকর (রা)"],
        answer: "আবু বকর (রা)",
        userAnswerIndex: -1
    },

    
    // Add more questions here
];



let currentQuestionIndex = 0;
let score = 0;
let timeLimit = 6; // Time limit for each question in seconds
let timer;

function startQuiz() {
    document.getElementById('timer').textContent = 'Timer: 5 seconds'; // Replace this with your quiz logic
    setTimeout(() => {
        alert('Quiz started!'); // Replace this with the actual quiz logic
        document.getElementById('timer').textContent = ''; // Reset the timer display
    }, 5000); // 5000 milliseconds (5 seconds)
}


function startTimer() {
    let timeRemaining = timeLimit;
    timer = setInterval(() => {
        timeRemaining--;
        if (timeRemaining >= 0) {
            document.getElementById('timer').textContent = `Time: ${timeRemaining}s`;
        } else {
            clearInterval(timer);
            checkAnswer(null); // Timeout, treat as if no answer selected
        }
    }, 1000);
}

function displayQuestion() {
    clearInterval(timer); // Clear previous timer
    startTimer(); // Start new timer for the current question

    const currentQuestion = questions[currentQuestionIndex];
    document.getElementById('question').textContent = currentQuestion.question;

    const optionsElement = document.getElementById('options');
    optionsElement.innerHTML = '';

    currentQuestion.options.forEach((option, index) => {
        const button = document.createElement('button');
        button.textContent = option;
        button.addEventListener('click', () => checkAnswer(index));
        optionsElement.appendChild(button);
    });
}

function checkAnswer(selectedOptionIndex) {
    const currentQuestion = questions[currentQuestionIndex];

    clearInterval(timer);

    if (selectedOptionIndex !== null) {
        currentQuestion.userAnswerIndex = selectedOptionIndex;

        if (selectedOptionIndex === currentQuestion.options.indexOf(currentQuestion.answer)) {
            score++;
        }
    }

    currentQuestionIndex++;

    if (currentQuestionIndex < questions.length) {
        displayQuestion();
    } else {
        showResult();
    }
}

function showResult() {
    clearInterval(timer);
    const resultContainer = document.getElementById('result-container');
    resultContainer.innerHTML = '<h2>Quiz Completed!</h2>';
    
    questions.forEach((question, index) => {
        const questionElement = document.createElement('div');
        questionElement.innerHTML = `<p>${index + 1}. ${question.question}</p>`;
        questionElement.innerHTML += `<p>Your Answer: ${question.options[question.userAnswerIndex]}</p>`;
        questionElement.innerHTML += `<p>Correct Answer: ${question.answer}</p>`;
        resultContainer.appendChild(questionElement);
    });

    document.getElementById('score').textContent = `Your Score: ${score} / ${questions.length}`;
    document.getElementById('timer').textContent = '';
    
    document.getElementById('restart-button').style.display = 'block'; // Show the restart button
}

function restartQuiz() {
    currentQuestionIndex = 0;
    score = 0;
    
    questions.forEach((question) => {
        question.userAnswerIndex = -1; // Reset user's answer for all questions
    });

    document.getElementById('result-container').innerHTML = ''; // Clear the result container
    document.getElementById('restart-button').style.display = 'none'; // Hide the restart button
    displayQuestion();
}

// ... (Your existing script.js code)



    document.getElementById('result-container').innerHTML = ''; // Clear the result container
    document.getElementById('restart-button').style.display = 'none'; // Hide the restart button
    displayQuestion();


// ... (Your existing script.js code)



displayQuestion();

