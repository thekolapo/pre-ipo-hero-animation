<script setup>
import checkIcon from "~/assets/images/check.svg";
import openAiLogo from "~/assets/images/openai.svg";
import anthropicLogo from "~/assets/images/anthropic.svg";

let headingCharacterCount = 0;
const headingSegments = [
  { text: "Trade on ", highlight: false },
  { text: "OpenAI", highlight: true },
  { text: " and ", highlight: false },
  { text: "Anthropic", highlight: true },
  { text: " pre-IPO", highlight: false },
].map((segment) => {
  const startIndex = headingCharacterCount;
  const letters = Array.from(segment.text);
  headingCharacterCount += letters.length;

  return { ...segment, letters, startIndex };
});

const openAiBluePoints = [
  [0, 51.8162],
  [21.0005, 54.5502],
  [35.7312, 82.2728],
  [44.8495, 68.1829],
  [55.3705, 68.1829],
  [61.6831, 54.5502],
  [66.5929, 51.8162],
  [87.635, 61.8197],
  [96.7532, 49.0933],
  [111.483, 42.2773],
  [115.691, 59.5441],
  [130.421, 42.2773],
  [143.046, 46.8177],
  [152.865, 40],
  [154.786, 61.8197],
  [173.206, 54.5502],
  [183.727, 71.3609],
  [198.456, 82.2728],
  [198.456, 68.1829],
  [222.304, 61.8197],
  [246.853, 46.8177],
  [246.853, 82.2728],
  [264.268, 82.2728],
  [277.715, 54.5336],
  [288.236, 88.1676],
  [303.667, 100.894],
  [313.486, 78.6228],
  [319.799, 118.62],
  [331.723, 106.803],
  [355.174, 98.5933],
  [370.3, 98.5933],
];

const openAiPurplePoints = [
  [369.398, 98.7759],
  [382.163, 93.0999],
  [389.589, 84.9722],
  [399.982, 103.15],
  [399.982, 120.851],
  [413.346, 124.2],
  [417.305, 93.0999],
  [432.648, 93.0999],
  [467.294, 64.8783],
  [485.112, 81.6178],
  [500.455, 81.6178],
  [508.869, 71.5714],
  [526.687, 64.8783],
  [534.111, 74.9202],
  [534.111, 53.8706],
  [554.898, 40],
  [566.282, 59.6103],
  [579.645, 59.6103],
  [579.645, 84.9722],
  [593.998, 93.0999],
];

const anthropicBluePoints = [
  [0, 158.039],
  [7.30173, 158.039],
  [15.7543, 168.969],
  [23.0533, 166.236],
  [30.7364, 130.37],
  [38.4195, 162.141],
  [45.7185, 150.527],
  [54.9382, 158.039],
  [71.8411, 147.111],
  [86.8232, 162.141],
  [98.3479, 122.859],
  [107.952, 147.111],
  [115.251, 52.8374],
  [124.086, 138.232],
  [131.385, 152.92],
  [134.459, 112.272],
  [153.666, 77.4303],
  [164.039, 32],
  [179.021, 112.272],
  [188.625, 97.5738],
  [198.613, 178.87],
  [211.674, 158.039],
  [220.509, 210.979],
  [220.509, 134.464],
  [232.418, 100.306],
  [241.638, 88.6926],
  [246.632, 182.286],
  [253.547, 187.41],
  [260.846, 144.029],
  [268.145, 122.859],
  [275.828, 144.029],
  [288.505, 130.37],
  [301.951, 88.6926],
  [318.085, 174.771],
  [330.762, 127.291],
];

const anthropicPurplePoints = [
  [330.688, 129.057],
  [356.725, 121.332],
  [384.849, 125.013],
  [398.494, 106.998],
  [412.553, 121.332],
  [432.4, 95.2332],
  [438.189, 100.748],
  [440.256, 110.307],
  [450.593, 129.057],
  [467.96, 72.0702],
  [476.643, 75.0125],
  [484.499, 58.1001],
  [493.182, 62.1454],
  [508.1, 67.0029],
  [521.923, 67.0029],
  [531.797, 80.1718],
  [538.708, 100.748],
  [561.417, 121.332],
  [577.215, 115.289],
  [594, 95.2332],
];

const pointPrice = (y, highY, lowY, highPrice, lowPrice) => {
  return (
    highPrice -
    ((y - highY) / (lowY - highY)) * (highPrice - lowPrice)
  ).toFixed(2);
};

const updateCardTilt = (event) => {
  if (event.pointerType === "touch") return;

  const card = event.currentTarget;
  const maxTiltDegrees = 4.2;
  const bounds = card.getBoundingClientRect();
  const x = ((event.clientX - bounds.left) / bounds.width) * 2 - 1;
  const y = ((event.clientY - bounds.top) / bounds.height) * 2 - 1;

  card.style.setProperty("--card-tilt-x", `${-y * maxTiltDegrees}deg`);
  card.style.setProperty("--card-tilt-y", `${x * maxTiltDegrees}deg`);
};

const resetCardTilt = (event) => {
  event.currentTarget.style.setProperty("--card-tilt-x", "0deg");
  event.currentTarget.style.setProperty("--card-tilt-y", "0deg");
};
</script>

<template>
  <main class="hero">
    <div class="hero__copy">
      <h1
        class="hero__heading"
        aria-label="Trade on OpenAI and Anthropic pre-IPO"
      >
        <span
          v-for="segment in headingSegments"
          :key="segment.text"
          :class="[
            'hero__heading-segment',
            segment.highlight && 'hero__highlight',
          ]"
          aria-hidden="true"
        >
          <span
            v-for="(letter, index) in segment.letters"
            :key="`${segment.text}-${index}`"
            class="hero__heading-letter"
            :style="{
              animationDelay: `${120 + (segment.startIndex + index) * 45}ms`,
            }"
            >{{ letter === " " ? "\u00a0" : letter }}</span
          >
        </span>
      </h1>
      <p class="hero__body">
        Take a view on their potential IPO prices with a Spectre account — the
        new way to trade tax-free¹.
      </p>
      <ul class="hero__list">
        <li class="hero__list-item">
          <img :src="checkIcon" alt="checkmark" />
          <span>No capital gains</span>
        </li>
        <li class="hero__list-item">
          <img :src="checkIcon" alt="checkmark" />
          <span>No financing costs</span>
        </li>
        <li class="hero__list-item">
          <img :src="checkIcon" alt="checkmark" />
          <span>Go long without leverage</span>
        </li>
      </ul>
    </div>
    <button class="hero__cta">Get Spectre</button>
    <div class="hero__cards">
      <div
        class="hero__card"
        @pointermove="updateCardTilt"
        @pointerleave="resetCardTilt"
      >
        <div class="hero__card-top">
          <img :src="openAiLogo" alt="OpenAI" class="hero__card-logo" />
          <div class="hero__card-prices">
            <div class="hero__card-prices-values">
              <span>1,383.58</span>
              <span>1,383.74</span>
            </div>
            <div class="hero__card-prices-meta">
              <span>L: 1,383.58</span>
              <span>0.16</span>
              <span>H: 1,383.74</span>
            </div>
          </div>
        </div>
        <!--prettier-ignore-->
        <svg class="hero__card-graph" width="594" height="291" viewBox="0 0 594 291" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path class="hero__chart-line hero__chart-line--purple" pathLength="1" d="M369.398 98.7759L382.163 93.0999L389.589 84.9722L399.982 103.15V120.851L413.346 124.2L417.305 93.0999H432.648L467.294 64.8783L485.112 81.6178H500.455L508.869 71.5714L526.687 64.8783L534.111 74.9202V53.8706L554.898 40L566.282 59.6103H579.645V84.9722L593.998 93.0999" stroke="#B281F8" stroke-width="2"/>
          <path opacity="0.4" d="M382.177 93.2138L370.301 96.5666V301.016H594.001V93.2138L579.648 85.0864V59.7257H566.286L554.903 40.1162L534.116 53.9862V75.0349L526.693 64.9934L508.876 71.6862L500.462 81.7322H485.12L467.303 64.9934L432.659 93.2138H417.317L413.358 124.313L399.995 120.964V103.264L389.602 85.0864L382.177 93.2138Z" fill="url(#openai-purple-fill)"/>
          <path opacity="0.4" d="M21.0005 54.5527L0 51.8182V295H370.3V122.271V98.6037H355.174L331.723 106.814L319.799 118.634L313.486 78.6296L303.667 100.905L288.236 88.1761L277.715 54.5362L264.268 82.2803H246.853V46.8189L222.304 61.8236L198.456 68.1879V82.2803L183.727 71.3664L173.206 54.5527L154.786 61.8236L152.865 40L143.046 46.8189L130.421 42.2777L115.691 59.5475L111.483 42.2777L96.7532 49.0949L87.635 61.8236L66.5929 51.8182L61.6831 54.5527L55.3705 68.1879H44.8495L35.7312 82.2803L21.0005 54.5527Z" fill="url(#openai-blue-fill)"/>
          <path class="hero__chart-line hero__chart-line--blue" pathLength="1" d="M0 51.8162L21.0005 54.5502L35.7312 82.2728L44.8495 68.1829H55.3705L61.6831 54.5502L66.5929 51.8162L87.635 61.8197L96.7532 49.0933L111.483 42.2773L115.691 59.5441L130.421 42.2773L143.046 46.8177L152.865 40L154.786 61.8197L173.206 54.5502L183.727 71.3609L198.456 82.2728V68.1829L222.304 61.8197L246.853 46.8177V82.2728H264.268L277.715 54.5336L288.236 88.1676L303.667 100.894L313.486 78.6228L319.799 118.62L331.723 106.803L355.174 98.5933H370.3" stroke="#3E68FF" stroke-width="2"/>
          <defs>
          <linearGradient id="openai-purple-fill" x1="482.151" y1="40.1162" x2="482.151" y2="301.016" gradientUnits="userSpaceOnUse">
          <stop stop-color="#B281F8"/>
          <stop offset="1" stop-color="#B281F8" stop-opacity="0"/>
          </linearGradient>
          <linearGradient id="openai-blue-fill" x1="185.15" y1="40" x2="185.15" y2="295" gradientUnits="userSpaceOnUse">
          <stop stop-color="#3E68FF"/>
          <stop offset="1" stop-color="#708FFF" stop-opacity="0"/>
          </linearGradient>
          </defs>
          <g
            v-for="(point, index) in openAiBluePoints"
            :key="`openai-blue-${index}`"
            class="hero__chart-point hero__chart-point--blue"
            tabindex="0"
            role="img"
            :aria-label="`OpenAI price ${pointPrice(point[1], 40, 124.2, 1383.74, 1383.58)}`"
          >
            <circle class="hero__chart-point-hit" :cx="point[0]" :cy="point[1]" r="9" />
            <g
              class="hero__chart-tooltip"
              :transform="`translate(${Math.max(0, Math.min(point[0] - 36, 522))} ${Math.max(4, point[1] - 35)})`"
              aria-hidden="true"
            >
              <foreignObject width="72" height="25">
                <div xmlns="http://www.w3.org/1999/xhtml" class="hero__chart-tooltip-glass">
                  {{ pointPrice(point[1], 40, 124.2, 1383.74, 1383.58) }}
                </div>
              </foreignObject>
            </g>
          </g>
          <g
            v-for="(point, index) in openAiPurplePoints"
            :key="`openai-purple-${index}`"
            class="hero__chart-point hero__chart-point--purple"
            tabindex="0"
            role="img"
            :aria-label="`OpenAI price ${pointPrice(point[1], 40, 124.2, 1383.74, 1383.58)}`"
          >
            <circle class="hero__chart-point-hit" :cx="point[0]" :cy="point[1]" r="9" />
            <g
              class="hero__chart-tooltip"
              :transform="`translate(${Math.max(0, Math.min(point[0] - 36, 522))} ${Math.max(4, point[1] - 35)})`"
              aria-hidden="true"
            >
              <foreignObject width="72" height="25">
                <div xmlns="http://www.w3.org/1999/xhtml" class="hero__chart-tooltip-glass">
                  {{ pointPrice(point[1], 40, 124.2, 1383.74, 1383.58) }}
                </div>
              </foreignObject>
            </g>
          </g>
        </svg>
        <div class="hero__card-change">
          <span class="hero__card-change-icon" aria-hidden="true"></span>
          <span>+0.88%</span>
        </div>
      </div>
      <div
        class="hero__card"
        @pointermove="updateCardTilt"
        @pointerleave="resetCardTilt"
      >
        <div class="hero__card-top">
          <img :src="anthropicLogo" alt="Anthropic" class="hero__card-logo" />
          <div class="hero__card-prices">
            <div class="hero__card-prices-values">
              <span>1,714.42</span>
              <span>1,714.58</span>
            </div>
            <div class="hero__card-prices-meta">
              <span>L: 1,714.42</span>
              <span>0.16</span>
              <span>H: 1,714.58</span>
            </div>
          </div>
        </div>
        <!--prettier-ignore-->
        <svg class="hero__card-graph" width="594" height="302" viewBox="0 0 594 302" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path opacity="0.4" d="M7.30173 158.039H0V292.479H330.762V127.291L318.085 174.771L301.951 88.6926L288.505 130.37L275.828 144.029L268.145 122.859L260.846 144.029L253.547 187.41L246.632 182.286L241.638 88.6926L232.418 100.306L220.509 134.465V210.979L211.674 158.039L198.613 178.87L188.625 97.5738L179.021 112.272L164.039 32L153.666 77.4303L134.459 112.272L131.385 152.92L124.086 138.232L115.251 52.8374L107.952 147.111L98.3479 122.859L86.8232 162.141L71.8411 147.111L54.9382 158.039L45.7185 150.527L38.4195 162.141L30.7364 130.37L23.0533 166.236L15.7543 168.969L7.30173 158.039Z" fill="url(#anthropic-blue-fill)"/>
          <path class="hero__chart-line hero__chart-line--blue" pathLength="1" d="M0 158.039H7.30173L15.7543 168.969L23.0533 166.236L30.7364 130.37L38.4195 162.141L45.7185 150.527L54.9382 158.039L71.8411 147.111L86.8232 162.141L98.3479 122.859L107.952 147.111L115.251 52.8374L124.086 138.232L131.385 152.92L134.459 112.272L153.666 77.4303L164.039 32L179.021 112.272L188.625 97.5738L198.613 178.87L211.674 158.039L220.509 210.979V134.464L232.418 100.306L241.638 88.6926L246.632 182.286L253.547 187.41L260.846 144.029L268.145 122.859L275.828 144.029L288.505 130.37L301.951 88.6926L318.085 174.771L330.762 127.291" stroke="#3E68FF" stroke-width="2"/>
          <path opacity="0.4" d="M356.725 121.553L330.688 129.305V292.476H594V95.3628L577.215 115.489L561.417 121.553L538.708 100.897L531.797 80.2489L521.923 67.034H508.1L493.182 62.1596L484.499 58.1001L476.643 75.0715L467.96 72.119L450.593 129.305L440.256 110.489L438.189 100.897L432.4 95.3628L412.552 121.553L398.494 107.168L384.849 125.246L356.725 121.553Z" fill="url(#anthropic-purple-fill)"/>
          <path class="hero__chart-line hero__chart-line--purple" pathLength="1" d="M330.688 129.057L356.725 121.332L384.849 125.013L398.494 106.998L412.553 121.332L432.4 95.2332L438.189 100.748L440.256 110.307L450.593 129.057L467.96 72.0702L476.643 75.0125L484.499 58.1001L493.182 62.1454L508.1 67.0029H521.923L531.797 80.1718L538.708 100.748L561.417 121.332L577.215 115.289L594 95.2332" stroke="#B281F8" stroke-width="2"/>
          <defs>
            <linearGradient id="anthropic-blue-fill" x1="165.381" y1="32" x2="165.381" y2="292.479" gradientUnits="userSpaceOnUse">
              <stop stop-color="#3E68FF"/>
              <stop offset="1" stop-color="#708FFF" stop-opacity="0"/>
            </linearGradient>
            <linearGradient id="anthropic-purple-fill" x1="462.344" y1="58.1001" x2="462.344" y2="292.476" gradientUnits="userSpaceOnUse">
              <stop stop-color="#B281F8"/>
              <stop offset="1" stop-color="#B281F8" stop-opacity="0"/>
            </linearGradient>
          </defs>
          <g
            v-for="(point, index) in anthropicBluePoints"
            :key="`anthropic-blue-${index}`"
            class="hero__chart-point hero__chart-point--blue"
            tabindex="0"
            role="img"
            :aria-label="`Anthropic price ${pointPrice(point[1], 32, 210.979, 1714.58, 1714.42)}`"
          >
            <circle class="hero__chart-point-hit" :cx="point[0]" :cy="point[1]" r="9" />
            <g
              class="hero__chart-tooltip"
              :transform="`translate(${Math.max(0, Math.min(point[0] - 36, 522))} ${Math.max(4, point[1] - 35)})`"
              aria-hidden="true"
            >
              <foreignObject width="72" height="25">
                <div xmlns="http://www.w3.org/1999/xhtml" class="hero__chart-tooltip-glass">
                  {{ pointPrice(point[1], 32, 210.979, 1714.58, 1714.42) }}
                </div>
              </foreignObject>
            </g>
          </g>
          <g
            v-for="(point, index) in anthropicPurplePoints"
            :key="`anthropic-purple-${index}`"
            class="hero__chart-point hero__chart-point--purple"
            tabindex="0"
            role="img"
            :aria-label="`Anthropic price ${pointPrice(point[1], 32, 210.979, 1714.58, 1714.42)}`"
          >
            <circle class="hero__chart-point-hit" :cx="point[0]" :cy="point[1]" r="9" />
            <g
              class="hero__chart-tooltip"
              :transform="`translate(${Math.max(0, Math.min(point[0] - 36, 522))} ${Math.max(4, point[1] - 35)})`"
              aria-hidden="true"
            >
              <foreignObject width="72" height="25">
                <div xmlns="http://www.w3.org/1999/xhtml" class="hero__chart-tooltip-glass">
                  {{ pointPrice(point[1], 32, 210.979, 1714.58, 1714.42) }}
                </div>
              </foreignObject>
            </g>
          </g>
        </svg>
        <div class="hero__card-change hero__card-change--negative">
          <span
            class="hero__card-change-icon hero__card-change-icon--down"
            aria-hidden="true"
          ></span>
          <span>-0.21%</span>
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@use "../assets/styles/home";
</style>
