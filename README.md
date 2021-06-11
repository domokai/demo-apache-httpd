tagname = v1.0.0, v2.0.0, v3.0.0
docker build --no-cache --progress plain -t domokai/apache-httpd-beta:<tagname> .
docker images
docker tag <id_image> domokai/apache-httpd-beta:<tagname>
docker push domokai/apache-httpd-beta:<tagname>