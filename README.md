
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
      "Authorization": "Bearer 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"
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
