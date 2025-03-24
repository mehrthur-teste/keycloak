If you can without Dockerfile you can run this:

[1] - docker network create demo-network

[2] - sudo docker run -d --name keycloak --network demo-network -p 8080:8080 -e KC_BOOTSTRAP_ADMIN_USERNAME=admin -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:latest start-dev
