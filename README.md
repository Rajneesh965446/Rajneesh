<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <title>Raj AI</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body style="text-align:center; font-family:Arial; background:#0f172a; color:white;">

  <h2>🤖 Raj AI</h2>
  <p>🎤 Bolo, main sun raha hoon</p>

  <button onclick="startListening()" style="font-size:20px;padding:15px 25px;">
    🎙 Speak
  </button>

  <p id="userText"></p>
  <p id="aiText"></p>

<script>
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.lang = 'hi-IN';

function startListening() {
  recognition.start();
}

recognition.onresult = function(event) {
  let userSpeech = event.results[0][0].transcript;
  document.getElementById("userText").innerText = "👤 Tum: " + userSpeech;

  let reply = "Main tumhari baat samajh raha hoon. Tum accha kar rahe ho Rajneesh.";

  document.getElementById("aiText").innerText = "🤖 Raj AI: " + reply;

  let speech = new SpeechSynthesisUtterance(reply);
  speech.lang = 'hi-IN';
  window.speechSynthesis.speak(speech);
}
</script>

</body>
</html>
