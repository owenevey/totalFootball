<template>
  <div id="leaguesContainer">
    <h3 id="leaguesTitle">Leagues</h3>
    <div id="leaguesItemsContainer">
      <HomeLeagueItem v-for="(leagueID, leagueName) in leagues" :name="leagueName" @click="selectLeague(leagueID)" />
    </div>
  </div>
</template>

<script setup>

import { useRouter } from "vue-router";
import HomeLeagueItem from "./HomeLeagueItem.vue";

const router = useRouter();

const selectLeague = (id) => {
  router.push({
    name: "league",
    params: { id: id },
  });
};

const leagues = {
  "Premier League": 39,
  "La Liga": 140,
  "Bundesliga": 78,
  "Serie A": 135,
  "Ligue 1": 61,
  "UEFA Champions League": 2,
  "UEFA Europa League": 3,
  "Eredivisie": 88,
  "Primeira Liga": 94,
  "Major League Soccer": 253,
  "Liga MX": 262,
  "Pro League": 307,
};
</script>

<style scoped>
#leaguesContainer {
  background-color: #ffffff;
  border-radius: 15px;
  border: 2px #f0f0f0 solid;
  height: fit-content;
  overflow: hidden;
}

#leaguesTitle {
  font-weight: 500;
  padding: 1.3rem;
  margin: 0;
}

#leaguesItemsContainer {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  overflow: hidden;
}
</style>
