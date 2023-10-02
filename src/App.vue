<script setup lang="ts">
import UserItem from './components/UserItem.vue'
import {ref, computed} from 'vue'

function TimestampConverter(time:number) {
  let date = new Date(time*1000);
  let years = date.getFullYear();
  let months = date.getMonth();
  if (months.toString().length < 2) {
    months = months.toString();
    months = `0${months}`;
  }
  let days = date.getDate();
  if (days.toString().length < 2) {
    days = days.toString();
    days = `0${days}`;
  }
  let hours = date.getHours();
  let minuts = date.getMinutes();
  let seconds = date.getSeconds();
  return `${days}.${months}.${years} ${hours}:${minuts}:${seconds}`
}

// random data array

const data = ref([
  {
    id: 1,
    name: "Alastor",
    age: 45,
    last_login: 1682941273
  },
  {
    id: 2,
    name: "Elvis",
    age: 35,
    last_login: 1683718873
  },
  {
    id: 3,
    name: "Sai",
    age: 18,
    last_login: 1620646873
  },
  {
    id: 4,
    name: "Memfis",
    age: 53,
    last_login: 1276166473
  },
  {
    id: 5,
    name: "Tiamat",
    age: 16,
    last_login: 1529062873
  },
  {
    id: 6,
    name: "Fenris",
    age: 80,
    last_login: 1671104473
  },
  {
    id: 7,
    name: "Nine",
    age: 23,
    last_login: 1647344473
  },
  {
    id: 8,
    name: "Ayaka",
    age: 25,
    last_login: 1660563673
  },
  {
    id: 9,
    name: "Inga",
    age: 10,
    last_login: 997872073
  },
  {
    id: 10,
    name: "Eleonora",
    age: 35,
    last_login: 1052995273
  },
  {
    id: 11,
    name: "Oink",
    age: 40,
    last_login: 1052995273
  },
  {
    id: 12,
    name: "Igdrasil",
    age: 60,
    last_login: 1053254473
  },
  {
    id: 13,
    name: "Loki",
    age: 35,
    last_login: 1053255013
  },
  {
    id: 14,
    name: "Noah",
    age: 2,
    last_login: 1274179813
  },
  {
    id: 15,
    name: "Kjaria",
    age: 8,
    last_login: 1647604213
  },
  {
    id: 16,
    name: "Minchia",
    age: 9,
    last_login: 1696178729
  },
  {
    id: 17,
    name: "Laila",
    age: 31,
    last_login: 1267451127
  },
  {
    id: 18,
    name: "Polpo",
    age: 38,
    last_login: 1267451127
  },
  {
    id: 19,
    name: "Scent",
    age: 12,
    last_login: 1000212327
  },
  {
    id: 20,
    name: "Odin",
    age: 5,
    last_login: 1005313527
  },
  {
    id: 21,
    name: "Landish",
    age: 25,
    last_login: 1173447927
  },
  {
    id: 22,
    name: "Bordless",
    age: 19,
    last_login: 1363178727
  },
  {
    id: 23,
    name: "Cucchiaio",
    age: 7,
    last_login: 1173793527
  },
  {
    id: 24,
    name: "Snowflake",
    age: 11,
    last_login: 1179060327
  },
  {
    id: 25,
    name: "Kjaria",
    age: 8,
    last_login: 1647604213
  }
])

let sliceStartIndex = ref(0);

let pageCounter = ref(5);

function ChangePage(startItemIndex:number) {
  sliceStartIndex.value = startItemIndex;
}

function BackButtonFunction() {
  if (sliceStartIndex.value > 0) {
    sliceStartIndex.value-=5;
  }
}

let nextButtonLimit = ref(20);

function NextButtonFunction() {
  if (sliceStartIndex.value < nextButtonLimit.value) {
    sliceStartIndex.value+=5;
  }
}

const query = ref('');

let queryUsers = computed(() => {
  let result = data.value.filter((user) => user.name.indexOf(query.value) !== -1);
  result.sort((user1, user2) => user1.age - user2.age)
  nextButtonLimit.value = result.length-5;
  pageCounter.value = result.length/5;
  result = result.slice(sliceStartIndex.value, sliceStartIndex.value + 5);
  return result;
})

</script>

<template>
  <header>
    Search by name
    <input 
      type="search" 
      placeholder="Start tiping in name" 
      v-model="query" 
    >
  </header>
  <main>
    <div class="users">
      <div 
        class="users__item" 
        v-for="user in queryUsers" 
        :key="user.id"
      >
        <UserItem 
          :user_id=user.id
          :user_name=user.name
          :user_age=user.age
          :user_last_login=TimestampConverter(user.last_login)
        />
        <button>Add user</button>
        <button>Customise</button>
        <button>Delete users</button>
      </div>
    </div>
  </main>
  <div class="slider">
    <button class="slider__back" v-if="pageCounter>1" @click=BackButtonFunction()>Back</button>
    <div class="slider__numbered-buttons">
      <button class="slider__page1" @click=ChangePage(0)>1</button>
      <button class="slider__page2" v-if="pageCounter>1" @click=ChangePage(5)>2</button>
      <button class="slider__page3" v-if="pageCounter>2" @click=ChangePage(10)>3</button>
      <button class="slider__page4" v-if="pageCounter>3" @click=ChangePage(15)>4</button>
      <button class="slider__page5" v-if="pageCounter>4" @click=ChangePage(20)>5</button>
    </div>
    <button class="slider__next" v-if="pageCounter>1" @click=NextButtonFunction()>Next</button>
  </div>
</template>

<style scoped>

header {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-top: 20px;
  margin-bottom: 20px;
}

header button {
  font-size: 20px;
}

.name-filter {
  margin: 10px;
}

.users {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

.users__item {
  border: 1px black solid;
}

.slider {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
  margin-top: 50px;
}
</style>
