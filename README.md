# Chat-Application-
Server-side (server.js)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  console.log('Client connected');

  ws.on('message', (message) => {
    console.log(`Received message: ${message}`);
    wss.clients.forEach((client) => {
      if (client !== ws && client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });

  ws.on('close', () => {
    console.log('Client disconnected');
  });
});

Client-side (index.html)
<!DOCTYPE html>
<html>
<head>
  <title>Chat Application</title>
  <script>
    const socket = new WebSocket('ws:                   

    socket.onmessage = (event) => {
      console.log(`Received message: ${event.data}`);
      document.write(`<p>${event.data}</p>`);
    };

    document.addEventListener('DOMContentLoaded', () => {
      const form = document.getElementById('form');
      form.addEventListener('submit', (event) => {
        event.preventDefault();
        const message = document.getElementById('message').value;
        socket.send(message);
        document.getElementById('message').value = '';
      });
    });
  </script>
</head>
<body>
  <form id="form">
    <input id="message" type="text" placeholder="Type a message...">
    <button>Send</button>
  </form>
</body>
</html>
