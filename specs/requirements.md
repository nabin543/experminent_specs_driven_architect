# Requirements Document

## Introduction

This specification defines a prep exam generator for the AWS Certified Generative AI Developer - Professional certification. The system creates an interactive HTML-based quiz with 85 questions, multiple-choice answers, and detailed justifications for each answer option. The questions are based on the exam guide topics and follow the format and complexity level of the official AWS certification practice exams.

## Glossary

- **Prep_Exam**: An interactive, self-contained HTML-based quiz containing 85 questions with multiple-choice answers, justifications, and all embedded CSS/JS
- **Question_Component**: A reusable HTML/CSS/JS component that displays a question, answer options, and validation functionality
- **Exam_Guide**: The AWS certification exam guide that defines the topics and domains covered
- **Justification**: An explanation of why each answer option is correct or incorrect
- **Validation_Button**: A UI element that reveals correct answers and justifications when clicked
- **Score_Counter**: A UI component displaying the running count of correct and incorrect answers
- **Countdown_Timer**: A UI component that counts down from 205 minutes, triggered by the first Validate click
- **Fixed_Header**: The dark navy header component that remains fixed at the top of the viewport during scrolling
- **Disclaimer_Text**: An informational notice displayed in the header clarifying the prep exam is for practice purposes only
- **Scenario_Question**: A question presenting a detailed real-world situation with multiple constraints
- **Trade_Off_Question**: A question requiring selection of the MOST/LEAST appropriate option based on criteria like operational overhead

## Requirements

### Requirement 1: Question Content

**User Story:** As a certification candidate, I want a comprehensive set of 85 professional-level questions covering all exam domains, so that I can practice for the AWS Certified Generative AI Developer exam at the appropriate difficulty level.

#### Acceptance Criteria

1. THE Prep_Exam SHALL contain exactly 85 questions covering the exam domains
2. WHEN creating questions, THE Prep_Exam SHALL include questions from all major exam topics:
    - Foundation models and generative AI concepts
    - Amazon Bedrock and model selection
    - Prompt engineering and optimization
    - Fine-tuning and customization
    - Retrieval Augmented Generation (RAG)
    - Responsible AI and security
    - Application development and deployment
3. WHEN a question has multiple correct answers, THE Prep_Exam SHALL clearly indicate this requirement

### Requirement 2: Question Complexity Standards

**User Story:** As a certification candidate, I want questions that match the complexity of the actual professional certification exam, so that I am adequately prepared.

#### Acceptance Criteria

1. THE Prep_Exam SHALL include at least 70% scenario-based questions with detailed real-world contexts
2. WHEN creating scenario questions, THE Prep_Exam SHALL include multiple constraints (e.g., performance requirements, data size limits, response time requirements)
3. THE Prep_Exam SHALL include questions requiring trade-off analysis using phrases like "MOST appropriate", "LEAST operational overhead", or "MINIMAL cost"
4. THE Prep_Exam SHALL include troubleshooting scenarios where symptoms are described and root cause must be identified
5. THE Prep_Exam SHALL include questions involving multi-service AWS architectures (e.g., Bedrock + Lambda + API Gateway + S3)
6. WHEN creating answer options, THE Prep_Exam SHALL ensure all options are plausible and technically valid, differing in appropriateness for the specific scenario
7. THE Prep_Exam SHALL include questions with specific configuration details, API parameters, or code/JSON snippets where appropriate
8. THE Prep_Exam SHALL include at least 15-18 multi-select questions requiring selection of 2 correct answers from 5 options

### Requirement 3: Answer Structure

**User Story:** As a certification candidate, I want each question to have clearly defined answer options, so that I can select my responses.

#### Acceptance Criteria

1. THE Prep_Exam SHALL provide 4-5 answer options per question
2. WHEN a question requires single selection, THE Prep_Exam SHALL indicate exactly one correct answer
3. WHEN a question requires multiple selections, THE Prep_Exam SHALL indicate all correct answers and specify how many to select
4. THE Prep_Exam SHALL include a justification for each answer option explaining why it is correct or incorrect

### Requirement 4: HTML Quiz Interface

**User Story:** As a certification candidate, I want a simple HTML interface to take the prep exam, so that I can practice without complex setup.

#### Acceptance Criteria

1. THE Prep_Exam SHALL be a single HTML file that works in any modern browser
2. THE Prep_Exam SHALL display all 85 questions sequentially on a single page
3. WHEN displaying a question, THE Prep_Exam SHALL show the question text and all answer options
4. THE Prep_Exam SHALL allow users to select answers by clicking on the options
5. WHEN a question requires multiple answers, THE Prep_Exam SHALL allow multiple selections

### Requirement 5: Question Component

**User Story:** As a developer, I want a reusable question component pattern, so that all 85 questions have consistent behavior and appearance.

#### Acceptance Criteria

1. THE Question_Component SHALL display the question number and text
2. THE Question_Component SHALL display all answer options as clickable elements
3. WHEN an answer option is clicked, THE Question_Component SHALL visually indicate the selection
4. THE Question_Component SHALL include a "Validate" button for each question
5. WHEN the Validate button is clicked, THE Question_Component SHALL reveal the correct answer(s)
6. WHEN the Validate button is clicked, THE Question_Component SHALL display justifications for all answer options

### Requirement 6: Validation Feedback

**User Story:** As a certification candidate, I want to see detailed feedback after validating my answer, so that I can learn from my mistakes.

#### Acceptance Criteria

1. WHEN validation is triggered, THE Question_Component SHALL highlight correct answers in green
2. WHEN validation is triggered, THE Question_Component SHALL highlight incorrect selected answers in red
3. WHEN validation is triggered, THE Question_Component SHALL display the justification text for each answer option
4. THE justification text SHALL explain why each option is correct or incorrect

### Requirement 7: Score Counter

**User Story:** As a certification candidate, I want to see a running count of my correct and incorrect answers, so that I can track my progress through the exam.

#### Acceptance Criteria

1. THE Prep_Exam SHALL display a Score_Counter in the Fixed_Header
2. THE Score_Counter SHALL show the count of questions answered correctly
3. THE Score_Counter SHALL show the count of questions answered incorrectly
4. WHEN a Validate button is clicked, THE Score_Counter SHALL recalculate and update the counts by scanning all validated questions on the page
5. THE Score_Counter SHALL be visible at all times via the Fixed_Header
6. THE Score_Counter SHALL use clear visual distinction between correct (green) and incorrect (red) counts

### Requirement 8: Countdown Timer

**User Story:** As a certification candidate, I want a countdown timer that simulates the real exam time limit, so that I can practice managing my time during the prep exam.

#### Acceptance Criteria

1. THE Prep_Exam SHALL display a Countdown_Timer in the Fixed_Header showing a countdown from 205 minutes
2. WHEN the first Validation_Button on the page is clicked, THE Countdown_Timer SHALL start counting down
3. WHILE the Countdown_Timer is running, THE Countdown_Timer SHALL display the remaining time in minutes and seconds format (e.g., "204:59")
4. WHEN subsequent Validation_Buttons are clicked after the timer has started, THE Countdown_Timer SHALL continue counting without restarting or resetting
5. WHEN the Countdown_Timer reaches zero, THE Countdown_Timer SHALL stop and display "0:00"
6. WHEN the page is refreshed, THE Countdown_Timer SHALL reset to the initial 205-minute value and remain stopped until the next Validate click

### Requirement 9: Fixed Header (UI Consolidation)

**User Story:** As a certification candidate, I want the exam title, countdown timer, score counter, and disclaimer always visible while scrolling, so that I can track my progress without scrolling back to the top.

#### Acceptance Criteria

1. THE Fixed_Header SHALL contain the exam title, subtitle, Countdown_Timer, Score_Counter, and Disclaimer_Text
2. THE Fixed_Header SHALL use CSS fixed positioning so it remains visible at the top of the viewport during scrolling
3. THE Score_Counter SHALL be embedded inside the Fixed_Header
4. THE Disclaimer_Text SHALL read: "This prep exam is for educational and practice purposes only. It does not replace or supplement official AWS Training & Certification materials."
5. THE Fixed_Header SHALL maintain the dark navy gradient background styling
6. WHILE the user scrolls through questions, THE Fixed_Header SHALL remain fully visible and accessible
7. THE Prep_Exam main content SHALL include adequate top padding to prevent content from being hidden behind the Fixed_Header

### Requirement 10: Minimal Design

**User Story:** As a certification candidate, I want a clean and simple interface, so that I can focus on the exam content without distractions.

#### Acceptance Criteria

1. THE Prep_Exam SHALL use basic CSS styling without external dependencies
2. THE Prep_Exam SHALL be functional without any external JavaScript libraries
3. THE Prep_Exam SHALL use readable fonts and adequate spacing
4. THE Prep_Exam SHALL work without requiring a web server (can be opened directly as a file)

### Requirement 11: Incremental Content Generation

**User Story:** As a developer, I want to generate questions in batches of 5, so that the work can be split into manageable tasks.

#### Acceptance Criteria

1. THE Prep_Exam content SHALL be created in 17 batches of 5 questions each
2. WHEN generating a batch, THE system SHALL ensure questions cover different topics within that batch
3. WHEN all batches are complete, THE Prep_Exam SHALL contain exactly 85 questions
