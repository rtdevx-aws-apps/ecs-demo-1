# ECS CI/CD Demo - Stack Simplify

Learnings from Kalyan Reddy Daida’s "AWS Fargate & ECS - Masterclass | Microservices, Docker, CFN
" Udemy course.

https://www.udemy.com/course/aws-fargate-ecs-masterclass-microservices-docker-cloudformation

---

CI/CD deployment demo to ECS Fargate cluster using GitHub Actions.

---

**One pipeline, two stages:**

1. **Build & Push Stage**    
    - Trigger: push to `main`        
    - Steps:        
        - Lint → Test → Build Docker image            
        - Authenticate to AWS via OIDC            
        - Build & push image to ECR with tag:            
            - `latest`                
            - Git SHA (`$GITHUB_SHA`)                
        - Update ECS Task Definition JSON with new image tag            
        - Store the rendered Task Definition as an artifact
            
2. **Deploy Stage**    
    - **STAG deploy** runs automatically after build        
    - **PROD deploy** requires manual approval        
    - Each deploy:        
        - Downloads the rendered Task Definition            
        - Registers a new Task Definition revision            
        - Updates the ECS service (`stag` or `prod`)            
        - Waits for ECS deployment to stabilize
