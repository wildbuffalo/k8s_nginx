
## Prerequisite 

Any kubernetes environments, it can be your local environment such as minikube, kind or in the public cloud.

## Deployment 

- kubectl apply -f k8s/deployments/apply

## Requirements

1. kubernetes pods to utilize nginx docker container, currently, we set replicas to 2.
2. Building a new nginx container base on alpine:3.10, see Dockerfile in the repo
   1. steps to build the container:
      1. docker build -t feirenliu/nginx .
      2. docker push feirenliu/nginx
3. The deployment contains liveness and readiness probes that make sure the app is always running. In order to loadbalance the apps we created the clusterIP in service.yaml file, then clients can visit the app through ingress controller url.

Bonus:
1. How would this be configured to maximize availability. 
   1. we set up the liveness readiness probes making sure the app always in running state and can be restart whenever needed
   2. set up the loadbalancer to distribute traffic loads  
   3. using hotizontal autoscale to scale more replicas when needed
2. What loads would this spinup be able to handle. 
   1. The chart for individual nginx server can handle see below. original doc https://www.nginx.com/blog/testing-the-performance-of-nginx-and-nginx-plus-web-servers/
  
      | CPUs	| 0 KB	| 1 KB	| 10 KB	| 100 KB |
      |-------|-------|-------|-------|--------|
      | 1	    | 71,561| 40,207| 23,308| 4,830  |
      | 2	| 151,325	| 85,139	| 48,654	| 9,871 |
      | 4	| 324,654	| 178,395	| 96,808	| 19,355 |
      | 8	| 647,213	| 359,576	| 198,818	| 38,900 |
      | 16	| 1,262,999	| 690,329	| 383,860	| 77,427 |
      | 32	| 2,197,336	| 1,207,959	| 692,804	| 90,430 |
      | 36	| 2,175,945	| 1,239,624	| 733,745	| 89,842 |

    2. The resources can be set in deployment.yaml file 
   
3. How would logging, security be applied. 
   1. The logging can be capture using agent in the kubernetes infrastructure such as splunk angent. see document below
      1. https://github.com/splunk/splunk-connect-for-kubernetes
   2. Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization.
   3. Utilize ssl certs when possible such as ingress controller.  

