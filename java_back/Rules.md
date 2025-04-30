# Java后端项目开发规则

## 1. 项目架构

### 1.1 技术栈
- 核心框架：Spring Boot 3.x
- 权限框架：Spring Security
- ORM框架：MyBatis Plus / Spring Data JPA
- 数据库：MySQL 8.x
- 缓存：Redis
- 消息队列：RabbitMQ / Kafka（可选）
- 文档工具：Swagger / OpenAPI 3.0
- 部署：Docker + Jenkins / GitHub Actions
- 服务发现与配置：Spring Cloud（微服务架构时）

### 1.2 目录结构
```
java_back/
├── src/
│   ├── main/
│   │   ├── java/com/dtms/
│   │   │   ├── config/         # 配置类
│   │   │   ├── controller/     # 控制器层
│   │   │   ├── service/        # 服务层
│   │   │   │   └── impl/       # 服务实现
│   │   │   ├── repository/     # 数据访问层
│   │   │   ├── entity/         # 实体类
│   │   │   ├── dto/            # 数据传输对象
│   │   │   ├── vo/             # 视图对象
│   │   │   ├── constant/       # 常量定义
│   │   │   ├── util/           # 工具类
│   │   │   ├── exception/      # 异常处理
│   │   │   ├── aspect/         # AOP切面
│   │   │   └── Application.java # 启动类
│   │   └── resources/
│   │       ├── application.yml  # 配置文件
│   │       ├── application-dev.yml  # 开发环境配置
│   │       ├── application-test.yml # 测试环境配置
│   │       ├── application-prod.yml # 生产环境配置
│   │       └── static/         # 静态资源
│   └── test/                   # 测试代码
├── pom.xml                     # Maven配置文件
├── Dockerfile                  # Docker配置文件
├── .gitignore                  # Git忽略文件
└── README.md                   # 项目说明
```

## 2. 编码规范

### 2.1 命名规范
- **包名**：全小写，单数形式，例如：`com.dtms.controller`
- **类名**：大驼峰命名法，例如：`UserController`
- **方法名**：小驼峰命名法，例如：`getUserById`
- **变量名**：小驼峰命名法，例如：`userId`
- **常量名**：全大写，下划线分隔，例如：`MAX_RETRY_COUNT`
- **接口名**：大驼峰命名法，建议以 I 开头（或不加），例如：`IUserService` 或 `UserService`
- **实现类**：大驼峰命名法，建议接口名 + Impl，例如：`UserServiceImpl`

### 2.2 代码风格
- 缩进使用4个空格（不使用Tab）
- 行宽不超过120个字符
- 方法最大行数不应超过80行
- 类最大行数不应超过1000行
- 善用设计模式，但不过度设计
- 遵循SOLID原则（单一职责、开闭原则、里氏替换、接口隔离、依赖倒置）
- 类、方法和变量添加合适的注释
- 删除未使用的导入和代码

### 2.3 注释规范
- 类注释：描述类的作用、作者、创建日期
```java
/**
 * 用户服务实现类，提供用户相关的业务逻辑
 *
 * @author yourname
 * @date 2023-01-01
 */
```

- 方法注释：描述方法功能、参数、返回值、异常
```java
/**
 * 根据用户ID获取用户信息
 *
 * @param id 用户ID
 * @return 用户信息
 * @throws NotFoundException 用户不存在时抛出
 */
```

- 变量/属性注释：复杂变量需要添加注释说明
```java
/** 最大重试次数 */
private static final int MAX_RETRY_COUNT = 3;
```

## 3. API设计规范

### 3.1 RESTful API设计
- 使用名词而非动词表示资源
- 使用复数名词表示资源集合
- 使用HTTP方法表示操作：
  - GET：获取资源
  - POST：创建资源
  - PUT：更新资源（全量更新）
  - PATCH：更新资源（部分更新）
  - DELETE：删除资源
- 使用HTTP状态码表示结果
- URL路径遵循层级关系，例如：`/api/v1/departments/{departmentId}/employees/{employeeId}`

### 3.2 接口版本控制
- 在URL中包含版本号，例如：`/api/v1/users`
- 版本号使用整数，如v1、v2
- 新版本发布后，确保向后兼容性或提供迁移文档

### 3.3 响应结构规范
```json
{
  "code": 200,
  "message": "操作成功",
  "data": {
    // 响应数据
  },
  "timestamp": 1609459200000
}
```

### 3.4 错误处理
- 使用合适的HTTP状态码
- 提供明确的错误信息
- 错误响应结构统一
```json
{
  "code": 400,
  "message": "请求参数错误",
  "errors": [
    {"field": "username", "message": "用户名不能为空"}
  ],
  "timestamp": 1609459200000
}
```

## 4. 数据库设计规范

### 4.1 命名规范
- 表名：小写，下划线分隔，复数形式，例如：`user_profiles`
- 字段名：小写，下划线分隔，例如：`user_id`
- 主键：使用`id`作为名称，自增长整数或UUID
- 外键：使用`表名_id`形式，例如：`department_id`

### 4.2 字段设计
- 每个表必须包含以下字段：
  - `id`：主键
  - `created_at`：创建时间
  - `updated_at`：更新时间
  - `created_by`：创建人（可选）
  - `updated_by`：更新人（可选）
  - `deleted`：逻辑删除标识（0未删除，1已删除）
- 合理使用索引，避免过度索引
- 尽量不使用`TEXT`类型，使用`VARCHAR`并指定最大长度
- 使用`TIMESTAMP`或`DATETIME`存储时间
- 合理设置字段默认值

### 4.3 SQL规范
- 查询语句使用`SELECT *`时必须在注释中说明原因
- 使用参数化查询防止SQL注入
- 大型查询使用分页处理
- 复杂查询优先考虑使用视图或存储过程
- 避免在循环中执行SQL语句，使用批处理

## 5. 安全规范

### 5.1 身份认证
- 实现JWT或OAuth2.0认证机制
- 用户密码必须加密存储（BCrypt或PBKDF2）
- 敏感操作需二次验证
- 设置合理的Token过期时间

### 5.2 授权控制
- 实现基于角色的访问控制（RBAC）
- API接口权限控制
- 数据权限控制（行级、列级）
- 使用Spring Security注解控制方法级别权限

### 5.3 数据安全
- 敏感数据传输必须使用HTTPS
- 敏感数据存储必须加密
- 日志中不记录敏感信息
- 接口返回数据脱敏处理

### 5.4 安全防护
- 防止XSS攻击
- 防止CSRF攻击
- 防止SQL注入
- 限制API请求频率
- 防止批量请求攻击

## 6. 测试规范

### 6.1 单元测试
- 每个服务类必须编写单元测试
- 测试覆盖率不低于80%
- 使用JUnit 5 + Mockito编写测试
- 遵循AAA（Arrange-Act-Assert）模式

### 6.2 集成测试
- 关键业务流程必须有集成测试
- 使用测试数据库或H2内存数据库
- 使用Spring Boot Test进行测试

### 6.3 API测试
- 使用Postman或RestAssured进行API测试
- 编写API自动化测试脚本
- 测试正常流程和异常流程

### 6.4 性能测试
- 使用JMeter或Gatling进行性能测试
- 设置关键API的性能指标
- 定期进行压力测试和负载测试

## 7. 文档规范

### 7.1 API文档
- 使用Swagger/OpenAPI 3.0生成API文档
- 为每个API添加详细的描述、参数说明和响应示例
- 文档应实时更新，与代码保持一致

### 7.2 代码文档
- 核心业务逻辑添加详细注释
- 复杂算法需要注释说明思路
- 定期使用Javadoc生成文档

### 7.3 项目文档
- README.md包含项目介绍、技术栈、环境要求、启动方式
- 提供详细的部署文档
- 提供开发环境搭建文档

## 8. 版本控制规范

### 8.1 Git分支管理
- 主分支：`main/master`，保持随时可发布状态
- 开发分支：`develop`，日常开发分支
- 功能分支：`feature/xxx`，新功能开发
- 发布分支：`release/vx.x.x`，版本发布准备
- 热修复分支：`hotfix/xxx`，生产bug修复

### 8.2 提交规范
- 提交信息格式：`<type>(<scope>): <subject>`
  - type：feat(新功能)、fix(修复)、docs(文档)、style(格式)、refactor(重构)、test(测试)、chore(构建/依赖)
  - scope：影响范围
  - subject：简要描述
- 示例：`feat(user): add user registration function`

### 8.3 版本发布流程
1. 从`develop`分支创建`release/vx.x.x`分支
2. 在`release`分支进行测试和bug修复
3. 测试通过后合并到`main/master`和`develop`分支
4. 在`main/master`分支打Tag发布

## 9. 构建与部署规范

### 9.1 构建规范
- 使用Maven进行项目构建
- 统一管理依赖版本
- 避免使用SNAPSHOT版本依赖
- 使用Maven Profile管理不同环境配置

### 9.2 部署规范
- 使用Docker容器化部署
- 配置CI/CD自动化部署流程
- 使用环境变量注入配置
- 日志集中收集与管理

### 9.3 监控规范
- 集成应用健康检查
- 使用Actuator暴露监控端点
- 使用Prometheus + Grafana监控系统
- 异常报警机制

## 10. 项目管理规范

### 10.1 需求管理
- 需求文档化并进行版本控制
- 需求评审确认
- 需求变更需走变更流程

### 10.2 任务管理
- 使用Jira/GitHub Projects等工具管理任务
- 任务粒度合理，单个任务工作量不超过2天
- 任务状态及时更新
- 每日站会同步进度

### 10.3 代码审查
- 所有代码必须经过代码审查
- 遵循代码审查清单
- 及时修复代码审查中发现的问题

### 10.4 发布管理
- 版本号遵循语义化版本（Semantic Versioning）
- 每次发布提供变更日志
- 重大变更提前通知 