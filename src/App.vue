<script setup>
import { defineComponent, h } from 'vue'

const SectionTitle = defineComponent({
  props: {
    label: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    return () => h('div', { class: 'section-title' }, [h('span'), h('h2', props.label)])
  },
})

const navItems = [
  { label: '首页', href: '#home' },
  { label: '系统案例', href: '#systems' },
  { label: '工程能力', href: '#engineering' },
  { label: '思考笔记', href: '#notes' },
  { label: '关于我', href: '#about' },
  { label: '联系我', href: '#contact' },
]

const metrics = [
  { value: '10+', label: '年工程实践' },
  { value: '50+', label: '项目经验' },
  { value: '1000w+', label: '日均渲染素材' },
]

const valuePillars = [
  '复杂创意流程结构化',
  '视频渲染链路工程化',
  '素材与数据资产闭环',
]

const capabilityCards = [
  {
    icon: 'cube',
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
    subtitle: '结构化管理万级素材资源',
    flow: ['资产接入', '资产建模', '标签体系', '审核管理', '调用分发'],
    footer: ['权限与分类建模', '统一检索体系', '权限与版本管理'],
    result: '支撑跨团队素材复用与资产治理',
  },
  {
    title: '一键成片平台',
    subtitle: '从创意到成片的自动化流水线',
    flow: ['创意结构', '素材匹配', '镜头生成', '渲染合成', '成片输出'],
    footer: ['结构公式驱动', '自动化生成', '批量渲染'],
    result: '将重复剪辑流程沉淀为可编排系统',
  },
  {
    title: '成片库系统',
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
    type: '系统设计',
    title: '为什么创意生产需要结构化？',
    date: '2026-05-20',
    text: '从工具化到系统化，拆解创意生产中真正影响效率的关键链路。',
  },
  {
    type: '视频工程',
    title: 'FFmpeg 滤镜图设计最佳实践',
    date: '2026-05-15',
    text: '深入解析滤镜链路的设计、调试与复杂视频混流方案。',
  },
  {
    type: '架构实践',
    title: '素材库的数据模型设计',
    date: '2026-05-10',
    text: '如何设计支持复用、检索和版本管理的创意素材模型。',
  },
  {
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
  { title: '前端应用', detail: 'Vue3', tone: 'green' },
  { title: 'API 网关', detail: 'Nginx', tone: 'green' },
  { title: '业务服务', detail: 'SpringBoot', tone: 'blue', wide: true },
  { title: '消息队列', detail: 'RocketMQ', tone: 'purple' },
  { title: '渲染引擎集群', detail: 'JavaCV + FFmpeg', tone: 'purple' },
]

const services = ['用户服务', '资产服务', '自动服务', '流程服务', '任务服务', '规则库']

const workModes = [
  { title: 'MVP 优先', text: '先把关键链路跑通，再用真实反馈收敛复杂度。' },
  { title: '结构公式驱动', text: '把创意经验抽象为规则、模板和可复用的工程模型。' },
  { title: '跨团队对齐', text: '用可视化流程和数据口径连接产品、创意与工程团队。' },
]
</script>

<template>
  <div class="site-shell">
    <header class="topbar">
      <a class="brand" href="#home" aria-label="Changjun Labs">
        <span>CJ</span>
        <small>Creative Tech Lead</small>
      </a>
      <nav aria-label="主导航">
        <a v-for="item in navItems" :key="item.href" :href="item.href">{{ item.label }}</a>
      </nav>
      <a class="outline-button" href="mailto:changjun@example.com">
        <span class="icon-mail" aria-hidden="true"></span>
        联系方式
      </a>
    </header>

    <main>
      <section id="home" class="hero section-band">
        <div class="hero-copy">
          <p class="eyebrow">Changjun Labs</p>
          <h1>Creative<br />Technology Lead</h1>
          <h2>将广告创意生产<span>工业化</span></h2>
          <p class="hero-text">
            专注企业级素材库与视频自动化生产系统建设，用工程能力解决创意生产的规模化与效率问题。
          </p>
          <div class="value-pills" aria-label="核心价值">
            <span v-for="pillar in valuePillars" :key="pillar">{{ pillar }}</span>
          </div>
          <div class="metrics" aria-label="核心指标">
            <div v-for="metric in metrics" :key="metric.label">
              <strong>{{ metric.value }}</strong>
              <span>{{ metric.label }}</span>
            </div>
          </div>
          <div class="hero-actions">
            <a class="primary-button" href="#systems">查看系统案例 <span aria-hidden="true">→</span></a>
            <a class="secondary-button" href="#about">了解我的能力</a>
          </div>
        </div>

        <div class="hero-visual" aria-label="创意生产自动化流程">
          <div class="orbit orbit-one"></div>
          <div class="orbit orbit-two"></div>
          <div class="grid-plane"></div>
          <div class="flow-stack">
            <article class="flow-card card-one">
              <small>Creative</small>
              <span class="line-icon lightbulb"></span>
              <strong>创意结构</strong>
            </article>
            <article class="flow-card card-two">
              <small>Structure</small>
              <span class="line-icon package"></span>
              <strong>镜头公式</strong>
            </article>
            <article class="flow-card card-three">
              <small>Asset</small>
              <span class="line-icon layers"></span>
              <strong>素材匹配</strong>
            </article>
            <article class="flow-card card-four">
              <small>Render</small>
              <span class="line-icon play"></span>
              <strong>自动渲染</strong>
            </article>
            <article class="flow-card card-five">
              <small>Distribution</small>
              <span class="line-icon send"></span>
              <strong>分发投放</strong>
            </article>
          </div>
          <aside class="ops-panel" aria-label="生产链路运行指标">
            <div>
              <span>Pipeline Health</span>
              <strong>99.9%</strong>
            </div>
            <div>
              <span>Render Queue</span>
              <strong>12,840</strong>
            </div>
            <div>
              <span>Asset Reuse</span>
              <strong>68%</strong>
            </div>
          </aside>
        </div>
      </section>

      <section id="systems" class="content-section">
        <SectionTitle label="我构建的系统" />
        <div class="capability-grid">
          <article v-for="(card, index) in capabilityCards" :key="card.title" class="capability-card">
            <div class="card-icon" :class="card.icon"></div>
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

      <section class="content-section">
        <SectionTitle label="核心系统与解决方案" />
        <div class="systems-grid">
          <article v-for="system in systems" :key="system.title" class="system-card">
            <h3>{{ system.title }}</h3>
            <p>{{ system.subtitle }}</p>
            <div class="mini-flow">
              <template v-for="(step, index) in system.flow" :key="step">
                <div class="mini-node">
                  <span></span>
                  <small>{{ step }}</small>
                </div>
                <b v-if="index < system.flow.length - 1">→</b>
              </template>
            </div>
            <div class="system-footer">
              <span v-for="item in system.footer" :key="item">{{ item }}</span>
            </div>
            <p class="system-result">{{ system.result }}</p>
          </article>
        </div>
      </section>

      <section id="engineering" class="content-section">
        <SectionTitle label="工程难题攻坚" />
        <div class="challenge-grid">
          <article v-for="challenge in challenges" :key="challenge.title" class="challenge-card">
            <div class="card-icon" :class="challenge.icon"></div>
            <h3>{{ challenge.title }}</h3>
            <ul>
              <li v-for="point in challenge.points" :key="point">{{ point }}</li>
            </ul>
            <a href="#contact">查看详情 →</a>
          </article>
        </div>
      </section>

      <section id="architecture" class="content-section architecture-section">
        <SectionTitle label="系统架构" />
        <div class="pipeline">
          <template v-for="(node, index) in pipelineNodes" :key="node.title">
            <div class="pipeline-node" :class="[node.tone, { wide: node.wide }]">
              <h3>{{ node.title }}</h3>
              <p>{{ node.detail }}</p>
              <div v-if="node.wide" class="service-row">
                <span v-for="service in services" :key="service">{{ service }}</span>
              </div>
            </div>
            <span v-if="index < pipelineNodes.length - 1" class="pipeline-arrow">→</span>
          </template>
        </div>
        <div class="data-row">
          <div class="pipeline-node green"><h3>对象存储</h3><p>MinIO / OSS</p></div>
          <span class="pipeline-arrow">→</span>
          <div class="pipeline-node blue"><h3>OLAP 数据库</h3><p>Doris</p></div>
          <span class="pipeline-arrow">→</span>
          <div class="pipeline-node blue"><h3>关系数据库</h3><p>MySQL</p></div>
        </div>
      </section>

      <section id="notes" class="content-section">
        <div class="section-heading-row">
          <SectionTitle label="思考与实践" />
          <a href="#contact">查看更多 →</a>
        </div>
        <div class="notes-grid">
          <article v-for="note in notes" :key="note.title" class="note-card">
            <div class="note-preview">
              <span></span><span></span><span></span>
            </div>
            <div class="note-meta">
              <strong>{{ note.type }}</strong>
              <time>{{ note.date }}</time>
            </div>
            <h3>{{ note.title }}</h3>
            <p>{{ note.text }}</p>
          </article>
        </div>
      </section>

      <section id="about" class="content-section about-section">
        <SectionTitle label="关于我" />
        <div class="work-mode-grid">
          <article v-for="mode in workModes" :key="mode.title" class="work-mode-card">
            <h3>{{ mode.title }}</h3>
            <p>{{ mode.text }}</p>
          </article>
        </div>
        <div class="about-grid">
          <article class="about-card">
            <h3>个人简介</h3>
            <p>
              广告创意技术负责人 / 全栈开发者，专注企业级素材库、工业化创意生产与视频自动化生产。
            </p>
            <p>
              擅长将复杂的创意需求转化为可复用、可扩展、可验证的工程系统，在产品、技术与业务之间建立高效协作桥梁。
            </p>
            <a class="secondary-button" href="#contact">下载简历 <span aria-hidden="true">↓</span></a>
          </article>
          <article class="about-card">
            <h3>核心能力</h3>
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
            <h3>技术栈</h3>
            <div class="tag-cloud">
              <span v-for="skill in skillGroups" :key="skill">{{ skill }}</span>
            </div>
          </article>
          <article id="contact" class="about-card contact-card">
            <h3>联系我</h3>
            <div class="contact-list">
              <a href="mailto:changjun@example.com"><span>✉</span> changjun@example.com</a>
              <a href="https://github.com" target="_blank" rel="noreferrer"><span>⌘</span> GitHub</a>
              <a href="https://linkedin.com" target="_blank" rel="noreferrer"><span>in</span> LinkedIn</a>
              <span><span>☎</span> 138 0013 8000</span>
            </div>
            <div class="qr-code" aria-label="微信二维码占位">
              <i v-for="n in 49" :key="n" :class="{ on: n % 2 === 0 || n % 7 === 0 || n % 13 === 0 }"></i>
            </div>
          </article>
        </div>
      </section>
    </main>

    <footer class="footer">
      <span>© 2026 Changjun. All rights reserved.</span>
      <strong>“结构公式指导开发，工程能力创造价值”</strong>
      <div>
        <a href="https://github.com" target="_blank" rel="noreferrer">GitHub</a>
        <a href="mailto:changjun@example.com">Email</a>
        <a href="https://linkedin.com" target="_blank" rel="noreferrer">LinkedIn</a>
      </div>
    </footer>
  </div>
</template>
