<script lang="ts">
  import { onMount, createEventDispatcher } from "svelte";
  import { T } from '@threlte/core'
  import { OrbitControls, AudioListener, interactivity, Environment } from '@threlte/extras'
  import { Debug } from '@threlte/rapier'
  import { Euler, Vector3 } from 'three'

  import Particle from './Particle.svelte'
  import Emitter from './Emitter.svelte'
  import Ground from './Ground.svelte'
  import Bg from '../lib/randomness.svelte'

  let dispatch = createEventDispatcher()
  const catched = () => {
        dispatch('won')
    }

  const { target } = interactivity()
  const getId = () => {
    return Math.random().toString(16).slice(2)
  }

  const getRandomPosition = () => {
    return new Vector3(0.5 - Math.random() * 1, 5 - Math.random() * 1 + 10, 0.5 - Math.random() * 1)
  }

  const getRandomRotation = () => {
    return new Euler(Math.random() * 10, Math.random() * 10, Math.random() * 10)
  }
  type Body = {
      id: string
      mounted: number
      position: Vector3
      rotation: Euler
    }
  const body: Body = {
        id: getId(),
        position: getRandomPosition(),
        rotation: getRandomRotation()
      }

  let timer = null
  let countdown = (Math.random()*(10-0 + 1))+0
  let isDelay = true
	onMount(() => {
    console.log ({countdown})
		timer = setInterval(() => {
			countdown -= 1
	  }, 1000)
	})

	$: {
    if (countdown < 1) {
      isDelay = false
			clearInterval(timer)
		}
	}
</script>


<T.PerspectiveCamera
  makeDefault
  position={[10, 10, 10]}
>
  <OrbitControls 
    enableZoom={false}
    enableRotate={false}
  <AudioListener />>
</T.PerspectiveCamera>


<T.DirectionalLight
  castShadow
  position={[8, 10, -3]}
/>

<T.GridHelper args={[10]} />

<Bg/>

<Ground />

<Debug />

<Emitter />

{#if !isDelay}
  <Particle on:click={catched}
      position={body.position}
      rotation={body.rotation}
    />
{/if}