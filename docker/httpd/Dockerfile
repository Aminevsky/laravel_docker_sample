FROM httpd:2.4

ARG CONF_DIRPATH='/usr/local/apache2/conf'

# Load modules 
# For php-fpm
RUN sed -i 's/#\(LoadModule proxy_module modules\/mod_proxy.so\)/\1/' ${CONF_DIRPATH}/httpd.conf
RUN sed -i 's/#\(LoadModule proxy_fcgi_module modules\/mod_proxy_fcgi.so\)/\1/' ${CONF_DIRPATH}/httpd.conf
# For Laravel
RUN sed -i 's/#\(LoadModule rewrite_module modules\/mod_rewrite.so\)/\1/' ${CONF_DIRPATH}/httpd.conf

# Load config file
RUN echo 'Include  conf/vhosts.conf' >> ${CONF_DIRPATH}/httpd.conf

COPY ./docker/httpd/vhosts.conf ${CONF_DIRPATH}
