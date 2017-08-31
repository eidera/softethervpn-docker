FROM amazonlinux:2017.03

RUN yum install -y gcc make ncurses-devel openssl-devel readline-devel

ARG SOFTETHER_VERSION="v4.22-9634-beta"
ARG HUB_NAME="DefaultHub"
ARG HUB_PASSWD="DefaultHubPassword"
ARG USER_NAME="DefaultUser"
ARG USER_PASSWD="DefaultUserPassword"
ARG COMMON_SECRET="commonsec"

RUN curl -L https://github.com/SoftEtherVPN/SoftEtherVPN/archive/${SOFTETHER_VERSION}.tar.gz | tar xzf - && \
    cd SoftEtherVPN-${SOFTETHER_VERSION:1} && \
    cp -f src/makefiles/linux_64bit.mak Makefile && \
    make && \
    make install && \
    make clean && \
    cd / && \
    rm -rf SoftEtherVPN-${SOFTETHER_VERSION:1}

RUN /usr/vpnserver/vpnserver start && \
    sleep 5 && \
    vpncmd /SERVER localhost /ADMINHUB:DEFAULT /CMD HubCreate ${HUB_NAME} /PASSWORD:${HUB_PASSWD} && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD UserCreate ${USER_NAME} /GROUP:none /REALNAME:none /NOTE:none && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD UserPasswordSet ${USER_NAME} /PASSWORD:${USER_PASSWD} && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD IPsecEnable /L2TP:yes /L2TPRAW:no /ETHERIP:no /PSK:${COMMON_SECRET} /DEFAULTHUB:${HUB_NAME} && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD SecureNatEnable && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD NatEnable && \
    vpncmd /SERVER localhost /ADMINHUB:${HUB_NAME} /CMD DHCPEnable && \
    /usr/vpnserver/vpnserver stop

ENTRYPOINT /usr/vpnserver/vpnserver execsvc

EXPOSE 500/udp 4500/udp
