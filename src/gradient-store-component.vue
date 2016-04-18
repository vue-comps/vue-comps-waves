// out: ..
<template lang="jade">
svg(xmlns="http://www.w3.org/2000/svg",style="position:absolute;opacity:0;visibility:hidden;pointer-events:none")
  defs
    radialGradient(:id="hash" cx="50%" cy="50%" r="75%" v-for="(color,hash) in colors")
      stop(offset="0%",style="stop-opacity:0.2;",:style="{stopColor:color}")
      stop(offset="40%",style="stop-opacity:0.3;",:style="{stopColor:color}")
      stop(offset="50%",style="stop-opacity:0.4;",:style="{stopColor:color}")
      stop(offset="60%",style="stop-opacity:0.5;",:style="{stopColor:color}")
      stop(offset="70%",style="stop-color:rgb(255,255,255);stop-opacity:0")
</template>

<script lang="coffee">
module.exports =

  data: ->
    colors: {}

  el: -> document.createElement "div"

  compiled: ->
    @$appendTo('body')

  beforeDestroy: ->
    @$remove()

  methods:
    getHash: (str) ->
      hash = 0
      for chr in str
        chr   = chr.charCodeAt()
        hash  = ((hash << 5) - hash) + chr
        hash |= 0
      return hash
    getId: (color) ->
      return @colors[color] if @colors[color]
      @$set("colors.#{color}", @getHash(color))
      return @colors[color]


</script>
