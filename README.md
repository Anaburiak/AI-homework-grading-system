# AI Homework Grading System (Telegram + n8n + LLM)
An event-driven AI automation system that receives student homework via Telegram,
evaluates it using LLM agents (Ollama / GigaChat), and stores structured feedback
and grades in a database with automated notifications.

<img width="1771" height="391" alt="Gigachat workflow" src="https://github.com/user-attachments/assets/2b76fe31-4a2b-4d92-b6be-59bde00de6ab" />

## Problem
Manual homework evaluation is time-consuming and does not scale well.
Teachers need fast, consistent feedback while students want immediate responses.

## Solution
This system automates homework evaluation using AI agents triggered by Telegram messages.
It integrates messaging, LLM evaluation, structured storage, and notifications
into a single automated pipeline.

## System Architecture
1. Student submits homework via Telegram bot
2. n8n triggers a workflow on incoming message
3. Student data is created/updated in NocoBase
4. Homework text is sent to:
   - Local Ollama (offline inference)
   - or GigaChat (cloud LLM)
5. AI evaluates homework using predefined criteria
6. Feedback and grade are:
   - sent to the student
   - sent to the teacher
   - stored in NocoBase
     
  https://github.com/Anaburiak/AI-homework-grading-system/blob/fb668ec494c8f9ab10638595995e8792bbb6debb/architecture/diagram.png

## AI Evaluation Logic
Homework is evaluated using three criteria:
- Understanding
- Application of the lesson tools
- Answer structure

I chose these criteria because they allow to rate answer generally without specifications. As a result, this worflow can be used regardless the theme of assignement.

The AI returns:
- Numeric grade
- Structured teacher-style feedback

## Technologies Used
- n8n (workflow orchestration)
- Telegram Bot API
- Ollama (local LLM inference)
- GigaChat API
- NocoBase (backend & database)
- JSON / REST APIs

## Security & Privacy
- No API tokens or credentials are stored in the repository
- Personal data is anonymized in examples
- All secrets are managed via environment variables

## Project Status
Actively used in a real educational workflow.
Future improvements include:
- rubric-based grading
- plagiarism detection
- analytics dashboard
