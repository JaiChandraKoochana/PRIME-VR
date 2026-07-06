<template>
  <div class="signin-background">
    <div class="signin-container">
      <h1 class="title">Create Account</h1>
      <p class="subtitle">Create account using email.</p>

      <!-- Two images below the subtitle -->
      <div class="images-container">
        <img src="./googleIcon.png" alt="Google" class="image" />
        <img src="./facebookIcon.png" alt="Facebook" class="image" />
      </div>

      <!-- OR line with horizontal lines -->
      <div class="or-container">
        <div class="line"></div>
        <span class="or-text">OR</span>
        <div class="line"></div>
      </div>

      <!-- Input boxes -->
      <div class="input-container">
        <input type="text" class="input-box" placeholder="Username / Email" />
        <input type="password" class="input-box" placeholder="Password" />
        <input type="password" class="input-box" placeholder="Confirm Password" />
      </div>

      <!-- Next Button -->
      <RouterLink to="/who" class="login-button">NEXT</RouterLink>
    </div>
  </div>
</template>
  
  <script>

  </script>
  
  <style scoped>
 
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
    /* width: 100%; */
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
  
  .images-container {
    display: flex;
    justify-content: center;
    width: 100%;
    margin-top: 15px; 
    gap: 40px; 
  }
  
  .image {
    width: 45px; 
    height: auto;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease-in-out;
  }
  
  .image:hover {
    transform: scale(1.05);
  }
  
  .or-container {
    display: flex;
    align-items: center;
    margin-top: 20px; 
    width: 100%;
    justify-content: center;
  }
  
  .line {
    width: 330px;
    height: 1px;
    background-color: white;
  }
  
  .or-text {
    margin: 0 10px;
    font-size: 1rem; 
    color: white;
    font-weight: 400;
  }
  
  .input-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 15px; 
    width: 100%; 
  }
  
  .input-box {
    width: 100%;
    max-width: 700px; 
    padding: 12px; 
    margin: 10px 0; 
    border-radius: 13px;
    border: 1px solid #ccc;
    background-color: white;
    color: #333;
    font-size: 1.1rem; 
    text-align: left;
  }
  
  .input-box::placeholder {
    color: #aaa;
    font-weight: 400;
  }
  
  .remember-forgot-container {
    display: flex;
    justify-content: space-between;
    width: 100%;
    max-width: 700px; 
    margin-top: 10px; 
  }
  
  .remember-container {
    display: flex;
    align-items: center;
  }
  
  .checkbox {
    width: 18px; 
    height: 18px; 
    border-radius: 100px;
    margin-right: 12px; 
  }
  
  .remember-text {
    font-size: 1.1rem; 
    color: white;
    font-weight: 500;
  }
  
  .forgot-password {
    font-size: 1.1rem; 
    color: white;
    font-weight: 500;
    text-decoration: none;
  }
  
  .forgot-password:hover {
    text-decoration: underline;
  }
  
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
  margin-top: 20px;
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
  </style>
  