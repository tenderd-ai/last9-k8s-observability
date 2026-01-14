# Platform Environment
./last9-otel-setup.sh \
    endpoint="https://otlp.last9.io:443" \
    token=<token> \
    monitoring-endpoint=<monitoring-endpoint> \
    username=<username> \
    password=<password> \
    env=platform \
    cluster=tenderd-platform-eks

# Stage Environment
./last9-otel-setup.sh \
    endpoint="https://otlp.last9.io:443" \
    token=<token> \
    monitoring-endpoint=<monitoring-endpoint> \
    username=<username> \
    password=<password> \
    env=stage \
    cluster=tenderd-stage-eks

# De-Stage Environment
./last9-otel-setup.sh \
    endpoint="https://otlp.last9.io:443" \
    token=<token> \
    monitoring-endpoint=<monitoring-endpoint> \
    username=<username> \
    password=<password> \
    env=de-stage \
    cluster=tenderd-de-stage-eks
