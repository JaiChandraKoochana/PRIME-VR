<template>
  <div class="signin-background">
    <!-- Blob Background -->
    <div class="blob-clipper">
      <div class="blob-background">
        <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
          <path
            fill="#FF5448"
            d="M30.8,-37.5C44.1,-32.6,62.1,-29.3,73,-18.4C83.8,-7.5,87.6,11.1,76.7,18.8C65.8,26.6,40.3,23.5,25.1,26.3C9.9,29.2,4.9,37.9,-5.8,45.9C-16.5,53.9,-33,61,-43,56.5C-53.1,52,-56.6,35.8,-59.5,20.8C-62.4,5.8,-64.7,-8,-57.6,-15.4C-50.6,-22.8,-34.3,-23.8,-23.1,-29.4C-12,-35,-6,-45.2,1.4,-47.1C8.7,-48.9,17.4,-42.4,30.8,-37.5Z"
            transform="translate(100 100)"
          />
        </svg>
      </div>
    </div>

    <!-- Sidebar -->
    <div class="sidebar">
      <div class="team-container">
        <div
          class="team-box"
          v-for="(team, index) in teams"
          :key="index"
          @click="selectTeam(team)"
        >
          <div class="team-name">
            <span v-if="editingTeamIndex !== index">{{ team }}</span>
            <input
              v-else
              v-model="editedTeamName"
              @blur="saveEdit(index)"
              class="edit-input"
              @keydown.enter="saveEdit(index)"
              @focus="highlightText"
            />
          </div>

          <!-- Delete button -->
          <div class="delete-circle" @click.stop="deleteTeam(index)">
            &#x2716;
          </div>

          <!-- Edit button -->
          <div class="edit-circle" @click.stop="startEdit(index, team)">
            ✎
          </div>
        </div>
      </div>
    </div>

    <div class="signin-container">
      <h1 class="title2">Select Your Team</h1>
      <div class="input-container">
        <input
          type="text"
          v-model="newTeamName"
          @keyup.enter="addTeam"
          class="input-box"
          placeholder="Enter Team Name"
        />
      </div>
    </div>

    <div class="circle-container">
      <div class="circle-large"></div>
    </div>

    <div class="circle-container2">
      <div class="circle-small"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTeamName: '',
      teams: [],
      selectedTeam: null,
      editingTeamIndex: null,
      editedTeamName: '',
    };
  },
  created() {
    const savedTeams = localStorage.getItem('teams');
    if (savedTeams) {
      this.teams = JSON.parse(savedTeams);
    }

    const savedTeamName = localStorage.getItem('selectedTeam');
    if (savedTeamName) {
      this.newTeamName = savedTeamName;
    }
  },
  methods: {
    addTeam() {
      if (this.newTeamName.trim() !== '') {
        this.teams.push(this.newTeamName.trim());
        this.newTeamName = '';
        localStorage.setItem('teams', JSON.stringify(this.teams));
        localStorage.setItem('selectedTeam', this.newTeamName);
      }
    },
    selectTeam(team) {
      if (this.editingTeamIndex === null) {
        this.selectedTeam = team;
        this.newTeamName = team;
        this.$nextTick(() => {
          this.$router.push({ name: 'upload', params: { teamName: team } });
        });
      }
    },
    deleteTeam(index) {
      this.teams.splice(index, 1);
      localStorage.setItem('teams', JSON.stringify(this.teams));
    },
    startEdit(index, team) {
      this.editingTeamIndex = index;
      this.editedTeamName = team;
      this.$nextTick(() => {
        const inputField = this.$el.querySelector('.edit-input');
        if (inputField) {
          inputField.focus();
          inputField.select();
        }
      });
    },
    saveEdit(index) {
      if (this.editedTeamName.trim() !== '') {
        this.teams[index] = this.editedTeamName.trim();
        this.editingTeamIndex = null;
        this.editedTeamName = '';
        localStorage.setItem('teams', JSON.stringify(this.teams));
      }
    },
    highlightText() {
      this.$nextTick(() => {
        const input = document.querySelector('.edit-input');
        input.select();
      });
    },
  },
};
</script>

<style scoped>
html,
body {
  background: linear-gradient(185deg, rgb(34, 32, 32) 80%, red 100%);
  height: 100%;
  margin: 0;
  padding: 0;
  height: 100vh;
  overflow-x: hidden;
}

/* Other styles */
.signin-background {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  position: relative;
  width: 100%;
  overflow: visible;
}

.blob-clipper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 610px;
  overflow: hidden;
  z-index: -1;
}

.blob-background {
  position: absolute;
  top: -700px;
  left: -600px;
  width: 1600px;
  height: 1600px;
  pointer-events: none;
}

.signin-container {
  font-family: 'Roboto';
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  width: 100%;
  max-width: 750px;
  text-align: left;
  padding: 20px 20px;
  box-sizing: border-box;
  height: 100%;
  max-height: 100vh;
  margin-top: 2%;
}

.title2 {
  font-size: 2rem;
  font-weight: 700;
  color: white;
  margin-top: 50px;
  margin-left: 20px;
}

.input-container {
  display: flex;
  flex-direction: column;
  justify-content: left;
  align-items: left;
  margin-top: 40px;
  width: 100%;
  margin-left: 20px;
}

.input-box {
  width: 100%;
  max-width: 700px;
  padding: 12px;
  margin: 30px 0;
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

/* ✅ Updated sidebar with white line on the left and transparent background */
.sidebar {
  position: fixed;
  top: 100px; /* Starts below the navbar */
  right: 0;
  width: 450px; /* Wider sidebar */
  height: calc(100vh - 100px); /* Fits below navbar */
  background-color: rgba(34, 32, 32, 0); /* Transparent background */
  color: #fff;
  padding: 20px 15px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  z-index: 100;
  overflow-y: auto;
  border-left: 3px solid white; /* White line on the left */
  border: none;
}

.team-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  overflow-y: auto;
}

/* Team boxes with transparent background and no borders */
.team-box {
  background-color: transparent; /* Transparent background for each team box */
  padding: 10px 15px;
  width: 100%;
  text-align: left;
  border-radius: 10px;
  font-size: 1rem;
  color: white;
  cursor: pointer;
  position: relative;
  transition: background-color 0.3s ease;
  border: none; /* No border for team boxes */
}

/* Change hover effect: Remove white hover, use blob's red color */
.team-box:hover {
  background-color: rgba(255, 84, 72, 0.1); /* Light red hover effect */
}

/* Team name styling */
.team-name {
  padding: 5px 0;
  text-align: left;
  transition: color 0.3s ease;
}

/* Hover effect on team name: change color to the blob's red color */
.team-name:hover {
  color: #ff5448; /* Matching blob color */
}

.delete-circle,
.edit-circle {
  position: absolute;
  top: 8px;
  font-size: 14px;
  cursor: pointer;
  display: none;
  color: white;
}

.delete-circle {
  right: 10px;
}
.edit-circle {
  right: 35px;
}

.team-box:hover .delete-circle,
.team-box:hover .edit-circle {
  display: block;
}

.edit-input {
  width: 100%;
  padding: 5px;
  font-size: 1rem;
  background-color: transparent;
  border: none;
  color: white;
  outline: none;
}

.team-container::-webkit-scrollbar {
  width: 6px;
}
.team-container::-webkit-scrollbar-thumb {
  background: #ff5448;
  border-radius: 10px;
}
.team-container::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}
</style>
