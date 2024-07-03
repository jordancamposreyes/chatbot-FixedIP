<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot de Soporte</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chat-container {
            width: 400px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 5px;
            overflow: hidden;
        }
        .chat-header {
            background-color: #007bff;
            color: white;
            padding: 10px;
            text-align: center;
        }
        .chat-body {
            padding: 10px;
            background-color: white;
            height: 400px;
            overflow-y: scroll;
        }
        .chat-footer {
            padding: 10px;
            background-color: #f4f4f4;
            text-align: center;
        }
        input[type="text"] {
            width: 80%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            padding: 10px;
            border: none;
            background-color: #007bff;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            Chatbot de Soporte
        </div>
        <div class="chat-body" id="chat-body">
            <!-- Mensajes del chat irán aquí -->
        </div>
        <div class="chat-footer">
            <input type="text" id="user-input" placeholder="Escribe tu consulta...">
            <button onclick="sendMessage()">Enviar</button>
        </div>
    </div>

    <script>
        const chatBody = document.getElementById('chat-body');

        function sendMessage() {
            const userInput = document.getElementById('user-input').value;
            if (userInput.trim() !== '') {
                appendMessage('user', userInput);
                document.getElementById('user-input').value = '';
                getResponse(userInput);
            }
        }

        function appendMessage(sender, message) {
            const messageElement = document.createElement('div');
            messageElement.className = sender;
            messageElement.innerText = message;
            chatBody.appendChild(messageElement);
            chatBody.scrollTop = chatBody.scrollHeight;
        }

        function getResponse(message) {
            const accessToken = 'jordancamposreyes'; // Reemplaza con tu token de acceso de Dialogflow

            fetch(`https://api.dialogflow.com/v1/query?v=20150910&query=${message}&lang=es&sessionId=12345`, {
                method: 'GET',
                headers: {
                    'Authorization': `Bearer ${accessToken}`
                }
            })
            .then(response => response.json())
            .then(data => {
                const reply = data.result.fulfillment.speech;
                appendMessage('bot', reply);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    </script>
</body>
</html>
