# Vue3前端项目开发规则

## 1. 项目架构

### 1.1 技术栈
- 核心框架：Vue 3.x
- 构建工具：Vite
- 路由管理：Vue Router 4.x
- 状态管理：Pinia
- UI框架：Element Plus / Ant Design Vue / Naive UI（根据项目需求选择）
- CSS预处理器：SCSS / Less
- HTTP客户端：Axios
- 代码规范：ESLint + Prettier
- 单元测试：Vitest + Vue Test Utils
- E2E测试：Cypress
- 类型检查：TypeScript
- 国际化：vue-i18n
- 图表：ECharts / D3.js（可选）

### 1.2 目录结构
```
vue_front/
├── public/                 # 静态资源，不经过构建
│   └── favicon.ico
├── src/
│   ├── assets/             # 静态资源，会经过构建
│   │   ├── fonts/          # 字体文件
│   │   ├── images/         # 图片资源
│   │   └── styles/         # 全局样式
│   │       ├── variables.scss  # 样式变量
│   │       ├── mixins.scss     # 样式混合
│   │       └── global.scss     # 全局样式
│   ├── components/         # 通用组件
│   │   ├── common/         # 基础通用组件
│   │   └── business/       # 业务通用组件
│   ├── composables/        # 组合式函数
│   ├── directives/         # 自定义指令
│   ├── hooks/              # 自定义hooks
│   ├── layouts/            # 布局组件
│   ├── router/             # 路由配置
│   │   ├── index.ts        # 路由入口
│   │   └── routes/         # 路由模块
│   ├── services/           # API服务
│   │   ├── api/            # API接口定义
│   │   ├── http.ts         # Axios配置与拦截器
│   │   └── index.ts        # API导出
│   ├── stores/             # Pinia状态管理
│   ├── utils/              # 工具函数
│   ├── views/              # 页面组件
│   │   └── [module]/       # 按模块划分页面
│   │       ├── components/ # 模块特定组件
│   │       └── [page].vue  # 页面组件
│   ├── App.vue             # 根组件
│   ├── main.ts             # 入口文件
│   ├── env.d.ts            # 环境变量类型声明
│   └── vite-env.d.ts       # Vite环境变量类型声明
├── tests/                  # 测试文件
│   ├── unit/               # 单元测试
│   └── e2e/                # E2E测试
├── types/                  # 全局类型声明
├── .env                    # 环境变量
├── .env.development        # 开发环境变量
├── .env.production         # 生产环境变量
├── .eslintrc.js            # ESLint配置
├── .prettierrc.js          # Prettier配置
├── tsconfig.json           # TypeScript配置
├── vite.config.ts          # Vite配置
├── package.json            # 依赖配置
└── README.md               # 项目说明
```

## 2. 编码规范

### 2.1 命名规范
- **文件名**：
  - 组件文件使用 PascalCase，例如：`UserProfile.vue`
  - 非组件文件使用 camelCase，例如：`userService.ts`
  - 页面组件放在views目录中，使用 PascalCase，例如：`UserList.vue`
  - 单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)

- **组件名**：
  - 组件名使用 PascalCase，例如：`UserProfile`
  - 基础组件应该以特定的前缀开头，例如：`Base`、`App` 或 `V`，例如：`BaseButton`
  - 单例组件应该以 `The` 前缀命名，例如：`TheHeader`
  - 紧密耦合的组件应该以父组件名作为前缀，例如：`UserProfileAddress`

- **属性名**：
  - Props 使用 camelCase，例如：`itemCount`
  - 模板中的 Props 使用 kebab-case，例如：`item-count`
  - 自定义事件使用 kebab-case，例如：`change-status`

- **变量名**：
  - 变量使用 camelCase，例如：`userData`
  - 全局常量使用 UPPER_SNAKE_CASE，例如：`MAX_COUNT`
  - 布尔值变量使用 `is`、`has`、`can` 等前缀，例如：`isLoading`、`hasData`

- **路由名**：
  - 路由名使用 kebab-case，例如：`user-profile`

### 2.2 Vue组件规范
- 组件选项顺序遵循：
  ```javascript
  export default {
    name: '',        // 组件名
    components: {},  // 子组件
    props: {},       // 属性
    emits: [],       // 事件
    setup() {},      // 组合式API
    data() {},       // 数据
    computed: {},    // 计算属性
    watch: {},       // 侦听器
    created() {},    // 生命周期钩子
    mounted() {},    // 生命周期钩子
    methods: {}      // 方法
  }
  ```

- 组合式API推荐使用 `<script setup>` 语法

- 组件模板结构：
  ```vue
  <template>
    <div class="component-name">
      <!-- 组件内容 -->
    </div>
  </template>

  <script setup lang="ts">
  // 组件逻辑
  </script>

  <style scoped lang="scss">
  /* 组件样式 */
  .component-name {
    /* 样式规则 */
  }
  </style>
  ```

### 2.3 代码风格
- 使用2个空格缩进
- 使用单引号 `'` 表示字符串
- 在单文件组件中使用 `<script setup>` 语法
- 对象和数组的最后一项后面不加逗号
- 优先使用 ES6+ 语法特性
- 避免在一个文件中放置太多代码，单文件不超过500行
- 优先使用 `const` 和 `let`，避免使用 `var`
- 使用箭头函数 `() => {}`
- 将复杂的逻辑提取到组合式函数或工具函数中
- 遵循ESLint和Prettier规则

### 2.4 TypeScript规范
- 为所有的变量、参数、返回值定义类型
- 避免使用 `any` 类型，优先使用 `unknown`
- 使用接口（interface）定义对象结构
- 使用类型别名（type）创建联合类型或交叉类型
- 为API响应数据定义接口
- 使用泛型增强代码复用性和类型安全性

## 3. 组件开发规范

### 3.1 组件设计原则
- 单一职责原则：一个组件只负责一个功能
- 高内聚低耦合：组件内部紧密关联，组件之间松散耦合
- 可复用性：设计通用组件，避免重复代码
- 可测试性：组件应易于单元测试
- 可维护性：代码简洁清晰，易于理解

### 3.2 组件分类
- **基础组件**：不包含业务逻辑，高度可复用，如按钮、输入框等
- **业务组件**：包含特定业务逻辑，如用户列表、订单详情等
- **页面组件**：对应路由页面，整合多个业务组件，如用户管理页面
- **布局组件**：定义页面结构和布局，如侧边栏、头部、页脚等

### 3.3 组件通信
- **Props Down**：父组件通过props向子组件传递数据
- **Events Up**：子组件通过事件向父组件传递消息
- **Provide/Inject**：跨多级组件传递数据
- **Pinia**：使用状态管理库管理全局状态
- **组合式API**：使用 `ref`、`reactive` 在组件间共享状态

### 3.4 组件复用策略
- 提取通用逻辑到组合式函数（Composables）
- 使用插槽（Slots）增强组件灵活性
- 使用高阶组件（HOC）或混入（Mixins）封装横切关注点
- 将常用功能封装为指令（Directives）

## 4. 路由管理规范

### 4.1 路由配置
- 使用路由懒加载减小初始加载体积
  ```javascript
  const routes = [
    {
      path: '/users',
      component: () => import('@/views/users/UserList.vue'),
      meta: {
        title: '用户列表',
        requiresAuth: true
      }
    }
  ]
  ```

- 使用路由元信息（meta）存储路由相关信息
- 为路由配置命名，便于编程式导航
- 路由参数使用props解耦组件和路由

### 4.2 路由组织
- 按模块拆分路由配置
- 使用路由嵌套表示页面层级关系
- 合理设置路由重定向和别名

### 4.3 导航守卫
- 使用全局前置守卫 `router.beforeEach` 进行权限控制
- 使用全局后置钩子 `router.afterEach` 更改页面标题
- 合理使用组件内守卫处理特定页面逻辑

## 5. 状态管理规范

### 5.1 Pinia使用规范
- 按模块划分store
- 使用选项式API或组合式API定义store
- 区分state、getters和actions

```javascript
// 选项式API
export const useUserStore = defineStore('user', {
  state: () => ({
    users: [],
    loading: false
  }),
  getters: {
    activeUsers: (state) => state.users.filter(user => user.active)
  },
  actions: {
    async fetchUsers() {
      this.loading = true
      try {
        const users = await userService.getUsers()
        this.users = users
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
      }
    }
  }
})

// 组合式API
export const useUserStore = defineStore('user', () => {
  const users = ref([])
  const loading = ref(false)
  
  const activeUsers = computed(() => users.value.filter(user => user.active))
  
  async function fetchUsers() {
    loading.value = true
    try {
      const response = await userService.getUsers()
      users.value = response
    } catch (error) {
      console.error(error)
    } finally {
      loading.value = false
    }
  }
  
  return { users, loading, activeUsers, fetchUsers }
})
```

### 5.2 状态管理原则
- 只将需要在多个组件间共享的状态放入store
- 将API调用封装在actions中
- 使用getters计算派生状态
- 避免在组件中直接修改store状态，统一通过actions修改

### 5.3 持久化状态
- 使用 `pinia-plugin-persistedstate` 实现状态持久化
- 只持久化必要的状态，避免存储敏感信息
- 根据需要选择存储位置（localStorage、sessionStorage）

## 6. API调用规范

### 6.1 API服务配置
- 使用Axios进行HTTP请求
- 配置全局拦截器处理请求和响应
- 集中管理API URL和方法
- 处理API错误和异常

```javascript
// http.ts
import axios from 'axios'

const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// 请求拦截器
http.interceptors.request.use(
  config => {
    const token = localStorage.getItem('token')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  error => Promise.reject(error)
)

// 响应拦截器
http.interceptors.response.use(
  response => response.data,
  error => {
    if (error.response) {
      // 处理HTTP错误状态码
      switch (error.response.status) {
        case 401:
          // 未授权，跳转到登录页
          break
        case 403:
          // 权限不足
          break
        case 404:
          // 资源不存在
          break
        default:
          // 其他错误
      }
    }
    return Promise.reject(error)
  }
)

export default http
```

### 6.2 API接口组织
- 按业务模块组织API接口
- 使用TypeScript接口定义请求参数和响应数据

```javascript
// api/user.ts
import http from '../http'
import type { User, UserCreateParams, UserUpdateParams } from '@/types/user'

export const userService = {
  getUsers: () => http.get<User[]>('/users'),
  getUserById: (id: string) => http.get<User>(`/users/${id}`),
  createUser: (params: UserCreateParams) => http.post<User>('/users', params),
  updateUser: (id: string, params: UserUpdateParams) => http.put<User>(`/users/${id}`, params),
  deleteUser: (id: string) => http.delete(`/users/${id}`)
}
```

### 6.3 数据请求策略
- 使用异步/等待（async/await）处理异步操作
- 实现请求缓存和防抖
- 处理并发请求
- 实现请求取消机制
- 优先使用组合式函数封装数据请求逻辑

```javascript
// composables/useUsers.ts
import { ref, onMounted } from 'vue'
import { userService } from '@/services/api/user'
import type { User } from '@/types/user'

export function useUsers() {
  const users = ref<User[]>([])
  const loading = ref(false)
  const error = ref<Error | null>(null)

  async function fetchUsers() {
    loading.value = true
    error.value = null
    try {
      users.value = await userService.getUsers()
    } catch (e) {
      error.value = e as Error
      console.error(e)
    } finally {
      loading.value = false
    }
  }

  onMounted(fetchUsers)

  return {
    users,
    loading,
    error,
    fetchUsers
  }
}
```

## 7. UI和样式规范

### 7.1 样式组织
- 使用BEM（Block, Element, Modifier）命名约定
- 使用SCSS或Less预处理器
- 全局样式放在assets/styles下
- 组件样式使用scoped或CSS模块
- 提取通用样式变量和混合

### 7.2 主题和样式变量
- 定义全局颜色、字体、间距等变量
- 支持亮色/暗色主题切换
- 使用CSS变量实现动态主题

```scss
// variables.scss
:root {
  // 颜色
  --primary-color: #1890ff;
  --success-color: #52c41a;
  --warning-color: #faad14;
  --error-color: #f5222d;
  --text-color: rgba(0, 0, 0, 0.85);
  --background-color: #fff;

  // 字体
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
  --font-size-sm: 12px;
  --font-size-base: 14px;
  --font-size-lg: 16px;

  // 间距
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  // 边框
  --border-radius-sm: 2px;
  --border-radius-base: 4px;
  --border-radius-lg: 8px;
  --border-color: #d9d9d9;
}

// 暗色主题
.dark-theme {
  --primary-color: #177ddc;
  --success-color: #49aa19;
  --warning-color: #d89614;
  --error-color: #d32029;
  --text-color: rgba(255, 255, 255, 0.85);
  --background-color: #141414;
  --border-color: #434343;
}
```

### 7.3 响应式设计
- 使用媒体查询适配不同屏幕尺寸
- 优先使用Flex和Grid布局
- 使用相对单位（rem、em、%）而非固定单位（px）
- 实现移动优先的设计模式
- 定义断点变量

```scss
// breakpoints.scss
$breakpoints: (
  xs: 0,
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px,
  xxl: 1600px
);

@mixin respond-to($breakpoint) {
  $min-width: map-get($breakpoints, $breakpoint);
  @media (min-width: $min-width) {
    @content;
  }
}
```

### 7.4 UI组件框架使用
- 统一使用一种UI框架
- 按需引入组件，减小打包体积
- 定制UI框架主题，符合产品设计
- 扩展UI组件，满足特定需求

## 8. 测试规范

### 8.1 单元测试
- 使用Vitest进行单元测试
- 使用Vue Test Utils测试组件
- 测试覆盖率不低于80%
- 测试组件的props、事件、插槽、状态等

```javascript
// UserCard.spec.ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import UserCard from '@/components/UserCard.vue'

describe('UserCard', () => {
  it('renders properly with props', () => {
    const user = {
      id: '1',
      name: 'John Doe',
      email: 'john@example.com'
    }
    
    const wrapper = mount(UserCard, {
      props: { user }
    })
    
    expect(wrapper.text()).toContain(user.name)
    expect(wrapper.text()).toContain(user.email)
  })
  
  it('emits edit event when edit button is clicked', async () => {
    const user = { id: '1', name: 'John', email: 'john@example.com' }
    const wrapper = mount(UserCard, { props: { user } })
    
    await wrapper.find('.edit-button').trigger('click')
    
    expect(wrapper.emitted()).toHaveProperty('edit')
    expect(wrapper.emitted().edit[0]).toEqual([user.id])
  })
})
```

### 8.2 组件测试策略
- 测试组件渲染输出
- 测试用户交互
- 测试props和事件
- 测试异步行为
- 模拟依赖（Vuex、Router等）

### 8.3 E2E测试
- 使用Cypress进行端到端测试
- 测试关键业务流程
- 模拟用户行为
- 断言页面状态和内容

```javascript
// cypress/e2e/login.spec.js
describe('Login', () => {
  beforeEach(() => {
    cy.visit('/login')
  })
  
  it('should login successfully with valid credentials', () => {
    cy.get('[data-test="username"]').type('admin')
    cy.get('[data-test="password"]').type('password')
    cy.get('[data-test="login-button"]').click()
    
    cy.url().should('include', '/dashboard')
    cy.get('[data-test="user-info"]').should('contain', 'admin')
  })
  
  it('should show error with invalid credentials', () => {
    cy.get('[data-test="username"]').type('admin')
    cy.get('[data-test="password"]').type('wrong-password')
    cy.get('[data-test="login-button"]').click()
    
    cy.get('[data-test="error-message"]').should('be.visible')
    cy.get('[data-test="error-message"]').should('contain', '用户名或密码错误')
  })
})
```

## 9. 构建与部署规范

### 9.1 环境配置
- 使用.env文件配置不同环境变量
- 区分开发、测试、生产环境
- 敏感信息不应直接写入代码或配置文件

```
# .env.development
VITE_API_BASE_URL=http://localhost:3000/api
VITE_APP_TITLE=DTMS开发环境

# .env.production
VITE_API_BASE_URL=https://api.example.com
VITE_APP_TITLE=DTMS系统
```

### 9.2 构建优化
- 使用动态导入实现代码分割
- 优化图片和静态资源
- 启用gzip或brotli压缩
- 提取第三方库到vendor chunk
- 启用缓存和持久化缓存

```javascript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { visualizer } from 'rollup-plugin-visualizer'
import viteCompression from 'vite-plugin-compression'

export default defineConfig({
  plugins: [
    vue(),
    viteCompression(), // 开启gzip压缩
    visualizer() // 可视化构建产物
  ],
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          // 将第三方库单独打包
          vendor: ['vue', 'vue-router', 'pinia', 'axios'],
          ui: ['element-plus'] // UI框架单独打包
        }
      }
    },
    // 启用持久化缓存
    cache: true
  }
})
```

### 9.3 部署流程
- 使用CI/CD自动化部署（GitHub Actions、Jenkins等）
- 实现环境隔离
- 部署前进行测试
- 配置错误监控和性能监控
- 实现回滚机制

## 10. 版本控制规范

### 10.1 Git分支管理
- 主分支：`main/master`，保持随时可发布状态
- 开发分支：`develop`，日常开发分支
- 功能分支：`feature/xxx`，新功能开发
- 发布分支：`release/vx.x.x`，版本发布准备
- 热修复分支：`hotfix/xxx`，生产bug修复

### 10.2 提交规范
- 使用约定式提交规范（Conventional Commits）
- 提交信息格式：`<type>(<scope>): <subject>`
  - type：feat(新功能)、fix(修复)、docs(文档)、style(样式)、refactor(重构)、test(测试)、chore(构建/依赖)
  - scope：影响范围
  - subject：简要描述
- 示例：`feat(user): add user profile page`

### 10.3 Code Review
- 所有代码必须经过Code Review
- 使用Pull Request/Merge Request进行Code Review
- 关注代码质量、性能、安全性和可读性
- Code Review通过后才能合并代码

## 11. 安全规范

### 11.1 前端安全最佳实践
- 防止XSS攻击：避免使用v-html，对用户输入进行转义
- 防止CSRF攻击：使用CSRF令牌，不将敏感操作放在GET请求中
- 避免在前端存储敏感信息
- 使用HTTPS协议
- 实现CSP（内容安全策略）
- 避免使用eval()和Function构造函数

### 11.2 敏感数据处理
- 不在localStorage/sessionStorage中存储敏感数据
- 使用HttpOnly和Secure标记保护Cookie
- 敏感信息传输时进行加密
- 避免将API密钥等硬编码在代码中

## 12. 性能优化

### 12.1 加载性能
- 使用路由懒加载
- 组件按需加载
- 图片优化（webp格式、懒加载、适当尺寸）
- 减少HTTP请求
- 使用CDN加载第三方库

### 12.2 运行时性能
- 避免重复渲染
- 使用v-show代替v-if（适当情况下）
- 使用计算属性缓存结果
- 使用v-memo优化大型v-for列表
- 避免深层次的响应式对象
- 合理使用v-once
- 使用虚拟滚动处理大数据列表

### 12.3 性能监控
- 使用Lighthouse进行性能评估
- 监控核心Web指标（LCP、FID、CLS）
- 实施性能预算
- 使用Performance API收集实际用户性能数据

## 13. 项目文档

### 13.1 组件文档
- 使用VitePress或Storybook制作组件文档
- 记录组件的props、事件、插槽
- 提供组件使用示例和最佳实践

### 13.2 项目文档
- README.md包含项目介绍、技术栈、环境要求、安装和启动方式
- 提供详细的开发文档，包括项目结构、配置说明、开发规范
- 接口文档和业务流程说明

## 14. 国际化

### 14.1 国际化实现
- 使用vue-i18n实现多语言支持
- 将翻译文本与代码分离
- 支持按需加载语言包
- 实现语言切换功能
- 处理复数、日期和数字格式

```javascript
// i18n/index.ts
import { createI18n } from 'vue-i18n'
import zh from './locales/zh.json'
import en from './locales/en.json'

const i18n = createI18n({
  legacy: false,
  locale: localStorage.getItem('locale') || 'zh',
  fallbackLocale: 'en',
  messages: {
    zh,
    en
  }
})

export default i18n
```

## 15. 可访问性（A11y）

### 15.1 可访问性最佳实践
- 使用语义化HTML元素
- 提供适当的替代文本（alt属性）
- 确保适当的颜色对比度
- 支持键盘导航
- 使用ARIA标签增强可访问性
- 测试屏幕阅读器兼容性 