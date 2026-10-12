# Báo Cáo Bài Tập 1: Kiểm Tra Cài Đặt Docker Engine Và Chạy Container Hello-World

- **Môn học**: DevOps Fundamentals (IT-209)
- **Session**: 09 - Docker Fundamentals & Containerization
- **Bài tập**: Bài 1 - Kiểm tra cài đặt Docker Engine và chạy Container Hello-World
- **Đường dẫn nộp bài trên GitHub**: `homework/session_09/ex1/`
- **Học viên**: HoQuocKhai
- **Email**: khaiho6040@gmail.com

---

## 1. Mục tiêu bài thực hành
1. **Kiểm tra phiên bản Docker CLI & Docker Daemon**: Xác thực Docker Client và Docker Server Engine đã được cài đặt, đang hoạt động bình thường và kết nối thông suốt với nhau.
2. **Kiểm tra quyền truy cập non-root**: Đảm bảo người dùng có thể thực thi các lệnh Docker CLI mà không cần tiền tố `sudo` (thông qua Docker group).
3. **Khởi chạy container đầu tiên**: Chạy container từ image mẫu `hello-world` trên Docker Hub để kiểm chứng toàn bộ quy trình: Pull image -> Tạo container -> Thực thi -> Xuất kết quả ra Terminal.

---

## 2. Các lệnh thực hiện và kết quả đầu ra (Terminal Output)

### 2.1. Lệnh 1: `docker version`
Kiểm tra chi tiết phiên bản Client và Server Engine.

```bash
docker version
```

**Kết quả đầu ra:**
```text
Client: Docker Engine - Community
 Version:           27.3.1
 API version:       1.47
 Go version:        go1.22.7
 Git commit:        ce12230
 Built:             Fri Sep 20 11:41:00 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          27.3.1
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.7
  Git commit:       4123503
  Built:            Fri Sep 20 11:41:00 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.22
  GitCommit:        7f7fdf5fed64eb6a7c49b4aa725924e98d46624f
 runc:
  Version:          1.1.14
  GitCommit:        v1.1.14-0-g2c9f560
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

---

### 2.2. Lệnh 2: `docker info`
Hiển thị thông tin tổng quan về cấu hình hệ thống, số lượng container, image, storage driver và runtime của Docker Daemon.

```bash
docker info
```

**Kết quả đầu ra:**
```text
Client: Docker Engine - Community
 Version:    27.3.1
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.17.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.29.7
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 27.3.1
 Storage Driver: overlay2
  Backing Filesystem: ext4
  Supports d_type: true
  Using metacopy: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7f7fdf5fed64eb6a7c49b4aa725924e98d46624f
 runc version: v1.1.14-0-g2c9f560
 init version: de40ad0
 Security Options:
  apparmor
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.8.0-45-generic
 Operating System: Ubuntu 24.04 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 7.658GiB
 Name: devops-node
 ID: 3b94:81cf:4e8a:92d1:77fa:1b2c:489d:91ae
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

---

### 2.3. Lệnh 3: `docker run hello-world`
Khởi chạy container từ image `hello-world`.

```bash
docker run hello-world
```

**Kết quả đầu ra:**
```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec661f44bd: Pull complete 
Digest: sha256:305243d734571da2d100c8c8b3c3140a6115f459b2a1e81191cffb11361ad719
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

---

## 3. Cơ chế hoạt động của `docker run hello-world`
Khi thực thi câu lệnh `docker run hello-world`, quá trình diễn ra qua 4 bước:
1. **Docker Client liên lạc với Docker Daemon**: Lệnh được gửi qua UNIX socket (`/var/run/docker.sock`) hoặc Named Pipe.
2. **Kiểm tra Image trong Local Cache**: Daemon tìm kiếm image `hello-world:latest` trên máy. Vì chưa có sẵn, Daemon tự động tải (Pull) image từ kho chứa công khai **Docker Hub**.
3. **Khởi tạo và chạy Container**: Daemon tạo ra một container mới từ image vừa tải, cấp phát tài nguyên và thực thi chương trình binary được định nghĩa trong image.
4. **Trả kết quả về Terminal**: Thông điệp từ container được stream ngược lại về Docker Client và hiển thị trên màn hình người dùng. Container sau đó tự động dừng (exit code 0).

---

## 4. Xử lý lỗi phân quyền (Troubleshooting Docker Permission)
Nếu chạy lệnh `docker` gặp lỗi:
```text
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock...
```

**Cách khắc phục**: Thêm tài khoản hiện tại vào nhóm `docker` của hệ thống để thực thi không cần `sudo`:
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Sau đó kiểm tra lại bằng lệnh:
```bash
docker ps
```
Nếu danh sách container hiển thị bình thường mà không báo lỗi phân quyền là cấu hình thành công.
