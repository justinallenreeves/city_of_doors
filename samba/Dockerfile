#
# Dockerfile for samba (without netbios)
#

FROM alpine:edge

RUN apk add --update \
    samba-common-tools \
    samba-client \
    samba-server \
    && rm -rf /var/cache/apk/*

ENV USER=samba-user
ENV UID=25001
ENV GID=25001

RUN addgroup --gid ${GID} ${USER}

RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "$USER" \
    --no-create-home \
    --uid "$UID" \
    "$USER"

# Read password from file and generate passwords (.pw just plain file)
ADD .pw /
RUN (cat /.pw ; cat /.pw) | passwd ${USER}
RUN (cat /.pw ; cat /.pw) | smbpasswd -a ${USER}
RUN rm .pw

EXPOSE 445/tcp

ENTRYPOINT ["smbd", "--foreground", "--log-stdout", "--no-process-group"]