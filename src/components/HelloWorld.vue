<template>
  <div class="hello">
    <template v-for="v in 32">
      <ul :key="'_' + v">
        <li>
          <a href="/">1.Core Docs</a>
        </li>
      </ul>
    </template>

    <v-affix :offset-top="100">
      <div class="btn">TEST: fixed at the top 100px</div>
    </v-affix>

    <template v-for="v in 64">
      <ul :key="v">
        <li>
          <a href="/">2.Core Docs</a>
        </li>
      </ul>
    </template>

    <v-affix :offset-bottom="150">
      <div class="btn">TEST: fixed at the bottom 150px</div>
    </v-affix>

    <template v-for="v in 32">
      <ul :key="'v' + v">
        <li>
          <a href="/">3.Core Docs</a>
        </li>
      </ul>
    </template>
  </div>
</template>

<script>
import VAffix from "./VAffix";
export default {
  name: "HelloWorld",
  components: { VAffix }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

.btn {
  padding: 15px 40px;
  background: #444;
  color: #fff;
  text-align: center;
  display: inline-block;
}
</style>
