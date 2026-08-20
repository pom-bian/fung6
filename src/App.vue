<script setup lang="ts">
import { computed, ref } from 'vue'

type Palette = { name: string; mood: string; label: string; colors: [string, string, string]; title: string; subtitle: string; number: string }

const palettes: Palette[] = [
  { name: '墨黑 × 象牙', mood: '安靜、經典，像一本未完的書', label: 'INK', colors: ['#111110', '#e9dfc9', '#b8aa8e'], title: 'quiet\nform', subtitle: 'The beauty of less, considered slowly', number: '01' },
  { name: '黑曜 × 銀灰', mood: '冷靜俐落的城市夜色', label: 'OBSIDIAN', colors: ['#111315', '#a8b0b5', '#3d454b'], title: 'after\nhours', subtitle: 'A study in midnight and movement', number: '02' },
  { name: '炭黑 × 銅棕', mood: '溫暖、有手感的成熟魅力', label: 'CHARCOAL', colors: ['#252321', '#bd8057', '#6e5141'], title: 'slow\nheat', subtitle: 'Objects with a little life in them', number: '03' },
  { name: '煤黑 × 玫瑰', mood: '浪漫裡藏著一點危險', label: 'NOIR', colors: ['#171315', '#c5757e', '#6d343d'], title: 'dark\nromance', subtitle: 'A softer kind of beautiful trouble', number: '04' },
  { name: '黑 × 琥珀', mood: '像深夜酒吧的一束燈', label: 'AMBER', colors: ['#161411', '#d49a42', '#6e4c1e'], title: 'last\nlight', subtitle: 'Stories from the golden hour after dark', number: '05' },
  { name: '黑鐵 × 鼠尾草', mood: '克制、自然，帶一點呼吸感', label: 'FERN', colors: ['#181b18', '#9cab86', '#465445'], title: 'wild\nquiet', subtitle: 'A field guide to the hidden green', number: '06' },
  { name: '黑 × 電光藍', mood: '清醒、前衛的未來感', label: 'SIGNAL', colors: ['#0d1017', '#5db8d5', '#254d68'], title: 'new\nfrequency', subtitle: 'A brighter signal in the dark', number: '07' },
  { name: '烏黑 × 奶油黃', mood: '俏皮卻仍然很有份量', label: 'BOLD', colors: ['#171612', '#e6d36f', '#857a3e'], title: 'make\nnoise', subtitle: 'The playful issue, with extra contrast', number: '08' },
  { name: '黑 × 螢光綠', mood: '叛逆、直接，不怕被看見', label: 'ACID', colors: ['#10130f', '#b6d94d', '#587026'], title: 'stay\nstrange', subtitle: 'Good taste does not have to behave', number: '09' },
  { name: '黑 × 紫羅蘭', mood: '神秘又華麗的午夜想像', label: 'VIOLET', colors: ['#151219', '#a98ad0', '#513b6a'], title: 'nocturne\nnotes', subtitle: 'A little magic after the sun goes down', number: '10' },
  { name: '黑 × 珊瑚紅', mood: '有膽量的熱度，漂亮而果敢', label: 'HEAT', colors: ['#191210', '#e8785d', '#7f3b2e'], title: 'burning\npoint', subtitle: 'A warm pulse beneath the surface', number: '11' },
  { name: '純黑 × 純白', mood: '最純粹的對比，永遠不過時', label: 'ABSOLUTE', colors: ['#050505', '#f7f5ef', '#8b8b86'], title: 'black &\nwhite', subtitle: 'The timeless issue, stripped back', number: '12' },
]

const selected = ref(0)
const palette = computed(() => palettes[selected.value])
function choosePalette(index: number) { selected.value = index }
</script>

<template>
  <main :style="{ '--black': palette.colors[0], '--accent': palette.colors[1], '--paper': palette.colors[2] }">
    <header>
      <a class="wordmark" href="./" aria-label="Blackroom home"><i></i>blackroom</a>
      <p>一場黑色配對遊戲</p>
      <span>{{ String(selected + 1).padStart(2, '0') }} / 12</span>
    </header>
    <section class="intro">
      <div><p class="kicker">BLACK EDITION · 2026</p><h1>黑色，<em>不只一種個性。</em></h1></div>
      <p class="instruction">選一組黑色搭配，看看它會為一本書或一本雜誌帶來什麼樣的心情。</p>
    </section>
    <section class="playground" aria-label="Black palette matching game">
      <div class="cover-area">
        <div class="cover-shadow"></div>
        <article class="cover" :key="palette.label">
          <div class="cover-top"><span>{{ palette.label }}</span><span>VOL. {{ palette.number }}</span></div>
          <div class="cover-art" aria-hidden="true"><i></i><b></b><strong></strong></div>
          <div class="cover-copy"><p>{{ palette.subtitle }}</p><h2>{{ palette.title }}</h2></div>
          <div class="cover-bottom"><span>BLACKROOM PRESS</span><span>¥ 980</span></div>
        </article>
        <div class="cover-note"><span></span>{{ palette.mood }}</div>
      </div>
      <aside class="palette-panel">
        <p class="panel-label">MATCH A MOOD · 深 → 亮</p>
        <div class="palette-list" role="radiogroup" aria-label="選擇黑色組合">
          <button v-for="(item, index) in palettes" :key="item.label" class="palette-choice" :class="{ active: selected === index }" type="button" role="radio" :aria-checked="selected === index" @click="choosePalette(index)">
            <span class="choice-number">{{ String(index + 1).padStart(2, '0') }}</span>
            <span class="choice-dots"><i v-for="color in item.colors" :key="color" :style="{ background: color }"></i></span>
            <span class="choice-name">{{ item.name }}</span>
          </button>
        </div>
        <p class="hint">點擊配對，換一本封面。</p>
      </aside>
    </section>
    <footer><span>黑色封面研究室</span><span>每一種黑，都有自己的故事。</span></footer>
  </main>
</template>
