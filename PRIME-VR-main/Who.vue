<template>
    <div class="signin-background">
      <div class="signin-container">
        <h1 class="title">Who Are You</h1>
        <p class="subtitle">Choose your role</p>
  
        <!-- Two Clickable Round Boxes -->
        <div class="role-container">
          <div
            class="role-box"
            :class="{ 'selected': selectedRole === 'player' }"
            @click="selectRole('player')"
          >
            <span v-if="selectedRole === 'player'" class="role-image">
              <!-- Add the image when "PLAYER" is selected -->
              <img src="./player.png" alt="Player" />
            </span>
          </div>
  
          <div
            class="role-box"
            :class="{ 'selected': selectedRole === 'coach' }"
            @click="selectRole('coach')"
          >
            <span v-if="selectedRole === 'coach'" class="role-image">
              <!-- Add the image when "COACH" is selected -->
              <img src="./player.png" alt="Coach" />
            </span>
          </div>
        </div>
  
        <!-- Next Button -->
        <RouterLink to="/team" class="login-button">NEXT</RouterLink>
  
        <!-- Back Text -->
        <RouterLink class="create-account" to="/create">BACK</RouterLink>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedRole: null, // Tracks the selected role (player or coach)
      };
    },
    methods: {
      selectRole(role) {
        if (this.selectedRole === role) {
          this.selectedRole = null; // Deselect if the same box is clicked again
        } else {
          this.selectedRole = role; // Select the new role
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Your existing styles remain the same */
  
  html, body {
    background: linear-gradient(185deg, rgb(34, 32, 32) 80%, red 80%);
    height: 100%;
    margin: 0;
    padding: 0;
    height: 100vh;
  }
  
  .signin-background {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .signin-container {
    font-family: 'Roboto';
    display: flex;
    flex-direction: column;
    justify-content: flex-start; 
    align-items: center;
    width: 100%;
    max-width: 750px; 
    text-align: center;
    padding: 20px 20px;
    box-sizing: border-box;
    height: 100%;
    max-height: 100vh; 
    margin-top: 2%; 
  }
  
  .title {
    font-size: 2rem;
    font-weight: 700;
    color: white;
    margin-top: 0;
  }
  
  .subtitle {
    font-size: 0.9rem;
    font-weight: 400;
    color: white;
    margin-top: 5px;
  }
  
  /* Login Button */
  .login-button {
    width: 100%;
    max-width: 700px;
    height: 50px;
    background-color: #FF5448;
    color: white;
    font-size: 1.2rem;
    font-weight: 700;
    border: none;
    border-radius: 13px;
    margin-top: 85px;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    text-decoration: none;
  }
  
  .login-button:hover {
    background-color: #FF3B32;
  }
  
  .router-link-active {
    text-decoration: none;
  }
  
  /* Create Account Text */
  .create-account {
    font-size: 1.6rem;
    color: white;
    font-weight: 500;
    text-decoration: none;
    margin-top: 25px;
    transition: all 0.3s ease;
  }
  
  .create-account:hover {
    transform: scale(1.05);
  }
  
  /* Styles for the role selection boxes */
  .role-container {
    display: flex;
    justify-content: space-evenly;
    width: 150%;
    margin-top: 30px;
  }
  
  .role-box {
    width: 220px;
    height: 220px;
    border: 3px solid white;
    border-radius: 10%;
    background-color: transparent;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    text-align: center;
    transition: all 0.3s ease;
    position: relative;
  }
  
  .role-box.selected {
    border-color: #FF5448; /* Change border to FF5448 when selected */
  }
  
  .role-box:hover {
    border-color: #FF3B32; /* Change border color on hover */
  }
  
  /* Styles for the image inside the box */
  .role-image {
  position: absolute;
  top: 30px; /* Slightly adjust the top value */
  width: 150px;
  height: 150px;
  border-radius: 50%;
  overflow: visible; /* Allow the image to overflow and prevent clipping */
}

.role-image img {
  width: 100%;
  height: 100%;
  object-fit: contain; /* Ensure the image covers the circle */
}

  
  /* Using ::after to add text below the boxes */
  .role-box:nth-child(1)::after {
    content: 'PLAYER';
    font-size: 1.5rem;
    font-weight: bold;
    color: white;
    position: absolute;
    bottom: -60px;  /* Moves the text below the box */
    left: 50%;
    transform: translateX(-50%);
  }
  
  .role-box:nth-child(2)::after {
    content: 'COACH';
    font-size: 1.5rem;
    font-weight: bold;
    color: white;
    position: absolute;
    bottom: -60px;  /* Moves the text below the box */
    left: 50%;
    transform: translateX(-50%);
  }
  </style>
  