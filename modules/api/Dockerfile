# 第一阶段：构建 Go 可执行文件
FROM golang:1.24.4 AS builder

WORKDIR /build

COPY modules/common/ /build/common/
COPY modules/api/ /build/api/
RUN go mod download
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o dashboard-api /build/api/main.go

# 第二阶段：精简运行环境
FROM alpine:3.20

WORKDIR /app
COPY --from=builder /build/dashboard-api .

# 可选：添加证书支持
RUN apk add --no-cache ca-certificates

EXPOSE 8080
ENTRYPOINT ["./dashboard-api"]
