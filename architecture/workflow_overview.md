In general, these workflows are similar to one another. However, I still consider the pypline and differences to be explained.

<!-- краткое описание каждого workflow. зачем он существует. какие входы / выходы -->


<img width="1766" height="281" alt="Ollama workflow" src="https://github.com/user-attachments/assets/b1b3a903-1196-42e6-b423-19127729fa8b" />

## 1. Webhook.
Triggers when message to bot on Telegram received. It automates collectioning of students' answers.

Example: 

Input: 
```
Simple:
Example 1: Making a cup of coffee in the morning using a coffee machine.
Example 2: Brushing my teeth before going to bed.

Complicated:
Example 1: Planning a monthly budget for my household.
Example 2: Preparing a tax declaration form with the help of an accountant.

Complex:
Example 1: Learning to play a new musical instrument by experimenting with different techniques.
Example 2: Managing relationships in a new team at work, adapting to different personalities.

Chaotic:
Example 1: Responding to a sudden power outage during an important online meeting.
Example 2: Dealing with an unexpected health emergency in the family.
```

Output:
```json
[{{{{
"id": 123456789,
"is_bot": false,
"first_name": "Nickname",
"username": "TgUserName",
"language_code": "ru"
}, 
"chat": {
  "id": 123456789,
  "first_name": "Nickname",
  "username": "TgUserName",
  "type": "private"
},
"date": 12345678910,
"text": "Simple:\nExample 1: Making a cup of coffee in the morning using a coffee machine.\nExample 2: Brushing my teeth before going to bed.\n\nComplicated:\nExample 1: Planning a monthly budget for my household.\nExample 2: Preparing a tax declaration form with the help of an accountant.\n\nComplex:\nExample 1: Learning to play a new musical instrument by experimenting with different techniques.\nExample 2: Managing relationships in a new team at work, adapting to different personalities.\n\nChaotic:\nExample 1: Responding to a sudden power outage during an important online meeting.\nExample 2: Dealing with an unexpected health emergency in the family."
}
},
"webhookUrl": "https://YOUR_SERVER/webhook-test/YOUR_TOKEN",
"executionMode": "test"
}]
```

## 2. Normalize Update
This node takes necessary fields of Webhook output and outputs them in a structured way. It cleans all unimportant inputs.

Example:

Output:

```json
[{{{
"first_name": "Nickname",
"username": "TgUserName",
"type": "private"
},
"date": 12345678910,
"text": "Simple:\nExample 1: Making a cup of coffee in the morning using a coffee machine.\nExample 2: Brushing my teeth before going to bed.\n\nComplicated:\nExample 1: Planning a monthly budget for my household.\nExample 2: Preparing a tax declaration form with the help of an accountant.\n\nComplex:\nExample 1: Learning to play a new musical instrument by experimenting with different techniques.\nExample 2: Managing relationships in a new team at work, adapting to different personalities.\n\nChaotic:\nExample 1: Responding to a sudden power outage during an important online meeting.\nExample 2: Dealing with an unexpected health emergency in the family."
},
"callback_query": null,
"poll_answer": null,
"chatId": 123456789
}]
```

## 3. Create Student
This node signs in new student to Nocobase. It keeps structured information about student in one place with ability to access it anytime.

Output:
```json
({
  "first_name": "Nickname",
  "chatId": 123456789,
  "username": "TgUserName",
  "workbook_id": 1,
  "text": "Simple:\nExample 1: Making a cup of coffee in the morning using a coffee machine.\nExample 2: Brushing my teeth before going to bed.\n\nComplicated:\nExample 1: Planning a monthly budget for my household.\nExample 2: Preparing a tax declaration form with the help of an accountant.\n\nComplex:\nExample 1: Learning to play a new musical instrument by experimenting with different techniques.\nExample 2: Managing relationships in a new team at work, adapting to different personalities.\n\nChaotic:\nExample 1: Responding to a sudden power outage during an important online meeting.\nExample 2: Dealing with an unexpected health emergency in the family."
})
```

## 4. Get workbooks
This node gives acces to the task that student sended assingnment to. It means to student's lesson materials in purpose of better rating made by AI Agent in later nodes. 

Output:
```json
({
  "id": 1,
  "title": "Cynefin Quadrant Classification",
  "lesson": "We all deal with different types of systems every day—from following a recipe to managing a team project. The Cynefin Quadrant framework helps us understand the nature of a problem or situation so we can respond to it more effectively. It breaks systems into four contexts: Simple (Clear): Cause and effect are obvious. The solution is known, and best practices apply. Complicated: Cause and effect exist but are not immediately obvious. Expert analysis and good practices are required. Complex: Cause and effect can only be understood in retrospect. The situation is unpredictable, so we must experiment, probe, and adapt. Chaotic: No clear cause-and-effect relationships. The priority is to act immediately to stabilize the situation before we can analyze it. The goal of this task is to move beyond theory and ground these concepts in your own experience. By identifying two personal examples for each quadrant, you'll practice recognizing the unique nature of different challenges and opportunities in your life.",
  "task_id": 2.2,
  "tast_text": "For each system, please give 2 examples from your life. Don’t use the ideas from the Exercise 1. Simple Example 1: Example 2: ;Complicated Example 1: Example 2: ; Complex Example 1: Example 2: ; Chaotic Example 1: Example 2:"
})
```

## 5. Ollama (AI agent)
This node posts query with http request to local Ollama. As a result, Ollama responses with scores and feedback to student's homework.

Output:
```json
{
  "final_score": "Final Score: 8/10",
  "summary": "Summary: Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience."
}
```

## 6. Parse AI response
This node cuts off unnecessary phrases and structurize Ollama's output. 

Output:
```json
{
  "AI_score": 8,
  "AI_feedback": "Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience.",
  "original_response": "{\"final_score\":\"Final Score: 8/10\",\"summary\":\"Summary: Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience.\"}",
  "processing_timestamp": "2024-01-15T12:00:00.000Z"
}
```

## 7. Update AI score and feedback
This node updates two fields of student's data in Nocobase. It eliminates professor's need to manually fill in all scores and remember feedback.

Output:
```json
{
  "AI_score": 8,
  "AI_feedback": "Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience."
}
```

## 8. Send hw teacher
This node sends structured message to teacher. It closes automation loop, so each time student's answer processed, teacher get already rated and well-explained score and feedback to be copypasted if meets teacher's opinion.

Output:
```
📝 <b>New Assignment Submission</b>
Student: Nickname (@TgUserName)
Module: Cynefin Quadrant Classification

<b>Answer:</b>
Simple:
Example 1: Making a cup of coffee in the morning using a coffee machine.
Example 2: Brushing my teeth before going to bed.

Complicated:
Example 1: Planning a monthly budget for my household.
Example 2: Preparing a tax declaration form with the help of an accountant.

Complex:
Example 1: Learning to play a new musical instrument by experimenting with different techniques.
Example 2: Managing relationships in a new team at work, adapting to different personalities.

Chaotic:
Example 1: Responding to a sudden power outage during an important online meeting.
Example 2: Dealing with an unexpected health emergency in the family.

<b>AI feedback:</b>
Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience.
<b>AI score:</b> 8

-----------------------------
To grade, <b>Reply</b> with:
[Score] [Comment]

Ref: #123456789
```

## 9. Send a text message
This node is optional bc it sends the same output as the previous node to user who sent message to the TG-bot. It was used by me to control the quality of AI answers and regulate if needed.

Output:
```
📝 New Assignment Submission
Student: Nickname (@TgUserName)
Module: Cynefin Quadrant Classification

Answer:
Simple:
Example 1: Making a cup of coffee in the morning using a coffee machine.
Example 2: Brushing my teeth before going to bed.

Complicated:
Example 1: Planning a monthly budget for my household.
Example 2: Preparing a tax declaration form with the help of an accountant.

Complex:
Example 1: Learning to play a new musical instrument by experimenting with different techniques.
Example 2: Managing relationships in a new team at work, adapting to different personalities.

Chaotic:
Example 1: Responding to a sudden power outage during an important online meeting.
Example 2: Dealing with an unexpected health emergency in the family.

AI feedback:
Nice work! Your examples are well-chosen and accurately reflect the core distinctions between the Cynefin domains. The logic behind each classification is clear and grounded in personal experience.
AI score: 8

-----------------------------
To grade, Reply with:
[Score] [Comment]

Ref: #123456789
```

<img width="1771" height="391" alt="Gigachat workflow" src="https://github.com/user-attachments/assets/2b76fe31-4a2b-4d92-b6be-59bde00de6ab" />
