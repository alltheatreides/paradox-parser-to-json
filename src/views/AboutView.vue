<template>
   <div class="about">
      <h1 class="title">About</h1>
      <p>
         This website was made for the
         <a target="_blank" href="https://gitlab.com/ave-maria"
            >AVE MARIA Project</a
         >
         by
         <a target="_blank" href="https://github.com/alltheatreides"
            >Atreides</a
         >
      </p>
   </div>
</template>

<script>
export default {
   mounted() {
      document.title = "JSON Paradox | About";
   },
};
</script>

<style>
.title {
   border-bottom: 2px solid var(--color-background-mute);
}
@media (min-width: 1024px) {
   .about {
      /* min-height: 100vh; */
      display: flex;
      flex-direction: column;
      /* margin-top: auto;
      margin-bottom: auto; */
      margin: auto;
      width: 100%;
      gap: 1rem;
      /* align-items: center; */
   }
}
</style>
