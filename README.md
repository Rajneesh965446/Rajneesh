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
        .container {
            width: 400px;
            height: 500px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
        }
        .setup {
            padding: 20px;
            text-align: center;
        }
        .chat-box {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
            border-bottom: 1px solid #ccc;
            display: none; /* Hidden until setup */
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
            display: none; /* Hidden until setup */
        }
        input, select {
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 5px;
            margin: 5px;
        }
        button {
            padding: 8px 15px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="setup" id="setup">
            <h2>Welcome to Dost AI!</h2>
            <p>Let's be friends. Tell me your name and gender so I can chat like a true dost.</p>
            <input type="text" id="user-name" placeholder="Your Name" required>
            <select id="user-gender" required>
                <option value="">Select Gender</option>
                <option value="boy">Boy</option>
                <option value="girl">Girl</option>
            </select>
            <br><br>
            <button onclick="startChat()">Start Chatting!</button>
        </div>
        
        <div class="chat-box" id="chat-box"></div>
        <div class="input-container" id="input-container">
            <input type="text" id="user-input" placeholder="Type your message...">
            <button onclick="sendMessage()">Send</button>
        </div>
    </div>

    <script>
        let userName = '';
        let userGender = '';
        let botGender = '';
        const apiKey = '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'; // Replace with your OpenAI API key

        function startChat() {
            userName = document.getElementById('user-name').value.trim();
            userGender = document.getElementById('user-gender').value;
            
            if (!userName || !userGender) {
                alert('Please enter your name and select gender!');
                return;
            }
            
            // Set bot gender opposite to user
            botGender = userGender === 'boy' ? 'girl' : 'boy';
            
            // Hide setup, show chat
            document.getElementById('setup').style.display = 'none';
            document.getElementById('chat-box').style.display = 'block';
            document.getElementById('input-container').style.display = 'flex';
            
            // Initial bot message
            const chatBox = document.getElementById('chat-box');
            const botDiv = document.createElement('div');
            botDiv.className = 'message bot-message';
            botDiv.textContent = `Hey ${userName}, I'm your ${botGender} Dost AI! Let's talk about anything—joys, sorrows, or just life. What's on your mind? 😊`;
            chatBox.appendChild(botDiv);
        }
        
        async function sendMessage() {
            const input = document.getElementById('user-input');
            const chatBox = document.getElementById('chat-box');
            const userMessage = input.value.trim();
            
            if (userMessage === '') return;
            
            // Add user message
            const userDiv = document.createElement('div');
            userDiv.className = 'message user-message';
            userDiv.textContent = userMessage;
            chatBox.appendChild(userDiv);
            
            // Show typing indicator
            const typingDiv = document.createElement('div');
            typingDiv.className = 'message bot-message';
            typingDiv.textContent = 'Typing...';
            chatBox.appendChild(typingDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
            
            // Get bot response
            const botResponse = await getBotResponse(userMessage);
            chatBox.removeChild(typingDiv);
            
            const botDiv = document.createElement('div');
            botDiv.className = 'message bot-message';
            botDiv.textContent = botResponse;
            chatBox.appendChild(botDiv);
            
            // Scroll to bottom
            chatBox.scrollTop = chatBox.scrollHeight;
            
            // Clear input
            input.value = '';
        }
        
        async function getBotResponse(message) {
            if (!apiKey || apiKey === 'YOUR_OPENAI_API_KEY') {
                // Fallback if no API key
                return getFallbackResponse(message);
            }
            
            try {
                const prompt = `You are a friendly ${botGender} AI named Dost, chatting with ${userName} (a ${userGender}). Act like a close friend: empathetic, supportive, and casual. Understand emotions in their message (e.g., happiness, sadness) and respond accordingly. Discuss joys, sorrows, or anything. Keep it light and fun. User said: "${message}"`;
                
                const response = await fetch('https://api.openai.com/v1/chat/completions', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${apiKey}`
                    },
                    body: JSON.stringify({
                        model: 'gpt-3.5-turbo',
                        messages: [{ role: 'user', content: prompt }],
                        max_tokens: 150
                    })
                });
                
                const data = await response.json();
                return data.choices[0].message.content.trim();
            } catch (error) {
                console.error('API Error:', error);
                return 'Oops, something went wrong! Tell me more anyway.';
            }
        }
        
        function getFallbackResponse(message) {
            const lowerMessage = message.toLowerCase();
            if (lowerMessage.includes('sad') || lowerMessage.includes('dukh')) {
                return `Aww, ${userName}, I'm here for you as your ${botGender} friend. What's making you sad? Let's talk it out. 🤗`;
            } else if (lowerMessage.includes('happy') || lowerMessage.includes('sukh')) {
                return `Yay, ${userName}! I'm so glad you're happy. Share the joy with me! 😄`;
            } else {
                return `That's cool, ${userName}. As your ${botGender} dost, I'm all ears. Tell me more!`;
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
