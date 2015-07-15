#############################################
# ADAM OSSENFORD RASPBERRY PI NGINX       ###
#############################################
FROM hominidae/armhf-wheezy
##############################
# DONT CRY ABOUT KEYS PLEAZ ##
##############################
RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 7638D0442B90D010

##################################
# SETUP ENV AND INSTALL NGINX  ###
##################################
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get install -y ca-certificates nginx && \
    rm -rf /var/lib/apt/lists/*

#############################################################
# forward request and error logs to docker log collector  ###
#############################################################
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

##############################
# SETUP VOLUME FOR NGINX   ###
##############################
VOLUME ["/var/cache/nginx"]

###########################
# EXPOSE HTTP AND HTTPS  ##
###########################
EXPOSE 80 443

############################
# RUN THIS COMMAND AND GO ##
############################
CMD ["nginx", "-g", "daemon off;"]
