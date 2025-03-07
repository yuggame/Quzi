<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Adventure</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            color: rgb(255, 255, 255);
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        .container {
            background: rgb(255, 255, 255);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
            text-align: center;
            width: 90%;
            max-width: 600px;
            color: black;
            transition: transform 0.3s ease-in-out, background-color 0.5s ease-in-out;
        }
        .question {
            font-size: 22px;
            margin-bottom: 20px;
            font-weight: bold;
        }
        .options button {
            background: #007bff;
            color: white;
            border: none;
            padding: 12px;
            margin: 8px;
            border-radius: 10px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
            transition: transform 0.2s;
        }
        .options button:hover {
            background: #000000;
            transform: scale(1.10);
        }
        .score-container {
            display: flex;
            justify-content: center;
            margin-top: 20px;
            gap: 20px;
        }
        .score-box {
            width: 80px;
            height: 80px;
            background: rgb(0, 247, 255);
            color: rgb(0, 0, 0);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        .hidden {
            display: none;
        }
        .fireworks {
            position: absolute;
            width: 100%;
            height: 100%;
            background: url('https://media.giphy.com/media/5hgYDDh5oqbmE4OKJ3/giphy.gif') center/cover;
            z-index: 10;
            display: none;
        }
    </style>
</head>
<body>
   
    <div class="fireworks" id="fireworks"></div>
    <div class="container" id="quiz-container">
        <h1>Quiz Adventure</h1>
        <div class="question" id="question">Loading question...</div>
        <div class="options" id="options"></div>
        <div class="score-container">
            <div class="score-box" id="correct-box">✔ 0</div>
            <div class="score-box" id="incorrect-box">✘ 0</div>
        </div>
    </div>
    
    <script>
       
 const questions = [
{ q: "What is the Schrödinger equation used for in quantum mechanics?", 
  options: ["Describing electron orbits", "Predicting atomic decay", "Describing wavefunctions", "Calculating entropy"], 
  answer: "Describing wavefunctions" 
},
{ q: "In differential geometry, what does the Ricci curvature tensor describe?", 
  options: ["Electromagnetic fields", "The shape of spacetime", "Fluid dynamics", "Wave propagation"], 
  answer: "The shape of spacetime" 
},
{ q: "What is the primary function of the endoplasmic reticulum in eukaryotic cells?", 
  options: ["DNA replication", "Protein and lipid synthesis", "Energy production", "Cell division"], 
  answer: "Protein and lipid synthesis" 
},
{ q: "Which of the following is NOT a fundamental interaction in the Standard Model of particle physics?", 
  options: ["Electromagnetic", "Gravitational", "Weak nuclear", "Baryonic"], 
  answer: "Baryonic" 
},
{ q: "In economics, what does the Nash Equilibrium represent?", 
  options: ["Optimal resource allocation", "A state where no player can benefit by changing strategy", "Maximum profit scenario", "A zero-sum game outcome"], 
  answer: "A state where no player can benefit by changing strategy" 
},
{ q: "What is the Turing Test designed to evaluate?", 
  options: ["A machine's ability to replicate human intelligence", "The complexity of an algorithm", "The limits of computation", "Data encryption techniques"], 
  answer: "A machine's ability to replicate human intelligence" 
},
{ q: "Which of the following is a characteristic of a non-Euclidean geometry?", 
  options: ["Parallel lines never meet", "Angles in a triangle always sum to 180°", "The existence of geodesic deviation", "Straight lines are infinite"], 
  answer: "The existence of geodesic deviation" 
},
{ q: "In astrophysics, what is the Chandrasekhar limit?", 
  options: ["The minimum mass required for a star to become a supernova", "The upper mass limit for a white dwarf", "The point where a black hole forms", "The threshold for nuclear fusion"], 
  answer: "The upper mass limit for a white dwarf" 
},
{ q: "Which sorting algorithm has the best worst-case time complexity?", 
  options: ["QuickSort", "MergeSort", "BubbleSort", "SelectionSort"], 
  answer: "MergeSort" 
},
{ q: "What does the uncertainty principle in quantum mechanics state?", 
  options: ["Energy cannot be created or destroyed", "It is impossible to know both the position and momentum of a particle with absolute precision", "Particles behave both as waves and particles", "Wavefunctions collapse upon observation"], 
  answer: "It is impossible to know both the position and momentum of a particle with absolute precision" 
},
{ q: "In general relativity, what is a geodesic?", 
  options: ["A shortcut in space", "The path of least resistance", "The trajectory of an object in curved spacetime", "A force acting on mass"], 
  answer: "The trajectory of an object in curved spacetime" 
},
{ q: "What does Gödel's Incompleteness Theorem state?", 
  options: ["Some mathematical truths cannot be proven within a system", "Every logical system is complete and consistent", "All axioms can be derived from set theory", "Infinity does not exist in mathematics"], 
  answer: "Some mathematical truths cannot be proven within a system" 
},
{ q: "In cryptography, what is a one-way function?", 
  options: ["A function that is easy to compute but hard to reverse", "A function used for symmetric encryption", "A function that generates random numbers", "A function with a single input and output"], 
  answer: "A function that is easy to compute but hard to reverse" 
},
{ q: "Which of the following is NOT a type of finite automaton?", 
  options: ["Deterministic", "Nondeterministic", "Turing-complete", "Pushdown"], 
  answer: "Turing-complete" 
},
{ q: "Which of the following statements about entropy in thermodynamics is TRUE?", 
  options: ["Entropy always decreases in an isolated system", "Entropy measures disorder in a system", "Entropy has no relation to energy", "Entropy remains constant in irreversible processes"], 
  answer: "Entropy measures disorder in a system" 
},
{ q: "In linguistics, what is a morpheme?", 
  options: ["A unit of sound", "A unit of meaning", "A sentence structure", "A type of verb"], 
  answer: "A unit of meaning" 
},
{ q: "What is the main purpose of Fourier analysis in signal processing?", 
  options: ["To remove noise from data", "To convert a function into a sum of sinusoids", "To compress digital signals", "To encode data for transmission"], 
  answer: "To convert a function into a sum of sinusoids" 
},
{ q: "Which principle in chemistry explains why elements in the same group of the periodic table have similar properties?", 
  options: ["Hund's Rule", "Pauli Exclusion Principle", "Electron Configuration", "Valence Shell Electron Pair Repulsion"], 
  answer: "Electron Configuration" 
},

{ q: "Which of the following is NOT a Maxwell equation in electromagnetism?", 
  options: ["Gauss's law", "Ampère's law", "Faraday's law", "Newton's third law"], 
  answer: "Newton's third law" 
}
];
        
        let currentQuestionIndex = 0;
        let correctCount = 0;
        let incorrectCount = 0;
        
        function loadQuestion() {
            if (currentQuestionIndex >= questions.length) {
                document.getElementById("quiz-container").innerHTML = `
                    <h1>Game Over!</h1>
                    <p>Your final score: ${correctCount} / ${questions.length}</p>
                    <button onclick="restartGame()">Play Again</button>
                `;
                if (correctCount === questions.length) {
                    document.getElementById("fireworks").style.display = "block";
                }
                return;
            }
            const questionObj = questions[currentQuestionIndex];
            document.getElementById("question").innerHTML = `Q${currentQuestionIndex + 1}: ${questionObj.q}`;
            document.getElementById("options").innerHTML = questionObj.options.map(option => 
                `<button onclick="checkAnswer(this, '${option}')">${option}</button>`
            ).join('');
        }
        
        function checkAnswer(button, answer) {
            const container = document.getElementById("quiz-container");
            if (answer === questions[currentQuestionIndex].answer) {
                container.style.backgroundColor = "lightgreen";
                correctCount++;
            } else {
                container.style.backgroundColor = "lightcoral";
                incorrectCount++;
                setTimeout(() => alert(`Correct Answer: ${questions[currentQuestionIndex].answer}`), 300);
            }
            setTimeout(() => {
                container.style.backgroundColor = "white";
                document.getElementById("correct-box").innerText = `✔ ${correctCount}`;
                document.getElementById("incorrect-box").innerText = `✘ ${incorrectCount}`;
                currentQuestionIndex++;
                loadQuestion();
            }, 800);
        }
        
        function restartGame() {
            currentQuestionIndex = 0;
            correctCount = 0;
            incorrectCount = 0;
            document.getElementById("fireworks").style.display = "none";
            document.getElementById("quiz-container").innerHTML = `
                <h1>Quiz Adventure</h1>
                <div class="question" id="question"></div>
                <div class="options" id="options"></div>
                <div class="score-container">
                    <div class="score-box" id="correct-box">✔ 0</div>
                    <div class="score-box" id="incorrect-box">✘ 0</div>
                </div>
            `;
            loadQuestion();
        }
        
        loadQuestion();
    </script>
</body>

</html>
