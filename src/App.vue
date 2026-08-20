<script setup lang="ts">
import { computed, ref } from 'vue'

type Round = { label: string; title: string; subtitle: string; colors: [string, string, string]; shape: string; question: string; correct: string; options: string[] }
type Answer = { round: number; selected: string; correct: string }

const rounds: Round[] = [
  { label: 'INK / 01', title: 'quiet\nform', subtitle: 'The beauty of less, considered slowly', colors: ['#111110', '#e9dfc9', '#b8aa8e'], shape: 'orb', question: '這張黑色封面，最接近哪一種感覺？', correct: '安靜、經典，像一本未完的書', options: ['安靜、經典，像一本未完的書', '有膽量的熱度，漂亮而果敢'] },
  { label: 'FERN / 02', title: 'wild\nquiet', subtitle: 'A field guide to the hidden green', colors: ['#181b18', '#9cab86', '#465445'], shape: 'leaf', question: '這張黑色封面，最接近哪一種感覺？', correct: '克制、自然，帶一點呼吸感', options: ['神秘又華麗的午夜想像', '克制、自然，帶一點呼吸感'] },
  { label: 'AMBER / 03', title: 'last\nlight', subtitle: 'Stories from the golden hour after dark', colors: ['#161411', '#d49a42', '#6e4c1e'], shape: 'sun', question: '這張黑色封面，最接近哪一種感覺？', correct: '像深夜酒吧的一束燈', options: ['像深夜酒吧的一束燈', '冷靜俐落的城市夜色'] },
  { label: 'NOIR / 04', title: 'dark\nromance', subtitle: 'A softer kind of beautiful trouble', colors: ['#171315', '#c5757e', '#6d343d'], shape: 'petal', question: '這張黑色封面，最接近哪一種感覺？', correct: '浪漫裡藏著一點危險', options: ['俏皮卻仍然很有份量', '浪漫裡藏著一點危險'] },
  { label: 'SIGNAL / 05', title: 'new\nfrequency', subtitle: 'A brighter signal in the dark', colors: ['#0d1017', '#5db8d5', '#254d68'], shape: 'signal', question: '這張黑色封面，最接近哪一種感覺？', correct: '清醒、前衛的未來感', options: ['清醒、前衛的未來感', '溫暖、有手感的成熟魅力'] },
  { label: 'CHARCOAL / 06', title: 'slow\nheat', subtitle: 'Objects with a little life in them', colors: ['#252321', '#bd8057', '#6e5141'], shape: 'rings', question: '這張黑色封面，最接近哪一種感覺？', correct: '溫暖、有手感的成熟魅力', options: ['溫暖、有手感的成熟魅力', '最純粹的對比，永遠不過時'] },
  { label: 'VIOLET / 07', title: 'nocturne\nnotes', subtitle: 'A little magic after the sun goes down', colors: ['#151219', '#a98ad0', '#513b6a'], shape: 'moon', question: '這張黑色封面，最接近哪一種感覺？', correct: '神秘又華麗的午夜想像', options: ['神秘又華麗的午夜想像', '叛逆、直接，不怕被看見'] },
  { label: 'ACID / 08', title: 'stay\nstrange', subtitle: 'Good taste does not have to behave', colors: ['#10130f', '#b6d94d', '#587026'], shape: 'acid', question: '這張黑色封面，最接近哪一種感覺？', correct: '叛逆、直接，不怕被看見', options: ['叛逆、直接，不怕被看見', '安靜、經典，像一本未完的書'] },
  { label: 'HEAT / 09', title: 'burning\npoint', subtitle: 'A warm pulse beneath the surface', colors: ['#191210', '#e8785d', '#7f3b2e'], shape: 'flare', question: '這張黑色封面，最接近哪一種感覺？', correct: '有膽量的熱度，漂亮而果敢', options: ['有膽量的熱度，漂亮而果敢', '克制、自然，帶一點呼吸感'] },
  { label: 'ABSOLUTE / 10', title: 'black &\nwhite', subtitle: 'The timeless issue, stripped back', colors: ['#050505', '#f7f5ef', '#8b8b86'], shape: 'contrast', question: '這張黑色封面，最接近哪一種感覺？', correct: '最純粹的對比，永遠不過時', options: ['像深夜酒吧的一束燈', '最純粹的對比，永遠不過時'] },
]

const round = ref(1)
const selected = ref<string | null>(null)
const gameOver = ref(false)
const answerHistory = ref<Answer[]>([])
const current = computed(() => rounds[round.value - 1])
const progress = computed(() => `${String(Math.min(round.value, rounds.length)).padStart(2, '0')} / ${rounds.length}`)
const score = computed(() => answerHistory.value.filter((answer) => answer.selected === answer.correct).length * 100)

function choose(option: string) {
  if (selected.value || gameOver.value) return
  selected.value = option
  answerHistory.value.push({ round: round.value, selected: option, correct: current.value.correct })
}

function nextRound() {
  if (round.value >= rounds.length) { gameOver.value = true; return }
  round.value += 1
  selected.value = null
}

function restart() {
  round.value = 1
  selected.value = null
  gameOver.value = false
  answerHistory.value = []
}
</script>

<template>
  <main>
    <nav aria-label="主要導覽">
      <a class="brand" href="./" aria-label="Blackroom home"><span class="brand-mark">B</span><span>black<span class="brand-dot">.</span>room</span></a>
      <div class="nav-meta"><span class="live-dot"></span> 黑色風格小遊戲</div>
      <div class="top-score"><span>{{ gameOver ? '分數' : '本回合' }}</span><strong>{{ gameOver ? score.toString().padStart(4, '0') : progress }}</strong></div>
    </nav>

    <section v-if="!gameOver" class="game-shell">
      <header class="game-heading">
        <div><p class="eyebrow">ROUND {{ progress }}</p><h1>{{ current.question }}</h1><p class="question-hint">看著這張圖，選一個最貼近的 mood。</p></div>
        <div class="streak"><span>✦</span> 黑色有很多種</div>
      </header>
      <div class="progress-track" aria-hidden="true"><span :style="{ width: `${round / rounds.length * 100}%` }"></span></div>

      <article class="style-card" :style="{ '--ink': current.colors[0], '--accent': current.colors[1], '--paper': current.colors[2] }">
        <div class="style-card-top"><span>{{ current.label }}</span><span>BLACKROOM PRESS</span></div>
        <div class="visual" :class="`visual-${current.shape}`" aria-label="黑色風格封面圖" role="img"><i></i><b></b><strong></strong></div>
        <div class="style-card-copy"><p>{{ current.subtitle }}</p><h2>{{ current.title }}</h2></div>
        <div class="style-card-bottom"><span>VOL. 2026</span><span>01 / 01</span></div>
      </article>

      <div class="choices-heading"><span>選出你的答案</span><span>每題 100 分</span></div>
      <div class="choices" role="group" aria-label="感覺選項">
        <button v-for="(option, index) in current.options" :key="option" class="choice" :class="{ chosen: selected === option, correct: selected === option && option === current.correct, wrong: selected === option && option !== current.correct }" type="button" @click="choose(option)">
          <span class="choice-letter">{{ String.fromCharCode(65 + index) }}</span><strong>{{ option }}</strong><span v-if="selected === option" class="choice-result">{{ option === current.correct ? '✓' : '×' }}</span>
        </button>
      </div>
      <div v-if="selected" class="answer-row" aria-live="polite"><strong :class="selected === current.correct ? 'answer-good' : 'answer-bad'">{{ selected === current.correct ? '答對了！這個感覺很準。' : `再想想，這張圖更像「${current.correct}」。` }}</strong><button class="next-button" type="button" @click="nextRound">{{ round === rounds.length ? '查看結果' : '下一題' }} <span>→</span></button></div>
    </section>

    <section v-else class="finish-shell">
      <p class="eyebrow">遊戲完成 · 共 {{ rounds.length }} 題</p><h1>你的黑色<br /><em>風格報告。</em></h1>
      <div class="final-score"><span>最後得分</span><strong>{{ score.toString().padStart(4, '0') }}</strong><small>10 題答對 {{ score / 100 }} 題</small></div>
      <div class="review-list"><article v-for="answer in answerHistory" :key="answer.round" class="review-card"><p>0{{ answer.round }} · {{ rounds[answer.round - 1].title.replace('\n', ' ') }}</p><div><span>你的選擇</span><strong>{{ answer.selected }}</strong></div><div><span>風格答案</span><strong>{{ answer.correct }}</strong></div><b :class="answer.selected === answer.correct ? 'is-correct' : 'is-wrong'">{{ answer.selected === answer.correct ? '✓ 這個情境判斷很準。' : '→ 下次再感受看看。' }}</b></article></div>
      <button class="restart-button" type="button" @click="restart">再玩一次 <span>↗</span></button>
    </section>
    <footer><span>© 2026 black.room</span><span>每一種黑，都有自己的故事。</span><span>10 題 · 輕鬆玩一下</span></footer>
  </main>
</template>
