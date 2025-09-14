1. Chuẩn bị môi trường
    Máy chính: Windows, đã cài Docker Desktop (Linux containers).
    Tạo 2 file trong thư mục: 
        Dockerfile
        docker-compose.yml

2. Build và chạy container 
    docker compose up -d --build
    docker ps
    -> thấy container desktop-minimal với port 2222 và 5901.

3. SSH vào container
    ssh -p 2222 docker@localhost
    # password: docker

    (Nếu cần root: ssh -p 2222 root@localhost, pass: root) nếu quên mật khẩu docker thì vào 
    root để set lại mật khẩu 


4. Cài desktop environment và VNC server
    sudo apt update
    sudo apt install -y xfce4 xfce4-goodies tightvncserver dbus-x11 x11-xserver-utils


5. Tạo mật khẩu cho VNC 
    vncpasswd
    # nhập password (VD: docker), chọn "n" khi hỏi view-only để thao tác được, không chỉ mỗi xem

6. Sửa cấu hình xstartup
    cat > ~/.vnc/xstartup <<'EOF'
    #!/bin/sh
    unset SESSION_MANAGER
    unset DBUS_SESSION_BUS_ADDRESS
    xrdb $HOME/.Xresources
    dbus-launch --exit-with-session startxfce4 &
    EOF
    chmod +x ~/.vnc/xstartup

7. Khởi động VNC server 
    vncserver :1 -geometry 1280x1080 -depth 24


8. Kết nối VNC server từ Windows
    Cài RealVNC viwer
    Mở và nhập địa chỉ: localhost:5901
    Nhập mật khẩu đã tạo ở bước 5.
    Sau đó sẽ thấy desktop XFCE trong container.

--------------------
- Dừng VNC server trong container 
    vncserver -kill :1

- Khởi động lại VNC server
    vncserver :1 -geometry 1280x1080 -depth 24

- Dừng container 
    docker stop desktop-minimal

- Start lại từ container
    docker start desktop-minimal