<template>
    <div>
      <nav class="navbar navbar-expand-lg bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">All Players</a>
          <button
            class="navbar-toggler"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
              <li class="nav-item">
                <select class="form-select" v-model="selectedTeam" @change="filterPlayers">
                  <option value="ALL">ALL</option>
                  <option value="IND">IND</option>
                  <option value="PAK">PAK</option>
                  <option value="AUS">AUS</option>
                  <option value="ENG">ENG</option>
                </select>
              </li>
            </ul>
          </div>
        </div>
      </nav>
  
     <div class="container text-center mt-4">
        <div class="row">
          <div class="col-12">
            <h1 class="mb-4">Batsmen</h1>
          </div>
          <div class="col-md-4 col-xxl-3" v-for="player in batsmen" :key="player.name">
            <div class="card mb-4">
              <img :src="player.image" class="card-img-top" alt="Player Image" />
              <div class="card-body">
                <h5 class="card-title">{{ player.name }}</h5>
                <p class="card-text">Matches: {{ player.matches }}</p>
                <p class="card-text">Runs: {{ player.runs }}</p>
                <p class="card-text">50s/100s: {{ player["50s"] }}/{{ player["100s"] }}</p>
                <p class="card-text">Highest Score: {{ player.highest_score }}</p>
                <p class="card-text">Team Name: {{ player.team_name }}</p>
              </div>
            </div>
          </div>
        </div>
  
        <div class="row mt-4">
          <div class="col-12">
            <h1 class="mb-4">Bowlers</h1>
          </div>
          <div class="col-md-4" v-for="player in bowlers" :key="player.name">
            <div class="card mb-4">
              <img :src="player.image" class="card-img-top" alt="Player Image" />
              <div class="card-body">
                <h5 class="card-title">{{ player.name }}</h5>
                <p class="card-text">Matches: {{ player.matches }}</p>
                <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
                <p class="card-text">Team Name: {{ player.team_name }}</p>
              </div>
            </div>
          </div>
        </div>
  
        <div class="row mt-4">
          <div class="col-12">
            <h1 class="mb-4">All Rounders</h1>
          </div>
          <div class="col-md-4" v-for="player in allRounders" :key="player.name">
            <div class="card mb-4">
              <img :src="player.image" class="card-img-top" alt="Player Image" />
              <div class="card-body">
                <h5 class="card-title">{{ player.name }}</h5>
                <p class="card-text">Matches: {{ player.matches }}</p>
                <p class="card-text">Runs: {{ player.runs }}</p>
                <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures }}</p>
                <p class="card-text">Team Name: {{ player.team_name }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    data() {
      return {
        selectedTeam: "ALL",
        players: [],
        filteredPlayers: [],
      };
    },
    created() {
      this.fetchPlayersData();
    },
    computed: {
      batsmen() {
        return this.filteredPlayers
          ? this.filteredPlayers.filter((player) => player.role === 2)
          : [];
      },
      bowlers() {
        return this.filteredPlayers
          ? this.filteredPlayers.filter((player) => player.role === 4)
          : [];
      },
      allRounders() {
        return this.filteredPlayers
          ? this.filteredPlayers.filter((player) => player.role === 3)
          : [];
      },
    },
    methods: {
      async fetchPlayersData() {
        try {
          const response = await axios.get(
            "http://localhost:3000/originalPlayers"
          );
  
          console.log(response.data);
          this.players = response.data;
          this.filteredPlayers = this.players;
        } catch (error) {
          console.error("Error fetching player data :", error);
        }
      },
      filterPlayers() {
        if (this.selectedTeam === "ALL") {
          this.filteredPlayers = this.players;
        } else {
          this.filteredPlayers = this.players.filter(
            (player) => player.team_name === this.selectedTeam
          );
        }
      },
    },
  };
  </script>
  
  <style>
  .navbar {
    margin-bottom: 20px;
  }
  
  .card {
    transition: transform 0.2s;
    flex-grow: 1;
  }
  
  .card:hover {
    transform: scale(1.05);
  }
  
  .card-img-top {
    height: 150px;
    object-fit: cover;
  }
  
  h1 {
    font-size: 2.5rem;
    font-weight: bold;
  }
  
  .form-select {
    width: auto;
    margin-left: 10px;
  }
</style>