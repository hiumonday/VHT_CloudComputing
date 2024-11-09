# Cloud Computing Lab Challenges
# Prerequisites
- Docker Desktop/Engine
- Minikube (Optional)
- kubectl
- Git

## Challenge Lab 02: Static Web Deployment


### Steps
1. **Copy Static Website to /website**

2. **Create nginx.conf in /nginx-config**

3. **Create Dockerfile**
   
4. **Build & Push Docker Image**
   docker build -t hiumonday/static-web:v1 .
   docker push hiumonday/static-web:v1

5. **Deploy to Kubernetes**
   kubectl apply -f k8s/deployment.yaml

6. **Result**
    - NOTE: docker-desktop context automatically maps localhost to the Kubernetes cluster
        Access via nodePort 
        ![screen_shots/image.png](screen_shots/image.png)
    - Curl to nodePort (context=minikube)
        ![screen_shots/image-1.png](screen_shots/image-1.png)


## Challenge Lab 03: Nginx Proxy Multi Apps

### Project Structure
Challenge_lab03/
├── web1/
│   ├── Dockerfile
│   ├── nginx-config/
│   │   └── nginx.conf
│   └── website/
│       └── index.html
├── web2/
│   ├── Dockerfile
│   ├── nginx-config/
│   │   └── nginx.conf
│   └── website/
│       └── index.html
├── nginx-proxy/
│   ├── Dockerfile
│   └── nginx.conf
└── k8s/
    ├── web1-deployment.yaml
    ├── web2-deployment.yaml
    └── nginx-proxy.yaml

### Steps
1. **Create Web1 & Web2**
   - Copy static web files to respective directories
   - Create Nginx configs for each web

2. **Create Nginx Proxy Config**
   upstream web1 {
       server web1-service:80;
   }

   upstream web2 {
       server web2-service:80;
   }

   server {
       listen 80;

       location /web1 {
           proxy_pass http://web1;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }

       location /web2 {
           proxy_pass http://web2;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }

3. **Build & Push Images**
   # Build web1
   cd web1
   docker build -t hiumonday/web1 .
   docker push hiumonday/web1

   # Build web2
   cd ../web2
   docker build -t hiumonday/web2 .
   docker push hiumonday/web2

   # Build nginx-proxy
   cd ../nginx-proxy
   docker build -t hiumonday/nginx-proxy .
   docker push hiumonday/nginx-proxy

4. **Deploy to Kubernetes**
   kubectl apply -f k8s/web1-deployment.yaml
   kubectl apply -f k8s/web2-deployment.yaml
   kubectl apply -f k8s/nginx-proxy.yaml

5. **Access Applications**
   # Get NodePort
   kubectl get service nginx-proxy

   # Access web1
   curl http://localhost:30090/web1

   # Access web2
   curl http://localhost:30090/web2

### Troubleshooting
1. **Check Pods Status**
   kubectl get pods
   kubectl describe pod <pod-name>

2. **Check Logs**
   kubectl logs <pod-name>

3. **Test Nginx Config**
   kubectl exec -it <pod-name> -- nginx -t

### Notes
- Ensure all services are properly named and labeled
- Check nginx configs for syntax errors
- Verify all paths in HTML files are correct
- Monitor logs for any routing issues

## References
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)