FROM       nginx:1.11
MAINTAINER Kiran Murugulla <Kiran.Murugulla@hotmail.com>

# Set timezone
RUN echo "America/Toronto" > /etc/timezone \  
 && dpkg-reconfigure -f noninteractive tzdata

# Install wget and install/updates certificates
RUN apt-get update \  
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    wget \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

# Add main NGINX config
COPY nginx.conf /etc/nginx/

# Add virtual hosts
COPY vhosts/ /etc/nginx/conf.d/

# Add static content
COPY html/ /usr/share/nginx/html/  