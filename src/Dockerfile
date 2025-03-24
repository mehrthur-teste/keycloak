# Etapa 1: Build otimizado 21.1.1
FROM quay.io/keycloak/keycloak:21.1.1 AS builder

# Habilita health check e métricas
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

# Define o banco de dados
ENV KC_DB=postgres

WORKDIR /opt/keycloak

# Build otimizado do Keycloak
RUN /opt/keycloak/bin/kc.sh build

# Etapa 2: Execução do Keycloak
FROM quay.io/keycloak/keycloak:21.1.1

# Copia o build otimizado
COPY --from=builder /opt/keycloak/ /opt/keycloak/

# Copia a configuração do cache para dentro do container
COPY cache-ispn.xml /opt/keycloak/conf/cache-ispn.xml

# Configura o banco de dados
ENV KC_DB=postgres
ENV KC_DB_URL=jdbc:postgresql://xxxxxxx:5432/keycloak
ENV KC_DB_USERNAME=xxxxxxxx
ENV KC_DB_PASSWORD=xxxxxxxxx

# Configuração para segurança e reconhecimento correto do HTTPS no ALB shield.smarters.io
ENV KC_CONTENT_SECURITY_POLICY="upgrade-insecure-requests; frame-ancestors 'self' https://... http://...;"
ENV KC_HOSTNAME=...
ENV KC_HOSTNAME_STRICT=true
ENV KC_HOSTNAME_STRICT_HTTPS=true
ENV KC_PROXY=edge
ENV KC_HTTP_ENABLED=true

# # Configuração do redis como cache externo
ENV KC_CACHE=ispn
ENV KEYCLOAK_CACHE_CONFIG_FILE=/opt/keycloak/conf/cache-ispn.xml

# Configuração do admin
ENV KEYCLOAK_ADMIN=12345
ENV KEYCLOAK_ADMIN_PASSWORD=12345###aBC

# Porta exposta do Keycloak (usamos HTTP porque o ALB faz HTTPS)
EXPOSE 8080

# Iniciar corretamente o Keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start"]
