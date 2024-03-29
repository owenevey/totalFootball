<template>
  <div id="lineupsContainer">
    <div class="lineupsEdgeRow">
      <div class="lineupsRowLeft">
        <img class="lineupsTeamLogo" :src="game.teams.home.logo" />
        <p v-if="hasLineups" class="lineupsFormation">
          {{ game.lineups[0].formation }}
        </p>
        <h3 id="lineupsHomeTeamName">{{ game.teams.home.name }}</h3>
      </div>

      <h3 id="lineupsTitle">Lineups</h3>

      <div id="awayTeamTopRow" class="lineupsRowRight">
        <h3 id="lineupsAwayTeamName">{{ game.teams.away.name }}</h3>
        <p v-if="hasLineups" class="lineupsFormation">
          {{ game.lineups[1].formation }}
        </p>
        <img class="lineupsTeamLogo" :src="game.teams.away.logo" />
      </div>
    </div>
    <Pitch
      :homeLineup="homeLineup"
      :awayLineup="awayLineup"
      :hasLineups="hasLineups"
      @selectPlayer="(playerID) => $emit('selectPlayer', playerID)"
    />
    <div class="lineupsEdgeRow">
      <div id="coachEdgeContainer">
        <div v-if="hasLineups" class="lineupsRowLeft">
          <img class="lineupsCoachImage" :src="game.lineups[0].coach.photo" />
          <h3 class="lineupsCoachName">{{ game.lineups[0].coach.name }}</h3>
        </div>
        <h3 v-if="hasLineups" id="lineupsCoachTitle">Coach</h3>
        <div v-if="hasLineups" class="lineupsRowRight">
          <h3 class="lineupsCoachName">{{ game.lineups[1].coach.name }}</h3>
          <img class="lineupsCoachImage" :src="game.lineups[1].coach.photo" />
        </div>
      </div>

      <div id="awayTeamBottomRow" class="lineupsRowRight">
        <h3 id="lineupsAwayTeamName">{{ game.teams.away.name }}</h3>
        <p v-if="hasLineups" class="lineupsFormation">
          {{ game.lineups[1].formation }}
        </p>
        <img class="lineupsTeamLogo" :src="game.teams.away.logo" />
      </div>
    </div>
  </div>
  <div v-if="hasLineups" id="coachCardContainer">
    <h3 id="lineupsCoachTitle">Coach</h3>

    <div id="coachCardRow">
      <div class="coachCardColumn">
        <img class="lineupsCoachImage" :src="game.lineups[0].coach.photo" />
        <h3 class="lineupsCoachName">{{ game.lineups[0].coach.name }}</h3>
      </div>

      <div class="coachCardColumn">
        <img class="lineupsCoachImage" :src="game.lineups[1].coach.photo" />
        <h3 class="lineupsCoachName">{{ game.lineups[1].coach.name }}</h3>
      </div>
    </div>

  </div>
</template>

<script setup>
const props = defineProps({
  game: Object,
  hasLineups: Boolean,
});

import { ref, toRefs } from "vue";
import Pitch from "./Pitch.vue";

const { game, hasLineups } = toRefs(props);
const homeLineup = ref({});
const awayLineup = ref({});

function parseLineup(lineupData) {
  const tempDict = {};
  for (const playerObj of lineupData.startXI) {
    if (playerObj.player.grid.substring(0, 1) in tempDict) {
      tempDict[playerObj.player.grid.substring(0, 1)].push({
        name: playerObj.player.name,
        number: playerObj.player.number,
        id: playerObj.player.id,
        kitColor:
          playerObj.player.pos === "G"
            ? "#" + lineupData.team.colors.goalkeeper.primary
            : "#" + lineupData.team.colors.player.primary,
        numberColor:
          playerObj.player.pos === "G"
            ? "#" + lineupData.team.colors.goalkeeper.number
            : "#" + lineupData.team.colors.player.number,
      });
    } else {
      tempDict[playerObj.player.grid.substring(0, 1)] = [
        {
          name: playerObj.player.name,
          number: playerObj.player.number,
          id: playerObj.player.id,
          kitColor:
            playerObj.player.pos === "G"
              ? "#" + lineupData.team.colors.goalkeeper.primary
              : "#" + lineupData.team.colors.player.primary,
          numberColor:
            playerObj.player.pos === "G"
              ? "#" + lineupData.team.colors.goalkeeper.number
              : "#" + lineupData.team.colors.player.number,
        },
      ];
    }
  }
  return tempDict;
}

if (hasLineups.value) {
  const homeLineupData = game.value.lineups[0];
  const awayLineupData = game.value.lineups[1];

  homeLineup.value = parseLineup(homeLineupData);
  awayLineup.value = parseLineup(awayLineupData);
}
</script>

<style scoped>
#lineupsContainer {
  background-color: #ffffff;
  border-radius: 15px;
  border: 2px #f0f0f0 solid;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-sizing: border-box;
}

.lineupsEdgeRow {
  width: 100%;
  min-height: 3.75rem;
  height: fit-content;
  display: flex;
  justify-content: center;
  align-items: center;
}

#lineupsTitle,
#lineupsCoachTitle {
  font-weight: 500;
}

.lineupsRowLeft {
  display: flex;
  flex: 1;
  justify-content: flex-start;
  align-items: center;
  margin: 0rem 1rem;
  gap: 1rem;
}

.lineupsRowRight {
  display: flex;
  flex: 1;
  justify-content: flex-end;
  align-items: center;
  margin: 0rem 1rem;
  gap: 1rem;
}

#coachEdgeContainer {
  display: flex;
  width: 100%;
}

.lineupsTeamLogo,
.lineupsCoachImage {
  object-fit: contain;
  width: 2rem;
  height: 2rem;
}

#lineupsHomeTeamName {
  font-weight: 400;
  text-align: left;
}

#lineupsAwayTeamName {
  font-weight: 400;
  text-align: right;
}

.lineupsCoachName {
  font-weight: 400;
  margin: 0.5rem;
}

.lineupsFormation {
  color: black;
  background-color: #ebebeb;
  padding: 0.3rem 0.5rem;
  border-radius: 10px;
  white-space: nowrap;
}

.lineupsCoachImage {
  border-radius: 50%;
}

#awayTeamBottomRow {
  display: none;
}

#coachCardContainer {
  display: none;
}

@media (max-width: 900px) {
  #lineupsTitle,
  #awayTeamTopRow {
    display: none;
  }

  #awayTeamBottomRow {
    display: flex;
  }

  #coachEdgeContainer {
    display: none;
  }

  #coachCardContainer {
    background-color: #ffffff;
    border-radius: 15px;
    border: 2px #f0f0f0 solid;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #coachCardRow {
    display: flex;
    width: 100%;
  }

  .coachCardColumn {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 0 1rem 1rem 1rem;
  }
}
</style>
