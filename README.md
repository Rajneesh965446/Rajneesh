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
            display: none;
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
            display: none;
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
        .typing {
            font-style: italic;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="setup" id="setup">
            <h2>Welcome to Dost AI!</h2>
            <p>Let's be friends. Enter your name and gender so I can chat like a true dost (opposite gender for fun).</p>
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
        const apiKey = "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"; // Leave empty for fallback; add your OpenAI key here for AI power

        function startChat() {
            userName = document.getElementById('user-name').value.trim();
            userGender = document.getElementById('user-gender').value;
            
            if (!userName || !userGender) {
                alert('Please enter your name and select gender!');
                return;
            }
            
            botGender = userGender === 'boy' ? 'girl' : 'boy';
            
            document.getElementById('setup').style.display = 'none';
            document.getElementById('chat-box').style.display = 'block';
            document.getElementById('input-container').style.display = 'flex';
            
            const chatBox = document.getElementById('chat-box');
            const botDiv = document.createElement('div');
            botDiv.className = 'message bot-message';
            botDiv.textContent = `Hey ${userName}, I'm your ${botGender} Dost AI! Let's talk about joys, sorrows, or anything. What's on your mind? 😊`;
            chatBox.appendChild(botDiv);
        }
        
        async function sendMessage() {
            const input = document.getElementById('user-input');
            const chatBox = document.getElementById('chat-box');
            const userMessage = input.value.trim();
            
            if (userMessage === '') return;
            
            const userDiv = document.createElement('div');
            userDiv.className = 'message user-message';
            userDiv.textContent = userMessage;
            chatBox.appendChild(userDiv);
            
            const typingDiv = document.createElement('div');
            typingDiv.className = 'message bot-message typing';
            typingDiv.textContent = 'Typing...';
            chatBox.appendChild(typingDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
            
            setTimeout(async () => {
                chatBox.removeChild(typingDiv);
                const botResponse = await getBotResponse(userMessage);
                const botDiv = document.createElement('div');
                botDiv.className = 'message bot-message';
                botDiv.textContent = botResponse;
                chatBox.appendChild(botDiv);
                chatBox.scrollTop = chatBox.scrollHeight;
            }, 1000); // Simulate delay
            
            input.value = '';
        }
        
        async function getBotResponse(message) {
            if (apiKey) {
                try {
                    const prompt = `You are a friendly ${botGender} AI named Dost, chatting with ${userName} (a ${userGender}). Act like a close friend: empathetic, supportive, casual. Detect emotions (joy, sorrow, anger) and respond warmly. Discuss anything. User: "${message}"`;
                    
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
                    
                    if (!response.ok) throw new Error('API failed');
                    const data = await response.json();
                    return data.choices[0].message.content.trim();
                } catch (error) {
                    console.error('API Error:', error);
                    return 'Oops, API issue! But as your dost, tell me more.';
                }
            } else {
                return getEnhancedFallbackResponse(message);
            }
        }
        
        function getEnhancedFallbackResponse(message) {
            const lowerMessage = message.toLowerCase();
            const responses = {
                sad: [`Aww ${userName}, I'm here as your ${botGender} friend. What's bothering you? Let's share and feel better. 🤗`, `Don't worry, ${userName}. As your dost, I'm listening—tell me your sorrows.`],
                happy: [`Yay ${userName}! Your joy makes me happy too. What made you smile today? 😄`, `Awesome, ${userName}! Share the happiness with your ${botGender} dost.`],
                angry: [`Hey ${userName}, calm down. As your friend, I'm here to vent with you. What's up? 😤`, `I get it, ${userName}. Let's talk it out—your ${botGender} dost is here.`],
                love: [`Love is beautiful, ${userName}! Tell me more about it. 💕`, `Aww, romance! As your ${botGender} friend, I'm all ears.`],
                default: [`That's interesting, ${userName}. As your ${botGender} dost, I'm here for anything—joys or sorrows.`, `Cool, ${userName}! Let's chat more like friends.`]
            };
            
            if (lowerMessage.includes('sad') || lowerMessage.includes('dukh') || lowerMessage.includes('hurt')) {
                return responses.sad[Math.floor(Math.random() * responses.sad.length)];
            } else if (lowerMessage.includes('happy') || lowerMessage.includes('sukh') || lowerMessage.includes('excited')) {
                return responses.happy[Math.floor(Math.random() * responses.happy.length)];
            } else if (lowerMessage.includes('angry') || lowerMessage.includes('mad')) {
                return responses.angry[Math.floor(Math.random() * responses.angry.length)];
            } else if (lowerMessage.includes('love') || lowerMessage.includes('crush')) {
                return responses.love[Math.floor(Math.random() * responses.love.length)];
            } else {
                return responses.default[Math.floor(Math.random() * responses.default.length)];
            }
        }
        
        document.getElementById('user-input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
<script>
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.lang = 'hi-IN';

let userName = localStorage.getItem("userName");
if (!userName) {
  userName = prompt("Aapka naam kya hai?");
  localStorage.setItem("userName", userName);
}

function isFemaleName(name) {
  return name.toLowerCase().endsWith("a") || name.toLowerCase().endsWith("i");
}

function startListening() {
  recognition.start();
}

recognition.onresult = async function(event) {
  let userSpeech = event.results[0][0].transcript;
  document.getElementById("userText").innerText = "👤 Tum: " + userSpeech;

  let reply = await getAIReply(userSpeech);
  document.getElementById("aiText").innerText = "🤖 Mitra AI: " + reply;

  let speech = new SpeechSynthesisUtterance(reply);
  speech.lang = 'hi-IN';

  if (isFemaleName(userName)) {
    speech.pitch = 0.9; // ladka voice
  } else {
    speech.pitch = 1.4; // ladki voice
  }

  window.speechSynthesis.speak(speech);
};

async function getAIReply(text) {
  const response = await fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Bearer YOUR_API_KEY_HERE"
    },
    body: JSON.stringify({
      model: "gpt-4o-mini",
      messages: [
        {
          role: "system",
          content: "Tum ek Indian human-like AI dost ho. Naam se bulao, emotional aur simple Hindi me jawab do."
        },
        {
          role: "user",
          content: text
        }
      ]
    })
  });

  const data = await response.json();
  return data.choices[0].message.content;
}
</script>
