<script setup>
import { defineComponent, h, onMounted, onUnmounted, ref } from 'vue'

const mouseX = ref(0)
const mouseY = ref(0)
const scrollY = ref(0)
const introProgress = ref(0)
const introComplete = ref(false)
const heroTitle = ref(null)
const introMotion = ref({
  startX: 0,
  startY: 0,
  targetX: 0,
  targetY: 0,
  viewportWidth: 1,
  viewportHeight: 1,
  titleWidth: 1,
  titleHeight: 1,
  fontSize: 92,
  lineHeight: 99,
  startScale: 2.6,
  endScale: 1,
})

const handleMouseMove = (e) => {
  mouseX.value = e.clientX
  mouseY.value = e.clientY
}

const handleScroll = () => {
  scrollY.value = window.scrollY
}

const syncIntroMotion = () => {
  const targetRect = heroTitle.value?.getBoundingClientRect()
  const titleStyle = heroTitle.value
    ? window.getComputedStyle(heroTitle.value)
    : null
  const titleWidth = targetRect?.width || 1
  const titleHeight = targetRect?.height || 1
  const fontSize = titleStyle ? parseFloat(titleStyle.fontSize) : 92
  const parsedLineHeight = titleStyle ? parseFloat(titleStyle.lineHeight) : NaN
  const lineHeight = Number.isFinite(parsedLineHeight) ? parsedLineHeight : fontSize * 1.08
  const startX = window.innerWidth / 2
  const startY = window.innerHeight / 2
  const startScale = window.innerWidth <= 720
    ? 1.8
    : window.innerWidth <= 1120
      ? 2.2
      : 2.65

  introMotion.value = {
    startX: startX - (titleWidth * startScale) / 2,
    startY: startY - (titleHeight * startScale) / 2,
    targetX: targetRect ? targetRect.left : startX - titleWidth / 2,
    targetY: targetRect ? targetRect.top : startY - titleHeight / 2,
    viewportWidth: window.innerWidth,
    viewportHeight: window.innerHeight,
    titleWidth,
    titleHeight,
    fontSize,
    lineHeight,
    startScale,
    endScale: 1,
  }
}

const getIntroTitleTransform = () => {
  const progress = introProgress.value
  const motion = introMotion.value
  const x = motion.startX + (motion.targetX - motion.startX) * progress
  const y = motion.startY + (motion.targetY - motion.startY) * progress
  const scale = motion.startScale + (motion.endScale - motion.startScale) * progress

  return `translate(${x} ${y}) scale(${scale})`
}

onMounted(() => {
  syncIntroMotion()
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('scroll', handleScroll)
  window.addEventListener('resize', syncIntroMotion)

  // 开场动画：从屏幕中心的大标题收束到 Hero 标题的实际位置。
  const duration = 2200
  const startTime = performance.now()

  const animate = (now) => {
    const elapsed = now - startTime
    const progress = Math.min(elapsed / duration, 1)
    // easeInOutCubic
    const eased = progress < 0.5
      ? 4 * progress * progress * progress
      : 1 - Math.pow(-2 * progress + 2, 3) / 2
    introProgress.value = eased

    if (progress < 1) {
      requestAnimationFrame(animate)
    } else {
      introComplete.value = true
    }
  }

  requestAnimationFrame(animate)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('resize', syncIntroMotion)
})

const SectionTitle = defineComponent({
  props: {
    label: {
      type: String,
      required: true,
    },
    icon: {
      type: String,
      default: 'sparkles',
    },
  },
  setup(props) {
    return () => h('div', { class: 'section-title' }, [
      h(Icon, { name: props.icon, class: 'section-title-icon' }),
      h('h2', props.label),
    ])
  },
})

const iconNodes = {
  home: [['path', { d: 'M3 10.8 12 3l9 7.8' }], ['path', { d: 'M5 10v10h14V10' }], ['path', { d: 'M9 20v-6h6v6' }]],
  layers: [['path', { d: 'm12 3 9 5-9 5-9-5 9-5Z' }], ['path', { d: 'm3 13 9 5 9-5' }], ['path', { d: 'm3 18 9 5 9-5' }]],
  cpu: [['rect', { x: '7', y: '7', width: '10', height: '10', rx: '2' }], ['path', { d: 'M9 1v3M15 1v3M9 20v3M15 20v3M20 9h3M20 15h3M1 9h3M1 15h3' }]],
  notebook: [['path', { d: 'M4 4h14a2 2 0 0 1 2 2v14H6a2 2 0 0 1-2-2V4Z' }], ['path', { d: 'M8 4v16M11 9h5M11 13h5' }]],
  user: [['path', { d: 'M20 21a8 8 0 0 0-16 0' }], ['circle', { cx: '12', cy: '7', r: '4' }]],
  users: [['path', { d: 'M16 21a6 6 0 0 0-12 0' }], ['circle', { cx: '10', cy: '7', r: '4' }], ['path', { d: 'M22 21a5 5 0 0 0-5-5' }], ['path', { d: 'M17 3.4a4 4 0 0 1 0 7.2' }]],
  mail: [['rect', { x: '3', y: '5', width: '18', height: '14', rx: '2' }], ['path', { d: 'm3 7 9 6 9-6' }]],
  github: [['path', { d: 'M15 22v-4a4 4 0 0 0-1-3c3 0 6-2 6-6 0-1.5-.5-2.7-1.4-3.7.1-.3.6-1.8-.1-3.3 0 0-1.2-.4-3.5 1.3A12 12 0 0 0 12 3c-1 0-2 .1-3 .3C6.7 1.6 5.5 2 5.5 2c-.7 1.5-.2 3-.1 3.3A5.2 5.2 0 0 0 4 9c0 4 3 6 6 6-.4.4-.7 1-.8 1.8-1.4.6-4.2 1-5.2-1.8' }], ['path', { d: 'M9 18c-3 .9-3-1.5-4-2' }]],
  linkedin: [['path', { d: 'M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-4 0v7h-4v-7a6 6 0 0 1 6-6Z' }], ['rect', { x: '2', y: '9', width: '4', height: '12' }], ['circle', { cx: '4', cy: '4', r: '2' }]],
  arrowRight: [['path', { d: 'M5 12h14' }], ['path', { d: 'm13 5 7 7-7 7' }]],
  download: [['path', { d: 'M12 3v12' }], ['path', { d: 'm7 10 5 5 5-5' }], ['path', { d: 'M5 21h14' }]],
  sparkles: [['path', { d: 'm12 3 1.6 4.4L18 9l-4.4 1.6L12 15l-1.6-4.4L6 9l4.4-1.6L12 3Z' }], ['path', { d: 'm19 15 .8 2.2L22 18l-2.2.8L19 21l-.8-2.2L16 18l2.2-.8L19 15Z' }], ['path', { d: 'm5 14 1 2.5L9 17l-3 .5L5 20l-1-2.5L1 17l3-.5L5 14Z' }]],
  check: [['path', { d: 'm20 6-11 11-5-5' }]],
  box: [['path', { d: 'm21 8-9-5-9 5 9 5 9-5Z' }], ['path', { d: 'M3 8v8l9 5 9-5V8' }], ['path', { d: 'M12 13v8' }]],
  film: [['rect', { x: '3', y: '5', width: '18', height: '14', rx: '2' }], ['path', { d: 'M7 5v14M17 5v14M3 9h4M3 15h4M17 9h4M17 15h4' }]],
  server: [['rect', { x: '3', y: '4', width: '18', height: '7', rx: '2' }], ['rect', { x: '3', y: '13', width: '18', height: '7', rx: '2' }], ['path', { d: 'M7 8h.01M7 17h.01' }]],
  code: [['path', { d: 'm16 18 6-6-6-6' }], ['path', { d: 'm8 6-6 6 6 6' }], ['path', { d: 'm14 4-4 16' }]],
  wave: [['path', { d: 'M2 12h3l2-7 4 14 3-9 2 2h6' }]],
  sync: [['path', { d: 'M21 12a9 9 0 0 0-15-6.7L3 8' }], ['path', { d: 'M3 3v5h5' }], ['path', { d: 'M3 12a9 9 0 0 0 15 6.7L21 16' }], ['path', { d: 'M16 16h5v5' }]],
  lightbulb: [['path', { d: 'M9 18h6' }], ['path', { d: 'M10 22h4' }], ['path', { d: 'M8 14a6 6 0 1 1 8 0c-.7.6-1 1.4-1 2H9c0-.6-.3-1.4-1-2Z' }]],
  package: [['path', { d: 'm16.5 9.4-9-5.2' }], ['path', { d: 'M21 8v8l-9 5-9-5V8l9-5 9 5Z' }], ['path', { d: 'M3.3 7.3 12 12l8.7-4.7M12 22V12' }]],
  play: [['circle', { cx: '12', cy: '12', r: '10' }], ['path', { d: 'm10 8 6 4-6 4V8Z' }]],
  send: [['path', { d: 'm22 2-7 20-4-9-9-4 20-7Z' }], ['path', { d: 'M22 2 11 13' }]],
  activity: [['path', { d: 'M22 12h-4l-3 8-6-16-3 8H2' }]],
  gauge: [['path', { d: 'M12 14l4-4' }], ['path', { d: 'M4 19a9 9 0 1 1 16 0' }]],
  archive: [['rect', { x: '3', y: '4', width: '18', height: '4', rx: '1' }], ['path', { d: 'M5 8v12h14V8' }], ['path', { d: 'M10 12h4' }]],
  wand: [['path', { d: 'M15 4V2M15 16v-2M8 9H6M20 9h-2M10.8 4.8 9.4 3.4M20.6 19.6l-6.2-6.2M10.8 13.2l-1.4 1.4M19.2 4.8l1.4-1.4' }], ['path', { d: 'm14 9 1 1' }]],
  database: [['ellipse', { cx: '12', cy: '5', rx: '8', ry: '3' }], ['path', { d: 'M4 5v14c0 1.7 3.6 3 8 3s8-1.3 8-3V5' }], ['path', { d: 'M4 12c0 1.7 3.6 3 8 3s8-1.3 8-3' }]],
  shield: [['path', { d: 'M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10Z' }], ['path', { d: 'm9 12 2 2 4-4' }]],
  tags: [['path', { d: 'M20.6 13.6 13 21.2 3 11.2V3h8.2l9.4 9.4a1.7 1.7 0 0 1 0 1.2Z' }], ['path', { d: 'M7.5 7.5h.01' }]],
  calendar: [['rect', { x: '3', y: '4', width: '18', height: '18', rx: '2' }], ['path', { d: 'M16 2v4M8 2v4M3 10h18' }]],
  pen: [['path', { d: 'M12 20h9' }], ['path', { d: 'M16.5 3.5a2.1 2.1 0 0 1 3 3L7 19l-4 1 1-4 12.5-12.5Z' }]],
  target: [['circle', { cx: '12', cy: '12', r: '9' }], ['circle', { cx: '12', cy: '12', r: '5' }], ['circle', { cx: '12', cy: '12', r: '1' }]],
  workflow: [['rect', { x: '3', y: '3', width: '6', height: '6', rx: '1' }], ['rect', { x: '15', y: '15', width: '6', height: '6', rx: '1' }], ['path', { d: 'M9 6h5a4 4 0 0 1 4 4v5' }], ['path', { d: 'm15 12 3 3 3-3' }]],
  terminal: [['path', { d: 'm4 17 6-5-6-5' }], ['path', { d: 'M12 19h8' }]],
  cloud: [['path', { d: 'M17.5 19H8a6 6 0 1 1 1.1-11.9A7 7 0 0 1 22 12a4.5 4.5 0 0 1-4.5 7Z' }]],
}

const Icon = defineComponent({
  props: {
    name: {
      type: String,
      required: true,
    },
  },
  setup(props, { attrs }) {
    return () => h(
      'svg',
      {
        ...attrs,
        class: ['icon', attrs.class],
        viewBox: '0 0 24 24',
        fill: 'none',
        stroke: 'currentColor',
        'stroke-width': '2',
        'stroke-linecap': 'round',
        'stroke-linejoin': 'round',
        'aria-hidden': 'true',
      },
      (iconNodes[props.name] || iconNodes.sparkles).map(([tag, nodeAttrs]) => h(tag, nodeAttrs)),
    )
  },
})

const navItems = [
  { icon: 'home', label: '首页', href: '#home' },
  { icon: 'layers', label: '系统案例', href: '#systems' },
  { icon: 'cpu', label: '工程能力', href: '#engineering' },
  { icon: 'target', label: '后续规划', href: '#roadmap' },
  { icon: 'notebook', label: '思考笔记', href: '#notes' },
  { icon: 'user', label: '关于我', href: '#about' },
  { icon: 'mail', label: '联系我', href: '#contact' },
]

const metrics = [
  { icon: 'gauge', value: '10+', label: '年工程实践' },
  { icon: 'target', value: '50+', label: '项目经验' },
  { icon: 'activity', value: '1000w+', label: '日均渲染素材' },
]

const valuePillars = [
  { icon: 'workflow', label: '复杂创意流程结构化' },
  { icon: 'film', label: '视频渲染链路工程化' },
  { icon: 'database', label: '素材与数据资产闭环' },
]

const capabilityCards = [
  {
    icon: 'box',
    title: '创意生产系统',
    points: ['企业级素材库', '成片库系统', '标签体系与检索', '生命周期管理', '素材生产全链路管理'],
  },
  {
    icon: 'film',
    title: '视频自动化引擎',
    points: ['基于 FFmpeg 的渲染引擎', 'JavaCV 视频处理', '滤镜图与转码策略', '多轨音频编排同步', '批量渲染与资源调度'],
  },
  {
    icon: 'server',
    title: '基础设施与数据',
    points: ['Doris OLAP 数据建模', 'RocketMQ 消息队列', 'K3s 集群与服务治理', 'GitLab CI/CD 流水线', '数据回流闭环'],
  },
]

const systems = [
  {
    title: '企业级素材库',
    icon: 'archive',
    subtitle: '结构化管理万级素材资源',
    flow: ['资产接入', '资产建模', '标签体系', '审核管理', '调用分发'],
    footer: ['权限与分类建模', '统一检索体系', '权限与版本管理'],
    result: '支撑跨团队素材复用与资产治理',
  },
  {
    title: '一键成片平台',
    icon: 'wand',
    subtitle: '从创意到成片的自动化流水线',
    flow: ['创意结构', '素材匹配', '镜头生成', '渲染合成', '成片输出'],
    footer: ['结构公式驱动', '自动化生成', '批量渲染'],
    result: '将重复剪辑流程沉淀为可编排系统',
  },
  {
    title: '成片库系统',
    icon: 'film',
    subtitle: '成片资产的生命周期管理',
    flow: ['成片入库', '版本管理', '投放管理', '数据回流', '效果分析'],
    footer: ['多版本管理', '投放效果关联', '效果回归分析'],
    result: '形成从投放到效果分析的数据回流',
  },
]

const challenges = [
  {
    icon: 'code',
    title: 'FFmpeg 滤镜图引擎重构',
    points: ['解决黑屏、红屏偶现问题', '重构组合滤镜链路', '稳定复杂混流流程', '降低失败重试成本'],
  },
  {
    icon: 'wave',
    title: '音频轨道合成优化',
    points: ['消除电子音问题', '解决时长漂移', '多轨道同步对齐', '采样率自适配'],
  },
  {
    icon: 'sync',
    title: '渲染与调度系统优化',
    points: ['批量任务智能调度', '渲染资源编排', '故障自动恢复', '整体处理能力 300%+'],
  },
]

const notes = [
  {
    icon: 'workflow',
    type: '系统设计',
    title: '为什么创意生产需要结构化？',
    date: '2026-05-20',
    text: '从工具化到系统化，拆解创意生产中真正影响效率的关键链路。',
  },
  {
    icon: 'film',
    type: '视频工程',
    title: 'FFmpeg 滤镜图设计最佳实践',
    date: '2026-05-15',
    text: '深入解析滤镜链路的设计、调试与复杂视频混流方案。',
  },
  {
    icon: 'database',
    type: '架构实践',
    title: '素材库的数据模型设计',
    date: '2026-05-10',
    text: '如何设计支持复用、检索和版本管理的创意素材模型。',
  },
  {
    icon: 'pen',
    type: '工程实践',
    title: '从 PRD 到系统落地的思考',
    date: '2026-05-05',
    text: '产品、架构和工程协同，推动复杂系统从想法到交付。',
  },
]

const skillGroups = [
  'Java',
  'SpringBoot',
  'Vue3',
  'JavaCV',
  'FFmpeg',
  'Doris',
  'RocketMQ',
  'Nginx K8s',
  'GitLab',
  'Python',
  'MySQL',
  'Redis',
  'MinIO',
  'Linux',
]

const pipelineNodes = [
  { icon: 'layers', title: '前端应用', detail: 'Vue3', tone: 'green' },
  { icon: 'shield', title: 'API 网关', detail: 'Nginx', tone: 'green' },
  { icon: 'server', title: '业务服务', detail: 'SpringBoot', tone: 'blue', wide: true },
  { icon: 'sync', title: '消息队列', detail: 'RocketMQ', tone: 'purple' },
  { icon: 'film', title: '渲染引擎集群', detail: 'JavaCV + FFmpeg', tone: 'purple' },
]

const services = ['用户服务', '资产服务', '自动服务', '流程服务', '任务服务', '规则库']

const workModes = [
  { icon: 'target', title: 'MVP 优先', text: '先把关键链路跑通，再用真实反馈收敛复杂度。' },
  { icon: 'workflow', title: '结构公式驱动', text: '把创意经验抽象为规则、模板和可复用的工程模型。' },
  { icon: 'users', title: '跨团队对齐', text: '用可视化流程和数据口径连接产品、创意与工程团队。' },
]

const roadmapItems = [
  {
    icon: 'workflow',
    phase: '近期',
    title: '沉淀创意生产方法论',
    text: '把素材库、成片库、一键成片平台中的共性流程继续抽象成可复用的结构公式和工程模板。',
    actions: ['完善镜头公式体系', '梳理素材标签标准', '形成案例复盘文档'],
  },
  {
    icon: 'sparkles',
    phase: '中期',
    title: '探索 AI 辅助创意工程',
    text: '围绕脚本生成、素材匹配、镜头推荐和效果归因，尝试把 AI 能力接入现有生产链路。',
    actions: ['创意脚本结构化', '素材智能检索', '渲染策略推荐'],
  },
  {
    icon: 'database',
    phase: '中长期',
    title: '建设数据资产闭环',
    text: '让投放效果、素材表现、模板质量和生产成本形成统一指标，为后续自动优化提供依据。',
    actions: ['效果数据回流', '资产价值评估', '生产效率看板'],
  },
  {
    icon: 'users',
    phase: '长期',
    title: '扩展技术影响力',
    text: '持续输出创意工程、视频自动化和系统架构实践，逐步形成可被团队复用的知识体系。',
    actions: ['技术文章沉淀', '内部工具产品化', '跨团队工程协作'],
  },
]

const flowIcons = {
  资产接入: 'cloud',
  资产建模: 'box',
  标签体系: 'tags',
  审核管理: 'shield',
  调用分发: 'send',
  创意结构: 'lightbulb',
  素材匹配: 'layers',
  镜头生成: 'film',
  渲染合成: 'play',
  成片输出: 'download',
  成片入库: 'archive',
  版本管理: 'sync',
  投放管理: 'target',
  数据回流: 'activity',
  效果分析: 'gauge',
}
</script>

<template>
  <div class="site-shell">
    <!-- 全局动态背景层 -->
    <div class="global-bg" aria-hidden="true">
      <div class="particle-field"></div>
      <div class="glow-orb orb-1"></div>
      <div class="glow-orb orb-2"></div>
      <div class="glow-orb orb-3"></div>
      <div class="scan-line"></div>
    </div>

    <!-- 鼠标跟随光晕 -->
    <div
      class="cursor-glow"
      aria-hidden="true"
      :style="{
        left: mouseX + 'px',
        top: mouseY + 'px',
      }"
    ></div>

    <header class="topbar">
      <a class="brand" href="#home" aria-label="Changjun Labs">
        <span>CJ</span>
        <small>Creative Tech Lead</small>
      </a>
      <nav aria-label="主导航">
        <a v-for="item in navItems" :key="item.href" :href="item.href">
          <Icon :name="item.icon" class="nav-icon" />
          {{ item.label }}
        </a>
      </nav>
      <a class="outline-button" href="mailto:caichangjun001@gmail.com">
        <Icon name="mail" />
        联系方式
      </a>
    </header>

    <!-- 全屏开场标题 -->
    <div
      class="intro-overlay"
      :class="{ complete: introComplete }"
      aria-hidden="true"
    >
      <!-- 黑色蒙版 -->
      <svg
        class="intro-mask-svg"
        :viewBox="`0 0 ${introMotion.viewportWidth} ${introMotion.viewportHeight}`"
        preserveAspectRatio="none"
      >
        <defs>
          <mask
            id="intro-title-cutout"
            maskUnits="userSpaceOnUse"
            x="0"
            y="0"
            :width="introMotion.viewportWidth"
            :height="introMotion.viewportHeight"
          >
            <rect
              x="0"
              y="0"
              :width="introMotion.viewportWidth"
              :height="introMotion.viewportHeight"
              fill="white"
            />
            <text
              class="intro-cutout-text"
              :transform="getIntroTitleTransform()"
              :style="{ fontSize: introMotion.fontSize + 'px' }"
              fill="black"
            >
              <tspan x="0" :y="introMotion.fontSize * 0.86">Creative</tspan>
            </text>
          </mask>
        </defs>
        <rect
          x="0"
          y="0"
          :width="introMotion.viewportWidth"
          :height="introMotion.viewportHeight"
          fill="#000000"
          mask="url(#intro-title-cutout)"
        />
      </svg>
    </div>

    <main>
      <section
        id="home"
        class="hero section-band"
        :style="{ '--parallax-y': scrollY * 0.15 + 'px' }"
      >
        <div class="hero-bg-layer" aria-hidden="true">
          <div class="hero-grid-bg"></div>
          <div class="hero-gradient-overlay"></div>
          <div class="hero-noise"></div>
        </div>
        <div class="hero-copy">
          <p class="eyebrow">Changjun Labs</p>
          <h1 ref="heroTitle">Creative</h1>
          <h2>将广告创意生产<span>工业化</span></h2>
          <p class="hero-text">
            专注企业级素材库与视频自动化生产系统建设，用工程能力解决创意生产的规模化与效率问题。
          </p>
          <div class="value-pills" aria-label="核心价值">
            <span v-for="pillar in valuePillars" :key="pillar.label">
              <Icon :name="pillar.icon" />
              {{ pillar.label }}
            </span>
          </div>
          <div class="metrics" aria-label="核心指标">
            <div v-for="metric in metrics" :key="metric.label">
              <Icon :name="metric.icon" class="metric-icon" />
              <strong>{{ metric.value }}</strong>
              <span>{{ metric.label }}</span>
            </div>
          </div>
          <div class="hero-actions">
            <a class="primary-button" href="#systems">查看系统案例 <Icon name="arrowRight" /></a>
            <a class="secondary-button" href="#about"><Icon name="user" />了解我的能力</a>
          </div>
        </div>

        <div class="hero-visual" aria-label="创意生产自动化流程">
          <div class="orbit orbit-one"></div>
          <div class="orbit orbit-two"></div>
          <div class="grid-plane"></div>
          <div class="flow-stack">
            <article class="flow-card card-one">
              <small>Creative</small>
              <span class="line-icon"><Icon name="lightbulb" /></span>
              <strong>创意结构</strong>
            </article>
            <article class="flow-card card-two">
              <small>Structure</small>
              <span class="line-icon"><Icon name="package" /></span>
              <strong>镜头公式</strong>
            </article>
            <article class="flow-card card-three">
              <small>Asset</small>
              <span class="line-icon"><Icon name="layers" /></span>
              <strong>素材匹配</strong>
            </article>
            <article class="flow-card card-four">
              <small>Render</small>
              <span class="line-icon"><Icon name="play" /></span>
              <strong>自动渲染</strong>
            </article>
            <article class="flow-card card-five">
              <small>Distribution</small>
              <span class="line-icon"><Icon name="send" /></span>
              <strong>分发投放</strong>
            </article>
          </div>
          <aside class="ops-panel" aria-label="生产链路运行指标">
            <div>
              <Icon name="activity" />
              <span>Pipeline Health</span>
              <strong>99.9%</strong>
            </div>
            <div>
              <Icon name="gauge" />
              <span>Render Queue</span>
              <strong>12,840</strong>
            </div>
            <div>
              <Icon name="archive" />
              <span>Asset Reuse</span>
              <strong>68%</strong>
            </div>
          </aside>
        </div>
      </section>

      <section
        id="systems"
        class="content-section section-with-bg"
        :style="{ '--parallax-y': (scrollY - 400) * 0.08 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <SectionTitle label="我构建的系统" icon="box" />
        <div class="capability-grid">
          <article v-for="(card, index) in capabilityCards" :key="card.title" class="capability-card">
            <div class="card-icon"><Icon :name="card.icon" /></div>
            <div>
              <h3>{{ card.title }}</h3>
              <ul>
                <li v-for="point in card.points" :key="point">{{ point }}</li>
              </ul>
            </div>
            <span class="card-number">{{ String(index + 1).padStart(2, '0') }}</span>
          </article>
        </div>
      </section>

      <section
        class="content-section section-with-bg alt"
        :style="{ '--parallax-y': (scrollY - 900) * 0.06 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <SectionTitle label="核心系统与解决方案" icon="workflow" />
        <div class="systems-grid">
          <article v-for="system in systems" :key="system.title" class="system-card">
            <div class="system-card-head">
              <span><Icon :name="system.icon" /></span>
              <h3>{{ system.title }}</h3>
            </div>
            <p>{{ system.subtitle }}</p>
            <div class="mini-flow">
              <template v-for="(step, index) in system.flow" :key="step">
                <div class="mini-node">
                  <span><Icon :name="flowIcons[step] || 'sparkles'" /></span>
                  <small>{{ step }}</small>
                </div>
                <b v-if="index < system.flow.length - 1"><Icon name="arrowRight" /></b>
              </template>
            </div>
            <div class="system-footer">
              <span v-for="item in system.footer" :key="item">{{ item }}</span>
            </div>
            <p class="system-result">{{ system.result }}</p>
          </article>
        </div>
      </section>

      <section
        id="engineering"
        class="content-section section-with-bg"
        :style="{ '--parallax-y': (scrollY - 1400) * 0.08 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <SectionTitle label="工程难题攻坚" icon="cpu" />
        <div class="challenge-grid">
          <article v-for="challenge in challenges" :key="challenge.title" class="challenge-card">
            <div class="card-icon"><Icon :name="challenge.icon" /></div>
            <h3>{{ challenge.title }}</h3>
            <ul>
              <li v-for="point in challenge.points" :key="point">{{ point }}</li>
            </ul>
            <a href="#contact">查看详情 <Icon name="arrowRight" /></a>
          </article>
        </div>
      </section>

      <section
        id="architecture"
        class="content-section architecture-section section-with-bg alt"
        :style="{ '--parallax-y': (scrollY - 1900) * 0.06 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <SectionTitle label="系统架构" icon="server" />
        <div class="pipeline">
          <template v-for="(node, index) in pipelineNodes" :key="node.title">
            <div class="pipeline-node" :class="[node.tone, { wide: node.wide }]">
              <Icon :name="node.icon" class="pipeline-icon" />
              <h3>{{ node.title }}</h3>
              <p>{{ node.detail }}</p>
              <div v-if="node.wide" class="service-row">
                <span v-for="service in services" :key="service">{{ service }}</span>
              </div>
            </div>
            <span v-if="index < pipelineNodes.length - 1" class="pipeline-arrow"><Icon name="arrowRight" /></span>
          </template>
        </div>
        <div class="data-row">
          <div class="pipeline-node green"><Icon name="cloud" class="pipeline-icon" /><h3>对象存储</h3><p>MinIO / OSS</p></div>
          <span class="pipeline-arrow"><Icon name="arrowRight" /></span>
          <div class="pipeline-node blue"><Icon name="database" class="pipeline-icon" /><h3>OLAP 数据库</h3><p>Doris</p></div>
          <span class="pipeline-arrow"><Icon name="arrowRight" /></span>
          <div class="pipeline-node blue"><Icon name="server" class="pipeline-icon" /><h3>关系数据库</h3><p>MySQL</p></div>
        </div>
      </section>

      <section
        id="roadmap"
        class="content-section roadmap-section section-with-bg"
        :style="{ '--parallax-y': (scrollY - 2200) * 0.07 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <div class="section-heading-row">
          <SectionTitle label="后续规划" icon="target" />
          <p class="section-kicker">从项目交付走向方法沉淀，再到智能化生产闭环。</p>
        </div>
        <div class="roadmap-grid">
          <article v-for="(item, index) in roadmapItems" :key="item.title" class="roadmap-card">
            <div class="roadmap-marker">
              <Icon :name="item.icon" />
              <span>{{ String(index + 1).padStart(2, '0') }}</span>
            </div>
            <small>{{ item.phase }}</small>
            <h3>{{ item.title }}</h3>
            <p>{{ item.text }}</p>
            <div class="roadmap-actions">
              <span v-for="action in item.actions" :key="action"><Icon name="check" />{{ action }}</span>
            </div>
          </article>
        </div>
      </section>

      <section
        id="notes"
        class="content-section section-with-bg"
        :style="{ '--parallax-y': (scrollY - 2800) * 0.08 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <div class="section-heading-row">
          <SectionTitle label="思考与实践" icon="notebook" />
          <a href="#contact">查看更多 <Icon name="arrowRight" /></a>
        </div>
        <div class="notes-grid">
          <article v-for="note in notes" :key="note.title" class="note-card">
            <div class="note-preview">
              <Icon :name="note.icon" />
              <span></span><span></span><span></span>
            </div>
            <div class="note-meta">
              <strong><Icon :name="note.icon" />{{ note.type }}</strong>
              <time><Icon name="calendar" />{{ note.date }}</time>
            </div>
            <h3>{{ note.title }}</h3>
            <p>{{ note.text }}</p>
          </article>
        </div>
      </section>

      <section
        id="about"
        class="content-section about-section section-with-bg alt"
        :style="{ '--parallax-y': (scrollY - 3300) * 0.06 + 'px' }"
      >
        <div class="section-bg" aria-hidden="true">
          <div class="section-bg-pattern"></div>
          <div class="section-bg-gradient"></div>
        </div>
        <SectionTitle label="关于我" icon="user" />
        <div class="work-mode-grid">
          <article v-for="mode in workModes" :key="mode.title" class="work-mode-card">
            <Icon :name="mode.icon" class="mode-icon" />
            <h3>{{ mode.title }}</h3>
            <p>{{ mode.text }}</p>
          </article>
        </div>
        <div class="about-grid">
          <article class="about-card">
            <h3><Icon name="user" />个人简介</h3>
            <p>
              广告创意技术负责人 / 全栈开发者，专注企业级素材库、工业化创意生产与视频自动化生产。
            </p>
            <p>
              擅长将复杂的创意需求转化为可复用、可扩展、可验证的工程系统，在产品、技术与业务之间建立高效协作桥梁。
            </p>
            <a class="secondary-button" href="#contact"><Icon name="download" />下载简历</a>
          </article>
          <article class="about-card">
            <h3><Icon name="target" />核心能力</h3>
            <div class="skill-bars">
              <label>
                系统设计与架构
                <span><i style="width: 92%"></i></span>
              </label>
              <label>
                视频工程与渲染
                <span><i style="width: 96%"></i></span>
              </label>
              <label>
                数据建模与分析
                <span><i style="width: 88%"></i></span>
              </label>
              <label>
                工程效率与 DevOps
                <span><i style="width: 91%"></i></span>
              </label>
              <label>
                产品思维与落地
                <span><i style="width: 86%"></i></span>
              </label>
            </div>
          </article>
          <article class="about-card">
            <h3><Icon name="terminal" />技术栈</h3>
            <div class="tag-cloud">
              <span v-for="skill in skillGroups" :key="skill"><Icon name="check" />{{ skill }}</span>
            </div>
          </article>
          <article id="contact" class="about-card contact-card">
            <h3><Icon name="mail" />联系我</h3>
            <div class="contact-list">
              <a href="mailto:caichangjun001@gmail.com"><span><Icon name="mail" /></span> caichangjun001@gmail.com</a>
              <a href="https://github.com/ccj-cxy" target="_blank" rel="noreferrer"><span><Icon name="github" /></span> GitHub</a>
              <a href="https://linkedin.com" target="_blank" rel="noreferrer"><span><Icon name="linkedin" /></span> LinkedIn</a>
            </div>
          </article>
        </div>
      </section>
    </main>

    <footer class="footer">
      <span>© 2026 Changjun. All rights reserved.</span>
      <strong>“结构公式指导开发，工程能力创造价值”</strong>
      <div>
        <a href="https://github.com/ccj-cxy/" target="_blank" rel="noreferrer"><Icon name="github" />GitHub</a>
        <a href="mailto:caichangjun001@gmail.com"><Icon name="mail" />Email</a>
        <a href="https://linkedin.com" target="_blank" rel="noreferrer"><Icon name="linkedin" />LinkedIn</a>
      </div>
    </footer>
  </div>
</template>
