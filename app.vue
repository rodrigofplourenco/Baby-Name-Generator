<script setup lang="ts">
  import { Gender, Popularity, Length, names } from "@/data";

  // Types
  interface IOptions {
    gender: Gender;
    popularity: Popularity;
    length: Length;
  };

  // State
  const optionsArray = [
    {
      title: "1) Choose a gender",
      category: "gender",
      buttons: [
        Gender.BOY,
        Gender.UNISEX,
        Gender.GIRL
      ]
    },
    {
      title: "2) Choose the name's popularity",
      category: "popularity",
      buttons: [
        Popularity.TRENDY,
        Popularity.UNIQUE
      ]
    },
    {
      title: "3) Choose the name's length",
      category: "length",
      buttons: [
        Length.LONG,
        Length.ALL,
        Length.SHORT
      ]
    }
  ];

  const options = reactive<IOptions>({
    gender: Gender.GIRL,
    popularity: Popularity.UNIQUE,
    length: Length.LONG
  });

  const generatedNames = ref<String[]>([]);

  // Methods
  const computeGeneratedNames = () => {
    const filteredNames = names
      .filter((name) => name.gender === options.gender)
      .filter((name) => name.popularity === options.popularity)
      .filter((name) => {
        if (options.length === Length.ALL)
          return true 

        return name.length === options.length
      })
      .map((name) => name.name);

    generatedNames.value = filteredNames;
  }

  const removeName =(index) => {
    const filteredNames = [...generatedNames.value];

    filteredNames.splice(index, 1);

    generatedNames.value = filteredNames;
  }
</script>

<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button below</p>

    <div class="options-container">
      <Option 
        v-for="option in optionsArray" 
        :key="option.title"
        :option="option"
        :options="options"
      />

      <button 
        class="primary"
        @click="computeGeneratedNames"
      >
        Find Names
      </button>
    </div>

    <div class="cards-container">
      <CardName 
        v-for="(name, index) in generatedNames"
        :key="name"
        :name="name"
        :index="index"
        @remove="removeName"
      />
    </div>
  </div>
</template>

<style scoped>
  * {
    box-sizing: border-box;
  }

  .container {
    font-family: Arial, Helvetica, sans-serif;
    color: rgb(27, 60, 138);

    max-width: 50rem;
    margin: 0 auto;
    text-align: center;
  }

  .container h1 {
    font-size: 3rem;
  }

  .container .options-container {
    background: rgb(255, 238, 236);
    border-radius: 2rem;
    padding: 1rem;
    width: 95%;
    margin: 0 auto;
    margin-top: 4rem;
    position: relative;
  }

  .options-container .primary {
    background: rgb(249, 87, 89);
    color: white;
    border-radius: 6.5rem;
    border: none;
    padding: 0.75rem 4rem;
    font-size: 1rem;
    margin-top: 1rem;
    cursor: pointer;
  }

  .container .cards-container {
    display: flex;
    justify-content: center;
    margin-top: 3rem;
    flex-wrap: wrap;
  }
</style>
