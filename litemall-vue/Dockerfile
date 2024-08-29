# 编译：docker build -t yz/litemall-vue:v0.1.0 -f Dockerfile .
# 构建：docker service create --network ht_net_pub --name litemall-vue -p 6255:6255 -d yz/litemall-vue:v0.1.0

# 基于node镜像来构建我们的前端环境
FROM dockerpull.com/node:latest as build-stage

# 设置工作目录
WORKDIR /usr/app

# 复制 package.json 和 package-lock.json (如果有)
COPY package*.json ./

# 安装项目依赖
RUN npm install --registry=https://registry.npmmirror.com/

# 复制项目文件和目录到工作目录
COPY . .

# 构建应用
RUN npm run build

# 用nginx作为web服务器
FROM nginx:latest as production-stage

# 从构建阶段复制构建结果到nginx目录
COPY --from=build-stage /app/dist /usr/share/nginx/html

# 暴露80端口
EXPOSE 6255

# 启动nginx，并且nginx将持续运行
CMD ["nginx", "-g", "daemon off;"]