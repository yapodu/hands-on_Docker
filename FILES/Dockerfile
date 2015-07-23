FROM centos:centos6
MAINTAINER Ryo Tamura <tamura@yapodu.jp>

RUN yum install sudo ntpdate -y
RUN rm -f /etc/localtime
RUN ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN ntpdate ntp.nict.jp

RUN rpm -ivh http://ftp-srv2.kddilabs.jp/Linux/distributions/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum install fio -y
ADD FIO /tmp/FIO/

RUN yum install httpd -y
RUN sed -i 's/Defaults.*requiretty/#Default\trequiretty/g' /etc/sudoers
RUN curl -L https://td-toolbelt.herokuapp.com/sh/install-redhat-td-agent2.sh | sh
ADD td-agent.conf /etc/td-agent/td-agent.conf

RUN /etc/init.d/httpd start
RUN chmod 744 /var/log/httpd
RUN chmod g+rx /var/log/httpd
RUN chmod o+x /var/log/httpd
RUN mkdir /var/log/td-agent/httpd/
RUN chown td-agent:td-agent /var/log/td-agent/httpd

# start fluentd and httpd
EXPOSE 80
ENTRYPOINT /etc/init.d/td-agent restart && /etc/init.d/httpd restart && /bin/bash
