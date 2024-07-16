### Run Nginx Proxy Manager in NFS on Docker

####
    mkdir -p /root/nginxproxy && cd /root/nginxproxy
    cat <<EOF | sudo tee /root/nginxproxy/docker-compose.yml
    version: '3.8'
    services:
      app:
        image: 'jc21/nginx-proxy-manager:latest'
        restart: unless-stopped
        ports:
          - '81:80'
          - '82:81'
          - '443:443'
        volumes:
          - nfsvolume-nginxproxy-data:/data
          - nfsvolume-nginxproxy-letsencrypt:/etc/letsencrypt
    volumes:
      nfsvolume-nginxproxy-data:
        driver: local
        driver_opts:
          type: "nfs"
          o: "addr=192.168.0.96,rw,nfsvers=4"
          device: ":/nfs-share/docker/nginxproxy/data"

      nfsvolume-nginxproxy-letsencrypt:
        driver: local
        driver_opts:
          type: "nfs"
          o: "addr=192.168.0.96,rw,nfsvers=4"
          device: ":/nfs-share/docker/nginxproxy/letsencrypt"

    EOF
####
    docker compose up -d
####
    docker compose ps
####
    docker ps

Log in to the Admin UI

When your docker container is running, connect to it on port 82 for the admin interface.

http://docker-host-ip:82

Default Admin User:

Email:    admin@example.com

Password: changeme
