<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jeopardy Game</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
            background: #000000; /* Black background */
            color: #FFFFFF; /* White text */
            text-align: center;
            margin: 0;
            padding: 0;
        }
        h1 {
            background-color: #333333; /* Dark gray background */
            color: #FFFFFF; /* White text */
            padding: 20px;
            margin: 0;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        table {
            width: 80%;
            margin: 20px auto;
            border-collapse: collapse;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        th, td {
            border: 2px solid #FFFFFF; /* White border */
            padding: 20px;
            cursor: pointer;
            transition: background-color 0.3s, color 0.3s;
        }
        th {
            background-color: #555555; /* Medium gray background */
            color: #FFFFFF; /* White text */
        }
        td {
            background-color: #777777; /* Lighter gray background */
            color: #FFFFFF; /* White text */
        }
        td:hover {
            background-color: #555555; /* Medium gray background */
            color: #FFFFFF; /* White text */
        }
        #questionBox {
            margin: 20px auto;
            display: none;
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        #question {
            font-size: 1.5em;
            margin-bottom: 10px;
        }
        #answer {
            padding: 10px;
            font-size: 1em;
            width: 80%;
            margin-bottom: 10px;
            background: #333333; /* Dark gray background */
            color: #FFFFFF; /* White text */
            border: 1px solid #FFFFFF; /* White border */
        }
        button {
            padding: 10px 20px;
            font-size: 1em;
            background-color: #555555; /* Medium gray background */
            color: #FFFFFF; /* White text */
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #777777; /* Lighter gray background */
        }
        #score {
            font-size: 1.2em;
            margin: 20px 0;
        }
        #result {
            font-size: 1.2em;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Cell Masters Jeopardy</h1>
    <div id="score">Score: 0</div>
    <table>
        <tr>
            <th>Functions</th>
            <th>Fill in the Blanks</th>
            <th>Definitions</th>
        </tr>
        <tr>
            <td onclick="showQuestion('Functions', 100)">100</td>
            <td onclick="showQuestion('Fill in the Blanks', 100)">100</td>
            <td onclick="showQuestion('Definitions', 100)">100</td>
        </tr>
        <tr>
            <td onclick="showQuestion('Functions', 200)">200</td>
            <td onclick="showQuestion('Fill in the Blanks', 200)">200</td>
            <td onclick="showQuestion('Definitions', 200)">200</td>
        </tr>
        <tr>
            <td onclick="showQuestion('Functions', 300)">300</td>
            <td onclick="showQuestion('Fill in the Blanks', 300)">300</td>
            <td onclick="showQuestion('Definitions', 300)">300</td>
        </tr>
        <tr>
            <td onclick="showQuestion('Functions', 400)">400</td>
            <td onclick="showQuestion('Fill in the Blanks', 400)">400</td>
            <td onclick="showQuestion('Definitions', 400)">400</td>
        </tr>
        <tr>
            <td onclick="showQuestion('Functions', 500)">500</td>
            <td onclick="showQuestion('Fill in the Blanks', 500)">500</td>
            <td onclick="showQuestion('Definitions', 500)">500</td>
        </tr>
    </table>

    <div id="questionBox">
        <h2 id="question"></h2>
        <input type="text" id="answer" placeholder="Your answer" onkeydown="if (event.key === 'Enter') checkAnswer()">
        <button onclick="checkAnswer()">Submit</button>
    </div>

    <div id="result"></div>

    <script>
        const questions = {
            Functions: {
                100: {
                    question: "This organelle is known as the powerhouse of the cell.",
                    answer: "mitochondria"
                },
                200: {
                    question: "This organelle is the site of photosynthesis in plant cells.",
                    answer: "chloroplasts"
                },
                300: {
                    question: "This organelle packages and sorts proteins and lipids.",
                    answer: "Golgi apparatus"
                },
                400: {
                    question: "This organelle is responsible for detoxifying harmful substances.",
                    answer: "peroxisome"
                },
                500: {
                    question: "This organelle contains digestive enzymes to break down waste.",
                    answer: "lysosome"
                }
            },
            "Fill in the Blanks": {
                100: {
                    question: "The ________ is the control center of the cell.",
                    answer: "nucleus"
                },
                200: {
                    question: "The ________ ER has ribosomes attached to it and is involved in protein synthesis.",
                    answer: "rough"
                },
                300: {
                    question: "The ________ ER is involved in lipid synthesis and detoxification.",
                    answer: "endoplasmic reticulum"
                },
                400: {
                    question: "Plant cells have a large ________ that stores water and nutrients.",
                    answer: "vacuole"
                },
                500: {
                    question: "The ________ provides energy for cellular activities by converting glucose into ATP.",
                    answer: "mitochondria"
                }
            },
            Definitions: {
                100: {
                    question: "This organelle contains the genetic material (DNA) of the cell.",
                    answer: "nucleus"
                },
                200: {
                    question: "This network of membranes is involved in protein and lipid synthesis.",
                    answer: "endoplasmic reticulum"
                },
                300: {
                    question: "These organelles are involved in energy production and are known as the cell's powerhouses.",
                    answer: "mitochondria"
                },
                400: {
                    question: "This organelle in plant cells captures light energy for photosynthesis.",
                    answer: "chloroplasts"
                },
                500: {
                    question: "This organelle contains enzymes for digesting macromolecules and cellular debris.",
                    answer: "lysosome"
                }
            }
        };

        let currentCategory = '';
        let currentPoints = 0;
        let score = 0;

        function showQuestion(category, points) {
            currentCategory = category;
            currentPoints = points;
            const questionData = questions[category][points];
            document.getElementById('question').innerText = questionData.question;
            document.getElementById('questionBox').style.display = 'block';
            document.getElementById('result').innerText = '';
        }

        function checkAnswer() {
            const userAnswer = document.getElementById('answer').value.trim().toLowerCase();
            const correctAnswer = questions[currentCategory][currentPoints].answer.toLowerCase();

            if (userAnswer === correctAnswer) {
                score += currentPoints;
                document.getElementById('result').innerText = 'Correct!';
                document.getElementById('result').style.color = 'green';
            } else {
                score -= currentPoints;
                document.getElementById('result').innerText = 'Incorrect! The correct answer is: ' + questions[currentCategory][currentPoints].answer;
                document.getElementById('result').style.color = 'red';
            }

            document.getElementById('score').innerText = 'Score: ' + score;
            document.getElementById('questionBox').style.display = 'none';
            document.getElementById('answer').value = '';
        }
    </script>
</body>
</html>
