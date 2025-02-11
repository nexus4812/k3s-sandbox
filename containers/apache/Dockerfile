FROM httpd:2.4
COPY containers/apache/httpd.conf /usr/local/apache2/conf/httpd.conf
RUN mkdir -p /var/www/public
# COPY web/public /var/www/public # if web app has html, css files

CMD ["apachectl", "-D", "FOREGROUND"]

# Build sample
# docker build -t apache -f containers/apache/Dockerfile .

# Run sample
# docker run -it --rm apache sh
# apachectl start # for test
