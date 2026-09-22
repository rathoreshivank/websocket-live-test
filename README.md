# WebSocket Live Test Rig

A small real-time messaging demo built with Node.js, the `ws` WebSocket library, and a browser client.

The browser connects to a WebSocket server running on `localhost:8080`. Messages sent from the browser are broadcast to every connected client.

## Features

- WebSocket server built with Node.js and `ws`
- Browser WebSocket client
- Real-time message broadcasting
- Connection status display
- Sent, received, and system event logging
- Client error and disconnect handling
- Node watch mode for development

## Project Structure

```text
.
|-- index.html    Browser client interface
|-- server.js     Node.js WebSocket server
|-- package.json  Project configuration and scripts
`-- README.md     Project documentation
```

## Requirements

- Node.js installed
- npm installed

## Installation

Install the project dependencies:

```powershell
npm install
```

The main dependency is the [`ws`](https://www.npmjs.com/package/ws) package.

## Run the Server

Start the server with:

```powershell
node server.js
```

For development, use the watch script:

```powershell
npm run dev
```

The server listens at:

```text
ws://localhost:8080
```

Keep the terminal running while using the browser client.

## Run the Browser Client

Open `index.html` in a browser after starting the server.

When the connection succeeds, the page displays:

```text
CONNECTED: ws://localhost:8080
```

Enter a message and select **Deploy Message**. The server broadcasts the message to all currently connected WebSocket clients.

## How It Works

1. `server.js` creates a WebSocket server on port `8080`.
2. The browser creates a WebSocket connection to `ws://localhost:8080`.
3. The server handles each new client through the `connection` event.
4. The browser sends a message through the form.
5. The server receives the message through the `message` event.
6. The server sends the message to every open client.
7. The browser displays the broadcast in the live log.

## Important WebSocket Events

### Server-side events

```js
wss.on("connection", (socket, request) => {})
socket.on("message", (data) => {})
socket.on("error", (error) => {})
socket.on("close", () => {})
```

### Browser-side events

```js
socket.addEventListener("open", () => {})
socket.addEventListener("message", (event) => {})
socket.addEventListener("close", () => {})
```

The Node.js `ws` package uses `.on(...)`, while the browser WebSocket API supports event listeners such as `.addEventListener(...)` and properties such as `.onopen`.

## Troubleshooting

### `EADDRINUSE: address already in use :::8080`

Another process is already using port `8080`. This usually means the server is already running. Close the old Node process or use the existing server instead of starting a second one.

### Browser connection fails

Check that:

- The server is running.
- The browser is connecting to `ws://localhost:8080`.
- The browser page is allowed to make local WebSocket connections.
- A VPN, proxy, extension, or browser security policy is not blocking the connection.

You can test the server with a Node client:

```powershell
node -e "import('ws').then(({default: WebSocket}) => { const socket = new WebSocket('ws://localhost:8080'); socket.on('open', () => { console.log('CONNECTED'); socket.close(); }); socket.on('error', error => console.error(error.message)); })"
```

## Learning Outcomes

This project demonstrates:

- WebSocket handshakes and persistent connections
- Event-driven Node.js programming
- Browser-to-server real-time communication
- Broadcasting messages to multiple clients
- Handling connection, message, error, and close events
- Using `event.preventDefault()` to stop a form reload
- Checking `WebSocket.OPEN` before sending data
- Diagnosing connection and port errors
