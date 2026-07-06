<template>
  <div class="settings-container">
    <!-- Sidebar -->
    <div class="sidebar">
      <h2 class="sidebar-title">Settings</h2>
      <ul>
        <li class="menu-item" :class="{ active: activeSection === 'Profile' }" @click="activeSection = 'Profile'">Profile</li>
        <li class="menu-item" :class="{ active: activeSection === 'Notifications' }" @click="activeSection = 'Notifications'">Notifications</li>
        <li class="menu-item" :class="{ active: activeSection === 'Languages' }" @click="activeSection = 'Languages'">Languages</li>
        <li class="menu-item" :class="{ active: activeSection === 'Help' }" @click="activeSection = 'Help'">Help</li>
      </ul>
    </div>

    <!-- Main Content -->
    <div class="main-content">
      <!-- Profile Section -->
      <div v-if="activeSection === 'Profile'" class="profile-section">
        <h1 class="main-title">Profile</h1>
        <h2 class="section-title">Basic Info</h2>

        <!-- Profile Picture Upload -->
        <div class="profile-picture-section">
          <div class="profile-picture-content">
            <span class="mini-title">Profile Picture</span>
            <div class="circle-fixed">
              <img v-if="profilePic" :src="profilePic" alt="Profile" class="profile-img" />
              <input type="file" id="profilePicInput" @change="uploadProfilePic" class="file-input" />
            </div>
            <span class="upload-text" @click="triggerFileInput">Upload New Picture</span>
          </div>
        </div>

        <!-- Scrollable Section -->
        <div class="scrollable-section video-container">
          <div class="divider light"></div>
          <div class="form-fields">
            <div class="field-row">
              <label class="field-label">Name</label>
              <div class="field-box"></div>
            </div>
            <div class="divider light"></div>

            <div class="field-row">
              <label class="field-label">Date of Birth</label>
              <div class="field-box"></div>
            </div>
            <div class="divider light"></div>

            <div class="field-row">
              <label class="field-label">Gender</label>
              <div class="field-box"></div>
            </div>
            <div class="divider light"></div>

            <div class="field-row">
              <label class="field-label">Email</label>
              <div class="field-box"></div>
            </div>
            <div class="divider light"></div>

            <div class="field-row">
              <label class="field-label">Group</label>
              <div class="field-box"></div>
            </div>
          </div>

          <div class="divider light"></div>
          <h2 class="section-title">Account Info</h2>
          <div class="form-fields">
            <div class="field-row">
              <label class="field-label">Username</label>
              <div class="field-box"></div>
            </div>
            <div class="divider light"></div>

            <div class="field-row">
              <label class="field-label">Password</label>
              <div class="field-box"></div>
            </div>
          </div>
        </div>
      </div>

      <!-- Notifications Section -->
      <div v-if="activeSection === 'Notifications'" class="notifications-section">
        <div class="notifications-content">
          <h1 class="main-title">Notifications</h1>

          <!-- Email Notifications -->
          <div class="notification-item">
            <span class="notification-text">Email Notifications</span>
            <label class="switch">
              <input type="checkbox" v-model="emailNotifications" />
              <span class="slider"></span>
            </label>
          </div>

          <div class="divider light"></div>

          <h2 class="section-title">Website Notifications</h2>
          <div class="notification-checkboxes">
            <div>
              <input type="checkbox" id="news" />
              <label for="news">News</label>
            </div>
            <div>
              <input type="checkbox" id="events" />
              <label for="events">Events</label>
            </div>
            <div>
              <input type="checkbox" id="uploads" />
              <label for="uploads">Uploads</label>
            </div>
            <div>
              <input type="checkbox" id="game-highlights" />
              <label for="game-highlights">Game Highlights</label>
            </div>
          </div>
        </div>
      </div>

<!-- Languages Section -->
<div v-if="activeSection === 'Languages'" class="languages-section">
  <h1 class="main-title">Language</h1>

  <div class="form-fields">
    <div class="field-row">
      <label class="field-label">Select Language</label>
      <select class="field-box select-dropdown" v-model="selectedLanguage">
        <option>English</option>
        <option>Spanish</option>
        <option>French</option>
        <option>German</option>
        <option>Chinese</option>
        <option>Japanese</option>
        <option>Arabic</option>
        <option>Russian</option>
        <option>Hindi</option>
        <option>Portuguese</option>
        <option>Korean</option>
        <option>Italian</option>
        <option>Turkish</option>
        <option>Polish</option>
        <option>Dutch</option>
        <option>Greek</option>
        <option>Swedish</option>
        <option>Hebrew</option>
        <option>Thai</option>
        <option>Vietnamese</option>
      </select>
    </div>
    <span class="mini-description">Choose the language you'd like to use</span>
  </div>
  <div class="divider light"></div>
  
  <!-- Privacy Section -->
<h1 class="main-title2">Privacy</h1>
<div class="form-fields">
  <div class="delete-account-row">
    <span class="field-label delete-label">Delete Account</span>
    <button class="delete-btn">Delete Account</button>
  </div>
  <span class="mini-description2">Permanently delete account & data.</span>
</div>

</div>

<!-- Help Section -->
<div v-if="activeSection === 'Help'" class="help-section">
  <div class="help-banner">
    <h1 class="help-title">Help</h1>
  </div>
  <div class="help-content">
    <p class="help-subtext">
      Need Help? Please go here: 
      <a href="https://mycharger.newhaven.edu" target="_blank" class="help-link">
        https://mycharger.newhaven.edu
      </a> 
      for more information.
    </p>
  </div>
</div>


    </div>
  </div>
</template>

<script>
export default {
  name: "Settings",
  data() {
    return {
      activeSection: 'Profile',
      profilePic: null,
      emailNotifications: false,
      selectedLanguage: "English",
    };
  },
  methods: {
    triggerFileInput() {
      document.getElementById("profilePicInput").click();
    },
    uploadProfilePic(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onloadend = () => {
          this.profilePic = reader.result;
        };
        reader.readAsDataURL(file);
      }
    },
  },
};
</script>

<style scoped>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
}

.settings-container {
  display: flex;
  height: 85vh;
  background: linear-gradient(130deg, rgb(34, 32, 32) 90%, rgb(255, 0, 0) 70%);
  color: white;
  font-family: 'Roboto', sans-serif;
}

.sidebar {
  width: 220px;
  padding: 30px 20px;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  box-sizing: border-box;
}

.sidebar-title {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 30px;
  color: white;
  margin-left: 10px;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.menu-item {
  font-size: 1.1rem;
  padding: 12px 16px;
  margin-bottom: 10px;
  cursor: pointer;
  border-radius: 26px;
  transition: background-color 0.3s ease;
}

.menu-item:hover,
.menu-item.active {
  background-color: #FF3B32;
}

.main-content {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 40px;
  overflow-y: auto;
}

/* Profile section */
.profile-section {
  max-width: 750px;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.main-title {
  font-size: 2.8rem;
  font-weight: bold;
  margin-bottom: 20px;
}

.main-title2 {
  font-size: 2.8rem;
  font-weight: bold;
  margin-bottom: 20px;
  margin-top: 30px;
}

.section-title {
  font-size: 1.6rem;
  font-weight: 600;
  margin-top: 30px;
  margin-bottom: 20px;
}

.profile-picture-section {
  display: flex;
  flex-direction: column;
  margin-bottom: 30px;
}

.profile-picture-content {
  display: flex;
  align-items: center;
  gap: 70px;
  margin-top: 10px;
}

.mini-title {
  font-size: 1rem;
  font-weight: 300;
}


.circle-fixed {
  width: 100px;
  height: 100px;
  background-color: #FF3B32;
  border-radius: 50%;
  flex-shrink: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  outline: 1px solid white;
}

.profile-img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.upload-text {
  color: #FF3B32;
  font-weight: bold;
  cursor: pointer;
  font-size: 0.85rem;
  margin-top: 55px;
  margin-left: -40px;
}

.form-fields {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.field-row {
  display: flex;
  align-items: center;
  gap: 20px;
}

.field-label {
  font-size: 1rem;
  width: 140px;
  min-width: 140px;
}

.field-box {
  flex: 1;
  padding: 8px 12px;
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  color: white;
  font-size: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  height: 40px;
}

.divider.light {
  height: 1px;
  background-color: rgba(255, 255, 255, 0.1);
  margin: 10px 0;
}

.file-input {
  display: none;
}

/* Nofication Section */
.notifications-section {
  display: flex;
  justify-content: center;
  width: 100%;
  padding: 20px;
  margin-left: -150px;
}

.notifications-content {
  text-align: left;
  width: 100%;
  max-width: 600px;
}

.notification-item {
  display: flex;
  align-items: center;
  margin-bottom: 40px;
}

.notification-text {
  font-size: 1.5rem;
  margin-right: 20px;
  margin-top: 30px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 30px;
  margin-top: 30px;
  margin-left: 90px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: 0.4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 22px;
  width: 22px;
  border-radius: 50%;
  left: 4px;
  bottom: 4px;
  background-color: white;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #ff5448;
}

input:checked + .slider:before {
  transform: translateX(20px);
}

.notification-checkboxes {
  margin-top: 30px;
  font-size: 1.2rem;
}

.notification-checkboxes div {
  margin-bottom: 20px;
}

.notification-checkboxes input {
  width: 25px;
  height: 25px;
  margin-right: 15px;
  border-radius: 20px;
}

.notification-checkboxes label {
  font-size: 1.3rem;
  color: white;
}

.video-container::-webkit-scrollbar {
  width: 6px;
}

.video-container::-webkit-scrollbar-thumb {
  background: #ff5448;
  border-radius: 10px;
}

.video-container::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}

.scrollable-section {
  max-height: 400px;
  overflow-y: scroll;
  padding-right: 20px;
}

.mini-description {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 5px;
  margin-left: 160px;
}

/* Language Section */
.mini-description2 {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 5px;
  margin-left: 0px;
}

.languages-section {
  margin-left: -400px; 
  margin-top: 10px;
}

.select-dropdown {
  background-color: rgba(255, 255, 255, 0.05);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 25px;
  padding: 10px;
  font-size: 1rem;
  appearance: none;
}

.select-dropdown option {
  background-color: #1f1f1f;
  color: white;
}

.delete-account-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}

.delete-label {
  font-size: 1rem;
  width: auto;
  min-width: unset;
  margin-right: auto;
}

.mini-description2 {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 5px;
  margin-left: 0px;
}

.delete-btn {
  padding: 8px 18px;
  background-color: transparent;
  color: white;
  border: 1px solid white;
  border-radius: 25px;
  font-size: 0.95rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  white-space: nowrap;
}

.delete-btn:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

/* Help section */
.help-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.help-banner {
  width: 85vw;
  background-color: #FF3B32;
  padding: 60px 20px;
  box-sizing: border-box;
  text-align: center;
}

.help-title {
  color: white;
  font-size: 3rem;
  margin: 0;
  font-weight: bold;
}

.help-content {
  margin-top: 40px;
  text-align: center;
  max-width: 700px;
  padding: 0 20px;
}

.help-subtext {
  color: rgba(255, 255, 255, 0.95);
  font-size: 1.2rem;
  line-height: 1.6;
}

.help-link {
  color: #ffc6c3;
  text-decoration: underline;
  font-weight: 500;
}


</style>
