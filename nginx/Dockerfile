FROM divadsn/nginx-pagespeed:latest
MAINTAINER David Sn "divad.nnamtdeis@gmail.com"

ENV PHPYMYADMIN_VERSION=4.7.7

# Install phpMyAdmin from source
RUN set -x && \
    URL="https://files.phpmyadmin.net/phpMyAdmin/${PHPYMYADMIN_VERSION}/phpMyAdmin-${PHPYMYADMIN_VERSION}-all-languages.tar.gz" && \
    # Download additional dependencies and phpMyAdmin tarball
    apk add --no-cache curl && \
    curl --output phpMyAdmin.tar.gz --location $URL && \
    apk del --no-cache curl gnupg && \
    # Extract tarball and install it
    tar xzf phpMyAdmin.tar.gz && \
    rm -f phpMyAdmin.tar.gz && \
    mv phpMyAdmin-$VERSION-all-languages /usr/share/phpmyadmin && \
    # Pre-configure phpMyAdmin to use /etc/phpmyadmin
    cd /usr/share/phpmyadmin && \
    rm -rf setup/ examples/ test/ po/ composer.json RELEASE-DATE-$VERSION && \
    sed -i "s@define('CONFIG_DIR'.*@define('CONFIG_DIR', '/etc/phpmyadmin/');@" libraries/vendor_config.php && \
    chown -R root:nobody /usr/share/phpmyadmin && \
    find /usr/share/phpmyadmin -type d -exec chmod 750 {} \; && \
    find /usr/share/phpmyadmin -type f -exec chmod 640 {} \;

VOLUME ["/etc/phpmyadmin"]

# Copy configuration files
COPY config/nginx /etc/nginx
COPY config/phpmyadmin /etc/phpmyadmin

# Open ports and start nginx
EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]

