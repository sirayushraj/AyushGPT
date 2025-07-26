<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gemini Chat</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            color: #ececf1;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            padding: 20px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            width: 100%;
            display: flex;
            flex-direction: column;
            height: calc(100vh - 40px);
            background: #343541;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background: #444654;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo h1 {
            font-size: 22px;
            font-weight: 600;
            background: linear-gradient(90deg, #10a37f, #1a73e8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .chat-history {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .message {
            display: flex;
            gap: 15px;
            padding: 15px;
            border-radius: 8px;
        }

        .user-message {
            background: #343541;
        }

        .bot-message {
            background: #444654;
        }

        .avatar {
            width: 36px;
            height: 36px;
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            background: #5436da;
        }

        .bot-avatar {
            background: #10a37f;
        }

        .avatar div {
            font-size: 18px;
            font-weight: bold;
        }

        .message-content {
            flex: 1;
            padding-top: 3px;
        }

        .message-content p {
            line-height: 1.6;
            font-size: 16px;
        }

        .typing-indicator {
            display: flex;
            align-items: center;
            gap: 5px;
            padding: 15px;
            background: #444654;
            border-radius: 8px;
        }

        .typing-indicator span {
            width: 8px;
            height: 8px;
            background: #10a37f;
            border-radius: 50%;
            display: inline-block;
            animation: bounce 1.5s infinite;
        }

        .typing-indicator span:nth-child(2) {
            animation-delay: 0.2s;
        }

        .typing-indicator span:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }

        .input-container {
            padding: 20px;
            background: #343541;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .input-box {
            display: flex;
            gap: 10px;
        }

        .input-box textarea {
            flex: 1;
            padding: 15px;
            border-radius: 8px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            background: #40414f;
            color: #ececf1;
            resize: none;
            height: 60px;
            font-size: 16px;
            outline: none;
        }

        .input-box button {
            width: 50px;
            height: 50px;
            border-radius: 8px;
            background: #10a37f;
            color: white;
            border: none;
            cursor: pointer;
            align-self: flex-end;
        }

        .info-text {
            text-align: center;
            padding: 40px 20px;
            color: rgba(255, 255, 255, 0.7);
        }

        .info-text h2 {
            margin-bottom: 15px;
            font-size: 24px;
        }

        .info-text p {
            max-width: 600px;
            margin: 0 auto;
            line-height: 1.6;
        }

        footer {
            text-align: center;
            padding: 15px 0;
            color: rgba(255, 255, 255, 0.6);
            font-size: 14px;
        }

        .features {
            display: flex;
            gap: 20px;
            margin-top: 30px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .feature-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            padding: 20px;
            width: 180px;
            text-align: center;
            transition: transform 0.3s ease;
        }

        .feature-card:hover {
            transform: translateY(-5px);
            background: rgba(16, 163, 127, 0.1);
        }

        .feature-card h3 {
            color: #10a37f;
            margin: 10px 0;
        }

        @media (max-width: 768px) {
            body {
                padding: 10px;
            }
            
            header {
                flex-direction: column;
                gap: 15px;
            }
            
            .features {
                flex-direction: column;
                align-items: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <h1>Gemini Chat Interface</h1>
            </div>
        </header>

        <div class="chat-history" id="chat-history">
            <div class="info-text">
                <h2>Welcome to Gemini Chat</h2>
                <p>Ask anything - I can help with explanations, ideas, writing, coding, and more!</p>
                
                <div class="features">
                    <div class="feature-card">
                        <h3>💬 Conversations</h3>
                        <p>Natural dialogue</p>
                    </div>
                    <div class="feature-card">
                        <h3>📝 Writing</h3>
                        <p>Content creation</p>
                    </div>
                    <div class="feature-card">
                        <h3>🔍 Research</h3>
                        <p>Information lookup</p>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="input-container">
            <div class="input-box">
                <textarea id="user-input" placeholder="Message Gemini..."></textarea>
                <button id="send-btn">➤</button>
            </div>
        </div>
        
        <footer>
            <p>Gemini Chat Interface | Powered by Google Gemini API</p>
        </footer>
    </div>

    <script>
        // DOM Elements
        const chatHistory = document.getElementById('chat-history');
        const userInput = document.getElementById('user-input');
        const sendBtn = document.getElementById('send-btn');
        
        // Send message function
        async function sendMessage() {
            const message = userInput.value.trim();
            if (!message) return;
            
            // Add user message to chat
            addMessageToChat(message, 'user');
            userInput.value = '';
            
            // Show typing indicator
            const typingIndicator = showTypingIndicator();
            
            try {
                // Call your backend API (replace with your actual endpoint)
                const response = await fetch('/api/gemini', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ prompt: message })
                });
                
                if (!response.ok) {
                    throw new Error(`API error: ${response.status}`);
                }
                
                const data = await response.json();
                
                // Remove typing indicator
                chatHistory.removeChild(typingIndicator);
                
                // Add bot response to chat
                addMessageToChat(data.response, 'bot');
            } catch (error) {
                // Remove typing indicator
                chatHistory.removeChild(typingIndicator);
                
                // Show error message
                addMessageToChat(`Error: ${error.message}`, 'bot');
            }
        }
        
        // Add message to chat history
        function addMessageToChat(content, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message');
            messageDiv.classList.add(sender === 'user' ? 'user-message' : 'bot-message');
            
            const avatarDiv = document.createElement('div');
            avatarDiv.classList.add('avatar');
            if (sender === 'bot') avatarDiv.classList.add('bot-avatar');
            
            const avatarText = document.createElement('div');
            avatarText.textContent = sender === 'user' ? 'U' : 'G';
            avatarDiv.appendChild(avatarText);
            
            const contentDiv = document.createElement('div');
            contentDiv.classList.add('message-content');
            
            const paragraph = document.createElement('p');
            paragraph.textContent = content;
            contentDiv.appendChild(paragraph);
            
            messageDiv.appendChild(avatarDiv);
            messageDiv.appendChild(contentDiv);
            
            chatHistory.appendChild(messageDiv);
            
            // Scroll to bottom
            chatHistory.scrollTop = chatHistory.scrollHeight;
        }
        
        // Show typing indicator
        function showTypingIndicator() {
            const typingDiv = document.createElement('div');
            typingDiv.classList.add('typing-indicator');
            
            const span1 = document.createElement('span');
            const span2 = document.createElement('span');
            const span3 = document.createElement('span');
            
            typingDiv.appendChild(span1);
            typingDiv.appendChild(span2);
            typingDiv.appendChild(span3);
            
            chatHistory.appendChild(typingDiv);
            chatHistory.scrollTop = chatHistory.scrollHeight;
            
            return typingDiv;
        }
        
        // Event listeners
        sendBtn.addEventListener('click', sendMessage);
        
        userInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        });
        
        // Initial focus on input
        userInput.focus();
    </script>
</body>
</html>
