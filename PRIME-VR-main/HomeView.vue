<template> 
  <main>
    <div class="content">
      <h1>P.R.I.M.E VR</h1>
      <p>
        PRIME-VR is a virtual reality football simulation. We want to improve the way UNH football players learn through having an effective and engaging way of studying game film.
      </p>

      <!-- Buy Button -->
      <div class="buy-button">
        BUY
      </div>
    </div>

    <!-- Circles -->
    <div class="circles">
      <div class="circle"></div>
    </div>
    <div class="circles2">
      <div class="circle2"></div>
    </div>
    <div class="circles3">
      <div class="circle3"></div>
    </div>

    <!-- VR headset Image -->
    <div class="image-container">
      <img src="./vrHeadset.png" alt="VR Headset" class="overlay-image" />
    </div>
  </main>
</template>

<style>
  body {
  background: linear-gradient(175deg, rgb(34, 32, 32) 80%, red 100%);
  margin: 0;
  padding: 0;
  font-family: 'Roboto'; 
  color: white;
  height: 100vh;
  position: relative;
}

  .content {
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    width: 50%;
    padding-left: 190px;
    z-index: 10;
  }

  h1 {
    font-size: 3rem;
    margin: 0;
  }

  p {
    font-size: 0.98rem;
    margin-top: 10px;
    line-height: 1.6;
    width: 100%; 
    max-width: 300px; 
    margin-bottom: 20px; 
  }

  .buy-button {
    display: inline-block;
    padding: 15px 45px; 
    background-color: #FF5448; 
    color: white;
    font-size: 1.2rem;
    text-align: center;
    text-decoration: none;
    border-radius: 25px; 
    margin-top: 20px;
    cursor: pointer;
    font-family: 'Roboto'; 
    transition: background-color 0.3s, color 0.3s, transform 0.3s;
    width: 100%; 
    max-width: 280px; 
  }

 
  .buy-button:hover {
    background-color: #e4150b;
    color: #ffffff; 
    transform: scale(1.05); 
  }


  .circles {
    position: absolute;
    right: 25%;
    top: 40%;
    transform: translateY(-50%);
    z-index: 5;
  }

  .circles2 {
    position: absolute;
    right: 31%;
    top: 60%;
    transform: translateY(-50%);
    z-index: 5;
  }

  .circles3 {
    position: absolute;
    right: 20%;
    top: 60%;
    transform: translateY(-50%);
    z-index: 5;
  }

 
  .circle {
    width: 250px;
    height: 250px;
    background-color: #FF5448; 
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
    animation: moveCircle 4s infinite ease-in-out;
  }

  .circle2 {
    width: 200px;
    height: 200px;
    background-color: #FF5448; 
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
    animation: moveCircle 6s infinite ease-in-out;
  }

  .circle3 {
    width: 150px;
    height: 150px;
    background-color: #FF5448; 
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
    animation: moveCircle 8s infinite ease-in-out;
  }

  /* Circles Animation */
  @keyframes moveCircle {
    0% {
      transform: translate(-50%, -50%) translateX(0);
    }
    50% {
      transform: translate(-50%, -50%) translateX(100px);
    }
    100% {
      transform: translate(-50%, -50%) translateX(0);
    }
  }

  .image-container {
    position: absolute;
    top: 25%;
    left: 80%;
    transform: translateX(-50%);
    z-index: 15;
  }

  .overlay-image {
    width: 500px;
    height: auto;
  }

  .overlay-image:hover {
    animation: spinOscillate 2s infinite alternate ease-in-out;
  }

  /* Image Animation */
  @keyframes spinOscillate {
    0% {
      transform: rotate(20deg);
    }
    100% {
      transform: rotate(-20deg);
    }
  }
</style>
