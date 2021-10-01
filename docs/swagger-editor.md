## Running the image from DockerHub


To use this, run the following: 
```docker pull swaggerapi/swagger-editor
docker run -d -p 80:8080 swaggerapi/swagger-editor
```

This will run Swagger Editor (in detached mode) on port 80 on your machine, so you can open it by navigating to `http://localhost` in your browser.

You can provide your own json or yaml definition file on your host:
`docker run -d -p 80:8080 -v $(pwd):/tmp -e SWAGGER_FILE=/tmp/swagger.json swaggerapi/swagger-editor`

You can provide a API document from your local machine — for example, if you have a file at `./bar/swagger.json`:
`docker run -d -p 80:8080 -e URL=/foo/swagger.json -v /bar:/usr/share/nginx/html/foo swaggerapi/swagger-editor`

You can specify a different base url for accessing the application - for example if you want the application to be available at `http://localhost/swagger-editor/`:
`docker run -d -p 80:8080 -e BASE_URL=/swagger-editor swaggerapi/swagger-editor`
