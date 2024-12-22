Real-Time Location Tracking Application

This application demonstrates a real-time location tracking system using Node.js, Socket.IO, and Leaflet.js. Users' geolocation data is tracked and displayed on a map in real time, and location updates are dynamically rendered for connected users.

Features

Real-time location tracking using Geolocation API.

Visualization of user locations on a map with Leaflet.js.

Dynamic addition and removal of user markers.

Broadcast user locations using Socket.IO.

Prerequisites

Ensure you have the following installed on your system:

Node.js (v14 or later)

npm (Node Package Manager)

Installation

Clone this repository:

git clone <repository_url>

Navigate to the project directory:

cd <project_directory>

Install dependencies:

npm install

Project Structure

.
├── app.js                 # Main application entry point
├── public/                # Static assets
│   ├── css/
│   │   └── style.css      # Styles for the application
│   └── js/
│       └── script.js      # Frontend logic for geolocation and map updates
├── views/
│   └── index.ejs          # HTML template rendered by Express
└── package.json           # Project metadata and dependencies

Usage

Running the Application

Start the server:

node app.js

Open your browser and navigate to:

http://localhost:5500

Interacting with the Application

Upon opening the application, your location will be tracked (with your permission) using the Geolocation API.

Your marker will be displayed on the map.

If other users connect, their locations will also appear on the map in real time.

Disconnecting a user removes their marker from the map.

Code Overview

Backend (app.js)

Server Setup: The application uses Express to serve static files and render the main page.

Socket.IO Integration: Handles real-time communication between clients. Each user's location is broadcast to all connected clients.

Key Code Snippets:

io.on("connection", function (socket) {
    socket.on("send-location", function (data) {
        io.emit("recive-location", { id: socket.id, ...data });
    });
    socket.on("disconnect", function () {
        io.emit("user-disconnected", socket.id);
    });
});

Frontend (script.js)

Geolocation Tracking: Uses navigator.geolocation.watchPosition to continuously track the user's location and send it to the server.

Map Updates: Integrates Leaflet.js to display and update user markers dynamically.

Key Code Snippets:

navigator.geolocation.watchPosition((position) => {
    const { latitude, longitude } = position.coords;
    socket.emit("send-location", { latitude, longitude });
}, (error) => {
    console.error(error);
}, {
    enableHighAccuracy: true,
    timeout: 5000,
    maximumAge: 0
});

socket.on("recive-location", (data) => {
    const { id, latitude, longitude } = data;
    map.setView([latitude, longitude]);
    if (markers[id]) {
        markers[id].setLatLng([latitude, longitude]);
    } else {
        markers[id] = L.marker([latitude, longitude]).addTo(map);
    }
});

Views (index.ejs)

Template Rendering: Defines the structure of the HTML page and includes stylesheets and JavaScript files.

Key Code Snippets:

<div id="map"></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
<script src="https://cdn.socket.io/4.7.5/socket.io.min.js"></script>
<script src="/js/script.js"></script>

Dependencies

Express: For serving the application and handling routes.

Socket.IO: For real-time bidirectional communication.

Leaflet.js: For rendering interactive maps.

Security Considerations

Geolocation Permissions: The application requests location permissions from the user. Ensure you handle this data responsibly and do not store it persistently.

CORS: Configure properly if deploying to a production environment.

License

This project is licensed under the MIT License.

Acknowledgements

Leaflet.js for its robust mapping capabilities.

Socket.IO for seamless real-time communication.

Feel free to extend or customize the application as needed!
Oshing Rana Magar
