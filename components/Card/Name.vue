<script setup lang="ts">
  interface INameCardProps {
    name: string;
    index: number;
  }

  // Props
  const props = defineProps<INameCardProps>();

  // Emits
  const emit = defineEmits(["remove"]);

  // Methods
  const removeName = () => {
    emit("remove", props.index);
  }
</script>


<template>
  <div 
    class="card" 
  >
    <h4>{{ name }}</h4>
    <p @click="removeName">x</p>
  </div>
</template>

<style scoped>
  .card {
    background: rgb(27, 60, 138);
    width: 28%;
    color: white;
    border-radius: 1rem;
    padding: 0.1rem;
    margin-right: 0.5rem;
    margin-bottom: 1rem;
    position: relative;
  }

  .card p {
    position: absolute;
    top: -17%;
    left: 92.5%;
    cursor: pointer;
    color: rgba(255, 255, 255, 0.178);
  }
</style>