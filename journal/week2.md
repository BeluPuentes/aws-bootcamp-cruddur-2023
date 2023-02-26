# Week 2 — Distributed Tracing
## HoneyComb
set environments variables 
```
export HONEYCOMB_API_KEY="*"
export gp env HONEYCOMB_API_KEY="*"

export HONEYCOMB_SERVICE_NAME="Cruddur"
export gp env HONEYCOMB_SERVICE_NAME="Cruddur"
```
Install Packages  
```
pip install opentelemetry-api
```
in the requirements.txt add
```
flask
flask-cors

opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```
Install 
```
pip install -r requirements.txt 
```
Update the app.py
Link: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/ae8e200de4e9c63cd7462278aad3c99f14e5fac6

Mock data 
![image](https://user-images.githubusercontent.com/93335543/221426797-96b23f7e-7ee5-41f3-8f16-ca55118d4e01.png)
![image](https://user-images.githubusercontent.com/93335543/221426941-003233fe-2ddb-46b5-859b-dab26d756f07.png)
![image](https://user-images.githubusercontent.com/93335543/221427147-8d266e8a-e217-48bc-baf3-a506bfd22d90.png)

add spans and attributies
![image](https://user-images.githubusercontent.com/93335543/221427954-89da0b55-86cd-4b81-9a5e-d054b9138921.png)
![image](https://user-images.githubusercontent.com/93335543/221428534-bb861784-fdd3-4ec6-815a-b6b53786b004.png)

HeatMap
![image](https://user-images.githubusercontent.com/93335543/221428922-b0e2ceb3-eac8-4daf-b4ac-acff999dd321.png)


CHALLENGE 
BUILD MY OWN QUERY 

What are the Slowest Traces in the Application 
This query identifies the slowest trace in your application, in terms of duration (duration_ms), and provides information about the specific events and spans that make up that trace.

VISUALIZE	WHERE	GROUP BY
MAX(duration_ms)	trace.parent_id does-not-exist	name
Use to:

to find potential performance issues
understand the root cause of slow response times
![image](https://user-images.githubusercontent.com/93335543/221430681-5b2c77fc-5a21-43be-853b-a1dd73cc9789.png)

