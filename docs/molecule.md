### ⚙️ Parametry wejściowe (`inputs`)

| Nazwa          | Typ    | Domyślna wartość                                              | Opis                                              |
| -------------- | ------ | ------------------------------------------------------------- | ------------------------------------------------- |
| `docker_image` | string | `registry.gitlab.com/pl.rachuna-net/containers/ansible:1.0.0` | Obraz Dockera z zainstalowanym Molecule i Ansible |

---
### 🧬 Zmienne środowiskowe

| Nazwa zmiennej              | Wartość                      | Opis                                                       |
| --------------------------- | ---------------------------- | ---------------------------------------------------------- |
| `CONTAINER_IMAGE_ANSIBLE`   | `$[[ inputs.docker_image ]]` | Obraz Dockera używany do uruchomienia testu                |
| `TEST_SSH_PUB_KEY`          | `$SSH_PUBLIC_KEY`            | Publiczny klucz SSH do komunikacji z instancjami testowymi |
| `ANSIBLE_HOST_KEY_CHECKING` | `false`                      | Wyłączenie sprawdzania kluczy hostów SSH przez Ansible     |
| `ANSIBLE_FORCE_COLOR`       | `true`                       | Wymuszenie kolorowego outputu Ansible                      |

> [!warning]
> ✅ **Wymagana zmienna środowiskowa**: `SSH_PUBLIC_KEY` (wymagana do komunikacji z hostami testowymi, np. VM w Proxmoxie)

---
### 🧱 Zależności

**Lokalne pliki include:**

* `/source/input_variables_molecule.yml` – konfiguracja zmiennych wejściowych.
* `/source/logo.yml` – logo komponentu.
* `before_script` odwołuje się do:

  * `.ssh_client_config` – konfiguracja klienta SSH,
  * `.logo` – wyświetlanie logo,
  * `.input-variables-molecule` – załadowanie zmiennych,
  * `.ansible_init` – inicjalizacja środowiska testowego.

---
### 🧪 Job: `molecule test`

* Wykonuje `molecule test` w roli Ansible.
* Sprawdza poprawność roli, provisioning, działanie zadań oraz cleanup.
* **Nie** wykonuje się automatycznie – uruchamiany manualnie lub przez inny job.

#### 📜 Skrypt

```bash
molecule test
```

---
### 🧪 Przykład użycia

```yaml
include:
  - component: $CI_SERVER_FQDN/pl.rachuna-net/cicd/components/unit-test/molecule@$COMPONENT_VERSION_UNIT_TEST
    inputs:
      docker_image: $CONTAINER_IMAGE_ANSIBLE

🧪 molecule test:
  rules:
    - if: '$CI_COMMIT_BRANCH == "feature/test-role"'
      when: manual
```