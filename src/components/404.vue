<template>
  <div>
    <div class="container">
      <h1>404</h1>
      <p>Oops! The page you requested was not found.</p>
      <a href="/index">Go back to the homepage</a>
    </div>
  </div>

</template>

<style scoped>
body {
  font-family: 'Helvetica Neue', sans-serif;
  font-size: 14px;
  line-height: 1.5;
  color: #333;
  background-color: #f5f5f5;
}

.container {
  margin: 0 auto;
  max-width: 800px;
  padding-top: 25vh;
  /* padding: 70px; */
  text-align: center;
}

h1 {
  font-size: 72px;
  margin-bottom: 20px;
  color: #333;
}

p {
  font-size: 24px;
  margin-bottom: 30px;
}

a {
  display: inline-block;
  padding: 10px 20px;
  font-size: 18px;
  font-weight: bold;
  background-color: #333;
  color: #fff;
  text-decoration: none;
  border-radius: 5px;
}

a:hover {
  background-color: #555;
}
</style>

<script>
export default {
  name: "404"
}
</script>
