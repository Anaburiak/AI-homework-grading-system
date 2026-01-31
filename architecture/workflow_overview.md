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
This node takes necessary fields of Webhook output and outputs them in a structured way. It helps clean all unimportant inputs.

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

<img width="1771" height="391" alt="Gigachat workflow" src="https://github.com/user-attachments/assets/2b76fe31-4a2b-4d92-b6be-59bde00de6ab" />
