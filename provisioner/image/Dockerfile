FROM almalinux:8 

# OSG RPMs
RUN dnf -y install https://repo.opensciencegrid.org/osg/24-main/osg-24-main-el8-release-latest.rpm && \
    dnf -y install epel-release dnf-plugins-core && \
    (dnf config-manager --set-enabled crb || dnf config-manager --set-enabled powertools) && \
    dnf -y install less which redhat-lsb-core && \
    dnf -y install python3-pip git && \
    dnf -y install boost-python3 && \
    dnf -y install condor python3-condor && \
    dnf clean all

RUN pip3 install kubernetes


# configure python packages
RUN mkdir -p /opt/provisioner && cd /opt/provisioner && git clone https://github.com/sfiligoi/prp-htcondor-portal.git && cd prp-htcondor-portal && git checkout main

RUN useradd provisioner && chown -R provisioner /opt/provisioner

RUN su provisioner -c '/usr/bin/ln -s /opt/provisioner/prp-htcondor-portal/provisioner/python/prp_provisioner /home/provisioner/prp_provisioner'

ADD provisioner_main.py /home/provisioner/provisioner_main.py
RUN chown provisioner /home/provisioner/provisioner_main.py

ADD setup_k8s_creds.sh /opt/provisioner/setup_k8s_creds.sh
ADD setup_htcondor_creds.sh /opt/provisioner/setup_htcondor_creds.sh
ADD provisioner_startup.sh /opt/provisioner/provisioner_startup.sh

#
# Requires env variables
# K8S_NAMESPACE
#
# Optionally
# CONDOR_HOST
#

CMD ["/opt/provisioner/provisioner_startup.sh"]
