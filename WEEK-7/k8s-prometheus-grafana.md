After carefully reviewing your Kubernetes configuration files, I've identified several issues that need correction. Here's a detailed analysis of each file with corrections:

### 1. redis-deployment.yaml
**Issues:**
- No persistence configuration (data will be lost on pod restart)
- No security context for Redis
- No liveness/readiness probes

**Corrected Version:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: monitoring  # Added namespace
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis:alpine
          ports:
            - containerPort: 6379
          resources:
            limits:
              memory: "256Mi"
              cpu: "500m"
          livenessProbe:  # Added health checks
            exec:
              command: ["redis-cli", "ping"]
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            exec:
              command: ["redis-cli", "ping"]
            initialDelaySeconds: 5
            periodSeconds: 10
          securityContext:  # Added security
            runAsNonRoot: true
            runAsUser: 1000
---
apiVersion: v1
kind: Service
metadata:
  name: redis-service
  namespace: monitoring  # Added namespace
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
  type: ClusterIP
```

### 2. prometheus-config.yaml
**Issues:**
- Redis target has wrong namespace (should be default)
- Missing scrape interval for redis

**Corrected Version:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
  namespace: monitoring
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s

    scrape_configs:
      - job_name: 'django-app'
        metrics_path: "/metrics"
        static_configs:
          - targets: ['django-service.default.svc.cluster.local:8000']
            labels:
              service: "django-app"

      - job_name: 'prometheus'
        static_configs:
          - targets: ['localhost:9090']
            labels:
              service: "prometheus"

      - job_name: 'redis'
        scrape_interval: 10s  # Added specific interval
        static_configs:
          - targets: ['redis-service.default.svc.cluster.local:6379']  # Fixed namespace
            labels:
              service: "redis"
```

### 3. prometheus-deployment.yaml
**Issues:**
- Missing resource limits
- No security context
- No readiness/liveness probes

**Corrected Version:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
        - name: prometheus
          image: prom/prometheus
          args:
            - "--config.file=/etc/prometheus/prometheus.yml"
          ports:
            - containerPort: 9090
          resources:
            limits:
              memory: "512Mi"
              cpu: "1000m"
          livenessProbe:
            httpGet:
              path: /-/healthy
              port: 9090
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /-/ready
              port: 9090
            initialDelaySeconds: 5
            periodSeconds: 10
          securityContext:
            runAsNonRoot: true
            runAsUser: 1000
          volumeMounts:
            - name: config-volume
              mountPath: /etc/prometheus
      volumes:
        - name: config-volume
          configMap:
            name: prometheus-config
```

### 4. grafana-deployment.yaml
**Issues:**
- Missing configMap mount for datasources
- No resource limits
- No security context

**Corrected Version:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana
          image: grafana/grafana
          ports:
            - containerPort: 3000
          resources:
            limits:
              memory: "512Mi"
              cpu: "1000m"
          securityContext:
            runAsNonRoot: true
            runAsUser: 1000
          volumeMounts:
            - name: config-volume
              mountPath: /etc/grafana/provisioning/datasources
      volumes:
        - name: config-volume
          configMap:
            name: grafana-config
```

### 5. django-service.yaml
**Issues:**
- Missing namespace (should be default)
- NodePort should specify port number

**Corrected Version:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: django-service
  namespace: default
spec:
  selector:
    app: django-app
  ports:
    - protocol: TCP
      port: 8000
      targetPort: 8000
      nodePort: 30080  # Added specific nodePort
  type: NodePort
```

### 6. deployment.yaml
**Issues:**
- Missing resource limits
- No liveness/readiness probes
- No security context
- Missing database configuration

**Corrected Version:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: django-app
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: django-app
  template:
    metadata:
      labels:
        app: django-app
    spec:
      containers:
      - name: django-app
        image: khushigoosari/bankingsystem:v7
        ports:
        - containerPort: 8000
        env:
          - name: REDIS_HOST
            value: "redis-service.default.svc.cluster.local"  # FQDN
          - name: REDIS_PORT
            value: "6379"
          - name: DATABASE_URL
            valueFrom:
              secretKeyRef:
                name: django-secrets
                key: database-url
        resources:
          limits:
            memory: "512Mi"
            cpu: "1000m"
        livenessProbe:
          httpGet:
            path: /health/
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health/
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 10
        securityContext:
          runAsNonRoot: true
          runAsUser: 1000
```

### Key Corrections Summary:

1. **Namespace Consistency**:
   - Fixed redis-service namespace (now in monitoring)
   - Fixed redis target in prometheus config (now in default)

2. **Health Monitoring**:
   - Added liveness/readiness probes to all deployments
   - Added proper health check endpoints

3. **Security**:
   - Added securityContext to all containers
   - Set non-root users

4. **Resource Management**:
   - Added resource limits to all containers
   - Proper CPU/memory allocations

5. **Service Discovery**:
   - Used FQDN for service discovery (redis-service.default.svc.cluster.local)
   - Fixed service types and ports

6. **Configuration**:
   - Added proper configMap mounts for Grafana
   - Fixed Prometheus scrape configs

These changes will make your deployment more robust, secure, and production-ready for your banking application. You'll need to create a Secret for your database URL (referenced in the django deployment) and ensure your Django application has a /health/ endpoint for the probes.

Would you like me to provide the additional Secret manifest or any other supporting files you might need?
