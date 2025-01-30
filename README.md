<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bush Video Platform</title>
  <style>
    /* Default Theme */
    body.default-theme {
      background-color: #f4f4f4;
      color: #333;
    }

    /* Dark Theme */
    body.dark-theme {
      background-color: #181818;
      color: #f4f4f4;
    }

    /* White Theme */
    body.white-theme {
      background-color: #ffffff;
      color: #000000;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
    }

    header {
      background-color: #333;
      color: white;
      padding: 1em;
      text-align: center;
    }

    main {
      padding: 20px;
    }

    /* Video Area */
    .video-area {
      width: 100%;
      height: 400px;
      background-color: #000;
      margin-bottom: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      position: relative;
    }

    .content-section {
      display: none;
    }

    .active-section {
      display: block;
    }

    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      background-color: #333;
      text-align: center;
      padding: 10px 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .bottom-nav a {
      color: white;
      text-decoration: none;
      padding: 10px;
      display: inline-block;
    }

    .bottom-nav a:hover {
      background-color: #555;
    }

    /* Circular App Box Layout */
    .app-boxes {
      display: flex;
      justify-content: center; /* Center the circular boxes */
      margin-top: 30px; /* Adjust margin to space out from the video area */
    }

    .app-boxes div {
      width: 120px;
      height: 120px;
      background-color: #f4f4f4;
      border-radius: 50%; /* Makes the boxes circular */
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s;
      font-size: 18px;
      text-align: center;
      margin: 0 10px; /* Adds space between the circles */
    }

    .app-boxes div:hover {
      background-color: #ddd;
    }

    .app-boxes span {
      color: #333;
    }

    /* Progress Bar */
    .progress-bar {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 5px;
      background-color: rgba(255, 255, 255, 0.5);
    }

    .progress-bar-inner {
      height: 100%;
      background-color: #ff6347;
      width: 0%;
    }

    .fullscreen-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      background-color: #333;
      color: white;
      border: none;
      padding: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body class="default-theme">
  <header>
    <h1>Bush Video Platform</h1>
  </header>

  <main>
    <!-- Video Area for Playing App Videos -->
    <div class="video-area" id="video-area">
      <!-- Placeholder for video iframe, will be dynamically changed -->
      <button class="fullscreen-btn" onclick="toggleFullScreen()">Go Fullscreen</button>
      <p>Loading video...</p>
      <div class="progress-bar">
        <div class="progress-bar-inner" id="progress-bar-inner"></div>
      </div>
    </div>

    <!-- App Boxes for TikTok, Instagram, Twitter, YouTube -->
    <div class="app-boxes">
      <div id="tiktok" onclick="loadVideo('TikTok')">
        <span>TikTok</span>
      </div>
      <div id="instagram" onclick="loadVideo('Instagram')">
        <span>Instagram</span>
      </div>
      <div id="twitter" onclick="loadVideo('Twitter')">
        <span>Twitter</span>
      </div>
      <div id="youtube" onclick="loadVideo('YouTube')">
        <span>YouTube</span>
      </div>
    </div>
  </main>

  <!-- Bottom Navigation Bar -->
  <div class="bottom-nav">
    <a href="javascript:void(0)" onclick="showSection('home')">Home</a>
    <a href="javascript:void(0)" onclick="showSection('live')">Live</a>
    <a href="javascript:void(0)" onclick="showSection('account')">Account</a>
    <a href="javascript:void(0)" onclick="showSection('settings')">Settings</a>
    <a href="javascript:void(0)" class="message-btn" onclick="showMessages()">Messages</a>
  </div>

  <script>
    let videoInterval;

    // Function to toggle themes
    function toggleTheme() {
      const body = document.body;
      if (body.classList.contains('default-theme')) {
        body.classList.remove('default-theme');
        body.classList.add('dark-theme');
      } else if (body.classList.contains('dark-theme')) {
        body.classList.remove('dark-theme');
        body.classList.add('white-theme');
      } else {
        body.classList.remove('white-theme');
        body.classList.add('default-theme');
      }
    }

    // Function to handle navigation
    function showSection(sectionId) {
      const sections = document.querySelectorAll('.content-section');
      sections.forEach(section => {
        section.classList.toggle('active-section', section.id === sectionId);
      });
    }

    // Function for the messages button
    function showMessages() {
      alert("Messages feature coming soon!");
    }

    // Function to load a random video from multiple platforms
    function loadRandomVideo() {
      const videoArea = document.getElementById('video-area');
      const platforms = ['TikTok', 'Instagram', 'Twitter', 'YouTube'];
      const randomPlatform = platforms[Math.floor(Math.random() * platforms.length)];
      loadVideo(randomPlatform); // Load random platform video
    }

    // Function to load a video from a specific platform
    function loadVideo(platform) {
      const videoArea = document.getElementById('video-area');
      const videoUrls = {
        TikTok: 'https://www.tiktok.com/embed/12345',
        Instagram: 'https://www.instagram.com/reel/12345/embed/',
        Twitter: 'https://twitter.com/i/videos/12345',
        YouTube: 'https://www.youtube.com/embed/12345'
      };

      // Set the video in the video area based on the platform clicked
      videoArea.innerHTML = `
        <iframe src="${videoUrls[platform]}" width="100%" height="400" allowfullscreen></iframe>
        <button class="fullscreen-btn" onclick="toggleFullScreen()">Go Fullscreen</button>
        <div class="progress-bar">
          <div class="progress-bar-inner" id="progress-bar-inner"></div>
        </div>
      `;

      startVideoProgress();
    }

    // Function to toggle fullscreen mode
    function toggleFullScreen() {
      const videoArea = document.getElementById('video-area');
      if (videoArea.requestFullscreen) {
        videoArea.requestFullscreen();
      } else if (videoArea.mozRequestFullScreen) { // Firefox
        videoArea.mozRequestFullScreen();
      } else if (videoArea.webkitRequestFullscreen) { // Chrome, Safari and Opera
        videoArea.webkitRequestFullscreen();
      } else if (videoArea.msRequestFullscreen) { // IE/Edge
        videoArea.msRequestFullscreen();
      }
    }

    // Function to simulate video progress (for demonstration)
    function startVideoProgress() {
      let progress = 0;
      const progressBar = document.getElementById('progress-bar-inner');

      clearInterval(videoInterval); // Clear any existing interval
      videoInterval = setInterval(() => {
        if (progress < 100) {
          progress += 0.5; // Update progress
          progressBar.style.width = progress + '%';
        } else {
          clearInterval(videoInterval); // Stop when progress reaches 100%
        }
      }, 1000); // Update every second
    }

    // Load a random video when the page loads
    window.onload = function() {
      loadRandomVideo(); // Load a random video from a platform when the page loads
    }
  </script>
</body>
</html>
