# 使用官方node.js基础镜像
# FROM node:16 as builder
FROM node:lts-alpine

# 设置工作目录
WORKDIR /app

# 复制package.json和package-lock.json到工作目录
COPY package*.json ./

# 安装依赖
RUN npm install

# 复制项目的所有文件到工作目录
COPY . .

# 定义环境变量
# ENV NODE_ENV=production

# 运行打包命令
RUN npm run build

# 使用Nginx镜像来运行构建好的项目
FROM nginx:stable-alpine

# 将Nginx的默认服务器配置文件复制并覆盖
COPY --from=0 /app/deploy/nginx/default.conf /etc/nginx/nginx.conf

# 将构建好的项目复制到Nginx镜像的/usr/share/nginx/html目录下
COPY --from=0 /app/dist /usr/share/nginx/html

# 暴露端口
EXPOSE 80

# 启动Nginx，并且Nginx将持续运行
CMD ["nginx", "-g", "daemon off;"]