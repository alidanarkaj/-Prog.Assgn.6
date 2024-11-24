# Demo Quiz App

## 1. Problem Statement

_The Quiz App is a web-based application that enables users to test their knowledge on various topics through a series of multiple-choice questions. The app provides immediate feedback on answers, scores users based on their performance, and incorporates a timer to add a sense of urgency to the quiz._

## 2. Table of Contents

- [Functional Features](#3-functional-features)
- [Directory Structure and Setup](#4-directory-structure-and-setup)
- [Explanation of Codebase](#5-explanation-of-codebase)
- [Acknowledgments](#6-acknowledgments)

## 3. Functional Features

- **Start Quiz:** Begin the quiz session with an introduction of the rules.
- **Question Navigation:** Navigate through a series of multiple-choice questions.
- **Timed Questions:** Answer each question within a set time limit (15 seconds).
- **Immediate Feedback:** Indicates whether the selected answer is correct or incorrect.
- **Score Calculation:** Dynamically calculates and displays the final score at the end of the quiz.
- **Progress Tracking:** Displays the current question number and total number of questions.
- **Replay Option:** Option to retake the quiz after completion.
- **Responsive Design:** Works seamlessly on devices of various screen sizes.

## 4. Directory Structure and Setup

The app's codebase is organized into the following key directories and files:

- **index.html:** The main HTML file that defines the structure of the quiz interface. It links the CSS for styling and JavaScript files for functionality.
- **css/style.css:** The CSS file responsible for the visual design, layout, and responsive behavior of the app.
- **js/questions.js:** Contains the array of quiz questions, their corresponding answer options, and correct answers.
- **js/quizApp.js:** Implements the core logic of the quiz, including navigation, timer, score calculation, and interaction handling.

### File Linking

- The `index.html` links the `style.css` file for styling.
- It defers loading the `questions.js` and `quizApp.js` files for functionality to ensure the DOM is fully loaded before the scripts execute.

## 5. Explanation of Codebase

### `index.html`
- **Purpose:** The foundational HTML structure for the app.
- **Features:**
  - Defines sections for the start button, rules/instruction box, quiz box, and result box.
  - Dynamically updates sections (`que_text`, `option_list`) based on the data and logic from JavaScript.
  - Links external resources like Font Awesome and Google Fonts for enhanced UI.
- **Dynamic Content Areas:** The app dynamically inserts questions, options, and results into specific areas defined in the HTML structure. For example:
```html
<div class="que_text"></div> <!-- For question text -->
<div class="option_list"></div> <!-- For answer options -->
<div class="score_text"></div> <!-- For user score -->
```
- **Buttons:**
  - Start, Next, Quit, and Restart buttons allow user interaction:

```html
<button class="next_btn">Next Que</button>
<button class="quit">Quit Quiz</button>
<button class="restart">Replay Quiz</button>
```

### `style.css`
- **Purpose:** Provides the visual style for the app.
- **Highlights:**
  - Customizes UI components like buttons, boxes, and timers using modern CSS techniques.
  - Animates transitions for smooth user experience.
  - Responsive design ensures compatibility across devices.
- **Key Classes:**
  - `.start_btn`, `.info_box`, `.quiz_box`, `.result_box`: Define distinct sections.
  - `.correct`, `.incorrect`: Provide feedback on user selections.
  - `.timer`, `.time_line`: Visually represent the remaining time.
- **Timer Design:** Adds a visual representation of time left:
```css
.quiz_box header .timer .timer_sec {
  font-size: 18px;
  font-weight: 500;
  height: 30px;
  width: 45px;
  color: #fff;
  background: #343a40;
  border: 1px solid #343a40;
  text-align: center;
  line-height: 30px;
}
```
- **Option Highlighting:** Correct and incorrect answers are highlighted with distinct colors:
```css
.option.correct {
  color: #155724;
  background: #d4edda;
  border: 1px solid #c3e6cb;
}
.option.incorrect {
  color: #721c24;
  background: #f8d7da;
  border: 1px solid #f5c6cb;
}
```

### `questions.js`
- **Purpose:** Holds the data for the quiz questions.
- **Structure:**
  - An array of objects where each object represents a question.
  - Each object includes:
    - `numb`: Question number.
    - `question`: The question text.
    - `options`: An array of answer choices.
    - `answer`: The correct answer.
- **Example:**
```javascript
{
  numb: 1,
  question: "What does HTML stand for?",
  answer: "Hyper Text Markup Language",
  options: [
    "Hyper Text Preprocessor",
    "Hyper Text Markup Language",
    "Hyper Text Multiple Language",
    "Hyper Tool Multi Language",
  ],
}
```
- **Adding More Questions:** Questions can be easily extended by duplicating the existing object structure. For example:
```javascript
{
  numb: 6,
  question: "What does JSON stand for?",
  answer: "JavaScript Object Notation",
  options: [
    "JavaScript Object Notation",
    "JavaScript Online Node",
    "Java Syntax Object Nodes",
    "Java Simple Object Notes",
  ],
}
```

### `quizApp.js`
- **Purpose:** Implements the quiz's interactivity and logic.
- **Main Functions:**
  - **Start Quiz:** Adds event listeners to buttons for user interaction.
  - **Question Display:** Dynamically loads questions and options from `questions.js`.
  - **Answer Validation:** Checks the user's selected answer and updates the score accordingly.
  - **Timer:** Displays a countdown for each question and handles timeout scenarios.
  - **Progress Tracking:** Keeps track of the current question and updates the progress bar.
  - **Result Display:** Shows the final score and provides replay or quit options.
- **Question Display Logic:** This function inserts a question and its options dynamically into the DOM:
```javascript
function showQuetions(index) {
  let que_tag = `<span>${questions[index].numb}. ${questions[index].question}</span>`;
  let option_tag = questions[index].options
    .map((option) => `<div class="option"><span>${option}</span></div>`)
    .join("");
  que_text.innerHTML = que_tag; // Inject question
  option_list.innerHTML = option_tag; // Inject options
}
```
- **Answer Selection:** Provides immediate feedback on the selected option:
```javascript
function optionSelected(answer) {
  clearInterval(counter); // Stop timer
  let userAns = answer.textContent;
  let correcAns = questions[que_count].answer;

  if (userAns === correcAns) {
    userScore++; // Increment score
    answer.classList.add("correct"); // Highlight correct answer
    answer.insertAdjacentHTML("beforeend", tickIconTag);
  } else {
    answer.classList.add("incorrect"); // Highlight incorrect answer
    answer.insertAdjacentHTML("beforeend", crossIconTag);
  }
}
```
- **Result Display:** Shows the score and a congratulatory message based on performance:
```javascript
function showResult() {
  const scoreText = document.querySelector(".score_text");
  let scoreTag =
    userScore > 3
      ? `<span>Congrats! You scored <p>${userScore}</p> out of <p>${questions.length}</p></span>`
      : `<span>Better luck next time! You scored <p>${userScore}</p> out of <p>${questions.length}</p></span>`;
  scoreText.innerHTML = scoreTag; // Insert dynamic score
}
```
- **Timer Logic:** Ensures the timer updates every second and disables options when time is up:
```javascript
function startTimer(time) {
  counter = setInterval(() => {
    timeCount.textContent = time;
    time--;
    if (time < 0) {
      clearInterval(counter);
      const correctAns = questions[que_count].answer;
      option_list.querySelectorAll(".option").forEach((option) => {
        if (option.textContent === correctAns) {
          option.classList.add("correct");
        }
        option.classList.add("disabled"); // Disable further selections
      });
    }
  }, 1000);
}
```

## 6. Acknowledgments
1. LifeParticle. _Markdown Cheatsheet_. Github. Retrieved from [https://github.com/lifeparticle/Markdown-Cheatsheet](https://github.com/lifeparticle/Markdown-Cheatsheet)
2. Adam P. _Markdown Cheatsheet_. Github. Retrieved from [https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
