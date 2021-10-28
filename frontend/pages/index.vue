<template></template>

<script lang="ts">
import Vue from 'vue'
import { Component } from 'vue-property-decorator'
import * as three from 'three'

@Component
export default class PageIndex extends Vue {
  scene: three.Scene | null = null
  globalCamera: three.PerspectiveCamera | null = null
  renderer: three.WebGLRenderer | null = null
  terrain: three.Mesh | null = null

  mounted() {
    // init threejs
    this.scene = new three.Scene()
    this.globalCamera = new three.PerspectiveCamera(
      100,
      window.innerWidth / window.innerHeight,
      0.01,
      10
    )
    this.scene.add(this.globalCamera)
    this.renderer = new three.WebGLRenderer({
      canvas: this.$refs.canvas! as HTMLCanvasElement,
    })
    this.renderer.setAnimationLoop(this.animation)
    this.renderer.setSize(window.innerWidth / 1.25, window.innerHeight / 1.25)
    document.body.appendChild(this.renderer.domElement)

    // setup terrain
    const terrainGeometry = new three.CylinderGeometry()
    const terrainMaterial = new three.MeshBasicMaterial()
    terrainMaterial.side = three.DoubleSide
    const edges = new three.EdgesGeometry(terrainGeometry)
    const line = new three.LineSegments(
      edges,
      new three.LineBasicMaterial({ color: 0x000000 })
    )
    this.scene.add(line)
    this.terrain = new three.Mesh(terrainGeometry, terrainMaterial)
    this.scene.add(this.terrain)
    this.initKeyDown()
  }
  animation() {
    this.renderer?.render(this.scene!, this.globalCamera!)
  }

  initKeyDown() {
    document.onkeydown = (e) => {
      if (e.key === 'ArrowUp') {
        this.globalCamera?.translateY(0.5)
      } else if (e.key === 'ArrowDown') {
        this.globalCamera?.translateY(-0.5)
      } else if (e.key === 'ArrowRight') {
        this.globalCamera?.translateX(0.5)
      } else if (e.key === 'ArrowLeft') {
        this.globalCamera?.translateX(-0.5)
      } else if (e.key === 'PageUp') {
        this.globalCamera?.translateZ(0.5)
      } else if (e.key === 'PageDown') {
        this.globalCamera?.translateZ(-0.5)
        console.debug(this.globalCamera.position.z)
      }
    }
  }
}
</script>
