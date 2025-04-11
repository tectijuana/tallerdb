# Requisitos previos

Este playbook está listo para que cualquier estudiante:

- Lance una EC2 Ubuntu 20.04 con acceso SSH.
- Instale Ansible
- Ejecute el playbook con:
```bash
sudo apt install ansible -y

wget URL_De_donde_esta_el_script
ansible-playbook deploy_sqlserver_local.yml
```

##  Setup completo y despliegue de MS SQL Server en localhost `deploy_sqlserver_local.yml`

```yaml
- name: Setup completo y despliegue de MS SQL Server en localhost
  hosts: localhost
  connection: local
  become: yes

  vars:
    mssql_version: "2019"
    sa_password: "StrongPassw0rd!"  # Reemplazar en producción por uso de Ansible Vault
    firewall_allowed_ports:
      - 22     # SSH
      - 1433   # SQL Server

  tasks:

    - name: Actualizar lista de paquetes y sistema completo
      block:
        - name: apt update
          apt:
            update_cache: yes

        - name: apt upgrade
          apt:
            upgrade: dist

        - name: Eliminar paquetes no necesarios
          apt:
            autoremove: yes
            autoclean: yes

    - name: Instalar herramientas necesarias
      apt:
        name:
          - curl
          - gnupg
          - ufw
        state: present

    - name: Permitir puertos necesarios en el firewall
      ufw:
        rule: allow
        port: "{{ item }}"
        proto: tcp
      loop: "{{ firewall_allowed_ports }}"

    - name: Habilitar UFW con política por defecto de denegar
      ufw:
        state: enabled
        policy: deny

    - name: Agregar clave pública del repositorio de Microsoft
      apt_key:
        url: https://packages.microsoft.com/keys/microsoft.asc
        state: present

    - name: Añadir el repositorio de SQL Server
      apt_repository:
        repo: "deb [arch=amd64] https://packages.microsoft.com/ubuntu/20.04/mssql-server-{{ mssql_version }}.list"
        state: present
        filename: "mssql-server"

    - name: Actualizar caché APT después de añadir repo
      apt:
        update_cache: yes

    - name: Instalar MS SQL Server
      apt:
        name: mssql-server
        state: present

    - name: Configurar MS SQL Server
      shell: "/opt/mssql/bin/mssql-conf -n setup accept-eula SA_PASSWORD='{{ sa_password }}' edition=Developer"
      args:
        creates: /var/opt/mssql/mssql.conf

    - name: Iniciar y habilitar el servicio MS SQL Server
      systemd:
        name: mssql-server
        enabled: yes
        state: started
    - name: Instalar herramientas de línea de comando de SQL Server
      block:
        - name: Agregar repositorio de mssql-tools
          shell: |
            curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
            sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/prod.list)"
          args:
            executable: /bin/bash

        - name: Instalar mssql-tools y dependencias
          apt:
            name:
              - mssql-tools
              - unixodbc-dev
            state: present
            update_cache: yes

        - name: Añadir mssql-tools al PATH
          lineinfile:
            path: ~/.bashrc
            line: 'export PATH="$PATH:/opt/mssql-tools/bin"'
            insertafter: EOF
            state: present

        - name: Recargar PATH
          shell: source ~/.bashrc
          args:
            executable: /bin/bash

    - name: Crear base de datos y tabla con datos de ejemplo
      shell: |
        /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '{{ sa_password }}' -Q "
        CREATE DATABASE TecDb;
        USE TecDb;
        CREATE TABLE Alumnos (
          id INT PRIMARY KEY,
          nombre NVARCHAR(50),
          edad INT
        );
        INSERT INTO Alumnos VALUES (1, 'Ana Pérez', 20);
        SELECT * FROM Alumnos;"
      args:
        executable: /bin/bash
```

---

¿Quieres que prepare una guía corta (`README.md`) estilo "paso a paso para alumnos"? 📄✨
