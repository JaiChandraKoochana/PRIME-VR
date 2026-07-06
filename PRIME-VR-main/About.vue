
<template>
    <main>
      <div class="content">
        <h1>About Us</h1>
        <p class="company-description">
          With PRIME-VR, we plan to increase engagement with film study by allowing players to view plays in a VR environment. Additionally, we want to shorten the amount of time players and coaches spend on film study by using AI to study films and provide statistics in an efficient manner.
        </p>
      </div>
  
      <!-- Arrow Controls-->
      <div class="arrow-container">
        <button @click="scrollLeft" class="arrow left">&#10094;</button>
        <button @click="scrollRight" class="arrow right">&#10095;</button>
      </div>
  
      <!-- Team Section -->
      <div class="team-container" ref="scrollContainer">
        <div class="team-member" v-for="(member, index) in teamMembers" :key="index">
          <div class="member-card">
            <img :src="member.photo" alt="TeamMember" class="team-photo" />
            <h3>{{ member.name }}</h3>
            <p class="member-description">{{ member.description }}</p>
          </div>
        </div>
      </div>
    </main>
    
  </template>
  
  <script>
import team1 from "@/views/Fredlyne.jpg";
import team2 from "@/views/Evan.png";
import team3 from "@/views/Matt.png";
import team4 from "@/views/ryan2.jpg";
import team5 from "@/views/Steve2.jpg";
import team6 from "@/views/hashism.jpg";
  export default {
    
    data() {
      return {
        teamMembers: [
          {
            name: "Fredlyne Antoine",
            photo: team1,
            description: "She is a senior studying Computer Science and minor in Graphic Design at the University of New Haven. After graduation she plans to work as a UX/UI Designer in a full time position.",
          },
          {
            name: "Evan Vojta",
            photo: team2,
            description: "He is a senior studying Computer Science at the University of New Haven. After graduation he plans to work at FactSet in a full time position.",
          },
          {
            name: "Matthew Rosenblum",
            photo: team3,
            description: "He is a undergraduate senior studying Computer Science at the University of New Haven. After graduation, he plans to pursue a career in Software Engineering.",
          },
          {
            name: "Ryan Larrea",
            photo: team4,
            description: "He is a senior studying Computer Science at the University of New Haven. He enjoys AI and app development and plans to pursue a career in those fields after graduation.",
          },
          {
            name: "Steve Nwachukwu",
            photo: team5,
            description: "He is a senior studying Computer Science at the University of New Haven. After graduation he plans on attending graduate school to study cybersecurity.",
          },
          {
            name: "Hashim Ali",
            photo: team6,
            description: "He is a senior studying Computer Science at the University of New Haven. After graduation Hashim plans to pursue a career in Software Engineering.",
          },
        ],
      };
    },
    mounted() {
      const container = this.$refs.scrollContainer;
      let isDown = false;
      let startX;
      let scrollLeft;
    
      container.addEventListener('mousedown', (e) => {
        isDown = true;
        container.classList.add('dragging');
        startX = e.pageX - container.offsetLeft;
        scrollLeft = container.scrollLeft;
      });
    
      container.addEventListener('mouseleave', () => {
        isDown = false;
        container.classList.remove('dragging');
      });
    
      container.addEventListener('mouseup', () => {
        isDown = false;
        container.classList.remove('dragging');
      });
    
      container.addEventListener('mousemove', (e) => {
        if (!isDown) return;
        e.preventDefault();
        const x = e.pageX - container.offsetLeft;
        const walk = (x - startX) * 2;
        container.scrollLeft = scrollLeft - walk;
      });
    },
    methods: {
      scrollLeft() {
        this.$refs.scrollContainer.scrollBy({ left: -300, behavior: "smooth" });
      },
      scrollRight() {
        this.$refs.scrollContainer.scrollBy({ left: 300, behavior: "smooth" });
      },
    },
  };
  </script>
  
  <style scoped>
  body {
    background: linear-gradient(175deg, rgb(34, 32, 32) 80%, red 100%);
    margin: 0;
    padding: 0;
    font-family: 'Roboto';
    color: white;
    overflow-x: hidden;
    overflow-y: hidden;
  }
  
  main {

    display: flex;
    justify-content: center;
    align-items: flex-start;
    text-align: center;
    padding: 100px 20px 60px;
 
  }
  
  .content {
    width: 150%;
    max-width: 2800px;
    margin: 0 auto;
    margin-top: -120px;
    margin-left: -450px;
  }
  
  h1 {
  font-size: 3.5rem;
  font-weight: 900;
  margin-bottom: 20px;
  color: white;
}

  .company-description {
    font-size: 1rem;
    max-width: 700px;
    margin: 0 auto 50px;
    line-height: 1.6;
    color: white;
    font-weight: 300;
  }
  
.arrow-container {
  position: absolute;
  top: 50%; 
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
  pointer-events: none;
  z-index: 11;
  opacity: 0;  
  transition: opacity 0.3s ease; 
}

/* Show arrows on hover */
.arrow-container:hover {
  opacity: 1;  
}

/* Arrow Button  */
.arrow {
  pointer-events: auto;
  background: transparent;  
  border: 2px solid #FF3B32; 
  color: #FF3B32; 
  font-size: 2rem;
  width: 60px;  
  height: 60px;  
  border-radius: 50%; 
  cursor: pointer;
  transition: background 0.3s ease, color 0.3s ease;
  margin-top: 110px;
}

.arrow:hover {
  background: #FF3B32; 
  color: white;  
}


  
  /* Team Container */
  .team-container {
    display: flex;
    flex-wrap: nowrap;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    scroll-snap-type: x mandatory;
    gap: 30px;
    padding-left: 20px;
    margin-top: 140px;
    scrollbar-width: none;
    -ms-overflow-style: none;
    cursor: grab;
  }
  
  .team-container.dragging {
    cursor: grabbing;
    user-select: none;
  }
  
  .team-container::-webkit-scrollbar {
    display: none;
  }
  
  .team-member {
    flex: 0 0 auto;
    scroll-snap-align: start;
  }
  
  .member-card {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid #ffffff;
    border-radius: 25px;
    padding: 20px;
    width: 250px;
    min-height: 350px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    transition: transform 0.3s ease, box-shadow 0.3s ease, height 0.3s ease;
    margin-right: 10px;
    box-shadow: 0 0 10px #767676;
   
  }
  
  
  .team-photo {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid #ffffff;
    margin-bottom: 16px;
  }
  
  .member-card h3 {
    margin: 0;
    font-size: 1.1rem;
    font-weight: 700;
    color: white;
  }
  
  .member-description {
    font-size: 0.9rem;
    font-weight: 500;
    margin-top: 10px;
    line-height: 1.4;
    color: white;
    text-align: center;
  }
  </style>

  <style> 
  body {
    background: linear-gradient(175deg, rgb(34, 32, 32) 80%, red 100%);
    margin: 0;
    padding: 0;
    font-family: 'Roboto';
    color: white;
    overflow-x: hidden;
    overflow-y: hidden;
  }
  
</style>
  