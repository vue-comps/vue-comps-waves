// out: ..
<template lang="pug">
div(
  v-bind:style="{position:'relative',overflow:'hidden',display:'inline-block',touchAction:'auto'}"
  v-touch:tap="show"
  @mouseup="release"
  @mousedown="show"
  )
  div(
    style="position:absolute;pointer-events:none;transform:translate(-50%,-50%);border-radius:50%;line-height:0"
    v-el:ripple-div
    )
    svg(
      xmlns="http://www.w3.org/2000/svg"
      height=10
      width=10
      style="position:relative;opacity:0;pointer-events:none;"
      )
      rect(
        x="0"
        y="0"
        width="100%"
        height="100%"
        v-bind:fill="gradUrl"
        )
  slot
</template>

<script lang="coffee">
Velocity = require("velocity-animate")
GradientStore = require('./gradient-store')
module.exports =

  mixins: [
    require("vue-mixins/setCss")
    require("vue-mixins/vue")
  ]

  props:
    color:
      type: String
      default: "black"
    speed:
      type: Number
      default: 1

  computed:
    gradUrl: ->
      return null unless @getId?
      console.log "test"
      return "url(#"+@getId(@color)+")"


  data: ->
    getId: null
    gradUrl: null
    ripples: []
    debug: ""

  methods:
    show: (e) ->
      return if e.pointerType == "mouse"
      isTouch = e.pointerType? and e.pointerType != "mouse"
      e = e.srcEvent if isTouch
      if e.offsetX
        x = e.offsetX
        y = e.offsetY
      else
        pos = e.target.getBoundingClientRect()
        x = e.x - pos.left
        y = e.y - pos.top
      size = Math.max(@$el.offsetWidth-x,@$el.offsetHeight-y,x,y)*3
      rippleDiv = @$els.rippleDiv.cloneNode(true)
      @setCss(rippleDiv,"top",y+"px")
      @setCss(rippleDiv,"left",x+"px")
      @$el.appendChild(rippleDiv)
      duration = size/@speed/0.35
      rippleEl = rippleDiv.firstChild
      Velocity rippleEl,{opacity:0.5},duration:duration*1/2,queue:false
      Velocity rippleEl,{width:size,height:size},
        duration:duration
        easing:"easeIn"
        queue:false
      if isTouch
        setTimeout (=> @hide({rippleDiv:rippleDiv,duration:duration*1/2})),duration*1/2
      else
        ripple = {rippleDiv:rippleDiv,duration:duration*1/2,released: false,timeouted: false}
        @ripples.push ripple
        setTimeout (=>
          ripple.timeouted = true
          if ripple.released
            @hide(ripple)
        ),duration*1/2
    hide: (ripple) ->
      Velocity ripple.rippleDiv.firstChild,{opacity:0},
        duration:ripple.duration
        complete: =>
          ripple.rippleDiv.remove()
          @ripples.splice(@ripples.indexOf(ripple),1)
    release: ->
      lastRipple = @ripples[@ripples.length-1]
      if lastRipple?
        lastRipple.released = true
        if lastRipple.timeouted
          @hide(lastRipple)

  ready: ->
    @getId = GradientStore(@Vue).getId
</script>
