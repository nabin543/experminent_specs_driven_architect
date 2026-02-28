# Design Document: GenAI Certification Prep Exam

## Overview

This design describes a simple, self-contained prep exam system for the AWS Certified Generative AI Developer - Professional certification. The system produces a single artifact:

- **Prep Exam (HTML)**: A single HTML file with embedded CSS and JavaScript that renders the quiz interface with all 85 questions, answers, and justifications

The design prioritizes simplicity and portability — the final HTML file can be opened directly in any browser without a web server. The exam features professional-level scenario-based questions, a real-time score counter, a 205-minute countdown timer, and a fixed header with an educational disclaimer.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Prep Exam Project                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │         prep-exam.html (self-contained)              │    │
│  │                                                      │    │
│  │  - Embedded CSS                                      │    │
│  │  - Embedded JavaScript                               │    │
│  │  - 85 Question Components (with justifications)      │    │
│  │  - Fixed Header                                      │    │
│  │    ├─ Score Counter                                  │    │
│  │    ├─ Countdown Timer                                │    │
│  │    └─ Disclaimer                                     │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

The HTML file is the final deliverable — a zero-dependency, self-contained prep exam that works offline in any modern browser.

## Components and Interfaces

### Component 1: HTML Quiz Interface

**File**: `educational-prep-exam-genai-pro.html`

**Purpose**: Render an interactive quiz from the question data

**Structure**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Educational Prep Exam - GenAI Professional</title>
    <style>/* Embedded CSS */</style>
</head>
<body>
    <header class="fixed-header">
        <h1>Educational content generation powered by Kiro</h1>
        <p>85 Questions | Select answers and click Validate to check</p>
        <div class="header-info-row">
            <div class="header-timer">
                <span class="timer-icon">⏱</span>
                <span id="countdownTimer">205:00</span>
            </div>
            <div class="header-score-counter">
                <span class="correct-count">Correct: 0</span>
                <span class="incorrect-count">Incorrect: 0</span>
                <span class="total-validated">Validated: 0/85</span>
            </div>
        </div>
        <p class="disclaimer">This prep exam is for educational and practice purposes only.
        It does not replace or supplement official AWS Training & Certification materials.</p>
    </header>
    <main>
        <!-- 85 Question Components -->
    </main>
    <script>/* Embedded JavaScript */</script>
</body>
</html>
```

### Component 2: Fixed Header

**Purpose**: Consolidate exam title, countdown timer, score counter, and disclaimer into a single fixed-position header that remains visible while scrolling.

**Contains**:
- Exam title and subtitle
- Countdown Timer (205 minutes, starts on first Validate click)
- Score Counter (correct/incorrect/validated counts)
- Educational disclaimer text

**CSS**:
```css
.fixed-header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    text-align: center;
    padding: 15px 20px;
    background: linear-gradient(135deg, #232f3e 0%, #37475a 100%);
    color: white;
    box-shadow: 0 2px 8px rgba(0,0,0,0.3);
}

.header-info-row {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 40px;
    margin: 8px 0;
    flex-wrap: wrap;
}

main {
    padding-top: 220px; /* Offset for fixed header height */
}
```

### Component 3: Score Counter

**Purpose**: Display real-time count of correct and incorrect answers, embedded inside the Fixed Header.

**JavaScript**:
```javascript
function updateScoreCounter() {
    let correct = 0, incorrect = 0, validated = 0;
    document.querySelectorAll('.question.validated').forEach(question => {
        validated++;
        const correctAnswers = question.dataset.correct.split(',');
        const userSelections = selections[question.dataset.question] || [];
        const isCorrect = correctAnswers.length === userSelections.length &&
            correctAnswers.every(ans => userSelections.includes(ans));
        if (isCorrect) correct++; else incorrect++;
    });
    document.getElementById('correctCount').textContent = correct;
    document.getElementById('incorrectCount').textContent = incorrect;
    document.getElementById('validatedCount').textContent = `${validated}/85`;
}
```

### Component 4: Countdown Timer

**Purpose**: Display a 205-minute countdown that starts on the first Validate click and cannot be restarted.

**JavaScript**:
```javascript
let timerStarted = false;
let timerInterval = null;
let remainingSeconds = 205 * 60;

function startTimer() {
    if (timerStarted) return;
    timerStarted = true;
    timerInterval = setInterval(() => {
        remainingSeconds--;
        if (remainingSeconds <= 0) {
            remainingSeconds = 0;
            clearInterval(timerInterval);
        }
        updateTimerDisplay();
    }, 1000);
}

function updateTimerDisplay() {
    const minutes = Math.floor(remainingSeconds / 60);
    const seconds = remainingSeconds % 60;
    document.getElementById('countdownTimer').textContent =
        `${minutes}:${seconds.toString().padStart(2, '0')}`;
}
```

The `startTimer()` function is called at the beginning of `validateQuestion()`. The `timerStarted` guard flag ensures only the first call starts the interval.

### Component 5: Question Component (HTML Pattern)

**Purpose**: Reusable HTML structure for each question.

```html
<div class="question" data-question="1" data-type="single" data-correct="A">
    <div class="question-header">
        <span class="question-number">Question 1</span>
        <span class="question-topic">Topic: Amazon Bedrock</span>
    </div>
    <div class="question-text"><p>Question text goes here...</p></div>
    <div class="options">
        <div class="option" data-option="A">
            <span class="option-letter">A)</span>
            <span class="option-text">Option A text</span>
            <div class="justification hidden">
                <strong>Incorrect</strong> - Explanation here...
            </div>
        </div>
        <!-- Options B, C, D... -->
    </div>
    <button class="validate-btn" onclick="validateQuestion(1)">Validate</button>
</div>
```

**CSS Classes**:
- `.question` — Container for each question
- `.option.selected` — User-selected option
- `.option.correct` — Correct answer (shown after validation, green)
- `.option.incorrect` — Incorrect selected answer (shown after validation, red)
- `.justification` — Explanation text (hidden until validation)
- `.validate-btn` — Validation button

## Data Models

### Question Data Model

```javascript
{
    questionNumber: number,      // 1-85
    topic: string,               // Exam domain
    type: "single" | "multiple", // Selection type
    selectCount: number,         // For multiple: how many to select
    text: string,                // Question text
    options: [
        {
            letter: string,      // A, B, C, D, E
            text: string,        // Option text
            isCorrect: boolean,
            justification: string
        }
    ]
}
```

### Exam Topics Coverage

| Domain | Weight | Questions (85 total) |
|--------|--------|---------------------|
| Domain 1: Fundamentals of Generative AI | 14% | 12 |
| Domain 2: Foundations of Generative AI Applications | 28% | 24 |
| Domain 3: Applications of Foundation Models | 28% | 24 |
| Domain 4: Responsible AI | 14% | 12 |
| Domain 5: Security and Compliance | 16% | 13 |

### Question Complexity Patterns

To match professional certification exam difficulty, questions follow these patterns:

#### Pattern 1: Scenario-Based Questions
Questions present detailed real-world situations with multiple constraints (performance requirements, data size limits, response time requirements).

#### Pattern 2: Trade-Off Analysis Questions
Questions require selecting the MOST/LEAST appropriate option based on criteria like operational overhead, cost, or complexity.

#### Pattern 3: Troubleshooting Scenarios
Questions describe symptoms and require root cause identification.

#### Pattern 4: Multi-Service Architecture
Questions involve integration of multiple AWS services (e.g., Bedrock + Lambda + API Gateway + S3).

#### Pattern 5: Configuration Details
Questions include specific API parameters, JSON configurations, or code snippets.

#### Pattern 6: Multi-Select Questions (Select 2)
At least 15-18 questions require selecting exactly 2 correct answers from 5 options.

### Answer Option Design Principles

1. All options must be technically plausible
2. Options differ in appropriateness for the specific scenario
3. Distractors address common misconceptions
4. Options vary in operational overhead
5. Include specific service names and features

## Correctness Properties

### Property 1: Question Count Invariant
*For any* complete Prep_Exam, the total number of questions SHALL equal exactly 85.
**Validates: Requirements 1.1, 11.3**

### Property 2: Answer Option Count
*For any* question in the Prep_Exam, the number of answer options SHALL be between 4 and 5 inclusive.
**Validates: Requirements 3.1**

### Property 3: Correct Answer Existence
*For any* question in the Prep_Exam, there SHALL exist at least one correct answer among the options.
**Validates: Requirements 3.2, 3.3**

### Property 4: Justification Completeness
*For any* question in the Prep_Exam, every answer option SHALL have a non-empty justification.
**Validates: Requirements 3.4**

### Property 5: Selection State Toggle
*For any* question component and any answer option, clicking the option SHALL toggle its selected state.
**Validates: Requirements 5.3**

### Property 6: Validation Reveals Correctness
*For any* question component, clicking the Validate button SHALL make all correct answers visually distinguishable from incorrect answers.
**Validates: Requirements 5.5, 6.1, 6.2**

### Property 7: Validation Reveals Justifications
*For any* question component, clicking the Validate button SHALL display the justification for every answer option.
**Validates: Requirements 5.6, 6.3**

### Property 8: Batch Size Consistency
*For any* batch of questions added to the Prep_Exam, the batch size SHALL be exactly 5 questions.
**Validates: Requirements 11.1**

### Property 9: Score Counter Accuracy
*For any* set of validated questions, the Score_Counter SHALL display the correct count of questions where user selections exactly match the correct answers, and the incorrect count for all other validated questions.
**Validates: Requirements 7.2, 7.3, 7.4**

### Property 10: Question Complexity Distribution
*For any* complete Prep_Exam, at least 70% of questions SHALL be scenario-based with multiple constraints.
**Validates: Requirements 2.1**

### Property 11: Multi-Select Question Count
*For any* complete Prep_Exam, there SHALL be at least 15 questions requiring selection of 2 correct answers.
**Validates: Requirements 2.8**

### Property 12: Timer Display Format
*For any* remaining time value between 0 and 12300 seconds (205 minutes), the Countdown_Timer display SHALL show the value formatted as `M:SS` where M is the integer minutes and SS is the zero-padded seconds.
**Validates: Requirements 8.3, 8.5**

### Property 13: Timer Start Idempotence
*For any* sequence of N calls to `startTimer()` where N ≥ 1, the timer SHALL start exactly once.
**Validates: Requirements 8.4**

## Error Handling

### Content Errors
- **Missing justification**: Flagged during review
- **Invalid correct answer**: If the correct answer letter doesn't match any option, the question is invalid
- **Duplicate question numbers**: Each question must have a unique number 1-85

### Runtime Errors (HTML)
- **Missing data attributes**: JavaScript gracefully handles missing data attributes
- **Invalid question type**: Defaults to "single" selection if type is not recognized

## Testing Strategy

### Manual Testing

1. **Visual Inspection**: Verify all 85 questions render correctly
2. **Selection Testing**: Click each option type and verify selection behavior
3. **Validation Testing**: Verify correct answers in green, incorrect in red, justifications displayed
4. **Browser Compatibility**: Test in Chrome, Firefox, Safari, Edge
5. **Fixed Header Testing**: Scroll through questions and verify header stays fixed with timer, score, and disclaimer visible
6. **Countdown Timer Testing**: Verify timer starts on first Validate, doesn't restart on subsequent clicks, stops at 0:00
7. **Content Padding**: Verify first question is not hidden behind the fixed header

### Content Validation
- Review each question for accuracy against AWS documentation
- Verify justifications are factually correct
- Ensure topic coverage matches exam guide percentages
