resource "kubernetes_ingress" "nginx-ingress" {
  metadata {
    name = "nginx-ingress"
		annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }

  spec {
    rule {
			host = "kube-nginx.toudherebarry.com"
      http {
        path {
          backend {
            service_name = "nginx-svc"
            service_port = 80
          }
          path = "/"
        }
      }
    }
  }
}