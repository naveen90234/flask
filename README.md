- install docker first
- then build the flask-app image 

	docker build -t flask .

- now run the container and map the port

	docker container run -d --name flask-app -p 8888:5000 flask

- Application should be accesible on http://localhost:8888

	now install nginx 
	systemctl start nginx

- create a custom configuration file in /etc/nginx/conf.d/flask.conf

server {
        listen 8080;
        server_name localhost;

        location / {
                proxy_pass http://localhost:8888;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                }
                }
	#Now nginx will redirect all the traffic from 8080 port to the container application running on 8888 port 

	systemctl restart nginx 

- application should be accesible on http://localhost:8080


