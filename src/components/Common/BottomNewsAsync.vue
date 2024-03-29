<template>
  <div id="newsContainer">
      <h3 id="newsTitle">News</h3>
      <div id="newsGridContainer">
        <NewsItemAsync
          v-for="article in articles.slice(0, 8)"
          class="gridItem"
          :title="article.title"
          :imageName="article.imageName"
          :timestamp="article.timestamp"
          @click="selectArticle(article)"
        />
      </div>
    </div>
</template>

<script setup>
import NewsItemAsync from "../News/NewsItemAsync.vue";

import { ref } from "vue";

const articles = ref(null);

import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient, ScanCommand } from "@aws-sdk/lib-dynamodb";

const fetchArticles = async () => {
  const client = new DynamoDBClient({
    region: "us-east-1",
    credentials: {
      accessKeyId: import.meta.env.VITE_APP_AWS_ACCESS_KEY_ID,
      secretAccessKey: import.meta.env.VITE_APP_AWS_SECRET_ACCESS_KEY,
    },
  });

  const docClient = DynamoDBDocumentClient.from(client);

  const command = new ScanCommand({
    TableName: "TotalFootball-News",
  });

  const response = await docClient.send(command);
  articles.value = response.Items.sort(() => 0.5 - Math.random());
};

await fetchArticles();

import { useRouter } from "vue-router";

const router = useRouter();

const selectArticle = (article) => {
  router.push({
    name: "article",
    params: { id: article.id, img: article.imageName.slice(0, -4) },
  });
};
</script>

<style scoped>
#newsContainer {
  width: 100%;
  margin: auto;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 2rem;
}

#newsTitle {
  margin: 0;
  font-weight: 500;
}

#newsGridContainer {
  display: grid;
  gap: 1rem;
  grid-template-columns: auto auto auto auto;
}


@media (max-width: 1150px) {
  #newsGridContainer {
    grid-template-columns: auto auto auto;
  }
}

@media (max-width: 750px) {
  #newsGridContainer {
    grid-template-columns: auto auto;
  }
}

@media (max-width: 500px) {
  #newsGridContainer {
    grid-template-columns: auto;
  }
}

@media (max-width: 800px) {
  #newsContainer {
    width: calc(100% - 2rem);
  }
}
</style>
