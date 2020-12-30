 - We dont want snowflakes, we prefer cattles :) 

 - Mounts: Bind Mounts and Volume Mountsdo 

 - Bind Mount

 ```
 cd static-assets-project
 
 docker run --mount type=bind,source="$(pwd)"/build,target=/usr/share/nginx/html -p 8080:80 nginx
```