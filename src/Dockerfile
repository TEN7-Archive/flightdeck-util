FROM alpine:3.14
MAINTAINER tess@ten7.com

# Override some Ansible defaults.
ENV ANSIBLE_ROLES_PATH /ansible/roles
ENV ANSIBLE_NOCOWS 1

# Update the package list and install Ansible.
RUN apk -U upgrade &&\
    apk add --update --no-cache ansible && \
    rm -rf /tmp/* \
           /var/cache/apk/* \
           /usr/lib/python3.6/site-packages/ansible/modules/windows/

# Copy the Ansible configuration files
COPY ansible-hosts /etc/ansible/hosts
COPY ansible.cfg /etc/ansible/ansible.cfg
COPY ansible /ansible

# Run the build.
RUN ansible-galaxy install -fr /ansible/requirements.yml && \
    ansible-playbook -i /ansible/inventories/all.ini /ansible/build.yml

# Switch to the service account.
USER util

# Set the entrypoint and default command.
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["/bin/bash"]
