job "nginx-job" {
  datacenters = ["dc1"]
  type = "service"
  group "nginx-group" {
    count = 1
    volume "nginx-volume" {
      type = "host"
      read_only = false
      source = "nomad_volume"
    }
    network {
      port "nginx-port" {
        to = 80
      }
    }
    task "nginx-task" {
      driver = "docker"
      volume_mount {
        volume = "nginx-volume"
        destination  = "/usr/share/nginx/html"
        read_only = false
      }
      config {
        image = "nginx:latest"
        ports = ["nginx-port"]
      }
      resources {
        cpu    = 100
        memory = 128
        network {
          mbits = 10
          port "http" {
              static = 8041
          }
        }
      }
      service {
        name = "nginx"
        tags = [ "nginx", "web", "urlprefix-/nginx" ]
        port = "http"
        check {
          type     = "tcp"
          interval = "10s"
          timeout  = "2s"
        }
      }
    }
  }
}
