FROM ubuntu:18.04

# With ModSecurity2
# RUN apt-get update && apt-get install -y nano git apache2 libapache2-mod-security2
# RUN mv /etc/modsecurity/modsecurity.conf-recommended modsecurity.conf
# RUN git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git /tmp/owasp-modsecurity-crs
# RUN mv /tmp/owasp-modsecurity-crs/crs-setup.conf.example /etc/modsecurity/crs-setup.conf
# RUN mv /tmp/owasp-modsecurity-crs/rules/ /etc/modsecurity/
# RUN a2enmod proxy proxy_fcgi proxy_http rewrite userdir headers security2

# Without ModSecurity2
RUN apt-get update && apt-get install -y nano apache2
RUN a2enmod proxy proxy_fcgi proxy_http rewrite userdir headers

COPY ./apache/vhosts.conf /etc/apache2/sites-available/000-default.conf
COPY ./apache/security.conf /etc/apache2/conf-available/security.conf

EXPOSE 80
CMD apachectl -D FOREGROUND
