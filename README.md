

# launch Cloudwatch agent to EKS 

ClusterName='' \
RegionName='' \
FluentBitHttpPort='2020' \
FluentBitReadFromHead='Off' \
[[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On' \
[[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On' \
git clone https://github.com/owenfok/cloudwatch.git | unzip cloudwatch.zip | cat cloudwatch/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${RegionName}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f -  
 
# Deleting the CloudWatch agent 

kubectl delete -f cloudwatch/cwagent-fluent-bit-quickstart.yaml
