<template lang="pug">
.container
  p
    a(href="https://github.com/vue-comps/vue-comps-waves/blob/master/dev/basic.vue") source
  waves
    button.btn black ripple
  waves(
    color="blue"
    style="margin-left:20px"
    )
    button.btn blue ripple

  waves(
    color="white"
    style="margin-left:20px"
    )
    button.btn white ripple
  br
  waves(
    color="red"
    style="width:100px;height:100px;background-color:blue"
    )
  waves(
    color="blue"
    style="margin-left:20px"
    )
    div(style="width:100px;height:100px;background-color:red")
</template>

<script lang="coffee">
require "hammer-timejs"
module.exports =
  mixins:[
    require("vue-mixins/vue")
  ]
  beforeCompile: ->
    @Vue.use(require('vue-touch'))
  components:
    "waves" : require "../src/waves.vue"

</script>
