# Capstone Project II: Using Kubernetes to Deploy a MongoDB Database with a Mongo Express Web-Based Admin Interface in AWS

A complete Kubernetes deployment for MongoDB database with Mongo Express web-based administration interface, optimized for AWS EKS.

## 🏗️ Architecture

This project deploys:
- **MongoDB Database** - Persistent database with EBS storage
- **Mongo Express** - Web-based MongoDB administration interface
- **AWS EBS CSI Driver** - For persistent volume management
- **LoadBalancer Service** - For external access to Mongo Express

## 📁 Project Structure

```
├── mongo-sc.txt                    # StorageClass for AWS EBS volumes
├── mongo-secret.txt                # MongoDB credentials (base64 encoded)
├── mongo-pvc.txt                   # Persistent Volume Claim
├── mongo-svc.txt                   # MongoDB service (ClusterIP)
├── mongodb-deployment.txt          # MongoDB deployment
├── mongo.config-map.txt            # ConfigMap for Mongo Express
├── mongo-express-svc.txt           # Mongo Express service (LoadBalancer)
├── mongo-express-deployment.txt    # Mongo Express deployment
└── README.md                       # This file
```

## 🚀 Quick Start

### Prerequisites

- AWS EKS cluster
- AWS EBS CSI driver installed
- kubectl configured to access your cluster

### 1. Install AWS EBS CSI Driver

```bash
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.19"
```

### 2. Deploy Resources

Deploy the resources in the following order:

```bash
# 1. Storage Class
kubectl apply -f mongo-sc.yaml

# 2. Secret (credentials are already base64 encoded)
kubectl apply -f mongo-secret.yaml

# 3. ConfigMap
kubectl apply -f mongo.config-map.yaml

# 4. Persistent Volume Claim
kubectl apply -f mongo-pvc.yaml

# 5. MongoDB Service
kubectl apply -f mongo-svc.yaml

# 6. MongoDB Deployment
kubectl apply -f mongodb-deployment.yaml

# 7. Mongo Express Service
kubectl apply -f mongo-express-svc.yaml

# 8. Mongo Express Deployment
kubectl apply -f mongo-express-deployment.yaml
```

### 3. Access Mongo Express

```bash
# Get the external IP of the LoadBalancer
kubectl get svc mongo-express-svc

# Access Mongo Express in your browser
# http://<EXTERNAL-IP>:8081
```

## 🔐 Security Configuration

### Update MongoDB Credentials

Before deploying, update the secret with your base64 encoded credentials:

```bash
# Encode your credentials
echo -n 'your-username' | base64
echo -n 'your-password' | base64

# Update mongo-secret.txt with the encoded values
```

### Default Credentials

- **Username**: `admin`
- **Password**: `password123`

## 🏷️ AWS Optimizations

- **EBS GP3 Volumes**: High-performance storage with encryption
- **Network Load Balancer**: Optimized for web applications
- **Volume Encryption**: Data encrypted at rest
- **Retain Policy**: Prevents accidental data loss

## 📊 Monitoring

### Check Deployment Status

```bash
# Check all pods
kubectl get pods

# Check services
kubectl get svc

# Check persistent volumes
kubectl get pvc

# View MongoDB logs
kubectl logs -f deployment/mongodb-deployment

# View Mongo Express logs
kubectl logs -f deployment/mongo-express-app
```

### Troubleshooting

```bash
# Check pod events
kubectl describe pod <pod-name>

# Check service endpoints
kubectl get endpoints

# Check storage class
kubectl get storageclass
```

## 🔧 Configuration Details

### Storage Configuration
- **Volume Type**: GP3 (AWS EBS)
- **Size**: 5Gi (expandable)
- **Encryption**: Enabled
- **File System**: ext4

### Network Configuration
- **MongoDB**: Internal access only (ClusterIP)
- **Mongo Express**: External access via LoadBalancer
- **Port**: 8081 (Mongo Express), 27017 (MongoDB)

### Resource Requirements
- **MongoDB**: Standard container resources
- **Mongo Express**: Lightweight web interface
- **Storage**: 5Gi minimum

## 🗑️ Cleanup

To remove all resources:

```bash
kubectl delete -f mongo-express-deployment.yaml
kubectl delete -f mongo-express-svc.yaml
kubectl delete -f mongodb-deployment.yaml
kubectl delete -f mongo-svc.yaml
kubectl delete -f mongo-pvc.yaml
kubectl delete -f mongo.config-map.yaml
kubectl delete -f mongo-secret.yaml
kubectl delete -f mongo-sc.yaml
```

## 📝 Notes

- MongoDB data persists in EBS volumes
- Mongo Express provides web-based database management
- All components are labeled for AWS cloud identification
- Storage class uses `WaitForFirstConsumer` for optimal placement

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the deployment
5. Submit a pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Happy Database Management! 🎉**
