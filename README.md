# hinna-pipeline

curl -X POST "http://jenkins.example.com/job/YOUR_PIPELINE/buildWithParameters" \
     -H "Authorization: Bearer le-token-here" \
     --data-urlencode "EMAIL=" \
     --data-urlencode "PASSWORD="

sudo /opt/bitnami/wp-cli/bin/wp user create admin "admin@gmail.com" --user_pass="admin" --role=administrator
