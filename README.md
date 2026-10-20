# Jarvis.ai-
My personal ai assignment 

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Jarvis AI</title>
<style>
body {
  background:#07111f; color:#e8f5ff;
  font-family:Arial,sans-serif;text-align:center;
  padding:25px 12px;
}
h1 {color:#55d9ff}
#orb {
  margin:30px auto;width:135px;height:135px;
  border:3px solid #55d9ff;border-radius:50%;
  display:grid;place-items:center;
  font-size:48px;box-shadow:0 0 30px #167aa5;
}
button {
  padding:14px 20px;margin:8px;border:0;
  border-radius:12px;background:#55d9ff;
  color:#06111d;font-size:16px;font-weight:bold;
}
#status,#reply {margin:20px auto;max-width:500px}
</style>
</head>
<body>
<h1>J.A.R.V.I.S</h1>
<p>Your Personal Voice Assistant</p>
<div id="orb">🎙️</div>
<button onclick="listen()">🎤 Talk to Jarvis</button>
<button onclick="help()">Help</button>
<p id="status">Tap Talk to Jarvis to start.</p>
<p id="reply">Hello! I am Jarvis. How can I help you?</p>

<script>
const statusText = document.getElementById("status");
const replyText = document.getElementById("reply");

function speak(text) {
  replyText.textContent = text;
  speechSynthesis.cancel();
  const voice = new SpeechSynthesisUtterance(text);
  voice.lang = "en-IN";
  speechSynthesis.speak(voice);
}

function help() {
  speak("You can say hello, what is the time,
  what is the date, or who are you.");
}

function listen() {
  const SR = window.SpeechRecognition ||
             window.webkitSpeechRecognition;

  if (!SR) {
    speak("Voice recognition is not supported.
    Please try Google Chrome.");
    return;
  }

  const rec = new SR();
  rec.lang = "en-IN";
  rec.interimResults = false;
  statusText.textContent = "Listening...";

  rec.onresult = function(event) {
    const text = event.results[0][0].transcript
                 .toLowerCase();
    statusText.textContent = "You said: " + text;

    if (text.includes("time")) {
      speak("The time is " +
        new Date().toLocaleTimeString("en-IN"));
    } else if (text.includes("date") ||
               text.includes("today")) {
      speak("Today's date is " +
        new Date().toLocaleDateString("en-IN"));
    } else if (text.includes("hello") ||
               text.includes("hey jarvis") ||
               text.includes("hi")) {
      speak("Hello Bharat! I am ready to help you.");
    } else if (text.includes("who are you")) {
      speak("I am Jarvis, your personal voice assistant.");
    } else if (text.includes("your name")) {
      speak("My name is Jarvis.");
    } else if (text.includes("how are you")) {
      speak("I am working well. How can I help you?");
    } else {
      speak("I heard you say " + text +
        ". I do not know that command yet.");
    }
  };

  rec.onerror = function() {
    statusText.textContent =
      "Could not hear you. Check microphone and internet.";
  };

  rec.onend = function() {
    if (statusText.textContent === "Listening...") {
      statusText.textContent = "Tap the microphone to try again.";
    }
  };

  rec.start();
}
</script>
</body>
</html>
