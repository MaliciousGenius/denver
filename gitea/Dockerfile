ARG GITEA_VERSION

FROM gitea/gitea:${GITEA_VERSION}

# copy scripts
ADD backup.sh /backup.sh
ADD restore.sh /restore.sh

# give execution rights on the scripts
RUN chmod +x /backup.sh /restore.sh
