<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Dost AI Chatbot</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chat-container {
            width: 400px;
            height: 500px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
        }
        .chat-box {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
            border-bottom: 1px solid #ccc;
        }
        .message {
            margin: 5px 0;
            padding: 8px;
            border-radius: 5px;
        }
        .user-message {
            background-color: #007bff;
            color: white;
            text-align: right;
        }
        .bot-message {
            background-color: #e9ecef;
            color: black;
        }
        .input-container {
            display: flex;
            padding: 10px;
        }
        input {
            flex: 1;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            padding: 8px 15px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            margin-left: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-box" id="chat-box">
            <div class="message bot-message">Hey there, my friend! I'm your Dost AI. How can I help you today? 😊</div>
        </div>
        <div class="input-container">
            <input type="text" id="user-input" placeholder="Type your message...">
            <button onclick="sendMessage()">Send</button>
        </div>
    </div>

    <script>
        function sendMessage() {
            const input = document.getElementById('user-input');
            const chatBox = document.getElementById('chat-box');
            const userMessage = input.value.trim();
            
            if (userMessage === '') return;
            
            // Add user message
            const userDiv = document.createElement('div');
            userDiv.className = 'message user-message';
            userDiv.textContent = userMessage;
            chatBox.appendChild(userDiv);
            
            // Generate bot response
            const botResponse = getBotResponse(userMessage);
            const botDiv = document.createElement('div');
            botDiv.className = 'message bot-message';
            botDiv.textContent = botResponse;
            chatBox.appendChild(botDiv);
            
            // Scroll to bottom
            chatBox.scrollTop = chatBox.scrollHeight;
            
            // Clear input
            input.value = '';
        }
        
        function getBotResponse(message) {
            const lowerMessage = message.toLowerCase();
            
            // Simple keyword-based responses (expand this for more "power")
            if (lowerMessage.includes('hello') || lowerMessage.includes('hi')) {
                return "Hello, my friend! What's up?";
            } else if (lowerMessage.includes('how are you')) {
                return "I'm doing great, thanks for asking! How about you?";
            } else if (lowerMessage.includes('name')) {
                return "I'm your Dost AI, your friendly companion!";
            } else if (lowerMessage.includes('joke')) {
                return "Why don't scientists trust atoms? Because they make up everything! 😂";
            } else if (lowerMessage.includes('bye')) {
                return "Goodbye, friend! Talk to you soon. 👋";
            } else {
                return "That's interesting! Tell me more, or ask me something else.";
            }
        }
        
        // Allow sending with Enter key
        document.getElementById('user-input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
