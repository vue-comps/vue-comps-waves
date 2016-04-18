# vue-comps-waves

Material design click animation. Done with svg and `Velocity.js`.
Depends on `vue-touch`.
Very lightweight if `Velocity.js` and `vue-touch` are present.

### [Demo](https://vue-comps.github.io/vue-comps-waves)

# Install

```sh
npm install --save-dev vue-comps-waves
```
or include `build/bundle.js`.

## Usage

```coffee
components:
  waves: require("vue-comps-waves")
  # or with bundle.js
  waves: window.vueComps.waves
]
```
```html
<waves>
  <button>Click me!</button>
</waves>
```

For examples see [`dev/`](dev/).

#### Props
| Name | type | default | description |
| ---:| --- | ---| --- |
| color | String | "black" | color of the effect |
| speed | Number | 0.37 | sped of the effect |


# Development
Clone repository
```sh
npm install
npm run dev
```
Browse to `http://localhost:8080/`.

## License
Copyright (c) 2016 Paul Pflugradt
Licensed under the MIT license.
