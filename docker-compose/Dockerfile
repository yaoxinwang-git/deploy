###############################################################################
#                                BUILDE
###############################################################################
# 打包依赖阶段使用golang作为基础镜像
FROM golang:1.16-alpine as builder

###############################################################################
#                                INSTALLATION
###############################################################################

# 启用go module
ENV CGO_ENABLED 0
ENV GOOS linux
ENV GO111MODULE auto

# 设置固定的项目路径
WORKDIR /app

# 整个源码复制到容器里面
COPY . .

RUN go build -ldflags="-s -w" -o ./accelerator-config -v ./cmd/accelerator-config/

FROM alpine:3.13

WORKDIR /app
# 复制配置文件
COPY --from=builder /app/accelerator-config /app/accelerator-config
COPY --from=builder /app/configs /app/configs
# 文件分发脚本
COPY --from=builder /app/playbook /app/playbook
COPY --from=builder /app/database/migrations /app/database/migrations
COPY --from=builder /app/migrate /app/migrate

RUN chmod +x /app/accelerator-config

EXPOSE 8000
CMD /app/accelerator-config