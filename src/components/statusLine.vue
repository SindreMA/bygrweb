<template>
  <div class="flex">
    <div
      class="circle"
      :class="{'bg-ligthGreen': status.toLowerCase() === 'sent', 'bg-ligthRed': status.toLowerCase() == 'not sent', 'bg-ligthYellow': status.toLowerCase()  == 'in progress'}"
    ></div>
    <div class="centerText">{{status}}</div>
  </div>
</template>

<script>
export default {
  props: ["status"]
};
</script>

<style scoped>
.circle {
  height: 16px;
  width: 16px;
  border-radius: 100%;
  margin-right: 5px;
}
.bg-ligthGreen {
  background-color: #b8de91;
}
.bg-ligthYellow {
  background-color: #fbd277;
}
.bg-ligthRed {
  background-color: #fb9683;
}
.centerText {
  text-align: center;
}
</style>
