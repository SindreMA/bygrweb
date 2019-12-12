<template>
    <div class="flex">
        <div class="circle" :class="{'bg-ligthGreen': status.toLowerCase() === 'sent', 'bg-ligthRed': status.toLowerCase() == 'not sent', 'bg-ligthYellow': status.toLowerCase()  == 'in progress'}"></div>
        <div class="centerText"> {{status}}</div>
    </div>
</template>

<script>
export default {
    props: ['status']
}
</script>

<style scoped>
.circle {
    height: 16px;
    width: 16px;
    border-radius: 100%;
    margin-right: 5px;
}
.bg-ligthGreen {
    background-color: #B8DE91;
}
.bg-ligthYellow {
    background-color: #FBD277;
}
.bg-ligthRed {
    background-color: #FB9683;
}
.centerText {
    text-align: center;
}
</style>