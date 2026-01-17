<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <title>Mitra AI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>

<body style="background:#0f172a;color:white;text-align:center;font-family:Arial">

<h1>🤖 Mitra AI</h1>
<p id="status">🎤 Speak dabao aur baat karo</p>

<button onclick="startListening()" style="font-size:22px;padding:15px 30px;">
🎙 Speak
</button>

<p id="userText"></p>
<p id="aiText"></p>

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

recognition.onresult = function(event) {
  let userSpeech = event.results[0][0].transcript;
  document.getElementById("userText").innerText = "👤 Tum: " + userSpeech;

  let reply = generateReply(userSpeech);
  document.getElementById("aiText").innerText = "🤖 Mitra AI: " + reply;

  let speech = new SpeechSynthesisUtterance(reply);
  speech.lang = 'hi-IN';

  // 👦👧 Voice logic (opposite gender)
  if (isFemaleName(userName)) {
    speech.pitch = 0.9; // ladka voice
  } else {
    speech.pitch = 1.4; // ladki voice
  }

  window.speechSynthesis.speak(speech);
};

function generateReply(text) {
  text = text.toLowerCase();

  if (text.includes("kaise ho")) {
    return "Main theek hoon " + userName + ", tum kaise ho?";
  }
  if (text.includes("dukhi")) {
    return "Mujhe afsos hai " + userName + ", main tumhare saath hoon.";
  }
  if (text.includes("padhai")) {
    return userName + ", focus rakho, tum accha kar rahe ho.";
  }
  if (text.includes("hello") || text.includes("hi")) {
    return "Hello " + userName + "! Tumse baat karke accha laga.";
  }

  return "Samajh raha hoon " + userName + ", aur batao.";
}
</script>

</body>
</html>
