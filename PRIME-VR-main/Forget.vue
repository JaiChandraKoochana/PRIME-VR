<template>
  <div class="signin-background">
    <div class="signin-container">
      <h1 class="title">Forgot Password</h1>
      
      <!-- Subtitle text (will remain horizontally aligned) -->
      <p class="subtitle">Please enter your email and we’ll send you a link to reset your password</p>

      <!-- Single Image (Centered) -->
      <div class="images-container">
        <img src="./!.png" alt="Forget" class="image" />
      </div>

      <!-- Input box for Email -->
      <div class="input-container">
        <input type="email" class="input-box" placeholder="Email" />
      </div>

      <!-- Request Link Button -->
      <button class="login-button">REQUEST LINK</button>

      <!-- Back to Login Link -->
      <RouterLink class="create-account" to="/signin">BACK TO LOGIN</RouterLink>
    </div>
  </div>
</template>

<script>

</script>

<style scoped>
  html, body {
    background: linear-gradient(185deg, rgb(34, 32, 32) 90%, red 100%);
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
    font-size: 1rem; 
    font-weight: 400;
    color: white;
    max-width: 80%; 
    text-align: center;
    margin-top: 15px;
  }

  .images-container {
    display: flex;
    justify-content: center;
    width: 100%;
    margin-top: 15px;
  }

  .image {
    width: 350px; 
    height: auto;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease-in-out;
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
  }

  .login-button:hover {
    background-color: #e14b39;
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
