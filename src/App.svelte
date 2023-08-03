<script lang="ts">
  import { onMount } from 'svelte'
  import WebFont from 'webfontloader'
  import fontCSS from '/font.css?url'
  import baseImg from './assets/base.png'
  import avatar from './assets/avatar.png'

  type Member = {
    name: string,
    avatarFiles?: FileList,
    avatarImg?: HTMLCanvasElement,
  }

  let fontLoaded: boolean = false
  let canvas: HTMLCanvasElement
  let defaultAvatar: HTMLCanvasElement
  let member0: Member = { name: '副社長' }
  let member1: Member = { name: '部下A' }
  let member2: Member = { name: '部下B' }
  let member3: Member = { name: '部下C' }
  let output: string

  const loadImage = (url: string) => (
    new Promise<HTMLImageElement>((resolve, _) => {
      const img = new Image()
      img.src = url
      img.onload = () => {
        resolve(img)
      }
    })
  )

  const loadAvatar = (member: Member) => (
    new Promise((resolve, _) => {
      if (!member.avatarFiles) {
        resolve(null)
        return
      }
      const reader = new FileReader()
      reader.onload = async () => {
        const img = await loadImage(reader.result as string)
        member.avatarImg = clipCircle(img)
        resolve(null)
      }
      reader.readAsDataURL(member.avatarFiles[0])
    })
  )

  const clipCircle = (img: HTMLImageElement) => {
    const cvs = document.createElement('canvas')
    cvs.width = 86
    cvs.height = 86
    const ctx = cvs.getContext('2d')
    ctx.beginPath()
    ctx.arc(43, 43, 43, 0, 2 * Math.PI, false)
    ctx.strokeStyle = 'rgba(0, 0, 0, 0)'
    ctx.stroke()
    ctx.clip()
    if (img.width > img.height) {
      ctx.drawImage(img, 43 - (img.width / img.height * 43), 0, img.width / img.height * 86, 86)
    } else {
      ctx.drawImage(img, 0, 43 - (img.height / img.width * 43), 86, img.height / img.width * 86)
    }
    return cvs
  }

  const render = async () => {
    output = ''
    if (!defaultAvatar) {
      defaultAvatar = clipCircle(await loadImage(avatar))
    }
    if (fontLoaded && canvas) {
      const img = await loadImage(baseImg)
      canvas.width = img.width
      canvas.height = img.height
      const ctx = canvas.getContext('2d')
      ctx.font = 'bold 30px \'SF Pro Text\', sans-serif'
      ctx.fillStyle = '#2b3039'
      ctx.drawImage(img, 0, 0)
      ctx.fillText(member0.name, 175, 170)
      ctx.fillText(member1.name, 175, 610)
      ctx.fillText(member2.name, 175, 833)
      ctx.fillText(member3.name, 175, 1056)
      await loadAvatar(member0)
      await loadAvatar(member1)
      await loadAvatar(member2)
      await loadAvatar(member3)
      ctx.drawImage(member0.avatarImg || defaultAvatar, 70, 142, 86, 86)
      ctx.drawImage(member1.avatarImg || defaultAvatar, 70, 582, 86, 86)
      ctx.drawImage(member2.avatarImg || defaultAvatar, 70, 805, 86, 86)
      ctx.drawImage(member3.avatarImg || defaultAvatar, 70, 1028, 86, 86)
      output = canvas.toDataURL()
    }
  }

  onMount(() => {
    WebFont.load({
      classes: false,
      custom: {
        families: ['SF Pro Text'],
        urls: [fontCSS]
      },
      active: () => {
        fontLoaded = true
        render()
      },
    })
  })
</script>

<main class="max-w-lg mx-auto p-6 flex flex-col gap-3">
  <h1 class="text-2xl font-bold text-center">副社長構文ジェネレーター</h1>
  <div class="join w-full">
    <label class="btn btn-primary join-item">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>
      <input type="file" accept="image/*" bind:files={member0.avatarFiles} class="hidden" />
    </label>
    <input bind:value={member0.name} class="input input-bordered input-primary join-item grow" />
  </div>
  <div class="join w-full">
    <label class="btn btn-primary join-item">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>
      <input type="file" accept="image/*" bind:files={member1.avatarFiles} class="hidden" />
    </label>
    <input bind:value={member1.name} class="input input-bordered input-primary join-item grow" />
  </div>
  <div class="join w-full">
    <label class="btn btn-primary join-item">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>
      <input type="file" accept="image/*" bind:files={member2.avatarFiles} class="hidden" />
    </label>
    <input bind:value={member2.name} class="input input-bordered input-primary join-item grow" />
  </div>
  <div class="join w-full">
    <label class="btn btn-primary join-item">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>
      <input type="file" accept="image/*" bind:files={member3.avatarFiles} class="hidden" />
    </label>
    <input bind:value={member3.name} class="input input-bordered input-primary join-item grow" />
  </div>
  <button on:click={render} class="btn btn-primary w-full">生成</button>
  <canvas bind:this={canvas} class="hidden" />
  {#if output}
    <img src={output} alt="outputImage" />
    <a href={output} download="">
      <button class="btn btn-primary w-full">ダウンロード</button>
    </a>
  {/if}
</main>
