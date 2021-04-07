PromQL query for App Version
Single Stat 
app_info with Fields selected as version

PromQL query for request statuses
Graph Type
sum by (status) (rate(flask_http_request_total[5m]))
Legend set to {{status}}