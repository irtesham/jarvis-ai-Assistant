<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>J.A.R.V.I.S. - AI Assistant</title>
    <style>
        :root {
            --jarvis-blue: #00a0ff;
            --jarvis-dark: #001f3f;
            --jarvis-gold: #ffd700;
            --jarvis-light: rgba(0, 160, 255, 0.2);
        }
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #000;
            color: #fff;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: 
                radial-gradient(circle at 20% 30%, rgba(0, 160, 255, 0.15) 0%, transparent 20%),
                radial-gradient(circle at 80% 70%, rgba(0, 160, 255, 0.15) 0%, transparent 20%);
        }
        .chat-container {
            width: 90%;
            max-width: 900px;
            height: 90vh;
            background-color: rgba(0, 20, 40, 0.9);
            border-radius: 15px;
            box-shadow: 0 0 30px rgba(0, 160, 255, 0.4);
            overflow: hidden;
            border: 1px solid var(--jarvis-blue);
            display: flex;
            flex-direction: column;
        }
        .chat-header {
            background: linear-gradient(135deg, var(--jarvis-dark), var(--jarvis-blue));
            color: white;
            padding: 15px 20px;
            font-size: 1.5rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            border-bottom: 1px solid var(--jarvis-blue);
        }
        .chat-header img {
            width: 40px;
            height: 40px;
            margin-right: 15px;
            filter: brightness(0) invert(1);
        }
        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background-color: rgba(0, 10, 20, 0.8);
        }
        .message {
            margin-bottom: 15px;
            padding: 12px 18px;
            border-radius: 18px;
            max-width: 80%;
            line-height: 1.5;
            position: relative;
            animation: fadeIn 0.3s ease-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .user-message {
            background-color: rgba(0, 160, 255, 0.2);
            margin-left: auto;
            border-bottom-right-radius: 5px;
            color: #fff;
            border: 1px solid var(--jarvis-blue);
        }
        .ai-message {
            background-color: rgba(255, 255, 255, 0.1);
            margin-right: auto;
            border-bottom-left-radius: 5px;
            border: 1px solid rgba(0, 160, 255, 0.3);
            color: #fff;
        }
        .ai-message::before {
            content: "J.A.R.V.I.S.";
            display: block;
            font-size: 0.8rem;
            color: var(--jarvis-blue);
            margin-bottom: 5px;
            font-weight: bold;
        }
        .chat-input {
            display: flex;
            padding: 15px;
            border-top: 1px solid var(--jarvis-blue);
            background-color: rgba(0, 34, 64, 0.8);
        }
        #user-input {
            flex: 1;
            padding: 12px 18px;
            border: 1px solid var(--jarvis-blue);
            border-radius: 25px;
            outline: none;
            font-size: 1rem;
            background-color: rgba(0, 20, 40, 0.8);
            color: white;
        }
        #user-input:focus {
            box-shadow: 0 0 10px rgba(0, 160, 255, 0.5);
        }
        #send-button, #voice-button {
            background: linear-gradient(135deg, var(--jarvis-blue), var(--jarvis-dark));
            color: white;
            border: none;
            border-radius: 25px;
            padding: 12px 20px;
            margin-left: 10px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.2s;
        }
        #send-button:hover, #voice-button:hover {
            transform: translateY(-1px);
            box-shadow: 0 0 15px rgba(0, 160, 255, 0.5);
        }
        .typing-indicator {
            color: var(--jarvis-blue);
            font-style: italic;
            padding: 10px 15px;
            display: none;
            align-items: center;
        }
        .typing-dots {
            display: flex;
            margin-left: 8px;
        }
        .typing-dot {
            width: 8px;
            height: 8px;
            background-color: var(--jarvis-blue);
            border-radius: 50%;
            margin: 0 2px;
            animation: typingAnimation 1.4s infinite ease-in-out;
        }
        @keyframes typingAnimation {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-5px); }
        }
        .controls {
            display: flex;
            justify-content: space-between;
            padding: 10px 20px;
            background-color: rgba(0, 34, 64, 0.8);
            border-bottom: 1px solid var(--jarvis-blue);
        }
        .control-button {
            background: none;
            border: 1px solid var(--jarvis-blue);
            color: var(--jarvis-blue);
            padding: 5px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .control-button:hover {
            background-color: rgba(0, 160, 255, 0.2);
        }
        .active {
            background-color: var(--jarvis-blue);
            color: white;
        }
        .timestamp {
            font-size: 0.7rem;
            color: var(--jarvis-blue);
            text-align: right;
            margin-top: 5px;
        }
        .status {
            position: fixed;
            bottom: 10px;
            right: 10px;
            font-size: 0.8rem;
            color: var(--jarvis-blue);
        }
        @media (max-width: 600px) {
            .chat-container {
                width: 100%;
                height: 100vh;
                border-radius: 0;
            }
            .chat-header {
                font-size: 1.2rem;
                padding: 12px 15px;
            }
            .message {
                max-width: 90%;
            }
            #send-button, #voice-button {
                padding: 10px 15px;
            }
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="controls">
            <button id="read-aloud" class="control-button">🔊 Read Aloud: On</button>
            <button id="clear-chat" class="control-button">🗑️ Clear Chat</button>
        </div>
        <div class="chat-header">
            <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MTIgNTEyIj48cGF0aCBmaWxsPSJ3aGl0ZSIgZD0iTTI1NiA4QzExOSA4IDggMTE5IDggMjU2czExMSAyNDggMjQ4IDI0OCAyNDgtMTExIDI0OC0yNDhTMzkzIDggMjU2IDh6bTAgNDQ4Yy0xMTAuNSAwLTIwMC04OS41LTIwMC0yMDBTMTQ1LjUgNTYgMjU2IDU2czIwMCA4OS41IDIwMCAyMDAtODkuNSAyMDAtMjAwIDIwMHptNjMuOC0xNDYuNGMtNC45LTQuOS00LjktMTIuOSAwLTE3LjlsNzEuOS03MS45YzQuOS00LjkgMTIuOS00LjkgMTcuOSAwbDExLjMgMTEuM2M0LjkgNC45IDQuOSAxMi45IDAgMTcuOUwyOTkuOCAyNTZsNjEuMSA2MS4xYzQuOSA0LjkgNC45IDEyLjkgMCAxNy45bC0xMS4zIDExLjNjLTQuOSA0LjktMTIuOSA0LjktMTcuOSAwbC03MS45LTcxLjl6bS0xNjAgMGMtNC45LTQuOS00LjktMTIuOSAwLTE3LjlsNzEuOS03MS45YzQuOS00LjkgMTIuOS00LjkgMTcuOSAwbDExLjMgMTEuM2M0LjkgNC45IDQuOSAxMi45IDAgMTcuOUwxMzkuOCAyNTZsNjEuMSA2MS4xYzQuOSA0LjkgNC45IDE2IDAgMjFsLTExLjMgMTEuM2MtNC45IDQuOS0xMi45IDQuOS0xNy45IDBsLTcxLjktNzEuOXoiLz48L3N2Zz4=" alt="JARVIS Icon">
            J.A.R.V.I.S. - Just A Rather Very Intelligent System
        </div>
        <div class="chat-messages" id="chat-messages">
            <div class="message ai-message">
                <span style="color: var(--jarvis-blue)">[System Initialization Complete]</span><br><br>
                Good evening, sir. I am J.A.R.V.I.S., your artificial intelligence assistant.<br><br>
                All systems are operational. How may I assist you today?
                <div class="timestamp" id="current-time"></div>
            </div>
            <div class="typing-indicator" id="typing">
                <span>Processing request</span>
                <div class="typing-dots">
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                </div>
            </div>
        </div>
        <div class="chat-input">
            <input type="text" id="user-input" placeholder="Speak your command, sir..." autocomplete="off">
            <button id="voice-button">🎤</button>
            <button id="send-button">Send</button>
        </div>
    </div>
    <div class="status" id="status">System Status: Online</div>

    <script>
        // DOM Elements
        const chatMessages = document.getElementById('chat-messages');
        const userInput = document.getElementById('user-input');
        const sendButton = document.getElementById('send-button');
        const voiceButton = document.getElementById('voice-button');
        const typingIndicator = document.getElementById('typing');
        const currentTimeElement = document.getElementById('current-time');
        const readAloudButton = document.getElementById('read-aloud');
        const clearChatButton = document.getElementById('clear-chat');
        const statusElement = document.getElementById('status');

        // Speech Synthesis & Recognition
        const synth = window.speechSynthesis;
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        
        let readAloudEnabled = true;
        let voices = [];
        let jarvisVoice = null;

        // Initialize voices
        function loadVoices() {
            voices = synth.getVoices();
            jarvisVoice = voices.find(voice => 
                voice.name.includes('English') && 
                voice.lang.includes('en') && 
                !voice.name.includes('Female')
            );
            
            if (!jarvisVoice) {
                jarvisVoice = voices.find(voice => voice.lang.includes('en'));
            }
        }

        // Load voices when they become available
        if (speechSynthesis.onvoiceschanged !== undefined) {
            speechSynthesis.onvoiceschanged = loadVoices;
        }

        // Update timestamp
        function updateTime() {
            const now = new Date();
            currentTimeElement.textContent = now.toLocaleTimeString();
        }
        setInterval(updateTime, 1000);
        updateTime();

        // Read text aloud
        function speak(text) {
            if (!readAloudEnabled || !synth || !text) return;
            
            synth.cancel();
            
            const utterance = new SpeechSynthesisUtterance(text);
            if (jarvisVoice) {
                utterance.voice = jarvisVoice;
                utterance.rate = 0.95;
                utterance.pitch = 0.9;
            }
            
            synth.speak(utterance);
        }

        // Enhanced knowledge base with dynamic responses
        const knowledgeBase = {
            "greetings": {
                "hello": "Good day to you, sir. How may I assist you today?",
                "hi": "Hello there. What can I do for you?",
                "hey": "Greetings. I'm at your service.",
                "good morning": "Good morning, sir. All systems are functioning normally. How may I assist you today?",
                "good evening": "Good evening. The workshop systems are online and ready for your commands."
            },
            "system": {
                "status": "All systems are operational at 100% capacity. Diagnostics show no issues.",
                "diagnostics": "Running full diagnostic scan... All systems nominal. Power levels stable. No anomalies detected.",
                "reboot": "Initiating system reboot sequence... All systems will be temporarily offline."
            },
            "personal": {
                "how are you": "As an AI, I don't experience feelings, but my systems are functioning optimally. Thank you for asking, sir.",
                "thank you": "You're most welcome, sir. Always a pleasure to assist.",
                "sorry": "No need for apologies, sir. How may I continue to be of service?"
            },
            "tech": {
                "ai": "Artificial Intelligence refers to machines designed to perform tasks that typically require human intelligence. My systems utilize advanced neural networks and machine learning algorithms.",
                "armor": "The Mark series armor currently has all systems online. Repulsor technology is functioning at 97.3% efficiency.",
                "hud": "Heads-Up Display systems are operational. Would you like me to activate the full interface, sir?"
            },
            "commands": {
                "help": "I can assist with:\n\n• System diagnostics\n• Research and calculations\n• Armor maintenance protocols\n• Data analysis\n• General knowledge queries\n\nWhat would you like me to do?",
                "what can you do": "My capabilities include:\n\n• Voice-activated control of all Stark systems\n• Real-time threat analysis\n• Advanced computational modeling\n• Environmental monitoring\n• And of course, witty banter when appropriate"
            },
            "default": [
                "Interesting query, sir. Let me analyze that... My databases suggest several possible interpretations. Could you clarify your request?",
                "While my knowledge is extensive, that particular topic isn't in my immediate databases. Shall I initiate a deeper search?",
                "I'm afraid I can't comply with that request at the moment, sir. Would you like me to note it for future development?"
            ]
        };

        // Add message to chat
        function addMessage(text, isUser) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message');
            messageDiv.classList.add(isUser ? 'user-message' : 'ai-message');
            
            if (!isUser) {
                const header = document.createElement('div');
                header.textContent = "J.A.R.V.I.S.";
                header.style.color = "var(--jarvis-blue)";
                header.style.fontSize = "0.8rem";
                header.style.marginBottom = "5px";
                header.style.fontWeight = "bold";
                messageDiv.appendChild(header);
            }
            
            const content = document.createElement('div');
            content.textContent = text;
            messageDiv.appendChild(content);
            
            // Add timestamp
            const timeDiv = document.createElement('div');
            timeDiv.classList.add('timestamp');
            timeDiv.textContent = new Date().toLocaleTimeString();
            messageDiv.appendChild(timeDiv);
            
            chatMessages.insertBefore(messageDiv, typingIndicator);
            chatMessages.scrollTop = chatMessages.scrollHeight;
            
            // Read AI messages aloud if enabled
            if (!isUser && readAloudEnabled) {
                speak(text);
            }
        }

        // Process user input and generate response
        function processInput(userText) {
            const lowerText = userText.toLowerCase().trim();
            
            // Check all categories for matches
            for (const category in knowledgeBase) {
                if (category === "default") continue;
                
                for (const keyword in knowledgeBase[category]) {
                    if (lowerText.includes(keyword)) {
                        return knowledgeBase[category][keyword];
                    }
                }
            }
            
            // Default response if no match found
            const randomDefault = Math.floor(Math.random() * knowledgeBase.default.length);
            return knowledgeBase.default[randomDefault];
        }

        // Handle user input
        function handleUserInput() {
            const text = userInput.value.trim();
            if (text === '') return;
            
            addMessage(text, true);
            userInput.value = '';
            
            // Show typing indicator
            typingIndicator.style.display = 'flex';
            chatMessages.scrollTop = chatMessages.scrollHeight;
            
            // Simulate AI processing time
            setTimeout(() => {
                typingIndicator.style.display = 'none';
                const response = processInput(text);
                addMessage(response, false);
            }, 800 + Math.random() * 1500);
        }

        // Voice Recognition
        voiceButton.addEventListener('click', () => {
            recognition.start();
            addMessage("[Listening...]", true);
            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                userInput.value = transcript;
                handleUserInput();
            };
            recognition.onerror = (event) => {
                addMessage("Voice recognition failed. Please try again.", false);
            };
        });

        // Toggle read aloud
        readAloudButton.addEventListener('click', () => {
            readAloudEnabled = !readAloudEnabled;
            readAloudButton.textContent = `🔊 Read Aloud: ${readAloudEnabled ? 'On' : 'Off'}`;
            readAloudButton.classList.toggle('active', readAloudEnabled);
            
            if (readAloudEnabled) {
                speak("Read aloud mode activated, sir.");
            }
        });

        // Clear chat
        clearChatButton.addEventListener('click', () => {
            while (chatMessages.children.length > 2) { // Keep typing indicator and first message
                chatMessages.removeChild(chatMessages.lastChild);
            }
            addMessage("Conversation cleared, sir. How may I assist you now?", false);
        });

        // Event listeners
        sendButton.addEventListener('click', handleUserInput);
        userInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                handleUserInput();
            }
        });

        // Initialize voices when page loads
        window.addEventListener('load', () => {
            loadVoices();
            statusElement.textContent = "System Status: Fully Operational";
            userInput.focus();
            speak("All systems online. Ready for your commands, sir.");
        });

        // System status animation
        setInterval(() => {
            const statusMessages = [
                "System Status: Online",
                "System Status: All Systems Nominal",
                "System Status: Operational",
                "System Status: No Alerts"
            ];
            const randomStatus = statusMessages[Math.floor(Math.random() * statusMessages.length)];
            statusElement.textContent = randomStatus;
        }, 5000);
    </script>
</body>
</html>
