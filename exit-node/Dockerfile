FROM ubuntu:26.04@sha256:d66b366dac61b575bee2aef07e9c26e8070096fbe84a3d4d3eb0da069b5d76db

RUN apt-get update && \
    apt-get install -y --no-install-recommends wget curl apt-transport-https ca-certificates && \
    wget -qO /etc/apt/trusted.gpg.d/nordvpn_public.asc https://repo.nordvpn.com/gpg/nordvpn_public.asc && \
    echo "deb https://repo.nordvpn.com/deb/nordvpn/debian stable main" > /etc/apt/sources.list.d/nordvpn.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends nordvpn && \
    apt-get install -y --no-install-recommends mtr iputils-ping traceroute less  && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENTRYPOINT /etc/init.d/nordvpn start && sleep 5 && /bin/bash -c "$@"
CMD bash