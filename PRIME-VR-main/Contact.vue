<template>
  <main>
    <div class="contact-box-wrapper">
      <!-- Rectangle Box Behind the Contact Box -->
      <div class="contact-box-rectangle">
        <div class="vertical-image-container">
          <div class="vertical-image-item">
            <img src="../views/FacebookLogo.png" alt="Facebook" class="vertical-image" />
          </div>
          <div class="vertical-image-item">
            <img src="../views/InstaLogo.png" alt="Instagram" class="vertical-image" />
          </div>
          <div class="vertical-image-item">
            <img src="../views/TikTokLogo.png" alt="TikTok" class="vertical-image" />
          </div>
        </div>
      </div>

      <div class="contact-box">
        <div class="contact-content">
          <h1 class="contact-title">Contact Us</h1>
          <p class="contact-text">
            Have any questions or concerns regarding PRIME-VR? How can we help?
          </p>
          <input class="input-field" type="text" placeholder="Name" id="name" />
          <input class="input-field" type="email" placeholder="Email" id="email" />
          <textarea class="input-field message-box" placeholder="Message" id="message"></textarea>

          <!-- Send Button -->
          <button class="send-button" id="sendButton">SEND MESSAGE</button>

          <!-- Success message (hidden by default) -->
          <p id="successMessage" style="display: none; color: green;">Message Sent Successfully!</p>
        </div>

        <!-- Images container on the right -->
        <div class="image-container">
          <div class="image-item">
            <img src="../views/LocationLogo.png" alt="Location" class="image" />
            <span class="image-text">300 Boston Post Rd </span>
          </div>
          <div class="image-item">
            <img src="../views/PhoneLogo.png" alt="Phone" class="image" />
            <span class="image-text">203 - 171 - 8000</span>
          </div>
          <div class="image-item">
            <img src="../views/EmailLogo.png" alt="Email" class="image" />
            <span class="image-text">mmekni@newhaven.edu</span>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>



<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Merriweather+Sans:ital,wght@0,300..800;1,300..800&display=swap');

body {
  background: linear-gradient(175deg, rgb(34, 32, 32) 80%, red 100%);
  margin: 0;
  padding: 0;
  font-family: 'Merriweather Sans', sans-serif;
  color: white;
  min-height: 100vh;
  font-weight: 700;
  position: relative;
}

/* Main container */
main {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 80px 0;
  position: relative;
}


.contact-box-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

/* Box Behind Contact */
.contact-box-rectangle {
  position: absolute;
  width: 95px;
  height: 215px;
  border-radius: 30px;
  background-color: #FF5448;
  bottom: 15px; 
  right: 80px; 
  z-index: 0; 
}

/* image inside orange rectangle */
.vertical-image-container {
  display: flex;
  flex-direction: column;
  gap: 15px; 
  position: absolute;
  top: 50%; 
  left: 65%;  
  transform: translate(-50%, -50%); 
}


.vertical-image-item {
  display: flex;
  justify-content: center;
  align-items: center;
}


.vertical-image {
  width: 40px;  
  height: 40px;
  object-fit: contain;
  border-radius: 10px;
 
}

/* Main contact box */
.contact-box {
  background-color: #DADADA;
  border-radius: 30px;
  margin-top: -50px;
  width: 90%;
  max-width: 1200px;
  height: 550px; 
  padding: 50px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden; /* circle being cut off */
  display: block;
  z-index: 1; 
}

/* large circle in top-right */
.contact-box::before {
  content: "";
  position: absolute;
  top: -160px;  
  right: -160px;  
  width: 400px;
  height: 400px;
  background-color: #FF5448;
  border-radius: 50%;
  opacity: 0.13;
  z-index: 1;
}

/* smaller circle in top-right */
.contact-box::after {
  content: "";
  position: absolute;
  top: 80px;  
  right: 80px;  
  width: 200px;  
  height: 200px;  
  background-color: #FF5448;
  border-radius: 50%;
  opacity: 0.13;
  z-index: 2; 
}

/* content aligned to the left */
.contact-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 550px;
  z-index: 3; 
}

.contact-title {
  font-size: 2.5rem;
  margin: 0;
  color: #FF5448;
  text-align: left;
  font-weight: 700;
}


.contact-text {
  font-size: 1rem;
  line-height: 1.6;
  color: #FF5448;
  text-align: left;
  max-width: none;
  width: 100%;
  font-weight: 400;
}


.input-field {
  padding: 14px;
  border: none;
  border-radius: 12px;
  background-color: rgba(255, 84, 72, 0.17); 
  font-size: 1rem;
  color: #000;
  outline: none;
  font-weight: 700;
}

.input-field::placeholder {
  color: rgba(0, 0, 0, 0.33); 
}

.message-box {
  height: 120px;
  resize: none;
}


.send-button {
  background-color: #FF5448;
  color: white;
  border: none;
  padding: 14px 24px;
  font-size: 1rem;
  font-family: 'Merriweather Sans', sans-serif;
  border-radius: 12px;
  cursor: pointer;
  align-self: flex-start;
  font-weight: 700;
  width: 550px;
}


.send-button:hover {
  background-color: #e14c40;
}


.image-container {
  position: absolute;
  margin-top: 200px;
  bottom: 180px; 
  right: 180px;  
  display: flex;
  flex-direction: column;
  gap: 15px;  
  z-index: 3;
  max-width: 550px;  
  width: auto;
}


.image-item {
  display: flex;
  align-items: center;
  gap: 20px; 
  width: 100%;  
  justify-content: flex-start; 
}


.image {
  width: 45px; 
  height: 45px;
  object-fit: cover;
  border-radius: 5px;  
}


.image-text {
  font-size: 12px;
  color: #968381;
  font-weight: 200;
  max-width: 1200px;
  width: 90%;
  flex-grow: 3; 
  white-space: nowrap;  
}
</style>
