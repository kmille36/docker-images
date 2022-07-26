FROM ubuntu:20.04
MAINTAINER kmille36

ENV DEBIAN_FRONTEND=noninteractive


#RUN apt update && apt install -y xfce4-terminal htop xfce4-goodies
RUN apt-get update && \
    apt-get install -y --no-install-recommends evince file-roller gpicview gtk2-engines-pixbuf xfce4 htop xfce4-goodies xfce4-whiskermenu-plugin xorg xserver-xorg xfce4-indicator-plugin xfce4-terminal numix-icon-theme numix-icon-theme-circle

RUN apt-get install -y xrdp locales supervisor sudo ibus ibus-mozc dbus dbus-x11 firefox

RUN locale-gen en_US && \
    apt-get install -y git tigervnc-standalone-server wget && \
    git clone https://github.com/novnc/noVNC.git /root/noVNC && \
    git clone https://github.com/novnc/websockify.git /root/noVNC/utils/websockify

RUN mkdir -p /var/run/dbus 

RUN sed -i -e 's/LogLevel=DEBUG/LogLevel=CORE/g' /etc/xrdp/xrdp.ini && \
    sed -i -e 's/SyslogLevel=DEBUG/SyslogLevel=CORE/g' /etc/xrdp/xrdp.ini && \
    sed -i -e 's/EnableSyslog=true/EnableSyslog=false/g' /etc/xrdp/xrdp.ini && \
    sed -i -e 's/LogLevel=DEBUG/LogLevel=CORE/g' /etc/xrdp/sesman.ini && \
    sed -i -e 's/SyslogLevel=DEBUG/SyslogLevel=CORE/g' /etc/xrdp/sesman.ini && \
    sed -i -e 's/EnableSyslog=true/EnableSyslog=false/g' /etc/xrdp/sesman.ini

RUN useradd -m -s /bin/bash -G sudo xuser

#COPY Adwaita-dark-Xfce-with-Qbox-mw4 /usr/share/themes/Adwaita-dark-Xfce-with-Qbox-mw4
COPY xfce-perchannel-xml /etc/xdg/xfce4/xfconf/xfce-perchannel-xml
COPY xfce-perchannel-xml /home/xuser/.config/xfce4/xfconf/xfce-perchannel-xml
COPY helpers.rc /etc/xdg/xfce4/helpers.rc

#RUN apt install firefox -y

RUN ln -s /root/noVNC/vnc.html /root/noVNC/index.html && \
    sed -i "s/UI.initSetting('resize', 'off');/UI.initSetting('resize', 'remote');/g" /root/noVNC/app/ui.js && \
    ln -s /usr/share/icons/Numix-Circle /usr/share/icons/KXicons && \ 
   # chown -R xuser /usr/share/themes/Adwaita-dark-Xfce-with-Qbox-mw4 && \
    chown -R xuser /usr/share/icons/KXicons && \
    chown -R xuser /etc/xdg/xfce4/xfconf/xfce-perchannel-xml

RUN echo "xfce4-session" > /etc/skel/.xsession
    
RUN apt-get install -y xfce4-taskmanager mousepad 

RUN wget https://149366088.v2.pressablecdn.com/wp-content/uploads/2022/03/jammy-jellyfish-wallpaper.jpg -O /usr/share/backgrounds/xfce/default.jpg

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY entrypoint.sh .
RUN chmod +x entrypoint.sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ENTRYPOINT ["./entrypoint.sh"]
