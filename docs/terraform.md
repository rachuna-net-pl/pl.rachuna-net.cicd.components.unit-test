**Wymagania**
- Obraz kontenera dostępny jest [tutaj](https://gitlab.com/pl.rachuna-net/containers/terraform/container_registry/8516490).

#### **Terraform plan** `✅ terraform plan`  
Job uruchamia `terraform init`, integrując się ze stanem zapisanym w repozytorium GitLab. Następnie, za pomocą polecenia `terraform plan`, przeprowadzana jest analiza zmian, które nastąpiłyby w infrastrukturze po zastosowaniu konfiguracji Terraform.  
