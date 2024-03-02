<script lang="ts">
  import { T } from '@threlte/core'
  import { AutoColliders } from '@threlte/rapier'
  import Picnic from '../lib/picnic.svelte'

</script>

<T.Group position={[0, -1, 0]}>
  <AutoColliders shape={'cuboid'}>
    <T.Mesh receiveShadow>
      <!----><T.BoxGeometry args={[10, -1, 10]} />
      <T.MeshStandardMaterial />
      <Picnic />
    </T.Mesh>  
  </AutoColliders>
</T.Group>
